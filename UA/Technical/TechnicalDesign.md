# Technical Design

[← На Головну](../Main.md)


Архітектура побудована на принципах модульності, Dependency Injection та чіткого розділення відповідальності.

## Dependency Injection (VContainer)
Ми використовуємо **VContainer** для керування залежностями.
- **LifetimeScope**: `Merge2LifetimeScope` — точка конфігурації для сцени Merge2. Тут також реєструються статичні дані рівня (`FieldData`, `ChipDataCollection`) як синглтони. `FieldData` може бути призначений динамічно, що дозволяє використовувати один і той самий LifetimeScope для різних конфігурацій поля (наприклад, у тестах).
- **Initialization**: `Merge2Initializer` виступає як Entry Point. Він отримує через конструктор ключові інтерфейси (`IFieldGrid`, `IFieldEventHandler`, `ChipFactory`, `IInputManager`) та ініціалізує поле через `IFieldInitializeCommand`.
- **Component Injection**: Всі ігрові сервіси та логічні класи отримують залежності через `[Inject]` або конструктор.

## Core Interfaces & Implementations
Основні абстракції системи та їх реалізації:

### Grid & Data
- **`IFieldGrid`** -> `FieldGrid`
  - **Призначення**: Управління станом сітки (2D масив `Cell`).
  - **Відповідальність**: Створення комірок, валідація координат, низькорівневі операції розміщення чіпів (`SetChipInCell`).
  - **Деталь `SetChipInCell`**: При встановленні чіпа `FieldGrid` призначає `chip.CellPosition` до `IChipChangeNotifier.Enqueue(...)`, щоб підписники отримували подію вже з актуальними координатами. При очищенні спочатку скидає occupancy (`ClearCells`), потім enqueue події `oldChip -> null`.

- **`IFieldInitializeCommand`** -> `FieldInitializeCommand`
  - **Призначення**: Команда ініціалізації рівня.
  - **Відповідальність**: Створення візуальної сітки та завантаження початкових чіпів. Отримує `FieldData` та `ChipDataCollection` через Injection.

- **`IChipChangeNotifier`** -> `DeferredChipChangeNotifier`
  - **Призначення**: Агрегація змін клітинок протягом кадру та єдиний `Flush` у `LateUpdate`.
  - **Відповідальність**: `FieldGrid` додає події через `Enqueue`, `FieldEventHandler` викликає `Flush`, а підписники отримують консистентний набір `ChipChangedEvent`.
  - **Деталі**: [Cell Observer System](../Features/CellObserverSystem.md).

- **`ICellSubscriber`** -> `CellSubscriber`, `PowerBoosterCellSubscriber`
  - **Призначення**: Контракт для компонентів, що реагують на зміни в сусідніх клітинках.
  - **Відповідальність**: `OnChipChangedCell` перев'язує підписки після переміщення, `OnChipDestroy` виконує cleanup перед знищенням чіпа, `OnObservedCellChipChanged` обробляє батч-події через [Cell Observer System](../Features/CellObserverSystem.md).

### Logic & Interaction
- **`IInputManager`** -> `InputManager`
  - **Призначення**: Абстракція системи вводу для Dependency Injection.
  - **Відповідальність**: Визначення подій вводу (OnTap, OnDragStart, OnDrag, OnDragEnd) та методів симуляції для тестування. Реалізація `InputManager` обробляє Unity Input System та трансформує події вводу в C# події.

- **`IFieldEventHandler`** -> `FieldEventHandler`
  - **Призначення**: Точка входу для подій системи вводу.
  - **Відповідальність**: Обробка Tap/Drag подій, координація взаємодії між IInputManager та логікою поля.

- **`IFreeCellFinder`** -> `FreeCellFinder`
  - **Призначення**: Алгоритмічний пошук вільного місця.
  - **Відповідальність**: Знаходження найближчої вільної комірки (спіральний пошук) для спавну або переміщення чіпів. Підтримує режим `onlyAround` для обмеження пошуку тільки сусідніми комірками.

- **`IChipMovingLogic`** -> `ChipMovingLogic`
  - **Призначення**: Комплексна логіка переміщення.
  - **Відповідальність**: Валідація переміщень, обробка колізій та розрахунок ланцюгових переміщень (relocation) інших чіпів, щоб звільнити місце.

- **`IPowerBoosterTarget`** -> `ChipGenerator` (`partial` у `ChipGenerator.PowerBoosterTarget.cs`)
  - **Призначення**: Контракт для сутностей, що можуть бути посилені `ChipPowerBooster`.
  - **Відповідальність**: Зберігання набору активних бустерів, реалізація apply/remove впливу, надання `JoinPoints` для join-візуалізації, експорт `BlockingState` для перевірки `CanReceiveModifiers`, та `NotifyEffectRemoved(int)` для сповіщення бустерів про зміни блокуючих ефектів.

- **`IChipFinder`** -> `NeighborChipFinder`
  - **Призначення**: Пошук сусідніх чіпів навколо клітинки з урахуванням розміру чіпа.
  - **Відповідальність**: Allocation-free ітерація по 4 границях bounding box чіпа, збір унікальних сусідів через `HashSet<Chip>`. Використовується `MergeableChipLogic` для `NotifyNeighborsOfMerge`.

## Visual Effects System
Візуальні ефекти для фішок реалізовані через систему інтерфейсів для гнучкості та розділення логіки.
- **`IEffect`**: Базовий інтерфейс для всіх ефектів фішок (активація, тригери, зміна комірок).
- **`IEffectContainer`**: Спеціалізований інтерфейс для візуалізації `ChipContainer`, розширює `IEffect` методом `UpdateElements`.
- **`IEffectGeneratorCharging`**: Спеціалізований інтерфейс для візуалізації зарядки `ChipGenerator`, розширює `IEffect` методом `OnCharging`.
- **`IEffectPowerBoosterJoin`**: Спеціалізований інтерфейс для join-візуалізації бустера (`OnJoin`, `OnLeave`, `Show`) між `ChipPowerBooster` та `IPowerBoosterTarget`.
- **`IChipSortingLayer`**: Контракт для управління шарами сортування (Sorting Layers) декількох рендерерів фішки. Дозволяє автоматично коригувати `sortingOrder` під час переміщення (drag), щоб фішка візуально знаходилася над полем.
- **`InterfaceRef<T>`**: Ми використовуємо спеціальну серіалізовану обгортку (`EffectRef`, `EffectContainerRef`, `EffectPowerBoosterJoinRef` тощо) для призначення MonoBehaviour, що реалізують інтерфейси, прямо в інспекторі Unity, забезпечуючи типізацію та модульність.

## Interaction Strategies
Використовуємо патерн "Стратегія". Логіка того, як фішки взаємодіють між собою під час перетягування, винесена в окремі компоненти:
- **Інтерфейс**: `IChipInteractionLogic`.
- **Реалізації (компоненти `DraggableChipLogic`)**:
  - `MergeableChipLogic` — логіка злиття сумісних фішок (згідно з налаштуваннями `ChipMergeData`).
  - `FillContainerLogic` — логіка додавання фішок у контейнери.
- **Особливості**:
  - Компоненти стратегій повинні бути розміщені на тому ж **GameObject**, що і `DraggableChipLogic`.
  - **Пріоритет**: Порядок компонентів в інспекторі Unity визначає черговість перевірки взаємодій. Перша стратегія, яка поверне `true` у методі `CanInteract`, буде обрана для виконання.
- Це дозволяє динамічно налаштовувати можливі взаємодії на полі, просто додаючи або видаляючи відповідні компоненти.

## Input System
Система вводу базується на **Unity Input System (New Package)**.
- **Asset**: `Merge2Input.inputactions` — містить визначення Action Maps (Game, UI).
- **Architecture**:
  - **Інтерфейс**: `IInputManager` — визначає контракт для системи вводу, дозволяє Dependency Injection через VContainer.
  - **Реалізація**: `InputManager` — клас-адаптер, що ініціалізує Generated Class `Merge2Input` та трансформує події вводу (Press, Drag) у C# події (`OnTap`, `OnDragStart`, `OnDrag`, `OnDragEnd`). Надає методи симуляції (`SimulateTap`, `SimulateDrag*`) для тестування в Unity Editor.
  - **Інтеграція**: `FieldEventHandler` підписується на події `IInputManager` і делегує їх обробку відповідним компонентам (фішкам або полю).

## Animation System
Анімації відокремлені від логіки даних.
- **Інтерфейс**: `IChipFlyAnimation`.
- **Призначення**: Керує візуальним переміщенням (Tweening) об'єктів.
- **Перевага**: Логіка `Cell` або `Chip` каже "перемістись туди", а система анімації вирішує "як" це зробити (швидкість, крива, ефекти), не блокуючи логічний стан гри.

## State Management
Проект використовує ScriptableObjects для збереження стану рівнів та метаданих.

### ChipData & SpecialData
`ChipData` зберігає базові параметри чіпа (тип, префаб, розмір), а розширювані дані винесені в `specialDatas` (`SerializeReference`).
- **Контракт**: `IChipSpecialData` — базовий інтерфейс для спеціалізованих конфігурацій.
- **Merge як SpecialData**: `ChipMergeData` тепер є одним із блоків `IChipSpecialData` і не зберігається окремим полем у `ChipData`.
- **Доступ**: 
  - `GetSpecialData<T>()` повертає типізований блок даних (`ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, `ChipExtraEffectsData` тощо).
  - `CreateSpecialData<T>()` — динамічно створює нову інстанцію спеціальних даних, додає її до колекції та повертає посилання. Зручний для тестів, коли потрібно клонувати `ChipData` та змінити його конфіг на льоту.
  - `AddSpecialData(IChipSpecialData)` — додає готовий екземпляр спеціальних даних у колекцію (використовується, зокрема, при клонуванні default-шаблонів у Chip Creator).
    ```csharp
    ChipData clonedData = originalData.Clone();
    ChipMergeData mergeData = clonedData.CreateSpecialData<ChipMergeData>();
    mergeData.Combinations = new MergeCombination[] { /* ... */ };
    ```
- **Перевага**: Дозволяє додавати нові типи даних без розширення базового `ChipData` окремими полями.
- **Blocker Effects як SpecialData**: Налаштування blocker-ефектів винесені в `ChipExtraEffectsData` (масив `ExtraEffectData[]` з `effectName` + `Prefab`), тому `Chip.InitEffects()` отримує конфігурацію через `GetSpecialData<ChipExtraEffectsData>()`.
- **Runtime доступ до merge**: Під час `Chip.Init` merge-дані кешуються у `Chip.MergeData`; `MergeableChipLogic` використовує саме цей доступ.

### FieldData & CellData
`FieldData` описує початковий стан поля. Кожна клітинка представлена структурою `CellData`:
- **FieldChipData**: Містить дані фішки (**ChipId**) та масив активних blocker-ефектів (**BlockerEffectIds**, наприклад `EffectConsts.Blockers.MoveLockedEffect`).
- **Позиція**: Координати якоря (top-left).
- **Розташування в коді**: `FieldData` і `FieldChipData` знаходяться в `Core/Scripts/Field/Data`.

### Runtime State
У грі інформація про активні extra-ефекти зберігається у `ChipRuntimeData.EffectEnables` (`HashSet<int>`). Цей набір використовується як індикатор стану — `Chip.InitEffects()` та `UpdateVisual()` активують ефекти, чиї ID є в наборі. А вже сам ефект, маючи `EffectBlockingSettings`, передає конкретні заборони до загального `CombinedBlockingState` чіпа (з яким вже працює ігрова логіка). Додатково, `EffectDestroyingData` (`Dictionary<int, EffectDestroyingRuntimeData>`) трекає прогрес руйнування ефектів при сусідніх злиттях. Це дозволяє динамічно змінювати стан фішок (наприклад, розблокувати після виконання певних умов), розмежовуючи візуальні ефекти та логіку блокування.

## Editor Tools
Ми надаємо спеціалізовані інструменти для полегшення процесу створення та налаштування контенту.
- **Level Editor**: Візуальний редактор рівнів, що дозволяє дизайнерам налаштовувати сітку та розміщувати фішки.
- **Property Drawers**: Спеціалізовані атрибути `[ChipSelector]` та `[EffectBlockerSelector]` інтегровані в `FieldChipData` для зручного вибору ID фішок та ефектів прямо в інспекторі.
- **Chip Creator**: Редактор `ChipData`, який підтримує редагування `specialDatas` (включно з поліморфними типами `IChipSpecialData`).
- **Undo/Redo (Command Pattern)**: Всі дії в редакторі інкапсульовані в об'єкти команд (`IEditorCommand`). Це дозволяє реалізувати надійну систему скасування та повторення дій, запобігаючи втраті прогресу при помилках редагування.
- **Validation**: Система автоматичної перевірки цілісності даних рівня перед збереженням.

## Context Awareness
Для полегшення розуміння коду (особливо для AI) використовується атрибут `[ContextCodeAnalyzer]`.
- **Поля**: `@purpose`, `@usage`, `@params`, `@notes`.
- **Використання**: Документування нетривіальної логіки прямо в коді. Це допомагає генерувати актуальну документацію та дає контекст при LLM-аналізі.
