# Merge2Module

[← На Головну](../Main.md) | [← До ChipInteractions](../Interactions/ChipInteractions.md)

`Merge2Module` — це модульний компонент ([Merge2Module.cs](../../../ChipModules/Merge2/Scripts/Merge2Module.cs)), що реалізує інтерфейс `IChipModule` і відповідає за механіку злиття двох сумісних фішок на базі конфігурації `ChipMerge2Data`.

Модуль розміщено у папці `ChipModules/Merge2/` під простором імен `Expecto.MergeBase.Merge2` та окремою збіркою `MergeBase.Merge2` (`defineConstraints: ["MERGE_BASE_MERGE2"]`).

## Purpose

Обробка перевірки сумісності фішок, пошуку цілей для підказок при перетягуванні та делегування розміщення, релокації сусідів і створення результатів злиття централізованому сервісу `IChipEvolutionService`.

## Architecture and Responsibility

### 1. `Merge2Module.cs` (Module Component)
Прикріплюється до базових префабів фішок (наприклад, `ChipAbstract`) і реалізує `IChipModule` **без** атрибута `[RequireModuleData]`, щоб не-мержабельні фішки без `ChipMerge2Data` залишалися валідними при перевірці в редакторі.
- **Залежності (`[Inject]`)**:
  - `IChipEvolutionService`: Сервіс валідації простору, планування релокацій сусідів, знищення цільової фішки, спавну результатів та підтвердження провайдера.
  - `IFieldGrid`: Отримання клітинок перетягуваної фішки для формування `cellsToFreeBuffer`.
- **Налаштування (`evolutionSettings`)**: Серіалізована структура `EvolutionExecutionSettings` (публічно доступна через `EvolutionSettings`), згрупована у три блоки:
  - `Triggers`: `DestroyTrigger = Chip.AnimatorTrigger.Destroy`, `EmptyDestroyTrigger = Chip.AnimatorTrigger.Destroy`, `SpawnTrigger = Chip.AnimatorTrigger.Merge`, `GrowTrigger = Chip.AnimatorTrigger.MergeGrow`, `MultiSpawnTrigger = Chip.AnimatorTrigger.Generate`
  - `SpawnFlight`: `KeepPrimaryChipInPlace = true` (основна результуюча фішка залишається у цільовій позиції, тоді як додаткові летять із `MultiChipFlightSettings = ChipFlightSettings.ChipRelocate`)
  - `Behavior`: `PlayMergeLight = false` (підсвітка результату керується через `MergeLightDragFeedback` на `DragInteractionResult.Full`), `ForceDestroyOnEmpty = false`, `NotifyNotEnoughSpace = true`

### 2. `ChipMerge2Data.cs` (Configuration)
Конфігураційний клас ([ChipMerge2Data.cs](../../../ChipModules/Merge2/Scripts/Data/ChipMerge2Data.cs)), що реалізує `IChipModuleData` та зберігається у `ChipData.moduleDatas`:
- **`Combinations`** (`MergeCombination[]`): Масив правил злиття, де кожний запис `MergeCombination` містить:
  - **`TargetChip`** (`ChipData`): Партнер, із яким дозволено злиття.
  - **`EvolutionProvider`** (`IChipEvolutionProvider`, `[SerializeReference]`): Поліморфна стратегія результату:
    - **`ConstantEvolutionProvider`**: Фіксований список `Chips` (`List<ChipSpawnData>`). Перший елемент (`Chips[0]`) стає основним результатом (із підтримкою збільшення розміру та релокації), решта (`Chips[1..]`) спавняться поруч.
    - **`RandomEvolutionProvider`**: Ваговий випадковий вибір (`WeightedProviderEntry[]`) серед вкладених провайдерів `IChipEvolutionProvider`. Результат кешується у `LastResult` до виклику `Confirm()`, тому при невдалій спробі злиття (наприклад, через брак місця) повторна спроба повертає той самий результат.
    - Кожен `ChipSpawnData` задає `ChipId` та опціональний масив `BlockerEffectIds` для початкових блокер-ефектів.
- **Ключові методи**:
  - `Init()`: Ініціалізує всі `EvolutionProvider` у `Combinations` (зокрема попередньо обчислює суми ваг у `RandomEvolutionProvider`).
  - `MergeableChips`: Повертає сумісні `ChipData` з валідними комбінаціями для підказок.
  - `TryGetEvolutionProvider(ChipData otherChip, out IChipEvolutionProvider provider)` / `CanMerge(ChipData otherChip)`: Перевіряють сумісність без передчасного виклику `GetEvolutionChips()`.

### 3. `MergeAvailableEffect.cs` (Visual Effect)
Компонент ефекту ([MergeAvailableEffect.cs](../../../ChipModules/Merge2/Scripts/Effects/MergeAvailableEffect.cs)), що наслідує `Effect` під ключем `EffectConsts.MergeAvailable` та налаштовується через `ChipEffectsData.OtherEffects`. Відстежує внутрішній стан `isActive` для запобігання дубльованим викликам `Activate` та `Deactivate` під час `OnInteractionUnderCellChanged`. Детальніше див. [Visual Effects](../Visuals/Effects.md#1-merge-available).

## How it Works

### Initialization (`Init`)
Під час `Chip.Init` модуль `Merge2Module` зберігає посилання на `chip`, кешує `data?.GetModuleData<ChipMerge2Data>()` та викликає `mergeData?.Init()`.

### Validation (`CanInteract`)
Перед початком злиття система перевіряє:
1. Обидві клітинки (`sourceCell`, `targetCell`) не `null` і містять фішки.
2. `sourceCell.Chip` збігається з власником модуля (`chip`) і не дорівнює `targetCell.Chip`.
3. Перетягувана фішка має дозвіл `sourceChip.BlockingState.CanBeMergedAsSource`, а цільова — `targetChip.BlockingState.CanBeMergedAsTarget`.
4. Конфігурація `mergeData` існує і `mergeData.CanMerge(targetChip.Data)` повертає `true` (без обчислення випадкового результату під час наведення).

### Execution (`OnInteract`)
Коли гравець відпускає фішку над валідною ціллю:
1. **Повторна валідація та резолв провайдера**: Повторно перевіряється `CanInteract(sourceCell, targetCell)`, визначається `mainCell` цілі та отримується `evolutionProvider` через `mergeData.TryGetEvolutionProvider(mainCell.Chip.Data, out evolutionProvider)`.
2. **Позначення звільнюваних клітинок**: Клітинки вихідної фішки (`sourceMainCell.CellPosition`, `sourceChip.Data.Size`) записуються у багаторазовий буфер `cellsToFreeBuffer` через `fieldGrid.GetCells(..., clear: true)`.
3. **Атомарна перевірка простору та планування (`IChipEvolutionService.CanEvolve`)**:
   - Викликається `evolutionService.CanEvolve(targetChip, evolutionProvider, out EvolutionPlan plan, evolutionSettings.Behavior.NotifyNotEnoughSpace, cellsToFree: cellsToFreeBuffer)`.
   - `ChipEvolutionPlanner` отримує список `ChipSpawnData` з `evolutionProvider.GetEvolutionChips()`, резолвить `ChipData`, за потреби обмежує якір основної фішки межами поля через `IChipMovingLogic.ClampCellToField`, перевіряє перетин/збільшення розміру та через `IChipMovingLogic.TryResolveCellPosition` підбирає позицію з 0 або мінімальною кількістю релокацій сусідів (ігноруючи заблоковані клітинки, `targetChip` та фішки у `cellsToFreeBuffer`).
   - Якщо провайдер повертає кілька фішок (`Chips[1..]`), `ChipEvolutionPlanner` додатково атомарно перевіряє вільні клітинки через `IFreeCellFinder.FindFreeCellsForChips` (`TryFindExtraChipPlacements`) з урахуванням звільнюваних і зайнятих релокаціями клітинок.
   - Якщо місця недостатньо: `ChipEvolutionService` викликає `IMergeEventHandler.RaiseNotEnoughSpaceForEvolution` (оскільки `Behavior.NotifyNotEnoughSpace == true`), `Merge2Module` викликає `targetChip.OnDraggingChipWithMoveLocked()` і повертає `false` із `DragInteractionResult.None` без знищення фішок та без `Confirm()`.
4. **Виконання злиття (`sourceChip.Destroy` + `IChipEvolutionService.Evolve`)**:
   - Вихідна фішка знищується негайно (`sourceChip.Destroy(sourceMainCell, true)`).
   - Викликається `evolutionService.Evolve(targetChip, plan, evolutionSettings)`, який через `ChipEvolutionExecutor`:
     - при порожньому плані (`plan.IsEmpty`) руйнує цільову фішку та викликає `plan.Provider?.Confirm()`;
     - інакше руйнує цільову фішку (`Destroy(mainCell, false, settings.Triggers.DestroyTrigger)`), виконує заплановані релокації сусідів (`chipMovingLogic.ChipsRelocate` із `ChipFlightSettings.ChipRelocate`), створює основний результат (`chips[0]`) із `ChipFlightSettings.ChipGrow` та тригером `Triggers.GrowTrigger` (`MergeGrow`) при збільшенні розміру або на місці з `Triggers.SpawnTrigger` (`Merge`) при `SpawnFlight.KeepPrimaryChipInPlace == true`, запускає політ додаткових фішок (`chips[1..]`) із `SpawnFlight.MultiChipFlightSettings` (`ChipFlightSettings.ChipRelocate`) і тригером `Triggers.MultiSpawnTrigger` (`Generate`), і викликає `plan.Provider?.Confirm()`.
   - `Merge2Module.OnInteract` завершується з `result = DragInteractionResult.Full` і повертає `true`.
5. **Пост-обробка в `DraggableChipLogic` та Feedback**: Після успішного `OnInteract` система `DraggableChipLogic` сповіщає сусідів цільової клітинки (`NotifyNeighborsOfInteraction`) та викликає `IDragFeedback.OnDragEndFeedback(..., DragInteractionResult.Full)`, що активує ефект `MergeLight` через `MergeLightDragFeedback`.

### Hint Discovery (`CollectHintTargets`)
Під час початку перетягування `MergeHintDragFeedback` викликає `Chip.CollectHintTargets`, який опитує `Merge2Module.CollectHintTargets`. Модуль перебирає `mergeData.MergeableChips`, знаходить активні незаблоковані екземпляри у `collections.MergeableChipsByData` і додає їх до множини цілей `targets` (виключаючи саму перетягувану фішку).

> [!NOTE]
> Якщо `OnInteract` повертає `false` (наприклад, через неможливість релокувати сусідів або нестачу місця для додаткових фішок), `DraggableChipLogic` повертає перетягувану фішку на вихідну позицію (`ChipFlightSettings.ChipSwap`), а `Confirm()` у провайдера не викликається.
