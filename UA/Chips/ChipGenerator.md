# ChipGenerator (Generator)

[← На Головну](../Main.md)

`ChipGenerator` — це спеціалізований компонент, який створює (спавнить) нові фішки на ігровому полі. Він працює як автомат станів (State Machine), що перемикається між заряджанням та готовністю до генерації.

## Architecture and Responsibility

### 1. `ChipGenerator.cs` (Main Component)
Клас `ChipGenerator` керує життєвим циклом генератора, об'єднуючи логіку оновлення та події.
- **Вхідні дані (Input)**:
  - **Manual**: Обробляє `OnTap` (клік гравця).
  - **Auto**: Підписується на `field.OnChangeField`, щоб автоматично спавнити при появі вільного місця.
- **Ініціалізація даних**: У `Init(ChipData)` читає конфігурацію через `data.GetSpecialData<ChipGeneratorData>()`. Якщо `ChipGeneratorData` відсутній, логіка генератора не запускається.
- **Update Loop**: У методі `Update` керує таймером перезарядки.
- **Залежності**:
  - `IFreeCellFinder`: Логіка пошуку найближчої вільної клітинки.
  - `ChipFactory`: Фабрика для створення нових об'єктів.

### 2. `ChipGeneratorData.cs` (Configuration)
Дані з налаштуванням та алгоритмами вибору. Реалізує `IChipSpecialData` та додається до `ChipData.specialDatas`.
- **Параметри**: `ChargeCount`, `ChargingTime`, `GenerationInterval`, `TotalRecharges`
- **Вибір фішок**: Використовує механізм **Weighted Random** для обрання типу (див. [MergeableChipLogic](../Interactions/MergeableChipLogic.md#result-calculation-weighted-random))

### 3. `ChipGeneratorRuntimeData.cs` (Runtime State)
Розширює базовий `ChipRuntimeData` для зберігання динамічного стану генератора.

**Успадковані властивості** (від `ChipRuntimeData`):
- **`EffectEnables`** (`HashSet<int>`): Набір активних extra-ефектів (з `EffectConsts`/`EffectConsts.Extra`). Впливає на логіку опосередковано через активацію відповідних ефектів, конфігурація яких згодом поповнює загальний `BlockingState` (див. [Chip Runtime Data](Chip.md#runtime-data)).

**Власні властивості**:
- **`IsCharged`**: Чи готовий генератор до створення нової фішки. Коли `true`, генератор може спавнити чіп (вручну або автоматично).
- **`ChargeCount`**: Кількість фішок, які генератор може створити в поточному циклі зарядки. Зменшується при кожній генерації.
- **`ChargingTimeLeft`**: Поточний час зарядки (в секундах). Збільшується в `Update()` до досягнення `CurrentTargetChargingTime`.
- **`RechargesLeft`**: Кількість циклів перезарядки, що залишилися. Коли досягає 0, генератор еволюціонує в `NextChipData` або знищується.
- **`IsWaitingForSpace`**: Стан очікування вільного місця (тільки для Auto режиму). Коли `true`, генератор чекає на появу вільної клітинки.
- **`CurrentTargetChargingTime`**: Поточний цільовий час для завершення зарядки. Може дорівнювати:
  - `ChargingTime` — при повній перезарядці (коли `ChargeCount` досяг 0).
  - `GenerationInterval` — при короткій затримці між генераціями в рамках одного циклу.

**Ініціалізація**: Створюється в конструкторі на основі `ChipGeneratorData` (отриманого через `GetSpecialData<ChipGeneratorData>()`), встановлюючи початкові значення з налаштувань.
6. **Update Visual**: Виклик `UpdateVisual()` для синхронізації ефектів після зміни стану заряду.

### Recharge
1. **Update**: У кожному кадрі збільшується `ChargingTimeLeft` до досягнення `CurrentTargetChargingTime`.
2. **Visuals**: Викликається подія `OnCharging` -> `ChipGeneratorEffect` оновлює маску прогресу.
3. **Complete**: Коли час вичерпано, відновлюється готовність (`IsCharged`). Якщо це був повний цикл, відновлюється кількість зарядів. Викликається `UpdateVisual()` (якщо чіп не перетягується).

## Effects and Visualization
Генератор керує двома ефектами, які зберігаються в `effects` словнику з ключами від `EffectConsts`:

- **`EffectConsts.GeneratorCharging`**: Звертання через `GetEffect<IEffectGeneratorCharging>(EffectConsts.GeneratorCharging)`
  - Реалізація: [ChipGeneratorRechargeEffect](../Visuals/Effects.md#3-chip-generator-recharge)
  - Відображає прогрес перезарядки. Активний, коли `IsCharged` = `false`.
  - Реалізує інтерфейс `IEffectGeneratorCharging`, його метод `OnCharging(float progress)` оновлює маску перезарядки.
  
- **`EffectConsts.GeneratorCharged`**: Звертання через `GetEffect(EffectConsts.GeneratorCharged)`
  - Реалізація: базовий `Effect`
  - Активний, коли `IsCharged` = `true` та чіп не перетягується.
  - Зазвичай це cyclic idle-анімація (світіння, пульсація), що показує гравцеві готовність об'єкта.

- **Animator**: Використовує тригери `Generate` (при спавні) та `Recharge` (при завершенні зарядки).

