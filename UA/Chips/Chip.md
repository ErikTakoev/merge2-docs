# Chip (Base Chip)

[← На Головну](../Main.md)

Базовий клас `Chip` є візуальним представленням та компонентом взаємодії для об'єктів на ігровому полі. Він відповідає за відображення стану, ефектів та обробку Unity подій (Input).

Сама логіка злиття (Merge) та переміщення винесена у відповідні логічні класи.

## Architecture and Responsibility

### 1. `Chip.cs` (Base Class)
Клас `Chip` є візуальним представленням та базовим компонентом.
- **Data (Configuration - `ChipData`)**: Зберігає посилання на `ChipData`, який містить налаштування:
  - **Type**: Ідентифікатор типу фішки (string).
  - **PrefabLink**: Посилання на префаб фішки.
  - **Size**: Розмір фішки в клітинках (Vector2Int).
  - **MergeData**: доступ до merge-конфігурації. Під час `Init(ChipData, ChipRuntimeData)` чіп кешує `data.GetSpecialData<ChipMergeData>()`.
  - **specialDatas**: Поліморфна колекція для додаткових типізованих налаштувань чіпа.
  
- **Special Data**:
  - **GetSpecialData<T>()**: Типізований доступ до елемента `specialDatas`.
  - **IChipSpecialData**: Базовий контракт для спеціалізованих даних. Реалізації: `ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, `ChipExtraEffectsData`.

- **Runtime**:
  - **CellPosition**: Поточна позиція фішки на сітці поля (Vector2Int). Оновлюється системою при переміщенні.
  - **RuntimeData**: Поточний стан (див. нижче).
  - **BlockingState**: (`CombinedBlockingState`) агрегований стан дозволів (наприклад, `CanBeMoved`, `CanBeMergedAsSource`), що визначається активними ефектами.
- **Visual Management**:
  - **SortingLayer** (`IChipSortingLayer`): Керує шарами сортування декількох рендерерів чіпа, забезпечуючи коректне відображення під час руху.
  - **AnimationNode** (`Transform`): Посилання на вузол анімації фішки, куди прикріплюються візуальні ефекти (типу `ParentChipAnimationNode`), що мають рухатися разом із фішкою.
- **Others**:
  - **LogEnable**: Прапорець для ввімкнення логування подій чіпа в консоль.
- **Effects**: Керується централізованою системою на основі `Dictionary<int, IEffect>` з хеш-ключами від `EffectConsts`.  
  Для повного каталогу див. [Visual Effects](../Visuals/Effects.md). Детальніше про логіку блокувань та руйнування див. [Chip Effect Blockers](../Features/ChipEffectBlockers.md).
- **Animations**: Має посилання на `Animator` для відтворення станів (наприклад, `Merge`, `Generate`, `MoveLocked`).

## Effect Management

Базовий клас `Chip` автоматично керує та розсилає сповіщення всім візуальним ефектам через централізовану систему на основі хеш-словника.

### Effect Storage & Access
- **`effects` (Dictionary<int, IEffect>)**: Словник всіх активних ефектів чіпа. Ключі — це хеш-коди з класу `EffectConsts`, що забезпечують типобезпечний доступ без необхідності пошуку по типу.
- **`GetEffect(int effectHash)`**: Отримує ефект за його EffectConsts ключем. Повертає `null` якщо не знайдено:
  ```csharp
  GetEffect(EffectConsts.MoveLocked)?.SendTrigger("MoveLocked", true);
  ```
- **`GetEffect<T>(int effectHash) where T : IEffect`**: Типізований доступ до ефекту з приведенням типу. Часто використовується для спеціалізованих інтерфейсів:
  ```csharp
  var containerEffect = GetEffect<IEffectContainer>(EffectConsts.ContainerRequirements);
  containerEffect?.UpdateElements(this, containers, false);
  ```

### Effect Initialization
- **`InitEffects()`**: Віртуальний метод, викликаний з `Init(...)` для ініціалізації всіх ефектів. Базова реалізація:
  1. Ітерує `ChipExtraEffectsData.Blockers` — для кожного елемента, чий `EffectId` є в `runtimeData.EffectEnables`, інстантіює префаб і додає в словник ефектів через `AddEffect`
  2. Створює та додає `CellHighlightEffect` з `ChipData.CellHighlightPrefab` (ключ: `EffectConsts.CellHighlight`)
  3. Створює та додає `ChipMergeAvailableEffect` з `ChipData.MergeAvailableEffectPrefab` (ключ: `EffectConsts.MergeAvailable`)
  4. Якщо вказано `ShadowEffectPrefab`, створює та додає `ShadowEffect` (ключ: `EffectConsts.ShadowEffect`)
  
  Цей метод призначений для перекриття в похідних класах (наприклад, `ChipGenerator` додає `GeneratorCharging` та `GeneratorCharged`).

- **`AddEffect(IEffect effect, int effectHash, bool activate)`**: Додає ефект до словника та опціонально активує його:
  ```csharp
  var effect = InstantiateEffect<IEffect>(data.CellHighlightPrefab);
  AddEffect(effect, EffectConsts.CellHighlight, true);
  ```

### Ефект-константи (EffectConsts)
Вся система ефектів використовує централізовані цілочисельні константи, що визначені у [EffectConsts.cs](../../../Core/Scripts/Chips/Effects/EffectConsts.cs):
- **Базові ефекти (1-7)**: `MergeAvailable`, `CellHighlight`, `ContainerRequirements`, `GeneratorCharged`, `GeneratorCharging`, `PBoosterConnectorCells`, `PBoosterJoin`
- **Blocker-ефекти (101+)** — `EffectConsts.Blockers`: `BoxEffect` (101), `ChainsEffect` (102), `MoveLockedEffect` (103)
- **Утиліти**: `GetIdByName(string)` — резолв рядкової назви в ID через словник `nameToId`

### Effect Lifecycle
- Всі ефекти, додані до словника `effects`, автоматично отримують сповіщення через методи `OnChangedCell()`, `OnInteractionOverCellChanged()` та `OnInteractionUnderCellChanged()`.
- **Effect Destroying**: Ефекти з `DestroyingSettings` підтримують поступове руйнування при сусідніх злиттях (детальніше: [Chip Effect Blockers](../Features/ChipEffectBlockers.md#effect-destroying-system)):
  - `InitDestroyingEffectsData()` сканує ефекти і створює `EffectDestroyingRuntimeData` записи.
  - `UpdatePrioritizingDestroyingEffect()` обирає ефект з найвищим `Priority` як `effectOfPrioritizingDestroying`.
  - `HandleDestroyingEffects()` інкрементує `NeighboringMergeCount` і викликає `TryDestroyEffect`.
  - `RemoveEffect(int effectId)` деактивує ефект, видаляє з словника та `EffectEnables`, прибирає блок з `BlockingState`, обирає наступний пріоритетний ефект, і оновлює візуал.
- Процес знищення чіпа підтримує анімації руйнування та є двохетапним:
  - **`Destroy(ICell mainCell, bool force, AnimatorTrigger destroyTrigger = AnimatorTrigger.Destroy)`**: Ініціює процес знищення.
    1. Перевіряє, чи чіп уже знищується (`IsDestroying`).
    2. Очищує occupancy в `FieldGrid` та `ChipCollections`.
    3. Викликає `ICellSubscriber.OnChipDestroy(mainCell)`.
    4. Якщо `force` є істиною або відсутній `Animator`, викликає `FinishDestroy()` негайно.
    5. Інакше надсилає вказаний тригер аніматора `destroyTrigger`.
  - **`FinishDestroy()`**: Завершує руйнування об'єкта. Може бути викликаний безпосередньо з `Destroy`, або через Unity Animation Event наприкінці анімації руйнування.
    1. Викликає `DestroyEffects(0f)`.
    2. Знищує GameObject чіпа.
    
    Під час знищення ефектів через `DestroyEffects` перевіряється властивість ефекту `IsSkipDestroy`. Якщо вона дорівнює `true`, цей ефект не знищується разом із чіпом (наприклад, коли ефект відв'язано за допомогою `SkipDestroy()` і він має дограти анімацію).
  
## Модульна архітектура та Композиція (IChipModule)

Починаючи з версії Merge Toolkit, реалізовано перехід від успадкування спеціалізованих чіпів до композиційного підходу. Клас [Chip](file:///Users/eriktakoev/Projects/MergeToolkit/merge2-unity/Assets/Expecto/MergeBase/Core/Scripts/Chips/Chip.cs) тепер виступає як контейнер (хост), а спеціалізована логіка винесена в окремі модулі, що реалізують інтерфейс [IChipModule](file:///Users/eriktakoev/Projects/MergeToolkit/merge2-unity/Assets/Expecto/MergeBase/Core/Scripts/Chips/Interfaces/IChipModule.cs):
- **`ContainerModule`**: Керує контейнерами та вимогами заповнення.
- **`GeneratorModule`**: Керує логікою генерації та підсиленням швидкості.
- **`PowerBoosterModule`**: Керує логікою підсилювачів та зв'язками з цілями.

### Делегування життєвого циклу модулям
Клас `Chip` автоматично збирає всі компоненти `IChipModule` на своєму GameObject за допомогою `GetComponents<IChipModule>()` і делегує їм виклики у ключових точках життєвого циклу:
- **`Init`**: Ініціалізація кожного модуля з передачею посилань на `Chip`, `ChipData` та `ChipRuntimeData`.
- **`InitRuntimeData`**: Реєстрація спеціалізованих даних стану в модулях.
- **`OnTap`**: Передача події тапу користувача.
- **`OnDragStart`, `OnDrag`, `OnDragEnd`**: Передача подій перетягування.
- **`OnChangedCell`**: Передача подій зміни поточної клітинки на полі.
- **`FinishDestroy`**: Очищення ресурсів модуля перед повним видаленням GameObject чіпа (метод `DestroyModule`).

### Керування ефектами модулів
Методи `AddEffect` та `RemoveEffect` класу `Chip` тепер мають область видимості `public virtual` (замість `protected virtual`), що дозволяє модулям керувати власними ефектами.
- При виклику `UpdateVisual` на чіпі, він додатково викликає `module.UpdateVisual()` для кожного зареєстрованого модуля.
- При видаленні ефекту через `RemoveEffect` викликається `module.OnEffectRemoved(effectId)`.

### Спеціалізовані рантайм-дані (IChipSpecialRuntimeData)
Клас `ChipRuntimeData` тепер містить список поліморфних даних стану:
- **`specialRuntimeDatas`** (`List<IChipSpecialRuntimeData>` з атрибутом `[SerializeReference]`).
- **`GetSpecialRuntimeData<T>()`**: Допоміжний метод для отримання конкретного типу даних стану для модуля (наприклад, `ChipGeneratorRuntimeData` або `ChipContainerRuntimeData`).

### Movement State Management
Система розрізняє **стан перетягування користувачем** та **візуальний стан переміщення**:

#### User Drag State
- **`SetDragging(bool)`**: Встановлює стан перетягування користувачем. Викликається `DraggableChipLogic` при початку/завершенні перетягування. Автоматично викликає `SetMoving(true)` при необхідності.
- **`IsDragging()`**: Перевіряє, чи перетягується чіп користувачем. Відстежує саме взаємодію з користувачем, а не лише візуальне переміщення.

#### Visual Movement State
- **`SetMoving(bool)`**: Керує візуальним станом переміщення.
  - Оновлює стан `IChipSortingLayer` для коригування шарів сортування рендерерів.
  - Сповіщає всі ефекти через метод `OnMovingStateChanged(chip, isMoving)`.
  - На старті руху (`true`) додає в `IChipChangeNotifier` тимчасову подію `NewChip=null` для поточної клітинки, щоб observer-системи одразу відреагували на "тимчасовий вихід" чіпа; при завершенні (`false`) викликає `UpdateVisual()`.
- **`IsMoving()`**: Перевіряє візуальний стан переміщення (за `sortingOrder`). Повертає `true` як для перетягування користувачем, так і для системного переміщення.

### Flight Settings (Налаштування польоту)
Кожен чіп містить налаштування польоту `FlightSettings` типу `ChipFlightSettings` (структура), що визначає параметри переміщення фішки по сітці поля:
- **`Duration`**: Тривалість польоту в секундах.
- **`FlightDelay`**: Затримка перед початком польоту.
- **`Type`**: Тип траєкторії польоту (`FlightType`):
  - `Linear` — лінійний рух.
  - `ArcBounce` — параболічний рух із відскоками при приземленні (за замовчуванням).
  - `HalfArcHalfBounce` — рух з меншою висотою дуги та меншим відскоком (наприклад, при звичайному переміщенні або зміні місць).
  - `HalfArc` — рух по низькій дузі без відскоків.
- **`SetFlightSettings(ChipFlightSettings settings)`**: Оновлює налаштування польоту для наступного переміщення чіпа.

### Other Methods
- **`OnDraggingChipWithMoveLocked()`**: Віртуальний метод, що викликається при спробі перетягнути заблокований чіп. Спочатку намагається відправити тригер `"MoveLocked"` у `effectOfPrioritizingDestroying` (ефект з найвищим пріоритетом руйнування); якщо його немає — у ефект з ключем `EffectConsts.Blockers.MoveLockedEffect`. Використовує `allowRepeat=true` для забезпечення візуального відгуку на кожну спробу.

### Merge System
Реалізує логіку сумісності та процес злиття двох фішок через механізм `IChipInteractionLogic`.  
Для повного опису (CanInteract, ExecuteInteraction, Weighted Random, Extra Chips, Relocation) див. [MergeableChipLogic](../Interactions/MergeableChipLogic.md).
