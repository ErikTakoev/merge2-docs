# Locked Areas and Deferred Cells

[← На Головну](../Main.md)

Цей документ описує систему **Заблокованих Ділянок** — механіку обмеження взаємодії гравця з окремими ділянками сітки до їхнього розблокування. Система підтримує два типи заблокованих клітинок: прості (з фішками, що миттєво завантажуються) та відкладені (з фішками, що з'являються після розблокування).

## Overview

**Заблоковані Ділянки** дозволяють дизайнерам створювати прогресивні рівні, де гравець поступово отримує доступ до нових частин поля. Коли ділянка заблокована:

- Клітинки у ділянці мають прапорець `IsBlocked = true`, що блокує будь-яку взаємодію вводу (натиск, перетягування)
- Видимо ділянка закривається ефектами (наприклад, туман, ворота, ланцюги), які управляються `LockedAreaEffect`
- Відкладені фішки не спавнять під час завантаження рівня, а чекають розблокування

Коли ділянка розблокується (через ігровий код або завершення умови):

- Клітинки розблоковуються (`IsBlocked = false`)
- `LockedAreaEffect` запускає анімацію деактивації (наприклад, зникнення туману)
- Для комірок типу `DeferredCell` спавнять збережені фішки (одразу якщо ефекти відсутні/force=true, або через Animation Event у `LockedAreaEffect`)

---

## Architecture

### Core Components

#### 1. `LockedAreaManager` (ILockedAreaManager)
Центральний менеджер, що управляє станом усіх заблокованих ділянок на полі.

**Відповідальність**:
- **Ініціалізація** (`Initialize`): Читає `FieldData.LockedAreas`, блокує всі клітинки закритих ділянок
- **Реєстрація ефектів** (`RegisterEffect`): Приймає реєстрацію від компонентів `LockedAreaEffect` та одразу викликає `Activate()` або `Deactivate()` залежно від поточного стану. Якщо вказаний `LockedAreaId` ефекту не існує у конфігурації поля, ефект автоматично вимикається
- **Розблокування** (`UnlockArea`): Розблоковує всі клітинки ділянки. Якщо `force` або для ділянки відсутні ефекти (`!hasEffects`), спавнить відкладені фішки через `SpawnDeferredChips(areaId)`. Запускає анімацію деактивації ефектів `DeactivateEffects(areaId, force)`
- **Спавн відкладених фішок** (`SpawnDeferredChips`): Публічний ідемпотентний метод для спавну фішок на ділянці `areaId`. Гарантує, що спавн відбувається лише один раз для кожної ділянки

#### 2. `LockedAreaEffect`
Візуальний компонент, що представляє графічне покриття заблокованої ділянки.

**Структура**:
- **`lockedAreaId`** (int): Унікальний ID ділянки, до якої належить ефект
- **`Animator`**: Компонент Animator з тригерами `"Activate"` (показ) та `"Deactivate"` (приховування)

**Методи**:
- **`Init(Chip chip, int effectId)`**: Зберігає `effectId` та реєструє себе у менеджері через `RegisterEffect(this)`
  > **Важливо**: `base.Init()` **не викликається** — базова реалізація читає `chip.Data` і деактивує чіп-ефекти, що не підходить для рівневих візуалів.
- **`Activate(Chip chip)`**: Активує GameObject та запускає анімацію через `Animator.SetTrigger("Activate")`
- **`Deactivate(Chip chip, bool force)`**: Запускає анімацію через `Animator.SetTrigger("Deactivate")`
- **`SpawnDeferredChips()`**: Може викликатися через Animation Event під час анімації розблокування або з коду. Делегує виклик `lockedAreaManager.SpawnDeferredChips(lockedAreaId)`
- **`FadeOutParticles(float duration)`**: Запускається через анімаційний івент під час деактивації. Зупиняє спавн нових часток та плавно зменшує їхню видимість до нуля за вказаний час `duration`


#### 3. `DeferredCell`
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

1. **`FieldInitializeCommand.CreateField()`**: Створює сітку комірок. Для координат у `CellsToLockAndDeferred` додає компонент `DeferredCell`.
2. **`LockedAreaManager.Initialize()`**: Блокує комірки на основі `FieldData.LockedAreas`.
   > **Важливо**: На цьому кроці `LockedAreaEffect` компоненти ще не зареєстровані (вони реєструються на Кроці 3).
3. **`FieldInitializeCommand.CreateLevelVisual()`**: Інстанціює `LevelVisualPrefab`. Кожен `LockedAreaEffect` під час `Init()` реєструється у менеджері (`RegisterEffect`) і одразу синхронізує свій стан (активує або деактивує візуал).
4. **`FieldInitializeCommand.LoadChips()`**: Для звичайних комірок спавнить фішки миттєво, а для комірок з `DeferredCell` викликає `SetupDeferredChip()` (зберігає конфігурацію без спавну).

---

## Unlocking and Deferred Chip Spawning

### Method UnlockArea(int areaId, bool force = false)
- **Розблокування комірок**: Встановлює `IsBlocked = false` для комірок ділянки та оновлює стан доступності фішок у колекціях (`chipCollections.OnChipBlockingChanged`).
- **Спавн відкладених фішок**: Якщо `force = true` або для ділянки відсутні ефекти (`!hasEffects`), спавнить відкладені фішки через `SpawnDeferredChips(areaId)`.
- **Деактивація ефектів**: Викликає `DeactivateEffects(areaId, force)` для запуску анімації відкриття.

### Method SpawnDeferredChips(int areaId)
- **Ідемпотентність**: За допомогою `spawnedDeferredAreaIds` гарантується, що фішки спавняться рівно один раз.
- **Розблокування та спавн**: Переконується, що комірки розблоковані, після чого викликає `deferredCell.SpawnDeferredChip()` для кожної відкладеної комірки ділянки.
- **Джерела виклику**: Викликається напряму з `UnlockArea` (якщо `force` або `!hasEffects`) або з `LockedAreaEffect.SpawnDeferredChips()` через Animation Event під час анімації розблокування.

---

## Integration with Level Editor

Налаштування заблокованих ділянок сітки, відкладених фішок (Deferred Cells), а також відповідні режими редактора описано в розділі **[Editors](../Editors.md)**.
