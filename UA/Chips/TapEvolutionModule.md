# TapEvolutionModule (Tap Evolution)

[← На Головну](../Main.md)

Модульний компонент [TapEvolutionModule](../../../Core/Scripts/Chips/TapEvolutionModule.cs) прикріплюється до базового GameObject фішки `Chip` і реалізує інтерфейс `IChipModule`. Він забезпечує еволюцію фішки в наступні фішки або її знищення при натисканні (тапі) користувача через стратегію `IChipEvolutionProvider` із підтримкою накладання блокерів (`ChipSpawnData`).

## Architecture and Responsibility

### 1. `TapEvolutionModule.cs` (Main Component)
Клас `TapEvolutionModule` керує обробкою тапу та виконанням еволюції, делегуючи процес сервісу `IChipEvolutionService`.
- **Властивості та конфігурація**:
  - `hintEffect` (`EffectHintRef`): Посилання на візуальну підказку тапу (`TapHint`).
  - `evolutionSettings` (`EvolutionExecutionSettings`): Серіалізовані налаштування еволюції (тригери анімацій `TapEvolutionDestroy`/`TapEvolutionSpawn`, точка польоту `EvolutionPoint`, затримки `DelayBeforeFirstSpawn`/`DelayBetweenSpawns`, прапорець `PlayMergeLight = true`). Доступні публічно через гетер `EvolutionSettings`.
- **Залежності (`[Inject]`)**:
  - `IChipEvolutionService`: Централізований сервіс еволюції для розміщення, спавну та польоту фішок.

### 2. `ChipTapEvolutionData.cs` (Configuration)
Конфігураційні дані для еволюції за тапом. Реалізує `IChipSpecialData` та додається до `ChipData.specialDatas`:
- **`EvolutionProvider`** (`IChipEvolutionProvider`): Поліморфна стратегія (`[SerializeReference]`), що визначає результат еволюції (`ConstantEvolutionProvider`, `RandomEvolutionProvider`) у вигляді списку `ChipSpawnData` (з опціональними блокерами `BlockerEffectIds`).

---

## Evolution Process (Flow)

1. **User Tap**: Коли користувач натискає на фішку, викликається метод `OnTap()`.
2. **Pre-Conditions**: Перевіряються умови:
   - Фішка не знищується (`!chip.IsDestroying`).
   - Фішка доступна для тапу (`chip.BlockingState.CanBeTaped == true`).
   - Фішка не рухається та не перетягується (`!chip.IsMoving && !chip.IsDragging()`).
3. **Execute Evolution**:
   - Перевіряє можливість еволюції та обчислює план розміщення через `evolutionService.CanEvolve(chip, evolutionData.EvolutionProvider, evolutionSettings, out EvolutionPlan plan)`. Якщо місця недостатньо, викликається `RaiseNotEnoughSpaceForEvolution`.
   - Виконує еволюцію через `evolutionService.Evolve(chip, plan, evolutionSettings)`.
   - Запускається анімація знищення батьківської фішки з тригером `evolutionSettings.DestroyTrigger` (`TapEvolutionDestroy`).
   - Нові фішки спавняться або летять у цільові клітинки з тригером `evolutionSettings.SpawnTrigger` (`TapEvolutionSpawn`).
   - Якщо провайдер порожній, батьківська фішка руйнується з тригером `evolutionSettings.EmptyDestroyTrigger` (`Destroy`).

---

## Visual Effects

- **`EffectConsts.TapHint`**: Звертання через `GetEffect(EffectConsts.TapHint)`
  - Підказка доступності тапу (наприклад, анімація пульсації чи пальця).
  - Активується в `UpdateVisual()`, якщо `chip.BlockingState.CanBeTaped == true`.
