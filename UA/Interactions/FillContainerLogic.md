# FillContainerLogic

[← На Головну](../Main.md) | [← До ChipInteractions](ChipInteractions.md)

`FillContainerLogic` — це реалізація `IChipInteractionLogic`, яка дозволяє фішкам взаємодіяти з контейнерами.

## Purpose
Дозволяє гравцеві перетягнути фішку-інгредієнт на фішку-контейнер для її наповнення та виконання завдань.

## How it Works

### Validation (CanInteract)
Система дозволяє взаємодію, якщо:
1. Цільова фішка є спадкоємцем класу `ChipContainer`.
2. Контейнер не має заборон на заповнення (`targetChip.BlockingState.CanBeFilled`).
3. Вихідна фішка дозволяє використання себе як ресурсу (`sourceChip.BlockingState.CanBeMergedAsSource`).
4. Вихідна фішка є сумісною з цим контейнером (перевіряється через `chipContainer.IsChipCompatible`).

### Execution (ExecuteInteraction)
При завершенні взаємодії:
1. Викликається `TryAddChip` у контейнера.
2. Якщо контейнер успішно прийняв фішку (вона відповідає умовам квесту і в контейнері є місце):
   - Вихідна фішка знищується.
   - Контейнер оновлює свій внутрішній стан.

## More Details on Containers
Сама логіка наповнення є дуже простою, оскільки делегує більшість роботи самому об'єкту контейнера. Для отримання інформації про те, як налаштувати типи інгредієнтів та нагороди, дивіться основний розділ:
- **[ChipContainer](../Chips/ChipContainer.md)**
