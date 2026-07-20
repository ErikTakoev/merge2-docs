# WaitEvolutionModule (Wait Evolution)

[← На Головну](../Main.md)

Логіка автоматичної еволюції фішки з часом реалізована за допомогою модульного компонента [WaitEvolutionModule](../../../Core/Scripts/Chips/WaitEvolutionModule.cs), який приєднується до базового GameObject фішки `Chip`. Компонент відстежує час очікування і після його закінчення замінює поточний чіп на інший випадковим чином згідно з заданими ймовірнісними вагами.

`WaitEvolutionModule` також реалізує інтерфейс `IPowerBoosterTarget`, що дозволяє прискорювати час до еволюції за допомогою сусідніх бустерів (якщо це увімкнено в конфігурації).

## Architecture and Responsibility

### 1. `WaitEvolutionModule.cs` (Main Component)
Клас `WaitEvolutionModule` керує таймером еволюції та процесом заміни фішки, реалізуючи інтерфейси `IChipModule` та `IPowerBoosterTarget`.
- **Ініціалізація даних**: У методі `Init(Chip, ChipData, ChipRuntimeData)` зчитує конфігурацію через `data.GetSpecialData<ChipWaitEvolutionData>()` та рантайм-дані `ChipWaitEvolutionRuntimeData`.
- **Update Loop**: У методі `Update()` зменшує залишок часу `TimeLeft` на `Time.deltaTime`, помножений на `powerMultiplier`. Коли час закінчується, ініціює еволюцію (якщо фішка не рухається і не перетягується користувачем).
- **Залежності**:
  - `IFieldGrid`: Для отримання доступу до поточної клітинки на сітці поля.
  - `ChipFactory`: Для створення нової фішки на місці старої.

### 2. `ChipWaitEvolutionData.cs` (Configuration)
Конфігураційні дані для еволюції з часом. Реалізує `IChipSpecialData` та додається до колекції `ChipData.specialDatas`.
- **Параметри**:
  - `EvolveTime`: Час очікування до еволюції в секундах (за замовчуванням 10 секунд).
  - `NextChips`: Список можливих фішок для еволюції з відповідними ймовірнісними вагами (структура `EvolutionTarget` з параметрами `ChipData` та `Weight`).
  - `IsAffectedByBoosters`: Прапорець, що визначає, чи впливають сусідні Power Boosters на швидкість таймера еволюції.
- **GetRandomNextChip()**: Метод вибору наступної фішки на основі зваженого рандому (Weighted Random). Якщо список порожній, повертає `null`.

### 3. `ChipWaitEvolutionRuntimeData.cs` (Runtime State)
Зберігає поточний стан процесу еволюції. Реалізує маркерний інтерфейс `IChipSpecialRuntimeData` та зберігається у списку `specialRuntimeDatas` в `ChipRuntimeData`.
- **Параметри**:
  - `TimeLeft`: Кількість секунд, що залишилися до еволюції. При ініціалізації встановлюється в значення `EvolveTime`.

## Evolution Process (Flow)

1. **Update Timer**: Таймер `TimeLeft` зменшується в кожному кадрі. Якщо ефект підсилення діє і дозволений (`IsAffectedByBoosters = true`), швидкість таймера масштабується за допомогою `powerMultiplier` (береться максимальна сила `Power` серед сусідніх бустерів).
2. **Visual Progress**: Кожні кадр розраховується нормалізований прогрес і передається ефекту через виклик події `OnCharging` -> `ChargingEffect.OnCharging`.
3. **Pre-Deactivation**: За 0.5 секунд до закінчення часу (`TimeLeft <= 0.5f`) візуальний ефект заряджання деактивується.
4. **Execute Evolution**: Після закінчення часу (`TimeLeft <= 0f`), якщо фішка не переміщується (`IsMoving = false`) та не перетягується користувачем:
   - Отримується випадкова наступна фішка через `GetRandomNextChip()`.
   - Поточний чіп знищується за допомогою `chip.Destroy(cell, false, AnimatorTrigger.WaitEvolutionDestroy)`.
   - На тій самій клітинці створюється новий чіп за допомогою `chipFactory.CreateChip(cell, nextChipData)` та запускається тригер анімації `WaitEvolutionSpawn`.
   - Якщо наступна фішка не визначена (повернуто `null`), поточний чіп просто руйнується.

## Effects and Visualization

- **`EffectConsts.GeneratorCharging`**: Звертання через `GetEffect<IChargingEffect>(EffectConsts.GeneratorCharging)`
  - Реалізація: [ChargingEffect](../Visuals/Effects.md#5-generator-charging)
  - Візуалізує хід еволюції (заповнення таймера-секундоміра та обертання стрілки).
  - Підписується на подію `OnCharging` модуля.
- **Animator**: Використовує тригери `WaitEvolutionDestroy` (для анімації зникнення поточної фішки) та `WaitEvolutionSpawn` (для анімації появи нової фішки).

## Power Booster Support

Якщо `IsAffectedByBoosters` дорівнює `true`, модуль реагує на сусідні Power Boosters:
- **`ApplyPowerBooster`**: Додає бустер до списку `appliedBoosters` та перераховує `powerMultiplier` як максимальну силу серед активних бустерів.
- **`RemovePowerBooster`**: Видаляє бустер зі списку та перераховує `powerMultiplier`.
- **`RecalculatePowerMultiplier`**: Встановлює `powerMultiplier` в 1f, якщо немає бустерів або вплив бустерів вимкнено, інакше вибирає `Max(booster.Power)`.
