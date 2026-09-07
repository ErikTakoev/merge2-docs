# Locked Areas and Deferred Cells

[← На Головну](../Main.md)

Цей документ описує систему **Заблокованих Ділянок** — механіку обмеження взаємодії гравця з окремими ділянками сітки до їхнього розблокування. Система підтримує два типи заблокованих клітинок: прості (з фішками, що миттєво завантажуються) та відкладені (з фішками, що з'являються після розблокування).

## Overview

**Заблоковані Ділянки** дозволяють дизайнерам створювати прогресивні рівні, де гравець поступово отримує доступ до нових частин поля. Коли ділянка заблокована:

- Клітинки у ділянці мають прапорець `IsBlocked = true`, що блокує будь-яку взаємодію вводу (натиск, перетягування)
- Видимо ділянка закривається ефектами (наприклад, туман, ворота, ланцюги), які управляються `LockedAreaEffect`
- Фішки, розташовані на заблокованих клітинках ділянки, отримують обмеження взаємодії через `EffectBlockingSettings` та відповідні субефекти (наприклад, ланцюги, лід)
- Відкладені фішки не спавнять під час завантаження рівня, а чекають розблокування

Коли ділянка розблокується (через ігровий код або завершення умови):

- Клітинки розблоковуються (`IsBlocked = false`)
- З наявних на ділянці фішок знімаються блокування та деактивуються субефекти ділянки (`RemoveFromChip`), оновлюється стан доступності фішок у колекціях
- `LockedAreaEffect` запускає анімацію деактивації (наприклад, зникнення туману)
- Для комірок типу `DeferredCell` спавнять збережені фішки (одразу якщо ефекти відсутні/force=true, або через Animation Event у `LockedAreaEffect`)

---

## Architecture

### Core Components

#### 1. LockedAreaManager (ILockedAreaManager)
Центральний менеджер, що управляє станом усіх заблокованих ділянок на полі.

**Відповідальність**:
- **Ініціалізація** (`Initialize`): Читає `FieldData.LockedAreas`, блокує всі клітинки закритих ділянок
- **Реєстрація ефектів** (`RegisterEffect`): Приймає реєстрацію від компонентів `ILockedAreaEffect`. Підтримує рівно один ефект на ділянку. Якщо вказаний `LockedAreaId` ефекту не існує у конфігурації поля, об'єкт ефекту автоматично вимикається. Якщо ділянка заблокована, прив'язує клітинки ділянки до ефекту у внутрішньому словнику `activeEffectByCell` та викликає `Activate()`, якщо вже розблокована — викликає `Deactivate(force: true)`
- **Перевірка ефекту за клітинкою** (`TryGetEffect`): Дозволяє перевірити, чи покрита клітинка активним ефектом заблокованої ділянки, та отримати посилання на `ILockedAreaEffect`
- **Застосування блокувань до фішки** (`ApplyToChip`): Якщо клітинка покрита активним ефектом ділянки, викликає `effect.ApplyToChip(chip)`. Використовується `ChipFactory` при появі нової фішки на заблокованій клітинці
- **Розблокування** (`UnlockArea`): Видаляє клітинки ділянки зі словника активних ефектів, розблоковує клітинки (`IsBlocked = false`), знімає блокування та субефекти з усіх наявних фішок ділянки (`effect.RemoveFromChip`) та оновлює стан їх доступності в `IChipCollections`. Якщо `force` або для ділянки відсутній ефект, спавнить відкладені фішки через `SpawnDeferredChips(areaId)`. Запускає анімацію деактивації ефекту `Deactivate(force)`
- **Спавн відкладених фішок** (`SpawnDeferredChips`): Публічний ідемпотентний метод для спавну фішок на ділянці `areaId`. Гарантує, що спавн відбувається лише один раз для кожної ділянки

#### 2. LockedAreaEffect (ILockedAreaEffect)
Візуальний компонент, що представляє графічне покриття заблокованої ділянки та імплементує `ILockedAreaEffect` (наслідує `MonoBehaviour, ILockedAreaEffect`).

**Структура**:
- **`lockedAreaId`** (int): Унікальний ID ділянки, до якої належить ефект
- **`blockingSettings`** (`EffectBlockingSettings`): Налаштування блокування взаємодій (перетягування, злиття тощо), які накладаються на покриті фішки через `Chip.BlockingState`
- **`subEffects`** (`SubEffect[]`): Конфігурація додаткових чіп-ефектів, що активуються на фішках у зоні. Кожен елемент містить `effectId`, а також необов'язкові затримки `activateDelay` та `deactivateDelay` у секундах
- **`animator`** (`Animator`): Компонент Animator з тригерами `"Activate"` (показ) та `"Deactivate"` (приховування)

**Методи**:
- **`Init()`**: Реєструє компонент у менеджері через `lockedAreaManager.RegisterEffect(this)`. Викликається з `VisualField` або `IsoVisualField` під час створення візуалу рівня
- **`Activate()`**: Скидає тригер `"Deactivate"` та запускає анімацію появи через `Animator.SetTrigger("Activate")`
- **`Deactivate(bool force = false)`**: Скидає тригер `"Activate"`. При `force = true` миттєво переводить аніматор у стан `"Deactivate"` без переходу, інакше запускає тригер `"Deactivate"`
- **`ApplyToChip(Chip chip)`**: Застосовує `blockingSettings` до блокуючого стану фішки (`chip.BlockingState.ApplyBlock`) та активує субефекти на фішці через `chip.GetEffect(se.effectId)?.Activate(chip)` (з підтримкою корутини затримки `activateDelay`)
- **`RemoveFromChip(Chip chip, bool force = false)`**: Знімає `blockingSettings` з блокуючого стану фішки (`chip.BlockingState.RemoveBlock`) та деактивує субефекти (з підтримкою затримки `deactivateDelay` або негайно при `force = true`)
- **`SpawnDeferredChips()`**: Може викликатися через Animation Event під час анімації розблокування або з коду. Делегує виклик `lockedAreaManager.SpawnDeferredChips(lockedAreaId)`

#### 3. DeferredCell
Додатковий `MonoBehaviour` компонент, що зберігає конфігурацію фішки та спавнить її після розблокування ділянки. Додається на той же GameObject, що й регулярна `Cell` або `IsoCell`.

**Методи**:
- **`Init(ICell cell)`**: Зв'язує компонент з власником (`Cell` або `IsoCell`)
- **`SetupDeferredChip(FieldData.CellData cellData, ChipData chipData)`**: Зберігає конфігурацію фішки для подальшого спавну
- **`SpawnDeferredChip()`**: Спавнить фішку через `ChipFactory` з накопленими даними. Викликається `LockedAreaManager.SpawnDeferredChips`
- **`TryGet(ICell cell, out DeferredCell deferredCell)`**: Статичний метод-помічник для пошуку компонента на `ICell`

---

## Data Structures

### FieldData.LockedAreaData
```csharp
public struct LockedAreaData
{
    public int LockedAreaId;                      // Унікальний ID ділянки
    public Vector2Int[] CellsToLock;             // Прості заблоковані клітинки
    public Vector2Int[] CellsToLockAndDeferred;  // Відкладені заблоковані клітинки
}
```

---

## Initialization and Lifecycle

### Scene Initialization Order (Merge2Initializer)

Під час запуску сцени відбувається послідовність кроків:

1. **`chipFactory.Init(...)`**: Отримує залежності, включаючи `ILockedAreaManager`.
2. **`FieldInitializeCommand.CreateField()`**: Створює сітку комірок. Для координат у `CellsToLockAndDeferred` додає компонент `DeferredCell`.
3. **`LockedAreaManager.Initialize()`**: Блокує комірки на основі `FieldData.LockedAreas`.
   > **Важливо**: На цьому кроці `ILockedAreaEffect` компоненти ще не зареєстровані (вони реєструються на Кроці 4).
4. **`FieldInitializeCommand.CreateLevelVisual()`**: Інстанціює `LevelVisualPrefab`. Кожен компонент `ILockedAreaEffect` під час `Init()` реєструється у менеджері (`RegisterEffect`) і одразу синхронізує свій стан (активує або деактивує візуал, а також зв'язує активні клітинки з ефектом).
5. **`FieldInitializeCommand.LoadChips()`**: Для звичайних комірок спавнить фішки через `ChipFactory.CreateChip`. Якщо комірка заблокована (`cell.IsBlocked`), фабрика автоматично застосовує до фішки налаштування блокування та субефекти через `lockedAreaManager.ApplyToChip(chip, cell)`. Для комірок з `DeferredCell` викликає `SetupDeferredChip()` (зберігає конфігурацію без створення фішки).

---

## Unlocking and Deferred Chip Spawning

### Method UnlockArea(int areaId, bool force = false)
- **Розблокування комірок**: Видаляє координати ділянки з `activeEffectByCell`, встановлює `IsBlocked = false` для комірок ділянки.
- **Зняття блокувань та ефектів з фішок**: Через `RemoveEffectFromAreaChips` знімає блокування та субефекти з наявних фішок (`effect.RemoveFromChip`) і оновлює стан їх доступності в колекціях (`chipCollections.OnChipBlockingChanged`).
- **Спавн відкладених фішок**: Якщо `force = true` або для ділянки відсутній зареєстрований ефект, спавнить відкладені фішки через `SpawnDeferredChips(areaId)`.
- **Деактивація ефектів**: Викликає `DeactivateEffects(areaId, force)` для запуску анімації відкриття.

### Method SpawnDeferredChips(int areaId)
- **Ідемпотентність**: За допомогою `spawnedDeferredAreaIds` гарантується, що фішки спавняться рівно один раз.
- **Розблокування та спавн**: Переконується, що комірки розблоковані, після чого викликає `deferredCell.SpawnDeferredChip()` для кожної відкладеної комірки ділянки.
- **Джерела виклику**: Викликається напряму з `UnlockArea` (якщо `force` або `!hasEffects`) або з `LockedAreaEffect.SpawnDeferredChips()` через Animation Event під час анімації розблокування.

---

## Integration with Level Editor

Налаштування заблокованих ділянок сітки, відкладених фішок (Deferred Cells), а також відповідні режими редактора описано в розділі **[Editors](../Editors.md)**.
