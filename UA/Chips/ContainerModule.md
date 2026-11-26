# ContainerModule (Container)

[← На Головну](../Main.md)

Логіка контейнера реалізована за допомогою модульного компонента [ContainerModule](../../../ChipModules/Container/Scripts/ContainerModule.cs), який прикріплюється до базового GameObject фішки `Chip`. Він поглинає інші чіпи, щоб отримати більш цінний.

Модуль розміщено у папці `ChipModules/Container/` під простором імен `Expecto.MergeBase.Container` та окремою збіркою `MergeBase.Container` (`defineConstraints: ["MERGE_BASE_CONTAINER"]`). У `MergeClassicLifetimeScope` під директивою `#if MERGE_BASE_CONTAINER` реєструються `ContainerCollectionHandler` (як `IInitializable` + `IChipCollectionHandler` + `AsSelf()`) та `ContainerHintNotifier`.

## Architecture and Responsibility

### 1. `ContainerModule.cs` (Container)
Клас `ContainerModule` зберігає стан наповнення та керує візуальними ефектами, реалізуючи інтерфейс `IChipModule`. Помічений атрибутом `[RequireModuleData(typeof(ChipContainerData))]`.
- **Властивості (Data)**:
  - `ChipContainerData`: Налаштування контейнера (список необхідних предметів `containers`, стратегія еволюції `EvolutionProvider` типу `IChipEvolutionProvider`), які отримуються через `data.GetModuleData<ChipContainerData>()`. `EvolutionProvider` повертає список фішок для створення після заповнення контейнера (з підтримкою накладання `BlockerEffectIds`). Також кожна потрібна фішка визначає префаб візуального елемента через власні дані модуля `ChipContainerElementData`.
  - `ContainerRequirements`: Публічна властивість тільки для читання (`IReadOnlyDictionary<ContainerInfo, int>`), що надає доступ до поточних вимог контейнера для `ContainerCollectionHandler`.
  - `evolutionSettings` (`EvolutionExecutionSettings`): Серіалізовані налаштування еволюції при заповненні контейнера, згруповані у `Triggers` (`DestroyTrigger`, `EmptyDestroyTrigger`, `SpawnTrigger`, `GrowTrigger`, `MultiSpawnTrigger`), `SpawnFlight` (`KeepPrimaryChipInPlace`, `MultiChipFlightSettings`, затримки, `EvolutionPoint`) та `Behavior` (`PlayMergeLight`, `ForceDestroyOnEmpty`, `NotifyNotEnoughSpace`). Доступні публічно через властивість `EvolutionSettings`.
- **Події та Делегати**:
  - `FillContainerDelegate`: Делегат для події оновлення стану контейнера.
  - `OnFillContainer`: Подія, що викликається при ініціалізації, додаванні предмету або повному заповненні.
- **Ефекти**:
  - **[ContainerHintEffect](../Visuals/Effects.md#3-container-requirements)**: Візуальний ефект, що реалізує `IEffectContainerHint` та візуалізує необхідні предмети ("бабли") над контейнером, а також показує візуальні підказки. Призначається через `EffectContainerHintRef` (InterfaceRef).

### 1.1. `ChipContainerRuntimeData.cs` (Runtime State)
Реалізує маркерний інтерфейс `IChipModuleRuntimeData` для зберігання динамічного стану контейнера всередині списку `moduleRuntimeDatas` у `ChipRuntimeData`.

**Власні властивості**:
- **`containers`**: Словник (`Dictionary<ContainerInfo, int>`), що відслідковує поточний прогрес заповнення кожної вимоги.
  - **Ключ (`ContainerInfo`)**: Визначення вимоги (посилання на `RequiredChip` типу `ChipData`, необхідна кількість `Count`).
  - **Значення (`int`)**: Поточна кількість зібраних предметів для цієї вимоги.
  - **Завершення**: Коли вимога виконана повністю (лічильник досягає `ContainerInfo.Count`), вона видаляється зі словника. Коли словник порожній — контейнер вважається заповненим.
  - **Конструктори `ContainerInfo`**: Клас має два конструктори — порожній (для Unity-серіалізації в Inspector) та параметризований `ContainerInfo(ChipData requiredChip, int count = 1)` для створення вимог у runtime-коді (наприклад, при клонуванні `ChipData` у тестах або динамічному формуванні вимог контейнера).

**Ініціалізація**: Створюється при ініціалізації рантайм-даних модуля через `InitRuntimeData` на основі `ChipContainerData` (через `GetModuleData<ChipContainerData>()`).

### 1.2. `ContainerCollectionHandler.cs` (Collections)
Клас `ContainerCollectionHandler` реалізує `IChipCollectionHandler`, `IInitializable` та `IDisposable`, інкапсулюючи індекс заповнюваних контейнерів у збірці `MergeBase.Container`:
- Підтримує словник `FillableChipsByData` (`Dictionary<ChipData, List<Chip>>`), де ключ — `ChipData` необхідної фішки, а значення — список активних контейнерів із незаблокованими клітинками та `BlockingState.CanBeFilled == true`.
- При ініціалізації реєструє себе як обробник життєвого циклу (`chipCollections.RegisterHandler(this)`) та реєструє словник під ключем `ChipCollectionType.Fillable` (`chipCollections.RegisterCollection(ChipCollectionType.Fillable, fillableChipsByData)`).
- При `Dispose()` відписує колекцію та обробник від `IChipCollections`.
- Через `OnContainerRequirementsChanged(ContainerModule, ChipData)` точково видаляє контейнер із ключа виконаної вимоги без перебирання всього словника.

### 2. Drag-and-Drop Interaction (`CanInteract` & `OnInteract`)
Логіка взаємодії при перетягуванні реалізована безпосередньо у `ContainerModule` як частина контракту `IChipModule`:
- **`CanInteract(ICell sourceCell, ICell targetCell)`**: Дозволяє взаємодію, якщо `sourceCell` і `targetCell` валідні, містять фішки, `targetCell.Chip` є цим контейнером (`targetChip == chip`), вхідна фішка не збігається з контейнером (`sourceChip != chip`), контейнер не заблокований для наповнення (`chip.BlockingState.CanBeFilled`) і `IsChipCompatible(sourceChip)` повертає `true`.
- **`OnInteract(ICell sourceCell, ICell targetCell, out DragInteractionResult result)`**: Повторно перевіряє `CanInteract` і викликає `TryAddChip(sourceCell.Chip)`. При успіху повертає `DragInteractionResult.Partial` (якщо в `ContainerRequirements` ще залишилися вимоги) або `DragInteractionResult.Full` (якщо контейнер повністю заповнено). При невдачі повертає `false` із `DragInteractionResult.None`.

Детальніше про життєвий цикл перетягування див. [ChipInteractions](../Interactions/ChipInteractions.md).

## Key Methods (`ContainerModule.cs`)

### `IsChipCompatible(Chip chip)`
Перевіряє, чи відповідає `Data` фішки (прямим посиланням `RequiredChip == data`) хоча б одній із активних вимог у `containerRuntimeData.containers`. Не змінює стан.

### `TryAddChip(Chip chip)`
Основний метод додавання фішки:
1. Знаходить відповідну вимогу в словнику `containers` за прямим збігом `RequiredChip == chip.Data`.
2. Якщо лічильник досягає необхідної кількості:
   - Якщо це остання активна вимога (`isFull`): попередньо перевіряє доступність місця для еволюції контейнера через `evolutionService.CanEvolve(this.chip, chipContainerData?.EvolutionProvider, out evolutionPlan, evolutionSettings.Behavior.NotifyNotEnoughSpace, cellsToFree: cellsToFreeBuffer)`, передаючи клітинки вхідного чіпа `chip` як такі, що звільняються (`cellsToFree`). Якщо місця немає — додавання відхиляється (`return false`), генерується подія `RaiseNotEnoughSpaceForEvolution`, а вхідна фішка залишається неушкодженою.
   - Видаляє виконану вимогу зі словника.
   - Знищує вхідну фішку (`chip.Destroy(chipCell, true)`), звільняючи зайняті нею клітинки.
   - Викликає `OnFillContainer` з прапорцем `isFull`.
   - **Завершення (`isFull == true`)**: Викликає `evolutionService.Evolve(this.chip, evolutionPlan, evolutionSettings)`, що руйнує сам контейнер та спавнить/переміщує нові фішки згідно з планом еволюції.
   - **Часткове виконання (`isFull == false`)**: Відправляє контейнеру анімаційний тригер `Recharge` та інформує колекції через `containerCollections?.OnContainerRequirementsChanged(this, key.RequiredChip)`.
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

1. **Input**: Гравець починає перетягувати фішку; `MergeHintDragFeedback` підсвічує сумісні контейнери, отримані через `IChipCollections.GetCollection(chip.Data, ChipCollectionType.Fillable)` (індекс підтримується `ContainerCollectionHandler`).
2. **Hover (`CanInteract`)**: `DraggableChipLogic` через `targetChip.CanInteract` -> `ContainerModule.CanInteract` перевіряє `BlockingState.CanBeFilled` і сумісність (`IsChipCompatible`).
3. **Drop (`OnInteract`)**: При відпусканні викликається `ContainerModule.OnInteract` -> `TryAddChip`.
4. **Update**: Контейнер оновлює вимоги, `ContainerHintEffect` оновлює "бабли", а `OnInteract` повертає `DragInteractionResult.Partial` або `DragInteractionResult.Full`.
5. **Complete**: Якщо вимог не залишилось, `TryAddChip` заздалегідь валідує місце через `IChipEvolutionService.CanEvolve(..., cellsToFree)` та запускає `Evolve` для заміни контейнера результуючими фішками.
