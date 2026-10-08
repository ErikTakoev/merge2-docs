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
  - **IChipSpecialData**: Базовий контракт для спеціалізованих даних. Реалізації: `ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, `ChipExtraEffectsData`, `ChipTapEvolutionData`.
  - **INextChipsProvider**: Поліморфна стратегія вибору наступних чіпів у `ChipTapEvolutionData` (`[SerializeReference]`). Реалізації: `ConstantNextChipsProvider`, `RandomNextChipsProvider`, `SequentialNextChipsProvider`.

- **Runtime**:
  - **CellPosition**: Поточна позиція фішки на сітці поля (Vector2Int). Оновлюється системою при переміщенні.
  - **RuntimeData**: Поточний стан (див. нижче).
  - **BlockingState**: (`CombinedBlockingState`) агрегований стан дозволів (наприклад, `CanBeMoved`, `CanBeMergedAsSource`, `CanDestroyEffects`), що визначається активними ефектами.
  - **EffectOfPrioritizingDestroying**: (`IEffect`) посилання на поточний найвищий за пріоритетом ефект, що підлягає руйнуванню при сусідніх взаємодіях.
- **Visual Management**:
  - **SortingLayer** (`IChipSortingLayer`): Керує шарами сортування декількох рендерерів чіпа, забезпечуючи коректне відображення під час руху.
  - **AnimationNode** (`Transform`): Посилання на вузол анімації фішки, куди прикріплюються візуальні ефекти (типу `ParentChipAnimationNode`), що мають рухатися разом із фішкою.
  - **LiftController** (`IChipLiftController`): Об'єкт керування висотою підйому фішки (наприклад, під час перетягування). Отримує посилання через серіалізоване поле `liftControllerRef`.
  - **MaterialController** (`IChipMaterialController`): Об'єкт керування матеріалами та шейдерними параметрами рендерерів фішки через `MaterialPropertyBlock` для збереження динамічного й статичного батчингу. Отримує посилання через серіалізоване поле `materialControllerRef` та ініціалізується в `Chip.Init()` (`MaterialController.Init(this)`).
- **Others**:
  - **LogEnable**: Прапорець для ввімкнення логування подій чіпа в консоль.
- **Effects**: Керується централізованою системою на основі `Dictionary<int, IEffect>` з хеш-ключами від `EffectConsts`.
  Для повного каталогу див. [Visual Effects](../Visuals/Effects.md). Детальніше про логіку блокувань та руйнування див. [Chip Effect Blockers](../Features/ChipEffectBlockers.md).
- **Animations**: Має посилання на `Animator` для відтворення станів (наприклад, `Merge`, `Generate`, `MoveLocked`, `TapEvolutionSpawn`, `TapEvolutionDestroy`). Підтримує паузу через `PauseAnimator(bool)` на час затримки польоту.

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
  var containerEffect = GetEffect<IEffectContainerHint>(EffectConsts.ContainerRequirements);
  containerEffect?.UpdateElements(this, containers, false);
  ```

### Effect Initialization
- **`InitEffects()`**: Віртуальний метод, викликаний з `Init(...)` для ініціалізації всіх ефектів. Базова реалізація:
  1. Ітерує `ChipExtraEffectsData.Blockers` — для кожного елемента, чий `EffectId` є в `runtimeData.EffectEnables`, інстантіює префаб і додає в словник ефектів через `AddEffect`
  2. Створює та додає `OtherEffects` з `ChipExtraEffectsData`
  3. Реєструє додаткові ефекти з масиву `extraEffects` (`EffectRef[]`), налаштовані на самому префабі чіпа
  4. Створює та додає `CellHighlightEffect` з `ChipData.CellHighlightPrefab` (ключ: `EffectConsts.CellHighlight`)
  5. Створює та додає `ChipMergeAvailableEffect` з `ChipData.MergeAvailableEffectPrefab` (ключ: `EffectConsts.MergeAvailable`)
  6. Якщо вказано `ShadowEffectPrefab`, створює та додає `ShadowEffect` (ключ: `EffectConsts.ShadowEffect`)

  Цей метод призначений для перекриття в похідних класах (наприклад, `ChipGenerator` додає `GeneratorCharging` та `GeneratorCharged`).

- **`AddEffect(IEffect effect, int effectHash, bool activate)`**: Додає ефект до словника та опціонально активує його:
  ```csharp
  var effect = InstantiateEffect<IEffect>(data.CellHighlightPrefab);
  AddEffect(effect, EffectConsts.CellHighlight, true);
  ```

### Effect Constants (EffectConsts)
Вся система ефектів використовує централізовані цілочисельні константи, що визначені у [EffectConsts.cs](../../../Core/Scripts/Chips/Effects/EffectConsts.cs):
- **Базові ефекти (1–13)**: `MergeAvailable`, `CellHighlight`, `ContainerRequirements`, `GeneratorCharged`, `GeneratorCharging`, `PBoosterConnectorCells`, `PBoosterJoin`, `ShadowEffect`, `MergeLight`, `MergeHint`, `TapHint`, `GlowEffect`, `SepiaEffect`
- **Blocker-ефекти (101+)** — `EffectConsts.Blockers`: `BoxEffect` (101), `WebEffect` (102), `MoveLockedEffect` (103), `BoxAndWebEffect` (104)
- **Утиліти**: `GetIdByName(string)` — резолв рядкової назви в ID через словник `nameToId`

### Effect Lifecycle
- Всі ефекти, додані до словника `effects`, автоматично отримують сповіщення через методи `OnChangedCell()`, `OnInteractionOverCellChanged()` та `OnInteractionUnderCellChanged()`.
- **Effect Destroying**: Ефекти з `DestroyingSettings` підтримують поступове руйнування при сусідніх злиттях (детальніше: [Chip Effect Blockers](../Features/ChipEffectBlockers.md#effect-destroying-system)):
  - `InitDestroyingEffectsData()` сканує ефекти і створює `EffectDestroyingRuntimeData` записи.
  - `UpdatePrioritizingDestroyingEffect()` обирає ефект з найвищим `Priority` як `effectOfPrioritizingDestroying` (доступний через публічну властивість `EffectOfPrioritizingDestroying`).
  - `HandleDestroyingEffects()` перевіряє `BlockingState.CanDestroyEffects` (якщо `false`, руйнування блокується), після чого інкрементує `NeighboringMergeCount` і викликає `TryDestroyEffect`.
  - `RemoveEffect(int effectId)` деактивує ефект, видаляє з словника та `EffectEnables`, прибирає блок з `BlockingState`, обирає наступний пріоритетний ефект, і оновлює візуал.
- Процес знищення чіпа підтримує анімації руйнування та є двохетапним:
  - **`Destroy(ICell mainCell, bool force, AnimatorTrigger destroyTrigger = AnimatorTrigger.Destroy)`**: Ініціює процес знищення.
    1. Перевіряє, чи чіп уже знищується (`IsDestroying`).
    2. Очищує occupancy в `FieldGrid` та `ChipCollections`.
    3. Викликає `ICellSubscriber.OnChipDestroy(mainCell)`.
    4. Якщо `force` є істиною або відсутній `Animator`, викликає `FinishDestroy()` негайно.
    5. Інакше деактивує активні ефекти через `DeactivateAllEffects()` та надсилає вказаний тригер аніматора `destroyTrigger` (наприклад, `AnimatorTrigger.Destroy` чи `AnimatorTrigger.TapEvolutionDestroy`).
  - **`FinishDestroy()`**: Завершує руйнування об'єкта. Може бути викликаний безпосередньо з `Destroy`, або через Unity Animation Event наприкінці анімації руйнування.
    1. Викликає `module.DestroyModule()` для кожного зареєстрованого модуля.
    2. Знищує та очищає всі прив'язані ефекти через `RemoveAllEffects()`.
    3. Знищує GameObject чіпа (`OnDestroy` також гарантує виклики `RemoveAllEffects()` та `DestroyModule()`).

    Під час деактивації та видалення ефектів перевіряється властивість `IsSkipDestroy`. Якщо вона дорівнює `true`, цей ефект пропускається (наприклад, коли ефект відв'язано за допомогою `SkipDestroy()` і він має дограти анімацію).

## Modular Architecture and Composition (IChipModule)

Починаючи з версії Merge Toolkit, реалізовано перехід від успадкування спеціалізованих чіпів до композиційного підходу. Клас [Chip](../../../Core/Scripts/Chips/Chip.cs) тепер виступає як контейнер (хост), а спеціалізована логіка винесена в окремі модулі, що реалізують інтерфейс [IChipModule](../../../Core/Scripts/Chips/Interfaces/IChipModule.cs):
- **`ContainerModule`**: Керує контейнерами та вимогами заповнення.
- **`GeneratorModule`**: Керує логікою генерації та підсиленням швидкості.
- **`PowerBoosterModule`**: Керує логікою підсилювачів та зв'язками з цілями.
- **`WaitEvolutionModule`**: Керує автоматичною еволюцією чіпа з часом, відстежуючи час очікування і замінюючи поточний чіп на інший.
- **`TapEvolutionModule`**: Керує еволюцією чіпа при натисканні (тапі) за допомогою стратегій `INextChipsProvider`. Підтримує спавн кількох фішок через `FreeCellFinder.FindFreeCellsForChips`, програє візуальні ефекти (тригери анімації `TapEvolutionSpawn`, `Generate` та `TapEvolutionDestroy`), а при відсутності вільних клітинок викликає `OnNotEnoughSpace` через `ScenarioEventHandler`.
- **`PauseAnimationOnDelay`**: Прапорець у `ChipFlightSettings`, що заморожує виконання анімації чіпа через `PauseAnimator(true)` під час активності затримки польоту (`flightDelay`).

### Lifecycle Delegation to Modules
Клас `Chip` автоматично збирає всі компоненти `IChipModule` на своєму GameObject за допомогою `GetComponents<IChipModule>()` і делегує їм виклики у ключових точках життєвого циклу:
- **`Init`**: Ініціалізація кожного модуля з передачею посилань на `Chip`, `ChipData` та `ChipRuntimeData`.
- **`InitRuntimeData`**: Реєстрація спеціалізованих даних стану в модулях.
- **`OnTap`**: Передача події тапу користувача.
- **`OnDragStart`, `OnDrag`, `OnDragEnd`**: Передача подій перетягування.
- **`OnChangedCell`**: Передача подій зміни поточної клітинки на полі.
- **`FinishDestroy`**: Очищення ресурсів модуля перед повним видаленням GameObject чіпа (метод `DestroyModule`).

### Module Effects Management
Методи `AddEffect` та `RemoveEffect` класу `Chip` тепер мають область видимості `public virtual` (замість `protected virtual`), що дозволяє модулям керувати власними ефектами.
- При виклику `UpdateVisual` на чіпі, він додатково викликає `module.UpdateVisual()` для кожного зареєстрованого модуля.
- При видаленні ефекту через `RemoveEffect` викликається `module.OnEffectRemoved(effectId)`.

### Specialized Runtime Data (IChipSpecialRuntimeData)
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
  - Оновлює стан `IChipSortingLayer` (`SetMoving(value)`) для коригування шарів сортування рендерерів на величину `MovingOrderOffset` (під час руху зміщення руху має пріоритет над ефектами).
  - Сповіщає всі ефекти через метод `OnMovingStateChanged(chip, isMoving)`.
  - На старті руху (`true`) додає в `IChipChangeNotifier` тимчасову подію `NewChip=null` для поточної клітинки, щоб observer-системи одразу відреагували на "тимчасовий вихід" чіпа; при завершенні (`false`) викликає `UpdateVisual()`.
- **`IsMoving()`**: Перевіряє візуальний стан переміщення. Повертає `true` як для перетягування користувачем, так і для системного переміщення.

### Sorting Layer and Visual Depth Management
Компонент `ChipSortingLayer` ([ChipSortingLayer.cs](../../Core/Scripts/Chips/ChipSortingLayer.cs)), що реалізує контракт `IChipSortingLayer` ([IChipSortingLayer.cs](../../Core/Scripts/Chips/Interfaces/IChipSortingLayer.cs)), керує порядком сортування (`sortingOrder`) усіх рендерерів чіпа. Він гарантує збереження відносного порядку глибини між різними спрайтами однієї фішки при змінах висоти, руху, активних ефектів, анімацій або ручних оверрайдів.

#### Initialization and Base Order Caching
- При виклику `Chip.Init` викликається `sortingLayer.Init()`.
- Кешуються вихідні значення `CachedOrder` для кожного рендерера з масиву `SortingLayers`.
- Заповнюються словники швидкого пошуку зсувів для активних ефектів (`effectOffsets`) та анімаційних тригерів (`animationOffsets`).
- Скидаються будь-які активні корутини анімацій, анімаційний оффсет та прапорець оверрайду (`sortingOrderOverride = null`).

#### Movement Sorting
- **`MovingOrderOffset`** (за замовчуванням `110`): однаковий зсув, що додається до `CachedOrder` усіх рендерерів під час переміщення фішки (`isMoving == true`).

#### Effects Sorting
- **`EffectsSortingData`** (`EffectSortingData[]`): конфігурація зсувів для конкретних ефектів (`[EffectSelector] EffectId` та `AdditionalOrder`).
- При активації ефекту (`Effect.Activate`) викликається `SetEffectActive(effectId, true)`, при деактивації — `SetEffectActive(effectId, false)`. Якщо активні кілька ефектів, використовується зсув останнього зареєстрованого ефекту.

#### Animation Sorting (AnimationsSortingData)
Функціонал **Animation Sorting** забезпечує динамічне тимчасове підвищення шару сортування фішки під час виконання конкретних анімацій (наприклад, під час спавну, підйому, зарядки або спеціальних дій), щоб фішка візуально перекривала сусідні елементи поля на час програвання кліпу.
- **Структура `AnimationSortingData`**:
  - `string TriggerName`: назва тригера / стану в `Animator`.
  - `int AdditionalOrder`: додатковий зсув сортування, який додається до базового порядку всіх рендерерів на час анімації.
- **Механізм роботи (`OnAnimationTrigger`)**:
  1. Коли `Chip.SendTrigger(trigger)` надсилає тригер в `Animator`, він автоматично сповіщає `sortingLayer?.OnAnimationTrigger(trigger)`.
  2. Якщо для `trigger` налаштовано зсув у `AnimationsSortingData`:
     - Негайно зупиняється будь-яка попередня активна корутина анімаційного сортування та скидається попередній оффсет.
     - Компонент автоматично визначає точну тривалість анімації (`duration`): перевіряє поточний стан `animator.GetCurrentAnimatorStateInfo(0)` та наступний стан `animator.GetNextAnimatorStateInfo(0)`, знаходить стан з іменем `triggerName` і зчитує `state.length`.
     - Застосовує `currentAnimationOffset = offset` та перераховує сортінг усіх рендерерів фішки.
     - Запускає корутину `AnimationSortingCoroutine(duration)`.
  3. По закінченню тривалості анімації корутина скидає `currentAnimationOffset = 0`, деактивує анімаційний стан і автоматично повертає сортінг рендерерів до базового рівня.
- **Переривання та безпека (Interruption & Safety)**:
  - Будь-який наступний виклик `OnAnimationTrigger` (навіть для тригера без налаштованого зсуву) негайно зупиняє поточну корутину анімаційного сортингу та скидає або замінює анімаційний зсув.
  - При деактивації об'єкта (`OnDisable`) активна корутина зупиняється, а анімаційний зсув скидається.

#### Manual Sorting Override
- **`SetSortingOrderOverride(int offset)`**: дозволяє примусово встановити довільний зсув сортування для фішки (наприклад, при показі в модальних діалогах, фокусуванні в туторіалі або спеціальних катсценах).
- **`ClearSortingOrderOverride()`**: скидає примусовий оверрайд та перераховує сортінг згідно з поточними динамічними станами.
- **Властивості стану**: `IsSortingOrderOverridden` (`bool`) та `SortingOrderOverride` (`int?`).

#### Priority Hierarchy
При одночасній наявності декількох факторів застосовується сувора ієрархія пріоритетів:
```
sortingOrderOverride > isMoving > activeEffects > animationOffset > baseOrder
```
1. **`sortingOrderOverride`**: має абсолютний найвищий пріоритет. Якщо задано, перекриває рух, ефекти та анімації.
2. **`isMoving` (`MovingOrderOffset`)**: діє під час перетягування або польоту фішки, перекриває активні ефекти та анімаційні зсуви.
3. **`activeEffects` (`EffectsSortingData`)**: діє при спокої фішки за наявності активних візуальних ефектів зі зсувом сортування.
4. **`animationOffset` (`AnimationsSortingData`)**: діє, коли фішка не рухається, немає оверрайду та активних ефектів зі зсувом.
5. **Базовий порядок (`baseOrder`)**: відновлює вихідні `CachedOrder` рендерерів, коли всі вищезазначені стани неактивні.

#### Spawning State
- **`IsSpawning`**: Властивість, що перевіряє, чи знаходиться фішка у процесі програвання анімації спавну (`Spawn`, `TapEvolutionSpawn`, `WaitEvolutionSpawn`, `EvolutionSpawn`).

#### Chip Lift Management
Висота підйому фішки над полем делегується окремому компоненту `IChipLiftController`:
- При початку перетягування у `DraggableChipLogic` викликається `Chip.LiftController?.StartFastLiftHeight()`, що плавно піднімає фішку.
- При завершенні перетягування викликається `Chip.LiftController?.StopLiftCoroutine()`.
- Під час польоту фішки (наприклад, snap back або релокація) висота підйому розраховується в `ChipFlyAnimation` і передається напряму: `chip.LiftController.LiftHeight = height`.
- Зміна висоти підйому автоматично транслюється ефекту тіні через метод `IShadowEffect.OnHeightChanged(height)`.

##### Blending Lift Height into Flight Animation
Коли фішка починає летіти, її поточна висота підйому (`chip.LiftController?.LiftHeight ?? 0f`) передається у `ChipFlyAnimation.StartAnimation` як параметр `initialLiftHeight`. Це забезпечує плавний перехід між висотою, на яку фішку підняв гравець під час drag, і дуговою траєкторією польоту:
- **Linear**: висота плавно зменшується від `initialLiftHeight` до `0` через `Mathf.Lerp`.
- **ArcBounce / HalfArcHalfBounce / HalfArc**: у фазі дуги поточна висота = `Mathf.Max(arcCurve, fadeLine)`, де `fadeLine = Mathf.Lerp(initialLiftHeight, 0f, tArc)`. Це не дає фішці "провалитися" нижче вихідного підйому.

Якщо фішка не була підняти (наприклад, при автоматичній релокації), `initialLiftHeight = 0f` і поведінка не відрізняється від базової.

### Flight Settings
Кожен чіп містить налаштування польоту `FlightSettings` типу `ChipFlightSettings` (структура), що визначає параметри переміщення фішки по сітці поля:
- **`Duration`**: Тривалість польоту в секундах.
- **`FlightDelay`**: Затримка перед початком польоту.
- **`Type`**: Тип траєкторії польоту (`FlightType`):
  - `Linear` — лінійний рух.
  - `ArcBounce` — параболічний рух із відскоками при приземленні (за замовчуванням).
  - `HalfArcHalfBounce` — рух з меншою висотою дуги та меншим відскоком (наприклад, при звичайному переміщенні або зміні місць).
  - `HalfArc` — рух по низькій дузі без відскоків.
- **`PauseAnimationOnDelay`**: Вказує, чи потрібно призупиняти аніматор чіпа під час затримки перед початком польоту.
- **`SetFlightSettings(ChipFlightSettings settings)`**: Оновлює налаштування польоту для наступного переміщення чіпа.

### Other Methods
- **`OnDraggingChipWithMoveLocked()`**: Віртуальний метод, що викликається при спробі перетягнути заблокований чіп. Спочатку намагається відправити тригер `"MoveLocked"` у `effectOfPrioritizingDestroying` (ефект з найвищим пріоритетом руйнування); якщо його немає — у ефект з ключем `EffectConsts.Blockers.MoveLockedEffect`. Використовує `allowRepeat=true` для забезпечення візуального відгуку на кожну спробу.

### Merge System
Реалізує логіку сумісності та процес злиття двох фішок через механізм `IChipInteractionLogic`.
Для повного опису (CanInteract, ExecuteInteraction, Weighted Random, Extra Chips, Relocation) див. [MergeableChipLogic](../Interactions/MergeableChipLogic.md).

