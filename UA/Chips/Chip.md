# Chip (Base Chip)

[← На Головну](../Main.md)

Базовий клас `Chip` є візуальним представленням та компонентом взаємодії для об'єктів на ігровому полі. Він відповідає за відображення стану, ефектів та обробку Unity подій (Input).

Сама логіка злиття (Merge) та переміщення винесена у відповідні логічні класи.

## Architecture and Responsibility

### 1. `Chip.cs` (Base Class)
Клас `Chip` є візуальним представленням та базовим компонентом.
- **Дані (Configuration - `ChipData`)**: Зберігає посилання на `ChipData`, який містить налаштування:
  - **Type**: Ідентифікатор типу фішки (string).
  - **PrefabLink**: Посилання на префаб фішки.
  - **Size**: Розмір фішки в клітинках (Vector2Int).
  - **specialDatas**: Поліморфна колекція (`SerializeReference`) для додаткових типізованих налаштувань чіпа, включно з `ChipMergeData`.
  - **GetSpecialData<T>()**: Типізований доступ до елемента `specialDatas` (або `default`, якщо тип не знайдено).
  - **IChipSpecialData**: Базовий контракт для спеціалізованих даних. Типові реалізації: `ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, `ChipMoveLockedData`.
- **Runtime Властивості**:
  - **CellPosition**: Поточна позиція фішки на сітці поля (Vector2Int). Оновлюється системою при переміщенні.
  - **RuntimeData**: Поточний стан (див. нижче).
  - **BlockingState**: (`CombinedBlockingState`) агрегований стан дозволів (наприклад, `CanBeMoved`, `CanBeMergedAsSource`), що визначається активними ефектами.
  - **MergeData**: Runtime-доступ до merge-конфігурації. Під час `Init(ChipData, ChipRuntimeData)` чіп кешує `data.GetSpecialData<ChipMergeData>()` у властивість `Chip.MergeData`.
  - **LogEnable**: Прапорець для ввімкнення логування подій чіпа в консоль.
- **Ефекти**: Керує візуальними ефектами, детальніше див. [Visual Effects](../Visuals/Effects.md):
  - `MergeAvailableEffect`: Підсвітка при можливості злиття ([ChipMergeAvailableEffect](../Visuals/Effects.md#2-merge-available)).
  - `CellHighlightEffect`: Підсвітка клітинки під фішкою ([CellHighlightEffect](../Visuals/Effects.md#1-cell-highlight)).
  - `MoveLockedEffect`: Візуальна індикація блокування ([Move Locked](../Visuals/Effects.md#6-move-locked)), що ініціалізується з `ChipMoveLockedData.Prefab` у `ChipData.specialDatas`. Цей ефект взаємодіє з налаштуваннями ефекту (`EffectBlockingSettings`), які застосовуються до фінального `BlockingState`.
- **Анімація**: Має посилання на `Animator` для відтворення станів (наприклад, `Merge`, `Generate`, `MoveLocked`).

## Effect Management

Базовий клас `Chip` автоматично відстежує всі візуальні ефекти, що належать йому, для коректної розсилки подій та очищення при знищенні.

### Effect Initialization
- **`InitEffects()`**: Віртуальний метод, який викликається з `Init(...)` для ініціалізації всіх ефектів. Базова реалізація створює та додає стандартні ефекти: `CellHighlight` і `MergeAvailable` з полів `ChipData`, а `MoveLocked` — через `GetSpecialData<ChipMoveLockedData>()` (поле `Prefab`). Цей метод призначений для перекриття в похідних класах, які можуть додавати свої спеціалізовані ефекти. Приклад: `ChipGenerator` перекриває цей метод, щоб додати `ChargedEffect` та `RechargeEffect`.

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
- **`ChipContainerRuntimeData`**: Додає словник прогресу заповнення контейнера (`containers`).


### 2. `MergeableChipLogic.cs` (Merge Logic)
Реалізує інтерфейс `IChipInteractionLogic`. Відповідає за перевірку можливості злиття та виконання самої операції.
- **Перевірка (`CanInteract`)**:
  - Перевіряє, що `sourceChip.MergeData != null`, і через `CanMerge` визначає, чи є цільова фішка (target) у списку допустимих партнерів.
- **Виконання (`ExecuteInteraction`)**:
  - Визначає результат злиття через `sourceCell.Chip.MergeData.GetNextChip(mainCell.Chip.Data)`.
  - Враховує ваги (Weighted Random), якщо для даної пари передбачено кілька результатів.
  - Обробляє ситуації, коли нова фішка більша за попередні (зсув сусідніх фішок через `IChipMovingLogic`).
  - Знищує обидві вихідні фішки.
  - Створює нову фішку через `ChipFactory`.
  - Запускає анімацію `Merge`.
  - **Extra Chips**: Якщо `MergeResult` містить `ExtraChip`, система перевіряє шанс його випадіння та спавнить додаткову фішку (використовуючи `FieldChipData` для ідентифікації) на найближчу вільну комірку.

### 3. `DraggableChipLogic.cs` (Control)
Обробляє Input гравця (Drag-and-Drop). Координує взаємодію між чіпами, викликаючи `IChipInteractionLogic` (наприклад, `MergeableChipLogic`) для перевірки та виконання дій.

## Merge Rules (ChipMergeData)

`ChipMergeData` — це об'єкт налаштувань, який визначає, з ким і як може зливатися чіп.
Він реалізує `IChipSpecialData` і зберігається в `ChipData.specialDatas`.

### Data Structure

1. **`MergeCombination`**: Визначає правила для конкретної пари фішок. Містить посилання на **`TargetChip`** (партнер) та список можливих результатів (**`Results`**).
2. **`MergeResult`**: Описує результат злиття (**`Result`**), його **Weight** (вагу) та можливий **`ExtraChip`**.
3. **`ExtraChip`**: Описує додаткову фішку (через `FieldChipData`) та шанс її отримання (**Chance**).

### Result Calculation

При злитті система використовує алгоритм **Weighted Random**:
- Збираються всі можливі результати для даного партнера.
- На основі їхніх ваг обирається один фінальний чіп.
- Якщо варіант лише один з вагою 100, він обирається гарантовано.

### Automation in the Editor

У `Chip Creator` merge-правила для `ChipMergeData` налаштовуються вручну в секції `Special Data`.
Система не додає auto-generated self-merge комбінацію.

## Merge Flow

1. **Input**: Гравець перетягує фішку (`DraggableChipLogic`).
2. **Move**: При переміщенні над іншою фішкою викликається `CanInteract`.
3. **Feedback**: Якщо `CanInteract` = true, на нижній фішці активується ефект (`mergeAvailableEffect`).
4. **Drop**: При відпусканні фішки викликається `ExecuteInteraction`.
5. **Result**: Система обирає результат, знищує старі фішки та створює нову через `ChipFactory`.
