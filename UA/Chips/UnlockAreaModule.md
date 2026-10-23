# UnlockAreaModule (Unlock Area Chip)

[← На Головну](../Main.md)

Модульний компонент [UnlockAreaModule](../../../Core/Scripts/Chips/UnlockAreaModule.cs) прикріплюється до базового GameObject фішки `Chip` і реалізує інтерфейс `IChipModule`. Він забезпечує відкриття заблокованих ділянок поля ([Locked Areas](../Features/LockedAreas.md)) при натисканні (тапі) на фішку та опціональний спавн або еволюцію в наступні фішки через `INextChipsProvider`.

## Architecture and Responsibility

### 1. `UnlockAreaModule.cs` (Module Component)
Клас `UnlockAreaModule` є реалізацією `IChipModule`, що керує процесом валідації тапу, запуском послідовного розблокування ділянок сітки та синхронізацією життєвого циклу знищення фішки.
- **Властивості та конфігурація**:
  - `hintEffect` (`EffectHintRef`): Посилання на візуальну підказку тапу (`TapHint`).
  - `delayBeforeFirstSpawn` (`float`): Початкова затримка перед спавном першої нової фішки при еволюції.
  - `delayBetweenSpawns` (`float`): Інтервал між спавном наступних фішок у батчі.
  - `generationPoint` (`Transform`): Точка спавну наступних фішок (якщо не призначено, використовується позиція поточної фішки).
- **Залежності (`[Inject]`)**:
  - `ILockedAreaManager`: Керує розблокуванням цільових ділянок сітки.
  - `IFieldGrid`: Доступ до комірок та координат сітки.
  - `ChipFactory`: Створення нових фішок після активації.
  - `IFreeCellFinder`: Перевірка та резервування вільних клітинок для наступних фішок.
  - `IScenarioEventHandler`: Сповіщення про сценарії та помилки (наприклад, `RaiseNotEnoughSpace`).

### 1.1. `ChipUnlockAreaData.cs` (Configuration Data)
Реалізує інтерфейс `IChipSpecialData` для налаштування фішок, які відкривають ділянки поля:
- **`Steps`** (`UnlockAreaStep[]`): Послідовний список кроків розблокування. Кожен елемент `UnlockAreaStep` містить:
  - `AreaId` (`int`): Унікальний ID заблокованої ділянки в `ILockedAreaManager`.
  - `Delay` (`float`): Затримка в секундах перед відкриттям цієї конкретної ділянки.
- **`Provider`** (`INextChipsProvider`): Поліморфна стратегія (`[SerializeReference]`), що визначає, які фішки з'являються після розблокування (наприклад, `ConstantNextChipsProvider`, `SequentialNextChipsProvider`). Якщо провайдер відсутній або повертає порожній список, фішка просто знищується.

### 1.2. `ChipNextChipsRuntimeData.cs` (Runtime State)
Зберігає індекс поточної послідовності для провайдерів типу `SequentialNextChipsProvider` у списку `specialRuntimeDatas` всередині `ChipRuntimeData`. Ініціалізується під час `InitRuntimeData`.

---

## Key Methods (`UnlockAreaModule.cs`)

### `Init(Chip chip, ChipData data, ChipRuntimeData runtimeData)`
Кешує посилання на базовий `Chip`, конфігурацію `ChipUnlockAreaData` та стан послідовності. Ініціалізує `Provider?.Init()`. Якщо налаштовано `hintEffect`, реєструє ефект у чіпі з хешем `EffectConsts.TapHint`.

### `CanFinishDestroy()`
Контракт інтерфейсу `IChipModule`. Повертає `!isUnlocking`. Якщо корутина розблокування ділянок ще активна, метод повертає `false`, тим самим блокуючи остаточне знищення GameObject фішки в `Chip.FinishDestroy()` та встановлюючи прапорець `isFinishDestroyPending = true`.

### `UpdateVisual()`
Оновлює видимість підказки тапу (`EffectConsts.TapHint`): якщо фішка доступна для тапу (`chip.BlockingState.CanBeTaped`) та зараз не виконується процес розблокування (`!isUnlocking`), ефект підказки активується (`tapHint.Activate(chip)`), інакше — деактивується.

### `OnTap()`
Обробляє натискання на фішку. Перевіряє валідність стану: фішка не повинна знищуватися (`!chip.IsDestroying`), переміщуватися (`!chip.IsMoving`, `!chip.IsDragging()`), вже перебувати в процесі відкриття (`!isUnlocking`) або бути заблокованою для тапу (`chip.BlockingState.CanBeTaped`). При успішній перевірці викликає `ExecuteUnlock()`.

### `ExecuteUnlock()`
1. Отримує список наступних фішок через `unlockData.Provider.GetNextChips(cachedSeqData)`.
2. Якщо наступні фішки налаштовані, викликає `freeCellFinder.FindFreeCellsForChips` для перевірки наявності вільних клітинок. Якщо вільного місця недостатньо, викликає `scenarioEventHandler?.RaiseNotEnoughSpace(chip)` і перериває виконання.
3. Встановлює `isUnlocking = true`, деактивує підказку `TapHint`.
4. Запускає паралельну корутину розблокування ділянок через `StartUnlockSequenceRoutine()`.
5. Одразу передає керування у `ExecuteDestructionAndSpawn(...)`.

### `ExecuteDestructionAndSpawn(...)`
1. Запускає знищення поточної фішки:
   - Якщо наступних фішок немає: `chip.Destroy(mainCell, false, Chip.AnimatorTrigger.Destroy)`.
   - Якщо фішки є: `chip.Destroy(mainCell, false, Chip.AnimatorTrigger.TapEvolutionDestroy)`.
2. Спавнить наступні фішки через `chipFactory.CreateChip` у знайдені цільові клітинки:
   - Для кожної фішки налаштовується розрахунок затримки польоту `flightDelay = delayBeforeFirstSpawn + (i * delayBetweenSpawns)`.
   - Застосовуються налаштування польоту `ChipFlightSettings` з параметром `pauseAnimationOnDelay = true` та надсилається тригер `Chip.AnimatorTrigger.Generate` або `TapEvolutionSpawn`.
3. Активує ефект `MergeLight` (`EffectConsts.MergeLight`) з параметром `Little = true`.
4. Фіксує використання провайдера через `unlockData.Provider.Confirm(cachedSeqData)`.

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
3. **Space Check**: Якщо фішка налаштована на спавн нових предметів після відкриття, система через `FindFreeCellsForChips` перевіряє доступність вільних клітинок на сітці. Якщо місця немає — подія скасовується з викликом `RaiseNotEnoughSpace`.
4. **Immediate Evolution & Destroy**: Поточна фішка негайно запускає анімацію знищення (`TapEvolutionDestroy`/`Destroy`), нові фішки спавняться та розлітаються по цільових клітинках.
5. **Parallel Unlock Sequence**: Паралельно корутина `UnlockSequenceRoutine` послідовно чекає вказані затримки `Delay` та відкриває кожну зону через `LockedAreaManager.UnlockArea`.
6. **Deferred Destroy**: Якщо анімація знищення фішки завершується раніше закінчення кроків відкриття, повне видалення GameObject відкладається (`CanFinishDestroy() == false`). Щойно останній крок розблокування завершено, `FinishDestroy()` доводить знищення до кінця.

