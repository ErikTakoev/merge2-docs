# Technical Design

[← На Головну](../Main.md)


Архітектура побудована на принципах модульності, Dependency Injection та чіткого розділення відповідальності.

## Dependency Injection (VContainer)
Ми використовуємо **VContainer** для керування залежностями.
- **LifetimeScope**: `Merge2LifetimeScope` та `IsoMergeLifetimeScope` реалізують інтерфейс `IMergeLifetimeScope` і є точками конфігурації для відповідних сцен Merge та IsoMerge. Тут реєструються дані рівня (`FieldData`, `ChipDataCollection`), а також глобальні налаштування злиття (`MergeSettings`) як синглтони. `FieldData` та `MergeSettings` можуть бути призначені динамічно (наприклад, у тестах).
- **Initialization**: `Merge2Initializer` виступає як Entry Point. Він отримує через конструктор ключові інтерфейси (`IFieldGrid`, `IFieldEventHandler`, `ChipFactory`, `IInputManager`) та ініціалізує поле через `IFieldInitializeCommand`.
- **Component Injection**: Всі ігрові сервіси та логічні класи отримують залежності через `[Inject]` або конструктор.

## Core Interfaces & Implementations
Основні абстракції системи та їх реалізації:

### Grid & Data
- **`ICell`** (Інтерфейс)
  - **Призначення**: Абстракція ігрової клітинки.
  - **Відповідальність**: Надання координат (`CellPosition`), доступ до розміщеного чіпа та управління візуальним станом (наприклад, підсвітка). Дозволяє реалізувати різні типи полів (прямокутні, ізометричні тощо), не змінюючи логіку ядра.

- **`IFieldGrid`** -> `FieldGrid`
  - **Призначення**: Управління станом сітки (2D масив `ICell`).
  - **Відповідальність**: Створення комірок, валідація координат, перевірка заблокованих зон (`HasBlockedCells`), низькорівневі операції розміщення чіпів (`SetChipInCell`).
  - **Деталь `SetChipInCell`**: При встановленні чіпа `FieldGrid` призначає `chip.CellPosition` до `IChipChangeNotifier.Enqueue(...)`, щоб підписники отримували подію вже з актуальними координатами. При очищенні спочатку скидає occupancy (`ClearCells`), потім enqueue події `oldChip -> null`.

- **`IMergeLifetimeScope`** (Інтерфейс)
  - **Призначення**: Уніфікований доступ до конфігурації LifetimeScope в Merge та IsoMerge.
  - **Відповідальність**: Дозволяє Editor- та Runtime-інструментам динамічно зчитувати та записувати `FieldData` і `MergeSettings`, а також отримувати доступ до контейнера `IObjectResolver` без жорсткої залежності від конкретної реалізації LifetimeScope.

- **`IFieldInitializeCommand`** -> `FieldInitializeCommand`
  - **Призначення**: Команда ініціалізації рівня.
  - **Відповідальність**: Створення візуальної сітки, завантаження початкових чіпів та ініціалізація камери. Отримує `FieldData`, `ChipDataCollection` та `IMergeCamera` через Injection.

- **`IChipChangeNotifier`** -> `DeferredChipChangeNotifier`
  - **Призначення**: Агрегація змін клітинок (реалізацій `ICell`) протягом кадру та єдиний `Flush` у `LateUpdate`.
  - **Відповідальність**: `FieldGrid` додає події через `Enqueue`, `FieldEventHandler` викликає `Flush`, а підписники отримують консистентний набір `ChipChangedEvent`.
  - **Деталі**: [Cell Observer System](../Features/CellObserverSystem.md).

- **`ICellSubscriber`** -> `CellSubscriber`, `PowerBoosterCellSubscriber`
  - **Призначення**: Контракт для компонентів, що реагують на зміни в сусідніх клітинках (`ICell`).
  - **Відповідальність**: `OnChipChangedCell` перев'язує підписки після переміщення, `OnChipDestroy` виконує cleanup перед знищенням чіпа, `OnObservedCellChipChanged` обробляє батч-події через [Cell Observer System](../Features/CellObserverSystem.md).

- **`ILockedAreaManager`** -> `LockedAreaManager`
  - **Призначення**: Управління станом заблокованих ділянок на рівні.
  - **Відповідальність**: Ініціалізація заблокованих ділянок, блокування комірок, реєстрація візуальних ефектів, розблокування ділянок та спавн відкладених фішок. Детально описано у [Locked Areas](../Features/LockedAreas.md).

- **`IChipCollections`** -> `ChipCollections`
  - **Призначення**: Централізований реєстр усіх активних фішок на полі, згрупованих за типом даних, а також надання високопродуктивних відфільтрованих індексів.
  - **Відповідальність**: Збереження `AllChipsByData`, а також підтримка актуальності словників `MergeableChipsByData` (фішки, доступні як цілі для злиття) та `FillableChipsByData` (контейнери, що очікують наповнення). Оновлюється при створенні/знищенні фішок, а також через події зміни блокувань (`OnChipBlockingChanged`) та оновлення вимог контейнера (`OnContainerRequirementsChanged`).

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
  - **Відповідальність**: Валідація переміщень, обробка колізій та розрахунок ланцюгових переміщень (relocation) інших чіпів, щоб звільнити місце. Методи переміщення (`ChipMoving`, `ChipsRelocate`) приймають параметр `ChipFlightSettings` для динамічного налаштування траєкторії та тривалості польоту фішок (наприклад, для різного візуального стилю при звичайному переміщенні або релокації).

- **`IPowerBoosterTarget`** -> `ChipGenerator` (`partial` у `ChipGenerator.PowerBoosterTarget.cs`)
  - **Призначення**: Контракт для сутностей, що можуть бути посилені `ChipPowerBooster`.
  - **Відповідальність**: Зберігання набору активних бустерів, реалізація apply/remove впливу, надання `JoinPoints` для join-візуалізації, експорт `BlockingState` для перевірки `CanReceiveModifiers`, та `NotifyEffectRemoved(int)` для сповіщення бустерів про зміни блокуючих ефектів.

- **`IChipFinder`** -> `NeighborChipFinder`
  - **Призначення**: Пошук сусідніх чіпів навколо клітинки з урахуванням розміру чіпа.
  - **Відповідальність**: Allocation-free ітерація по 4 границях bounding box чіпа, збір унікальних сусідів через `HashSet<Chip>`. Використовується `DraggableChipLogic` для `NotifyNeighborsOfInteraction`.

### Camera & Visual Field
- **`IMergeCamera`** -> `MergeCamera` (2D) / `IsoMergeCamera` (Isometric)
  - **Призначення**: Абстракція камери для підтримки різних типів проекцій (2D та ізометрична).
  - **Відповідальність**: Ініціалізація ортографічного розміру/позиції камери, обробка перетягування (drag), зуму та керування обмеженнями (bounds) переміщення камери на ігровому полі.

- **`IVisualField`** -> `VisualField` (2D) / `IsoVisualField` (Isometric)
  - **Призначення**: Контракт для кореневого компонента візуального представлення ігрового поля.
  - **Відповідальність**: Ініціалізація візуального представлення поля, підлаштування під розмір сітки, налаштування меж камери та ініціалізація ефектів (наприклад, заблокованих ділянок).

### Scenario & Quests
- **`IScenarioEventHandler`** -> `ScenarioEventHandler`
  - **Призначення**: Контракт для передачі подій сценаріїв та квестів.
  - **Відповідальність**: Трансляція життєвого циклу чіпів та змін на полі (створення чіпа, знищення, зняття блокуючого ефекту, розблокування зони) у C# події та події Unity Visual Scripting (EventBus). Виступає сполучною ланкою між ядром Merge та сценаріями.

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
- **Реалізація**: `ChipFlyAnimation` підтримує різні типи траєкторій польоту (`FlightType`: `Linear`, `ArcBounce`, `HalfArcHalfBounce`, `HalfArc`), які конфігуруються через `ChipFlightSettings` і передаються як параметр до методу `StartAnimation(...)`.
- **Перевага**: Логіка `Cell` або `Chip` каже "перемістись туди", а система анімації вирішує "як" це зробити (швидкість, крива, ефекти), не блокуючи логічний стан гри.

## State Management
Проект використовує ScriptableObjects для збереження стану рівнів та метаданих.

### ChipData & SpecialData
`ChipData` зберігає базові параметри чіпа (тип, префаб, розмір), а розширювані дані винесені в `specialDatas` (`SerializeReference`).
- **Контракт**: `IChipSpecialData` — базовий інтерфейс для спеціалізованих конфігурацій.
- **Merge як SpecialData**: `ChipMergeData` є одним із блоків `IChipSpecialData` для конфігурації злиття чіпів.
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
- **BlockedCells**: Масив координат (`Vector2Int[]`), які визначають заблоковані клітинки на рівні, що ініціалізуються як непрохідні.
- **Позиція**: Координати якоря (top-left) для розміщених фішок у `CellData`.
- **Розташування в коді**: `FieldData` і `FieldChipData` знаходяться в `Core/Scripts/Field/Data`.
- **MergeSettings**: Глобальні налаштування сцени, які зокрема містять `CellPrefab` для створення стандартних ігрових клітинок.

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
