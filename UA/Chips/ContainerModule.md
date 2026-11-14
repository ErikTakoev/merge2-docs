# ContainerModule (Container)

[← На Головну](../Main.md)

Логіка контейнера тепер реалізована за допомогою модульного компонента [ContainerModule](../../../Core/Scripts/Chips/ContainerModule.cs), який прикріплюється до базового GameObject фішки `Chip`. Він дозволяє фішці накопичувати інші фішки для виконання квестів або замовлень.

## Architecture and Responsibility

### 1. `ContainerModule.cs` (Container)
Клас `ContainerModule` зберігає стан наповнення та керує візуальними ефектами, реалізуючи інтерфейс `IChipModule`.
- **Властивості (Data)**:
  - `ChipContainerData`: Налаштування контейнера (список необхідних предметів `containers`, стратегія еволюції `EvolutionProvider` типу `IChipEvolutionProvider`), які отримуються через `data.GetSpecialData<ChipContainerData>()`. `EvolutionProvider` повертає список фішок для створення після заповнення контейнера (з підтримкою накладання `BlockerEffectIds`). Також кожна потрібна фішка визначає префаб візуального елемента через власні спеціальні дані `ChipContainerElementData`.
  - `ContainerRequirements`: Публічна властивість тільки для читання (`IReadOnlyDictionary<ContainerInfo, int>`), що надає доступ до поточних вимог контейнера для підсистеми `ChipCollections`.
  - `evolutionSettings` (`EvolutionExecutionSettings`): Серіалізовані налаштування еволюції при заповненні контейнера (тригери анімацій, прапорці). Доступні публічно через властивість `EvolutionSettings`.
- **Події та Делегати**:
  - `FillContainerDelegate`: Делегат для події оновлення стану контейнера.
  - `OnFillContainer`: Подія, що викликається при ініціалізації, додаванні предмету або повному заповненні.
- **Ефекти**:
  - **[ContainerHintEffect](../Visuals/Effects.md#3-container-requirements)**: Спеціальний ефект, що реалізує `IEffectContainerHint` та візуалізує необхідні предмети ("бабли") над контейнером, а також показує візуальні підказки. Призначається через `EffectContainerHintRef` (InterfaceRef).

### 1.1. `ChipContainerRuntimeData.cs` (Runtime State)
Реалізує маркерний інтерфейс `IChipSpecialRuntimeData` для зберігання динамічного стану контейнера всередині списку `specialRuntimeDatas` у `ChipRuntimeData`.

**Власні властивості**:
- **`containers`**: Словник (`Dictionary<ContainerInfo, int>`), що відслідковує поточний прогрес заповнення кожної вимоги.
  - **Ключ (`ContainerInfo`)**: Визначення вимоги (посилання на `RequiredChip` типу `ChipData`, необхідна кількість `Count`).
  - **Значення (`int`)**: Поточна кількість зібраних предметів для цієї вимоги.
  - **Завершення**: Коли вимога виконана повністю (лічильник досягає `ContainerInfo.Count`), вона видаляється зі словника. Коли словник порожній — контейнер вважається заповненим.
  - **Конструктори `ContainerInfo`**: Клас має два конструктори — порожній (для Unity-серіалізації в Inspector) та параметризований `ContainerInfo(ChipData requiredChip, int count = 1)` для створення вимог у runtime-коді (наприклад, при клонуванні `ChipData` у тестах або динамічному формуванні вимог контейнера).

**Ініціалізація**: Створюється при ініціалізації рантайм-даних модуля через `InitRuntimeData` на основі `ChipContainerData` (через `GetSpecialData<ChipContainerData>()`).

### 2. `FillContainerLogic.cs` (Logic)
Керує процесом додавання фішок до контейнера через механізм `IChipInteractionLogic`.  
Детальний опис (CanInteract, ExecuteInteraction, цільові об'єкти) див. [FillContainerLogic](../Interactions/FillContainerLogic.md).

## Key Methods (`ContainerModule.cs`)

### `IsChipCompatible(Chip chip)`
Перевіряє, чи відповідає `Data` фішки (прямим посиланням `RequiredChip == data`) хоча б одній із активних вимог у `containerRuntimeData.containers`. Не змінює стан.

### `TryAddChip(Chip chip)`
Основний метод додавання фішки:
1. Знаходить відповідну вимогу в словнику `containers` за прямим збігом `RequiredChip == chip.Data`.
2. Якщо лічильник досягає необхідної кількості:
   - Якщо це остання активна вимога (`isFull`): попередньо перевіряє доступність місця для еволюції контейнера через `evolutionService.CanEvolve(this.chip, chipContainerData?.EvolutionProvider, evolutionSettings, out evolutionPlan, cellsToFree: cellsToFreeBuffer)`, передаючи клітинки вхідного чіпа `chip` як такі, що звільняються (`cellsToFree`). Якщо місця немає — додавання відхиляється (`return false`), генерується подія `RaiseNotEnoughSpaceForEvolution`, а вхідна фішка залишається неушкодженою.
   - Видаляє виконану вимогу зі словника.
   - Знищує вхідну фішку (`chip.Destroy(chipCell, true)`), звільняючи зайняті нею клітинки.
   - Викликає `OnFillContainer` з прапорцем `isFull`.
   - **Завершення (`isFull == true`)**: Викликає `evolutionService.Evolve(this.chip, evolutionPlan, evolutionSettings)`, що руйнує сам контейнер та спавнить/переміщує нові фішки згідно з планом еволюції.
   - **Часткове виконання (`isFull == false`)**: Відправляє контейнеру анімаційний тригер `Recharge` та інформує колекції через `chipCollections.OnContainerRequirementsChanged(this, key.RequiredChip)`.
3. Якщо лічильник ще не повний — інкрементує його значення, знищує вхідну фішку (`chip.Destroy(chipCell, true)`) та викликає `OnFillContainer` з прапорцем `false`.
4. **Результат**: Повертає `true`, якщо фішка була успішно прийнята.

## Visual Effects

### ContainerHintEffect.cs
Спеціалізований ефект, що реалізує `IEffectContainerHint` для візуалізації вимог контейнера та відтворення візуальних підказок. Керує Panel над фішкою, в якій відображаються потрібні предмети.

Ефект зберігається в словнику `effects` з ключем `EffectConsts.ContainerRequirements` та доступний через:
```csharp
var containerEffect = GetEffect<IEffectContainerHint>(EffectConsts.ContainerRequirements);
containerEffect?.UpdateElements(this, containers, false);
```

Детальний опис реалізації ефекту див. у [Effects.md](../Visuals/Effects.md#3-container-requirements).

## Flow

1. **Input**: Гравець починає перетягувати фішку.
2. **Move**: Система через `FillContainerLogic.CanInteract` постійно перевіряє сумісність при наведенні на контейнери.
3. **Check**: Контейнер через `IsChipCompatible` підтверджує, що фішка йому потрібна.
4. **Drop**: При відпусканні викликається `ExecuteInteraction` -> `TryAddChip`.
5. **Update**: Контейнер видаляє/змінює вимоги, `ContainerHintEffect` оновлює "бабли" (або ховає їх, якщо вимога виконана).
6. **Complete**: Якщо вимог не залишилось, контейнер замінюється на нову фішку (result).
