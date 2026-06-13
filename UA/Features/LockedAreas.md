# Заблоковані ділянки (Locked Areas) та відкладені фішки (Deferred Cells)

[← На Головну](../Main.md)

Цей документ описує систему **Заблокованих Ділянок** — механіку обмеження взаємодії гравця з окремими ділянками сітки до їхнього розблокування. Система підтримує два типи заблокованих клітинок: прості (з фішками, що миттєво завантажуються) та відкладені (з фішками, що з'являються після розблокування).

## Огляд

**Заблоковані Ділянки** дозволяють дизайнерам створювати прогресивні рівні, де гравець поступово отримує доступ до нових частин поля. Коли ділянка заблокована:

- Клітинки у ділянці мають прапорець `IsBlocked = true`, що блокує будь-яку взаємодію вводу (натиск, перетягування)
- Видимо ділянка закривається ефектами (наприклад, туман, ворота, ланцюги), які управляються `LockedAreaEffect`
- Відкладені фішки не спавнять під час завантаження рівня, а чекають розблокування

Коли ділянка розблокується (через ігровий код або завершення умови):

- Клітинки розблоковуються (`IsBlocked = false`)
- `LockedAreaEffect` запускає анімацію деактивації (наприклад, зникнення туману)
- Для комірок типу `DeferredCell` миттєво спавнять збережені фішки

---

## Архітектура

### Основні компоненти

#### 1. `LockedAreaManager` (ILockedAreaManager)
Центральний менеджер, що управляє станом усіх заблокованих ділянок на полі.

**Відповідальність**:
- **Ініціалізація** (`Initialize`): Читає `FieldData.LockedAreas`, блокує всі клітинки закритих ділянок
- **Реєстрація ефектів** (`RegisterEffect`): Приймає реєстрацію від компонентів `LockedAreaEffect` та одразу викликає `Activate()` або `Deactivate()` залежно від поточного стану
- **Розблокування** (`UnlockArea`): Розблоковує всі клітинки ділянки, спавнить відкладені фішки, запускає анімацію деактивації ефектів

**DI Ін'єкція**:
```csharp
// У Merge2Initializer або будь-якому gameplay-компоненті
[Inject] private readonly ILockedAreaManager lockedAreaManager;

// Ініціалізація під час старту сцени
lockedAreaManager.Initialize();

// Розблокування ділянки під час гри
lockedAreaManager.UnlockArea(areaId: 1);
```

#### 2. `LockedAreaEffect`
Візуальний компонент, що представляє графічне покриття заблокованої ділянки.

**Структура**:
- **`lockedAreaId`** (int): Унікальний ID ділянки, до якої належить ефект
- **`Animator`**: Компонент Animator з тригерами `"Activate"` (показ) та `"Deactivate"` (приховування)

**Методи**:
- **`Init(Chip chip, int effectId)`**: Викликається `FieldInitializeCommand.CreateLevelVisual()` після інстанціації рівневої візуалізації. Зберігає `effectId` та реєструє себе у менеджері:
  ```csharp
  this.effectId = effectId;
  lockedAreaManager.RegisterEffect(this);
  ```
  > **Важливо**: `base.Init()` **не викликається** — базова реалізація читає `chip.Data` і деактивує чіп-ефекти, що не підходить для рівневих візуалів.
- **`Activate(Chip chip)`**: Активує GameObject та запускає анімацію через `Animator.SetTrigger("Activate")`. `chip` може бути `null` (рівневі ефекти не прив'язані до фішки)
- **`Deactivate(Chip chip, bool force)`**: Запускає анімацію через `Animator.SetTrigger("Deactivate")` (зазвичай з фейд-аутом). `chip` може бути `null`
- **`FadeOutParticles(float duration)`**: Запускається через анімаційний івент під час деактивації. Зупиняє спавн нових часток та плавно зменшує їхню видимість (альфа-канал) до нуля протягом вказаного часу `duration` (в секундах), запобігаючи різким перепадам у розмірі або кольорі.


#### 3. `DeferredCell` (компонент-доповнення до Cell)
Додатковий `MonoBehaviour` компонент, що зберігає конфігурацію фішки та спавнить її після розблокування ділянки. Додається на той же GameObject, що й регулярна `Cell` або `IsoCell`.

**Методи**:
- **`Init(ICell cell)`**: Зв'язує компонент з власником (`Cell` або `IsoCell`). Викликається `FieldGrid` одразу після додавання компонента
- **`SetupDeferredChip(FieldData.CellData cellData, ChipData chipData)`**: Зберігає конфігурацію фішки для подальшого спавну (викликається під час `FieldInitializeCommand.LoadChips`)
- **`SpawnDeferredChip()`**: Спавнить фішку через `ChipFactory` з накопленими даними. Викликається `LockedAreaManager.UnlockArea`. Повертає створену фішку або `null`
- **`TryGet(ICell cell, out DeferredCell deferredCell)`**: Статичний метод-помічник для пошуку компонента на `ICell`

**Особливості**:
- Не реалізує сам `ICell` — виступає лише як накопичувач даних
- Дані фішки зберігаються внутрішньо до спавну, фішка не знаходиться у `ownerCell.Chip` до розблокування
- При спавну копіює стан `BlockerEffectIds` з `FieldData` для консистентності з регулярним завантаженням



## Структури даних

### `FieldData.LockedAreaData`
```csharp
[System.Serializable]
public struct LockedAreaData
{
    public int LockedAreaId;                      // Унікальний ID ділянки
    public Vector2Int[] CellsToLock;             // Прості заблоковані клітинки
    public Vector2Int[] CellsToLockAndDeferred;  // Відкладені заблоковані клітинки
}
```

**Поля**:
- **`LockedAreaId`**: Унікальний ідентифікатор для розрізнення ділянок
- **`CellsToLock`**: Координати клітинок, що блокуються, але фішки на них завантажуються миттєво (якщо визначені у `FieldChipData`)
- **`CellsToLockAndDeferred`**: Координати клітинок, на яких фішки не завантажуються до розблокування. `FieldGrid` автоматично додає компонент `DeferredCell` до звичайного Cell-GameObject

**Інтеграція у FieldData**:
```csharp
public class FieldData : ScriptableObject
{
    [SerializeField] private LockedAreaData[] lockedAreas;
    public LockedAreaData[] LockedAreas => lockedAreas;

#if UNITY_EDITOR
    // Editor-only: викликається LevelEditorWindow при збереженні рівня
    public void SetLockedAreas(LockedAreaData[] newLockedAreas) => lockedAreas = newLockedAreas;
#endif
}
```

---

## Ініціалізація та Життєвий цикл

### Порядок ініціалізації сцени (Merge2Initializer)

Під час запуску сцени відбувається послідовність кроків:

#### Крок 1: `FieldInitializeCommand.CreateField()`
Створює сітку комірок:

1. **Побудова набору для відкладених комірок** (`BuildDeferredCellsSet`)
2. **Інстанціація комірок**: 
   - Спавнить стандартний `Cell` або кастомний префаб з `CellPrefabCollection`
   - Для координат у `deferredCellsSet` **додає компонент** `DeferredCell` на той же GameObject `Cell`
   - Регулярні комірки залишаються без цього компонента
3. **DI Ін'єкція**: Кожна комірка (та `DeferredCell`) отримує залежності через `resolver.Inject()`

#### Крок 2: `LockedAreaManager.Initialize()`
Блокує комірки на основі `FieldData.LockedAreas`:

```csharp
foreach (FieldData.LockedAreaData area in fieldData.LockedAreas)
{
    if (!unlockedAreaIds.Contains(area.LockedAreaId))
    {
        SetAreaBlocked(area, true);
    }
}
```

> **Важливо**: На цьому кроці `LockedAreaEffect` компоненти ще **не зареєстровані**. Ефекти реєструються пізніше — у Кроці 3.

#### Крок 3: `FieldInitializeCommand.CreateLevelVisual()`
Інстанціює `LevelVisualPrefab` та ініціалізує рівневу візуалізацію:

1. Спавнить `fieldData.LevelVisualPrefab` через `resolver.Instantiate()`
2. Викликає `IVisualField.InitVisualField()` на кореневому компоненті
3. **Кожен `LockedAreaEffect`** під час своєї ініціалізації (`Init`) реєструється у менеджері:
   ```csharp
   this.effectId = effectId;
   lockedAreaManager.RegisterEffect(this);
   ```
4. `RegisterEffect` **одразу синхронізує** стан ефекту — викликає `Activate(null)` або `Deactivate(null, force: true)` залежно від поточного стану `unlockedAreaIds`

#### Крок 4: `FieldInitializeCommand.LoadChips()`
Завантажує фішки з `FieldData`:

1. **Для звичайних комірок**: Спавнить фішку миттєво через `ChipFactory.CreateChip(cell, chipData)`

2. **Для комірок з `DeferredCell` компонентом**: 
   ```csharp
   if (DeferredCell.TryGet(cell, out var deferredCell))
       deferredCell.SetupDeferredChip(cellData, chipData);
   else
       ChipFactory.CreateChip(cell, chipData);
   ```
   - `SetupDeferredChip()` зберігає конфігурацію, але не створює фішку
   - Фішка буде спавнена пізніше при `LockedAreaManager.UnlockArea()`

---

## Розблокування та Спавн відкладених фішок

### Метод `UnlockArea(int areaId, bool force = false)`

Розблоковує ділянку та запускає всі пов'язані дії:

```csharp
public void UnlockArea(int areaId, bool force = false)
{
    if (unlockedAreaIds.Contains(areaId) && !force)
        return;

    if (!TryGetArea(areaId, out FieldData.LockedAreaData area))
    {
        Debug.LogWarning($"LockedAreaManager: Area {areaId} not found");
        return;
    }

    // 1. Розблокування всіх комірок ділянки
    unlockedAreaIds.Add(areaId);
    SetAreaBlocked(area, false);

    // 2. Спавн відкладених фішок (лише для CellsToLockAndDeferred)
    SpawnDeferredChips(area);

    // 3. Деактивація візуальних ефектів
    DeactivateEffects(areaId, force);
}
```

**Параметри**:
- **`areaId`**: ID ділянки для розблокування
- **`force`** (опціонально): Перевитісняє перевірки умов розблокування (якщо такі є)

**Послідовність подій**:

1. **Розблокування комірок** (`IsBlocked = false`):
   - `Cell.OnTap`, `Cell.OnDragStart`, `Cell.OnDrag`, `Cell.OnDragEnd` більше не ігнорують вводи
   - `FieldGrid` дозволяє розміщувати фішки на цих комірках

2. **Спавн відкладених фішок** (`DeferredCell.SpawnDeferredChip()`):
   - Викликає `ChipFactory.CreateChip(ownerCell, deferredChipData, ...)`
   - Фішка розміщується у клітинці через власника (`ownerCell`)
   - Копіює стан `BlockerEffectIds` з вихідних `FieldData`
   - Спамляються всі стандартні подій (CellObserver сповіщення, Init, InitEffects)

3. **Деактивація ефектів** (`LockedAreaEffect.Deactivate(null, immediate)`):
   - Запускається Animator тригер `"Deactivate"`
   - Якщо `force=true` (передається з параметра `UnlockArea`), анімація може програтися миттєво
   - GameObject залишається на сцені (або знищується по завершенню анімації, залежно від реалізації)

---

## Інтеграція з рівневим редактором

Налаштування заблокованих ділянок сітки, відкладених фішок (Deferred Cells), а також відповідні режими редактора, малювання на сітці та команди Undo/Redo описано в загальному розділі **[Editors](../Editors.md)**.

---

## Валідація даних

### Правила валідації (FieldData.ValidateLevel)

Перед збереженням рівня перевіряються:

1. **Межі сітки**: Всі координати комірок у `LockedAreaData` перевіряються окремо для `CellsToLock` та `CellsToLockAndDeferred`
   ```csharp
   foreach (var cell in area.CellsToLock)
   {
       if (cell.x < 0 || cell.x >= fieldSize.x || cell.y < 0 || cell.y >= fieldSize.y)
           errors.Add($"Locked Area {area.LockedAreaId}: cell {cell} is outside field bounds {fieldSize}");
   }
   foreach (var cell in area.CellsToLockAndDeferred)
   {
       if (cell.x < 0 || cell.x >= fieldSize.x || cell.y < 0 || cell.y >= fieldSize.y)
           errors.Add($"Locked Area {area.LockedAreaId}: deferred cell {cell} is outside field bounds {fieldSize}");
   }
   ```

2. **Взаємна виключність**: Жодна комірка не повинна перебувати одночасно в `CellsToLock` та `CellsToLockAndDeferred` однієї ділянки
   ```csharp
   foreach (var cell in area.CellsToLock)
   {
       if (Array.IndexOf(area.CellsToLockAndDeferred, cell) >= 0)
           errors.Add($"Locked Area {area.LockedAreaId}: cell {cell} exists in both CellsToLock and CellsToLockAndDeferred");
   }
   ```

3. **Унікальність ID**: Перевірка унікальності `LockedAreaId` виконується на рівні редактора (автоінкремент при додаванні нової ділянки через `+`). `ValidateLevel` цю перевірку не реалізує

---

## VContainer Реєстрація

### Реєстрація у LifetimeScope

```csharp
// У Merge2LifetimeScope
public class Merge2LifetimeScope : LifetimeScope
{
    protected override void Configure(IContainerBuilder builder)
    {
        // ... інші реєстрації ...
        
        builder.Register<LockedAreaManager>(Lifetime.Singleton)
            .As<ILockedAreaManager>();
        
        builder.RegisterEntryPoint<Merge2Initializer>();
    }
}
```

### Ініціалізація у Merge2Initializer

```csharp
// Merge2Initializer реалізує IInitializable (VContainer), а не MonoBehaviour
public class Merge2Initializer : IInitializable
{
    [Inject] private readonly ILockedAreaManager lockedAreaManager;
    [Inject] private readonly IFieldInitializeCommand fieldInitializeCommand;
    
    public void Initialize()
    {
        // 1. Створюємо сітку комірок + DeferredCell компоненти
        fieldInitializeCommand.CreateField();
        
        // 2. Блокуємо комірки відповідно до FieldData.LockedAreas
        lockedAreaManager.Initialize();
        
        // 3. Спавнимо рівневу візуалізацію та реєструємо LockedAreaEffect-и
        fieldInitializeCommand.CreateLevelVisual();
        
        // 4. Завантажуємо фішки (відкладені зберігаються, не спавняться)
        fieldInitializeCommand.LoadChips();
    }
}
```

**Важливо**: Порядок кроків критичний:
- `Initialize()` **після** `CreateField()` — комірки мають існувати до блокування
- `CreateLevelVisual()` **після** `Initialize()` — ефекти реєструються з уже відомим станом блокування і одразу синхронізуються через `RegisterEffect`
- `LoadChips()` **останнім** — відкладені комірки вже заблоковані та готові приймати `SetupDeferredChip`

---

## Приклад: Розблокування ділянки під час гри

```csharp
// У будь-якому компоненті гри
public class LevelController : MonoBehaviour
{
    [Inject] private readonly ILockedAreaManager lockedAreaManager;
    
    public void OnQuestCompleted(int questAreaId)
    {
        // Розблокувати ділянку після завершення квесту
        lockedAreaManager.UnlockArea(questAreaId);
        
        // Логування (опціонально)
        Debug.Log($"Area {questAreaId} unlocked!");
    }
}
```