# ChipInteractions (Interactions)

[← На Головну](../Main.md)

Цей розділ описує механізм, за допомогою якого загальна логіка перетягування фішок (`DraggableChipLogic`) делегує виконання специфічних взаємодій (таких як злиття або наповнення контейнера) через інтерфейс `IChipInteractionLogic`, а також керує візуальним зворотним зв'язком під час перетягування через інтерфейс `IDragFeedback`.

## Architecture Overview

Система взаємодій та візуального фідбеку побудована на принципі декупажу (роз'єднання). `DraggableChipLogic` відповідає за фізичне переміщення фішки та виявлення цільових об'єктів під нею, але не містить жорстко закодованої логіки конкретних ігрових взаємодій чи візуальних ефектів.

Замість цього система використовує списки обробників, що реалізують відповідні інтерфейси:
- `IChipInteractionLogic` — для перевірки та виконання взаємодій (злиття, контейнери).
- `IDragFeedback` — для відображення візуальних підказок у життєвому циклі перетягування (підсвітка сумісних фішок, лічильники груп).

## Transition Process

### 1. Initialization
При старті (`Awake`), `DraggableChipLogic` збирає всі компоненти на об'єкті, що реалізують `IChipInteractionLogic` та `IDragFeedback`:
```csharp
IChipInteractionLogic[] interactionLogics = GetComponents<IChipInteractionLogic>();
IDragFeedback[] feedbacks = GetComponents<IDragFeedback>();
```

### 2. Drag Lifecycle & Feedback Callbacks
Під час життєвого циклу перетягування `DraggableChipLogic` сповіщає всі компоненти `IDragFeedback`:
- **`OnDragStart`**: Викликається `feedback.OnDragStartFeedback(Chip)`. Компоненти фідбеку (наприклад, `MergeHintDragFeedback`) активують відповідні підказки.
- **`OnDrag` (при зміні якоря)**: Коли фішка переміщується на нову клітинку, викликається `feedback.OnDragFeedback(Chip, prevCell, newCell)`. Використовується для оновлення динамічного фідбеку при зміні позиції.
- **`OnDragEnd`**: Викликається `feedback.OnDragEndFeedback(Chip)`. Всі візуальні підказки деактивовуються.

### 3. Continuous Interaction Checking (OnDrag)
Під час перетягування у методі `UpdateInteractionState` система постійно перевіряє доступні логіки взаємодії:
- Визначається список клітинок, які займає фішка (враховуючи розмір фішки).
- Для кожної логіки `IChipInteractionLogic` викликається `CanInteract(sourceCell, targetCell)`.
- Якщо логіка повертає `true`, вона встановлюється як `currentMergableLogic`.

### 4. Interaction Completion (OnDragEnd)
Коли гравець відпускає фішку:
- Спочатку викликаються фідбек-методи завершення `OnDragEndFeedback`.
- Якщо знайдено активну `currentMergableLogic`, викликається `ExecuteInteraction(sourceCell, targetCell)`.
- Якщо взаємодія успішна (`ExecuteInteraction` повертає `true`), `DraggableChipLogic` виконує уніфіковані ефекти пост-інтеракції:
  - Сповіщення сусідніх фішок про проведення взаємодії за допомогою `NotifyNeighborsOfInteraction`.
  - Активацію візуального ефекту підсвічування `MergeLight` на цільовій фішці.
- Якщо взаємодія не знайдена або неможлива, фішка повертається на місце або переміщується (Swap/Move) через `IChipMovingLogic`.

## IChipInteractionLogic Interface

Кожен реалізований обробник взаємодії повинен мати два основні методи:
- `CanInteract`: Перевірка валідності (наприклад, чи однакові ідентифікатори фішок для злиття).
- `ExecuteInteraction`: Безпосередня зміна стану гри (знищення старих фішок, створення нових).

## IDragFeedback Interface

Кожен компонент візуального фідбеку реалізує три класичні методи життєвого циклу:
- `OnDragStartFeedback(Chip chip)`: Початок перетягування.
- `OnDragFeedback(Chip chip, ICell prevCell, ICell newCell)`: Зміна клітинки-якоря під час перетягування.
- `OnDragEndFeedback(Chip chip)`: Завершення або скасування перетягування.

## Implementations
- **[MergeableChipLogic](MergeableChipLogic.md)** — Логіка злиття двох фішок в одну вищого рівня.
- **[FillContainerLogic](FillContainerLogic.md)** — Логіка додавання фішки в контейнер.
- **`MergeHintDragFeedback`** — Компонент фідбеку, що активує ефекти `MergeHint` на сумісних фішках та контейнерах.
