# GeneratorModule (Generator)

[← На Головну](../Main.md)

Логіка генерації тепер реалізована за допомогою модульного компонента [GeneratorModule](../../../Core/Scripts/Chips/GeneratorModule.cs), який прикріплюється до базового GameObject фішки `Chip`. Він працює як автомат станів (State Machine), що перемикається між заряджанням та готовністю до генерації.

`GeneratorModule` також реалізує інтерфейс `IPowerBoosterTarget`, що дозволяє прискорювати генерацію сусідніми бустерами.

## Architecture and Responsibility

### 1. `GeneratorModule.cs` (Main Component)
Клас `GeneratorModule` керує життєвим циклом генератора на чіпі, реалізуючи інтерфейси `IChipModule` та `IPowerBoosterTarget`.
- **Властивості та конфігурація**:
  - `rechargeEffect` (`ChargingEffectRef`): Ефект індикації перезарядки.
  - `chargedEffect` (`EffectRef`): Ефект готовності генератора.
  - `evolutionSettings` (`EvolutionExecutionSettings`): Серіалізовані налаштування еволюції при вичерпанні циклів генератора (тригери анімацій, прапорець `ForceDestroyOnEmpty`). Доступні публічно через властивість `EvolutionSettings`.
  - `generationSettings` (`GenerationExecutionSettings`): Налаштування анімації та затримки спавну фішок (`DelayBetweenSpawns`). Доступні публічно через властивість `GenerationSettings`.
- **Вхідні дані (Input)**:
  - **Manual**: Обробляє `OnTap` через делегування подій з `Chip` (перевіряє стан блокування `BlockingState.CanBeTaped` та `BlockingState.CanGenerate`).
  - **Auto**: Підписується на `field.OnChangeField`, щоб автоматично спавнити при появі вільного місця (перевіряє `BlockingState.CanGenerate`).
- **Ініціалізація даних**: У `Init(Chip, ChipData, ChipRuntimeData)` читає конфігурацію через `data.GetSpecialData<ChipGeneratorData>()`. Якщо `ChipGeneratorData` відсутній, логіка генератора не запускається. Ініціалізує провайдери через `generatorData.GenerationProvider?.Init()` та `generatorData.EvolutionProvider?.Init()`.
- **Update Loop**: У методі `Update` перевіряє стан блокування чіпа (`chip.BlockingState.CanCharge`). Якщо зарядка заблокована, процес заряджання зупиняється; якщо дозволена і генератор розряджений, таймер `ChargingTimeLeft` інкрементується з урахуванням множника швидкості `powerMultiplier`.
- **Залежності**:
  - `IFreeCellFinder`: Логіка атомарного пошуку вільних клітинок (`FindFreeCellsForChips`).
  - `ChipFactory`: Фабрика для створення нових об'єктів.
  - `ChipDataCollection`: Колекція для резолву `ChipData` за ідентифікатором `ChipId`.
  - `IChipEvolutionService`: Сервіс для валідації та виконання еволюції після вичерпання зарядів/перезарядок.
  - `IScenarioEventHandler`: Для сповіщень про нестачу вільного місця.
  - `IFieldGrid`: Для перевірки меж та отримання клітинок поля.

### 2. `ChipGeneratorData.cs` (Configuration)
Дані з налаштуванням та алгоритмами вибору. Реалізує `IChipSpecialData` та додається до `ChipData.specialDatas`.
- **Параметри**: `ChargeCount`, `ChargingTime`, `GenerationInterval`, `TotalRecharges`, `EvolutionProvider`, `GenerationProvider`
- **Стратегії генерації (`IChipGenerationProvider`)**: Поліморфна стратегія (`[SerializeReference]`), що визначає результат генерації у вигляді списку `ChipSpawnData` (з опціональними блокерами `BlockerEffectIds`):
  - **`ConstantGenerationProvider`**: Повертає фіксований список `List<ChipSpawnData> Chips`.
  - **`RandomGenerationProvider`**: Обирає випадковий вкладений під-провайдер за зваженими коефіцієнтами (`WeightedProviderEntry[] Entries`).
  - **`SequenceGenerationProvider`**: Послідовно перемикає кроки (`[SerializeReference] IChipGenerationProvider[] Steps`) за формулою `SequenceIndex % Steps.Length`, циклічно повторюючи послідовність.
- **Еволюція**: Поле `EvolutionProvider` типу `IChipEvolutionProvider` визначає фішки для створення після вичерпання всіх циклів перезарядок. Якщо поточна генерація остання перед еволюцією, виконується попередня перевірка вільного місця як під згенеровані фішки (будь-якого розміру), так і під еволюцію через `IChipEvolutionService.CanEvolve(..., cellsToExclude)`. Лише при успішній перевірці обох умов спавняться фішки та викликається `Evolve(chip, plan, evolutionSettings)` з використанням налаштувань `evolutionSettings`.

### 3. `ChipGeneratorRuntimeData.cs` (Runtime State)
Реалізує маркерний інтерфейс `IChipSpecialRuntimeData` для зберігання динамічного стану генератора всередині списку `specialRuntimeDatas` у `ChipRuntimeData`.

**Власні властивості**:
- **`IsCharged`**: Чи готовий генератор до створення нової фішки. Коли `true`, генератор може спавнити чіп (вручну або автоматично).
- **`ChargeCount`**: Кількість фішок, які генератор може створити в поточному циклі зарядки. Зменшується при кожній генерації.
- **`ChargingTimeLeft`**: Поточний час зарядки (в секундах). Збільшується в `Update()` до досягнення `CurrentTargetChargingTime`.
- **`RechargesLeft`**: Кількість циклів перезарядки, що залишилися. Коли досягає 0, генератор еволюціонує через `EvolutionProvider` або знищується.
- **`IsWaitingForSpace`**: Стан очікування вільного місця (тільки для Auto режиму). Коли `true`, генератор чекає на появу вільної клітинки.
- **`SequenceIndex`**: Поточний індекс послідовності для `SequenceGenerationProvider`. Інкрементується тільки при успішному спавні та виклику `Confirm` на провайдері.
- **`CurrentTargetChargingTime`**: Поточний цільовий час для завершення зарядки. Може дорівнювати:
  - `ChargingTime` — при повній перезарядці (коли `ChargeCount` досяг 0).
  - `GenerationInterval` — при короткій затримці між генераціями в рамках одного циклу.

**Ініціалізація**: Створюється при ініціалізації рантайм-даних модуля через `InitRuntimeData` на основі `ChipGeneratorData` (отриманого через `GetSpecialData<ChipGeneratorData>()`).

### Recharge
1. **Update**: У кожному кадрі збільшується `ChargingTimeLeft` до досягнення `CurrentTargetChargingTime`.
2. **Visuals**: Викликається подія `OnCharging` -> `ChipGeneratorEffect` оновлює маску прогресу.
3. **Complete**: Коли час вичерпано, відновлюється готовність (`IsCharged`). Якщо це був повний цикл, відновлюється кількість зарядів. Викликається `UpdateVisual()` (якщо чіп не перетягується).

## Effects and Visualization
Генератор керує двома ефектами, які зберігаються в `effects` словнику з ключами від `EffectConsts`:

- **`EffectConsts.GeneratorCharging`**: Звертання через `GetEffect<IChargingEffect>(EffectConsts.GeneratorCharging)`
  - Реалізація: [ChargingEffect](../Visuals/Effects.md#5-generator-charging)
  - Відображає прогрес перезарядки. Активний, коли `IsCharged` = `false`.
  - Реалізує інтерфейс `IChargingEffect`, його метод `OnCharging(float progress)` оновлює маску перезарядки та повертає стрілку.

- **`EffectConsts.GeneratorCharged`**: Звертання через `GetEffect(EffectConsts.GeneratorCharged)`
  - Реалізація: базовий `Effect`
  - Активний, коли `IsCharged` = `true` та чіп не перетягується.
  - Зазвичай це cyclic idle-анімація (світіння, пульсація), що показує гравцеві готовність об'єкта.

- **Animator**: Використовує тригери `Generate` (при спавні), `Recharge` (при завершенні зарядки), `EvolutionDestroy` (при руйнуванні генератора під час еволюції) та `EvolutionSpawn` (при створенні еволюційованого чіпа на місці зношеного генератора).
