# Visual Effects

[← На Головну](../Main.md)

Цей документ описує систему візуальних ефектів, що використовуються для забезпечення зворотного зв'язку (Feedback) гравцеві під час взаємодії з фішками (Chips) на ігровому полі.

## Base System

### `Effect.cs`
Базовий клас для всіх ефектів. Реалізує `IEffect` та надає віртуальні методи для керування життєвим циклом ефекту:
- **`Init(Chip chip)`**: Ініціалізує ефект, налаштовує позицію залежно від розміру чіпа, деактивує за замовчуванням.
- **`Activate(Chip chip)`**: Вмикає об'єкт ефекту.
- **`Deactivate(Chip chip)`**: Вимикає об'єкт ефекту.
- **`OnChangedCell(Cell sourceCell, Cell targetCell)`**: Викликається при переміщенні фішки.
- **`OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`**: Обробка зміни клітин під час Drag-and-Drop.

**Інтерфейси**:
- **`IEffect`**: Основний контракт для всіх ефектів (`Activate`, `Deactivate`, `Init` тощо).
- **`IEffectContainer`**: Розширює `IEffect` для візуалізації специфіки контейнерів.
- **`IEffectGeneratorCharging`**: Розширює `IEffect` для візуалізації прогресу зарядки.

**Додаткові можливості**:
- **Animator Integration**: Якщо налаштовано `sendAnimatorTrigger`, методи `Activate` та `Deactivate` автоматично відправляють тригери `"Activate"` та `"Deactivate"` в компонент `Animator`, а також скидають протилежні тригери для запобігання анімаційним артефактам.
- **`SendTrigger(string triggerName, bool allowRepeat = false)`**: Дозволяє відправити довільний тригер в `Animator` ефекту. Використовується для спеціальних взаємодій, таких як анімація при спробі перетягнути заблокований чіп (`MoveLocked`). Параметр `allowRepeat` дозволяє ігнорувати налаштування `dontRepeatTrigger` для конкретних викликів.
- **`ResetTrigger(string triggerName)`**: Скидає вказаний анімаційний тригер в `Animator` ефекту. Викликається при переключенні анімаційних станів, щоб запобігти конфліктам між протилежними тригерами (наприклад, скидає `"Deactivate"` перед надсиланням `"Activate"`). Безпечно обробляє випадок, коли `Animator` значення `null`.

---

## Implemented Effects

### 1. Cell Highlight
**Клас**: `CellHighlightEffect.cs`
**Використовується в**: [Chip](../Chips/Chip.md)

Динамічно створює та відображає підсвітку під фішкою. Підтримує багатокоміркові фішки, створюючи сітку хайлайтів відповідно до розміру (`Size`) фішки.
- **Параметри**:
  - `highlightPrefab`: Префаб візуального елемента підсвітки.
  - `color`: Колір підсвітки.
  - `order`: Зсув по осі Z для правильного рендерингу над полем.
- **Shared Material**: Перший створений хайлайт генерує `Material` (через `SpriteRenderer.material`), всі наступні використовують `sharedMaterial` для спільного кольору.
- **Extensibility**: Клас спроектований для наслідування та перевизначення:
  - `CreateHighlights()`: Віртуальний — базова реалізація створює сітку за `chipSize`. Може бути перевизначений для іншої геометрії (наприклад, [§ 7 Power Booster Connector Highlight](#7-power-booster-connector-highlight)).
  - `DestroyHighlights()`: Віртуальний — очищує `highlights` список та `sharedMaterial`.
  - `CreateHighlight(Vector3)`: Віртуальний — створює один елемент підсвітки з префабу.
  - `OnChangedCell(Cell, Cell)`: Віртуальний — реагує на зміну клітинки (активація/деактивація `gameObject`).

### 2. Merge Available
**Клас**: `ChipMergeAvailableEffect.cs`
**Використовується в**: [Chip](../Chips/Chip.md)

Активується на фішці, яка знаходиться "знизу", коли гравець заносить над нею іншу фішку, з якою можливе злиття.
- **Особливості**:
  - `autoSize`: Автоматично масштабує ефект під розмір фішки.
  - `autoPosition`: Центрує ефект відносно фішки.
  - Використовує `Animator` з тригерами `Activate` та `Deactivate`.

### 3. Chip Generator Recharge
**Клас**: `ChipGeneratorRechargeEffect.cs`
**Реалізує**: `IEffectGeneratorCharging`
**Використовується в**: [ChipGenerator](../Chips/ChipGenerator.md)

Візуалізує процес перезарядки генератора. Зазвичай реалізовано через зміну локальної позиції маски (`maskRectTransform`), що створює ефект заповнення іконки знизу вгору.
- **Метод `OnCharging(float progress)`**: Отримує значення від 0 до 1 та оновлює візуалізацію.

### 4. Chip Container
**Клас**: `ChipContainerEffect.cs`
**Реалізує**: `IEffectContainer`
**Використовується в**: [ChipContainer](../Chips/ChipContainer.md)

Відображає Panel з іконками предметів, які необхідні контейнеру для виконання квесту.

- **Компоненти**:
  - `Animator`: Керує анімацією появи (`Activate`) та зникнення (`Deactivate`) панелі.
  - `LayoutForElements`: Контейнер для іконок предметів.
  - `PanelSpriteRenderer`: Фон, розмір якого адаптується під кількість елементів.
- **Конфігурація (`ElementInfo`)**:
  - Ефект містить масив налаштувань для різної кількості елементів (наприклад, для 1 або 2 предметів).
  - Кожен запис налаштовує ширину панелі, ширину layout-групи та масштаб іконок.
- **Логіка (`UpdateElements`)**:
  1. Отримує поточний список вимог (`containers`).
  2. Знаходить відповідний `ElementInfo` на основі кількості вимог.
  3. Адаптує розмір фону та групи.
  4. Очищає старі елементи та створює нові префаби (`ContainerElementPrefab`) для кожної вимоги.
  5. Якщо контейнер заповнений (`isFull`), викликає `ClearElements` та деактивує ефект.

### 5. Generator Charged
**Використовується в**: [ChipGenerator](../Chips/ChipGenerator.md)

Візуальний ефект, що активується, коли генератор повністю зарядився і готовий до створення нової фішки.
- **Активація**: Коли `IsCharged` стає `true` (через поле `chargedEffect`).
- **Деактивація**: Після успішної генерації фішки або під час процесу перезарядки.
- **Тип**: Використовує базовий клас `Effect`. Зазвичай це cyclic idle-анімація (світіння, пульсація), що показує гравцеві готовність об'єкта до взаємодії.

### 6. Move Locked
**Клас**: `Effect` (базовий)
**Використовується в**: [Chip](../Chips/Chip.md)

Візуальний ефект, що відображається на чіпі, коли його переміщення заблоковано.
- **Активація**: 
  - Коли `ChipRuntimeData.IsMoveLocked` встановлено в `true` через метод `UpdateVisual()`.
  - Автоматично обробляється в `Chip.UpdateVisual()`.
- **Деактивація**: 
  - Коли `IsMoveLocked` стає `false`.
  - Автоматично обробляється в `Chip.UpdateVisual()`.
- **Поведінка**:
  - Використовує базовий клас `Effect` з підтримкою Animator triggers (`Activate`/`Deactivate`).
  - Зазвичай реалізовано як іконка замка або інший візуальний індикатор, що показує гравцеві неможливість переміщення.
  - Перевірка можливості переміщення виконується через метод `Chip.CanMoving()`, який використовується в `DraggableChipLogic`.
  - **Інтерактивний зворотний зв'язок**: При спробі перетягнути заблокований чіп викликається `Chip.OnDraggingChipWithMoveLocked()`, який відтворює спеціальну анімацію `MoveLocked` через метод `Effect.SendTrigger()`. Це дозволяє показати гравцеві, що чіп заблокований, навіть якщо він намагається його перемістити.
- **Налаштування**:
  - Префаб ефекту задається в `ChipData.MoveLockedEffectPrefab`.
  - Створюється автоматично в `Chip.InitEffects()`, якщо префаб вказано.

### 7. Power Booster Connector Highlight
**Клас**: `PowerBoosterConnectorCellsHighlightEffect.cs` (наслідує `CellHighlightEffect`)
**Використовується в**: [ChipPowerBooster](../Chips/ChipPowerBooster.md)

Підсвічує клітинки, за якими спостерігає бустер (його `ObservedCellPositions`), показуючи гравцеві зону впливу Power Booster.
- **Параметри**:
  - `globalAlpha`: Глобальна прозорість ефекту (шейдерний параметр `_Alpha`).
  - `distractionAmount`: Інтенсивність візуального спотворення (шейдерний параметр `_DistractionAmount`).
  - `waitTimeBeforePowerEffect`: Час очікування перед першим запуском анімації Power Effect.
- **Power Effect**: Корутіна `StartPowerEffect` після `waitTimeBeforePowerEffect` секунд відправляє анімаційний тригер `"PowerBooster"` на чіп та ефект, потім перезапускається з подвоєним інтервалом.
- **CreateHighlights()**: Перевизначає базовий метод — створює хайлайти за позиціями `connectorCellPositions` (зміщення відносно `originCellPosition` бустера), а не за сіткою `chipSize`.
- **OnChangedCell**: Перевизначає базовий — оновлює `originCellPosition` та `connectorCellPositions` з `ChipPowerBooster.CellSubscriber.ObservedCellPositions`, потім перестворює хайлайти.
- **Deactivate**: Зупиняє корутіну Power Effect та скидає `globalAlpha` до 0.
- **Update**: Кожен кадр оновлює шейдерні параметри `_Alpha` та `_DistractionAmount` на `sharedMaterial`.

### 8. Power Booster Join Links
**Клас**: `PowerBoosterJoinEffect.cs` (наслідує `Effect`, реалізує `IEffectPowerBoosterJoin`)
**Використовується в**: [ChipPowerBooster](../Chips/ChipPowerBooster.md)

Відповідає за динамічні particle-лінки між бустером і кожним активним `IPowerBoosterModifier`.
- **Join API**: `ChipPowerBooster` викликає `OnJoin`/`OnLeave` через контракт `IEffectPowerBoosterJoin` (`EffectPowerBoosterJoinRef`) під час apply/remove модифікаторів.
- **JoinPoints**: Ефект використовує `JoinPoints` бустера та модифікатора, обирає найближчі кандидати, а далі періодично перебіндовує активні лінки через корутіну `ChangeJoinPointsCoroutine` (`changeJoinPointsTime`).
- **Cleanup**: `OnLeave` і `Deactivate` зупиняють particle systems, планують `Destroy` по `startLifetime` і очищують runtime-словник лінків; `SetMoving(true)` у бустері також викликає `joinEffect.Deactivate(...)`.
