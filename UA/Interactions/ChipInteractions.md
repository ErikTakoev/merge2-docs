# ChipInteractions (Interactions)

[← На Головну](../Main.md)

Цей розділ описує механізм, за допомогою якого загальна логіка перетягування фішок (`DraggableChipLogic`) делегує виконання специфічних взаємодій (таких як злиття або наповнення контейнера) через інтерфейс `IChipInteractionLogic`.

## Architecture Overview

Система взаємодій побудована на принципі декупажу (роз'єднання). `DraggableChipLogic` відповідає за фізичне переміщення фішки та виявлення цільових об'єктів під нею, але він "не знає" деталей того, що саме має статися при накладанні однієї фішки на іншу.

Замість жорсткого кодування логіки злиття в основний клас перетягування, система використовує список обробників, що реалізують інтерфейс `IChipInteractionLogic`.

## Transition Process

### 1. Initialization
При старті (`Awake`), `DraggableChipLogic` збирає всі компоненти на об'єкті, що реалізують `IChipInteractionLogic`:
```csharp
IChipInteractionLogic[] interactionLogics = GetComponents<IChipInteractionLogic>();
```

### 2. Continuous Checking (OnDrag)
Під час перетягування, у методі `UpdateInteractionState`, система постійно перевіряє доступні логіки взаємодії:
- Визначається список клітинок, які займає фішка (враховуючи розмір фішки).
- Для кожної логіки викликається `CanInteract(sourceCell, targetCell)`.
- Якщо логіка повертає `true`, вона встановлюється як `currentMergableLogic`.

### 3. Interaction Completion (OnDragEnd)
Коли гравець відпускає фішку:
- Якщо знайдено активну `currentMergableLogic`, викликається `ExecuteInteraction(sourceCell, targetCell)`.
- Якщо взаємодія успішна, обробка завершується.
- Якщо взаємодія не знайдена або неможлива, фішка повертається на місце або переміщується (Swap/Move) через `IChipMovingLogic`.

## IChipInteractionLogic Interface

Кожен реалізований обробник повинен мати два основні методи:
- `CanInteract`: Перевірка валідності (наприклад, чи однакові ідентифікатори фішок для злиття).
- `ExecuteInteraction`: Безпосередня зміна стану гри (знищення старих фішок, створення нових).

## Implementations
- **[MergeableChipLogic](MergeableChipLogic.md)** — Логіка злиття двох фішок в одну вищого рівня.
- **[FillContainerLogic](FillContainerLogic.md)** — Логіка додавання фішки в контейнер.
