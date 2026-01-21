# ChipGenerator (Генератор)

[← На Головну](../Main.md)

`ChipGenerator` — це спеціалізований компонент, який створює (спавнить) нові фішки на ігровому полі. Він працює як автомат станів (State Machine), що перемикається між заряджанням та готовністю до генерації.

## Архітектура та Відповідальність

### 1. `ChipGenerator.cs` (Основний Компонент)
Клас `ChipGenerator` керує життєвим циклом генератора, об'єднуючи логіку оновлення та події.
- **Вхідні дані (Input)**:
  - **Manual**: Обробляє `OnTap` (клік гравця).
  - **Auto**: Підписується на `field.OnChangeField`, щоб автоматично спавнити при появі вільного місця.
- **Update Loop**: У методі `Update` керує таймером перезарядки.
- **Залежності**:
  - `IFreeCellFinder`: Логіка пошуку найближчої вільної клітинки.
  - `ChipFactory`: Фабрика для створення нових об'єктів.

### 2. `ChipGeneratorData.cs` (Налаштування)
Дані містять налаштування та алгоритми вибору.
- **Властивості**:
  - `SpawnParameters`: Ваги ймовірностей для різних типів фішок.
   - `ChargeCount/ChargingTime`: Параметри балансу.
   - `GenerationInterval`: Час затримки між генераціями в рамках одного циклу заряду.
   - `TotalRecharges`: Ліміт життя генератора.
- **Логіка (`GenerateChipData`)**: Метод, що виконує зважений випадковий вибір (`Weighted Random`) для визначення наступної фішки.

### 3. `ChipGeneratorRuntimeData.cs` (Стан)
Окремий клас для зберігання динамічного стану.
- **Поля**:
  - `IsCharged`: Чи готовий до спавну.
  - `RechargesLeft`: Скільки циклів залишилось.
  - `IsWaitingForSpace`: Стан очікування (для Auto режиму).
  - `CurrentTargetChargingTime`: Поточний час, необхідний для зарядки (може дорівнювати `ChargingTime` або `GenerationInterval`).

## Процес (Flow)

### Генерація
1. **Trigger**: Подія `OnTap` або `Update` (якщо Auto).
2. **Check**: Перевірка наявності заряду (`IsCharged`) та вільного місця (`IFreeCellFinder`).
3. **Select**: Виклик `generatorData.GenerateChipData()` для вибору типу фішки.
4. **Spawn**: `ChipFactory.CreateChip` у знайденій клітинці.
5. **Consume**: Зменшення `ChargeCount`.
   - Якщо `ChargeCount > 0`: Встановлюється коротка перезарядка (`GenerationInterval`).
   - Якщо `ChargeCount == 0`: Зменшується `RechargesLeft` та встановлюється повна перезарядка (`ChargingTime`).

### Перезарядка (Recharge)
1. **Update**: У кожному кадрі збільшується `ChargingTimeLeft` до досягнення `CurrentTargetChargingTime`.
2. **Visuals**: Викликається подія `OnCharging` -> `ChipGeneratorEffect` оновлює маску прогресу.
3. **Complete**: Коли час вичерпано, відновлюється готовність (`IsCharged`). Якщо це був повний цикл, відновлюється кількість зарядів.

## Ефекти та Візуалізація
- **[ChipGeneratorEffect](../Visuals/Effects.md#3-chip-generator-прогрес-генератора)**: Відображає прогрес перезарядки (через `maskRectTransform`). Активний під час процесу зарядки.
- **[GeneratorChargedEffect](../Visuals/Effects.md#5-generator-charged-готовність-генератора)**: Додатковий ефект, що активується, коли генератор повністю заряджений (готовий до генерації). Використовується для візуального підкреслення стану готовності (Idle).
- **Animator**: Використовує тригери `Generate` (при спавні) та `Recharge` (при завершенні зарядки).
