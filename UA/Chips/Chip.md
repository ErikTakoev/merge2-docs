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
- При виклику `Destroy(Cell, bool force)`, система:
  1. Очищує occupancy в `FieldGrid`
  2. Викликає `ICellSubscriber.OnChipDestroy(mainCell)`
  3. Знищує всі ефекти зі словника `effects` з відповідною затримкою
  4. Запускає знищення самого `gameObject` з затримкою: 0.1s (якщо `force=true`) або 0.3s (якщо `force=false`). 
  
  Більша затримка при `force=false` дозволяє відіграти фінальні анімації (наприклад, анімацію злиття в ізометрії).

### Extensions for Specialized Chips
Спеціалізовані чіпи розширюють `InitEffects()` для додавання власних ефектів:
- **`ChipGenerator.InitEffects()`**: Додає `GeneratorCharging` та `GeneratorCharged` ефекти
- **`ChipContainer.InitEffects()`**: Додає `ContainerRequirements` ефект, реалізує `IEffectContainer`
- **`ChipPowerBooster.InitEffects()`**: Додає `PBoosterConnectorCells` та `PBoosterJoin` ефекти

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

### Other Methods
- **`OnDraggingChipWithMoveLocked()`**: Віртуальний метод, що викликається при спробі перетягнути заблокований чіп. Спочатку намагається відправити тригер `"MoveLocked"` у `effectOfPrioritizingDestroying` (ефект з найвищим пріоритетом руйнування); якщо його немає — у ефект з ключем `EffectConsts.Blockers.MoveLockedEffect`. Використовує `allowRepeat=true` для забезпечення візуального відгуку на кожну спробу.

### Extensions for Specialized Chips
- **`ChipGeneratorRuntimeData`**: Додає стан зарядки, таймери, лічильники перезарядок.
- **`ChipContainerRuntimeData`**: Додає словник прогресу заповнення контейнера.


### Merge System
Реалізує логіку сумісності та процес злиття двох фішок через механізм `IChipInteractionLogic`.  
Для повного опису (CanInteract, ExecuteInteraction, Weighted Random, Extra Chips, Relocation) див. [MergeableChipLogic](../Interactions/MergeableChipLogic.md).
