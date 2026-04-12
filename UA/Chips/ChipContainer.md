# ChipContainer (Container)

[← На Головну](../Main.md)

`ChipContainer` — це інтерактивна фішка, яка приймає в себе інші фішки для виконання квестів або замовлень. Вона успадковується від базового класу `Chip` і розширює його функціонал можливістю накопичення предметів.

## Architecture and Responsibility

### 1. `ChipContainer.cs` (Container)
Клас `ChipContainer` зберігає стан наповнення та керує візуальними ефектами.
- **Властивості (Data)**:
  - `ChipContainerData`: Налаштування контейнера (список необхідних предметів `containers`, нагорода `NextChipData`), які отримуються через `data.GetSpecialData<ChipContainerData>()`.
- **Події та Делегати**:
  - `FillContainerDelegate`: Делегат для події оновлення стану контейнера.
  - `OnFillContainer`: Подія, що викликається при ініціалізації, додаванні предмету або повному заповненні.
- **Події та Делегати**:
  - `FillContainerDelegate`: Делегат для події оновлення стану контейнера.
  - `OnFillContainer`: Подія, що викликається при ініціалізації, додаванні предмету або повному заповненні.
- **Ефекти**:
  - **[ChipContainerEffect](../Visuals/Effects.md#3-container-requirements)**: Спеціальний ефект, що реалізує `IEffectContainer` та візуалізує необхідні предмети ("бабли") над контейнером. Призначається через `EffectContainerRef` (InterfaceRef).

### 1.1. `ChipContainerRuntimeData.cs` (Runtime State)
Розширює базовий `ChipRuntimeData` для зберігання динамічного стану контейнера.

**Успадковані властивості** (від `ChipRuntimeData`):
- **`IsMoveLocked`**: Індикатор заблокованості (спадкується від базового стану). Впливає на логіку опосередковано через активацію ефекту, конфігурація якого згодом поповнює загальний `BlockingState` (див. [Chip Runtime Data](Chip.md#runtime-data)).

**Власні властивості**:
- **`containers`**: Словник (`Dictionary<ContainerInfo, int>`), що відслідковує поточний прогрес заповнення кожної вимоги.
  - **Ключ (`ContainerInfo`)**: Визначення вимоги (тип предмету, ID, необхідна кількість).
  - **Значення (`int`)**: Поточна кількість зібраних предметів для цієї вимоги.
  - **Завершення**: Коли вимога виконана повністю (лічильник досягає `ContainerInfo.Count`), вона видаляється зі словника. Коли словник порожній — контейнер вважається заповненим.

**Ініціалізація**: Створюється в `ChipContainer.Init()` на основі даних з `ChipContainerData` (через `GetSpecialData<ChipContainerData>()`). Початкові значення лічильників встановлюються в 0.

### 2. `FillContainerLogic.cs` (Logic)
Керує процесом додавання фішок до контейнера через механізм `IChipInteractionLogic`.  
Детальний опис (CanInteract, ExecuteInteraction, цільові об'єкти) див. [FillContainerLogic](../Interactions/FillContainerLogic.md).

## Key Methods (`ChipContainer.cs`)

### `IsChipCompatible(Chip chip)`
Перевіряє, чи відповідає `Type` або `Name` фішки хоча б одній із активних вимог у `containerRuntimeData.containers`. Не змінює стан.

### `TryAddChip(Chip chip)`
Основний метод додавання фішки:
1. Знаходить відповідну вимогу в словнику `containers`.
2. Якщо лічильник досягає необхідної кількості:
   - Видаляє вимогу зі словника.
   - Відправляє тригер анімації `Recharge`.
   - Викликає `OnFillContainer` з прапорцем `isFull`.
   - **Завершення**: Якщо всі вимоги виконані, контейнер знищується через `this.Destroy(cell)`, а на його місці створюється `NextChipData` за допомогою `ChipFactory`.
3. Якщо лічильник ще не повний — просто інкрементує його значення.
4. **Результат**: Повертає `true`, якщо фішка була успішно прийнята.

## Visual Effects

### ChipContainerEffect.cs
Спеціалізований ефект, що реалізує `IEffectContainer` для візуалізації вимог контейнера. Керує Panel над фішкою, в якій відображаються потрібні предмети.

Ефект зберігається в словнику `effects` з ключем `EffectConsts.ContainerRequirements` та доступний через:
```csharp
var containerEffect = GetEffect<IEffectContainer>(EffectConsts.ContainerRequirements);
containerEffect?.UpdateElements(this, containers, false);
```

Детальний опис реалізації ефекту див. у [Effects.md](../Visuals/Effects.md#3-container-requirements).

## Flow

1. **Input**: Гравець починає перетягувати фішку.
2. **Move**: Система через `FillContainerLogic.CanInteract` постійно перевіряє сумісність при наведенні на контейнери.
3. **Check**: Контейнер через `IsChipCompatible` підтверджує, що фішка йому потрібна.
4. **Drop**: При відпусканні викликається `ExecuteInteraction` -> `TryAddChip`.
5. **Update**: Контейнер видаляє/змінює вимоги, `ChipContainerEffect` оновлює "бабли" (або ховає їх, якщо вимога виконана).
6. **Complete**: Якщо вимог не залишилось, контейнер замінюється на нову фішку (result).
