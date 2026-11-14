# UnlockAreaModule (Unlock Area Chip)

[← На Головну](../Main.md)

Модульний компонент [UnlockAreaModule](../../../Core/Scripts/Chips/UnlockAreaModule.cs) прикріплюється до базового GameObject фішки `Chip` і реалізує інтерфейс `IChipModule`. Він забезпечує відкриття заблокованих ділянок поля ([Locked Areas](../Features/LockedAreas.md)) при натисканні (тапі) на фішку та опціональний спавн або еволюцію в наступні фішки через `IChipEvolutionProvider` із підтримкою накладання блокерів (`ChipSpawnData`).

## Architecture and Responsibility

### 1. `UnlockAreaModule.cs` (Module Component)
Клас `UnlockAreaModule` є реалізацією `IChipModule`, що керує процесом валідації тапу, запуском послідовного розблокування ділянок сітки та синхронізацією життєвого циклу знищення фішки.
- **Властивості та конфігурація**:
  - `hintEffect` (`EffectHintRef`): Посилання на візуальну підказку тапу (`TapHint`).
  - `evolutionSettings` (`EvolutionExecutionSettings`): Серіалізовані налаштування еволюції (тригери анімацій знищення/спавну, точка польоту `EvolutionPoint`, затримки `DelayBeforeFirstSpawn`/`DelayBetweenSpawns`, прапорець `PlayMergeLight`). Доступні публічно через властивість `EvolutionSettings`.
- **Залежності (`[Inject]`)**:
  - `ILockedAreaManager`: Керує розблокуванням цільових ділянок сітки.
  - `IChipEvolutionService`: Централізований сервіс еволюції для розміщення, спавну та польоту фішок.

### 1.1. `ChipUnlockAreaData.cs` (Configuration Data)
Реалізує інтерфейс `IChipSpecialData` для налаштування фішок, які відкривають ділянки поля:
- **`Steps`** (`UnlockAreaStep[]`): Послідовний список кроків розблокування. Кожен елемент `UnlockAreaStep` містить:
  - `AreaId` (`int`): Унікальний ID заблокованої ділянки в `ILockedAreaManager`.
  - `Delay` (`float`): Затримка в секундах перед відкриттям цієї конкретної ділянки.
- **`EvolutionProvider`** (`IChipEvolutionProvider`): Поліморфна стратегія (`[SerializeReference]`), що визначає, які фішки з'являються після розблокування (`ConstantEvolutionProvider`, `RandomEvolutionProvider`) у вигляді `ChipSpawnData` (з опціональними `BlockerEffectIds`). Якщо провайдер відсутній або повертає порожній список, фішка просто знищується.

---

## Key Methods (`UnlockAreaModule.cs`)

### `Init(Chip chip, ChipData data, ChipRuntimeData runtimeData)`
Кешує посилання на базовий `Chip` та конфігурацію `ChipUnlockAreaData`. Ініціалізує `EvolutionProvider?.Init()`. Якщо налаштовано `hintEffect`, реєструє ефект у чіпі з хешем `EffectConsts.TapHint`.

### `CanFinishDestroy()`
Контракт інтерфейсу `IChipModule`. Повертає `!isUnlocking`. Якщо корутина розблокування ділянок ще активна, метод повертає `false`, тим самим блокуючи остаточне знищення GameObject фішки в `Chip.FinishDestroy()` та встановлюючи прапорець `isFinishDestroyPending = true`.

### `UpdateVisual()`
Оновлює видимість підказки тапу (`EffectConsts.TapHint`): якщо фішка доступна для тапу (`chip.BlockingState.CanBeTaped`) та зараз не виконується процес розблокування (`!isUnlocking`), ефект підказки активується (`tapHint.Activate(chip)`), інакше — деактивується.

### `OnTap()`
Обробляє натискання на фішку. Перевіряє валідність стану: фішка не повинна знищуватися (`!chip.IsDestroying`), переміщуватися (`!chip.IsMoving`, `!chip.IsDragging()`), вже перебувати в процесі відкриття (`!isUnlocking`) або бути заблокованою для тапу (`chip.BlockingState.CanBeTaped`). При успішній перевірці викликає `ExecuteUnlock()`.

### `ExecuteUnlock()`
1. Перевіряє можливість еволюції та наявність вільного місця через `evolutionService.CanEvolve(chip, unlockData.EvolutionProvider, evolutionSettings, out EvolutionPlan plan)`. Якщо вільного місця на полі недостатньо, виконання зупиняється.
2. Встановлює `isUnlocking = true`, виконує еволюцію `evolutionService.Evolve(chip, plan, evolutionSettings)` та деактивує підказку `TapHint`.
3. Запускає корутину розблокування `StartUnlockSequenceRoutine()`. Оскільки `isUnlocking == true`, остаточне видалення GameObject відкладається до завершення корутини.

### `UnlockSequenceRoutine()`
Корутина послідовного розблокування:
1. Ітерує кроки масиву `unlockData.Steps`.
2. Якщо на кроці вказано `step.Delay > 0f`, виконує `yield return new WaitForSeconds(step.Delay)`.
3. Викликає `lockedAreaManager?.UnlockArea(step.AreaId, force: false)`.
4. Після завершення всіх кроків скидає `isUnlocking = false` та очищує посилання на корутину.
5. Якщо фішка очікує завершення знищення (`chip.IsFinishDestroyPending || chip.IsDestroying`), викликає `chip.FinishDestroy()` для фінального видалення об'єкта.

---

## Visual Effects

### TapHint (EffectConsts.TapHint)
Візуальна підказка у вигляді пальця/стрілки над фішкою, яка сигналізує гравцеві про можливість натиснути на неї. Керується через `hintEffect` (`EffectHintRef`) та автоматично приховується на час розблокування зон.

### MergeLight (EffectConsts.MergeLight)
Імпульсний ефект спалаху світла, що активується під час успішного запуску еволюції або знищення ключової фішки.

---

## Flow

1. **Tap**: Гравець натискає на фішку з модулем `UnlockAreaModule`.
2. **Validation**: Модуль перевіряє доступність тапу (`CanBeTaped`) та відсутність активних переміщень.
3. **Space Check**: Якщо фішка налаштована на еволюцію після відкриття, система через `evolutionService.CanEvolve` перевіряє доступність вільних клітинок на сітці. Якщо місця немає — подія скасовується з викликом `RaiseNotEnoughSpaceForEvolution`.
4. **Immediate Evolution & Destroy**: Поточна фішка негайно запускає анімацію знищення (`TapEvolutionDestroy`/`Destroy`), нові фішки спавняться та розлітаються по цільових клітинках через `evolutionService.Evolve`.
5. **Parallel Unlock Sequence**: Паралельно корутина `UnlockSequenceRoutine` послідовно чекає вказані затримки `Delay` та відкриває кожну зону через `LockedAreaManager.UnlockArea`.
6. **Deferred Destroy**: Якщо анімація знищення фішки завершується раніше закінчення кроків відкриття, повне видалення GameObject відкладається (`CanFinishDestroy() == false`). Щойно останній крок розблокування завершено, `FinishDestroy()` доводить знищення до кінця.

