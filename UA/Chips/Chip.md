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
  - **IChipSpecialData**: Базовий контракт для спеціалізованих даних. Типові реалізації: `ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, `ChipMoveLockedData`.

- **Runtime**:
  - **CellPosition**: Поточна позиція фішки на сітці поля (Vector2Int). Оновлюється системою при переміщенні.
  - **RuntimeData**: Поточний стан (див. нижче).
  - **BlockingState**: (`CombinedBlockingState`) агрегований стан дозволів (наприклад, `CanBeMoved`, `CanBeMergedAsSource`), що визначається активними ефектами.
- **Others**:
  - **LogEnable**: Прапорець для ввімкнення логування подій чіпа в консоль.
- **Effects**: Керує трьома основними ефектами (Cell Highlight, Merge Available, Move Locked).  
  Для повного каталогу, дизайну та реалізації див. [Visual Effects](../Visuals/Effects.md).
- **Animations**: Має посилання на `Animator` для відтворення станів (наприклад, `Merge`, `Generate`, `MoveLocked`).

## Effect Management

Базовий клас `Chip` автоматично відстежує всі візуальні ефекти, що належать йому, для коректної розсилки подій та очищення при знищенні.

### Effect Initialization
- **`InitEffects()`**: Віртуальний метод, який викликається з `Init(...)` для ініціалізації всіх ефектів. Базова реалізація створює та додає стандартні ефекти: `CellHighlight` і `MergeAvailable` з полів `ChipData`, а `MoveLocked` — через `GetSpecialData<ChipMoveLockedData>()`. Цей метод призначений для перекриття в похідних класах, які можуть додавати свої спеціалізовані ефекти. Приклад: `ChipGenerator` перекриває цей метод, щоб додати `ChargedEffect` та `RechargeEffect`.

### Effect List Management
- **`effects` (List<Effect>)**: Список усіх активних ефектів чіпа. Використовуєтся для ітерації при зміні стану клітинок або взаємодії.
- **`InstantiateEffect<T>(GameObject prefab)`**: Допоміжний метод для створення ефектів з префабів. Він автоматично інстанціює об'єкт та викликає `Init(this)`.

### Effect Lifecycle
- Всі ефекти, додані до списку `effects`, автоматично отримують сповіщення через методи `OnChangedCell`, `OnInteractionOverCellChanged` та `OnInteractionUnderCellChanged`.
- При виклику `Destroy(Cell)`, базова реалізація спочатку очищає occupancy в `FieldGrid`, потім викликає `ICellSubscriber.OnChipDestroy(mainCell)`, знищує всі ефекти зі списку `effects`, і лише після цього запускає відкладене `Destroy(gameObject, 0.1f)`.

### Movement State Management
Система розрізняє **стан перетягування користувачем** та **візуальний стан переміщення**:

#### User Drag State
- **`SetDragging(bool)`**: Встановлює стан перетягування користувачем. Викликається `DraggableChipLogic` при початку/завершенні перетягування. Автоматично викликає `SetMoving(true)` при необхідності.
- **`IsDragging()`**: Перевіряє, чи перетягується чіп користувачем. Відстежує саме взаємодію з користувачем, а не лише візуальне переміщення.

#### Visual Movement State
- **`SetMoving(bool)`**: Керує візуальним станом переміщення (змінює `sortingOrder`). Викликається як при перетягуванні користувачем, так і при автоматичному переміщенні системою. На старті руху (`true`) додає в `IChipChangeNotifier` тимчасову подію `NewChip=null` для поточної клітинки, щоб observer-системи одразу відреагували на "тимчасовий вихід" чіпа; при завершенні (`false`) викликає `UpdateVisual()`.
- **`IsMoving()`**: Перевіряє візуальний стан переміщення (за `sortingOrder`). Повертає `true` як для перетягування користувачем, так і для системного переміщення.

### Other Methods
- **`OnDraggingChipWithMoveLocked()`**: Віртуальний метод, що викликається при спробі перетягнути заблокований чіп. Відтворює анімацію `MoveLocked` на чіпі та його ефекті блокування, надаючи візуальний зворотний зв'язок гравцеві. Використовує параметрі `allowRepeat=true` для ефекту, щоб кожна спроба супроводжувалася візуальним відгуком.

### Extensions for Specialized Chips
- **`ChipGeneratorRuntimeData`**: Додає стан зарядки, таймери, лічильники перезарядок.
- **`ChipContainerRuntimeData`**: Додає словник прогресу заповнення контейнера.


### Merge System
Реалізує логіку сумісності та процес злиття двох фішок через механізм `IChipInteractionLogic`.  
Для повного опису (CanInteract, ExecuteInteraction, Weighted Random, Extra Chips, Relocation) див. [MergeableChipLogic](../Interactions/MergeableChipLogic.md).
