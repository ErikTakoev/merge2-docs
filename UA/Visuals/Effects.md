# Visual Effects

[← На Головну](../Main.md)

Цей документ описує систему візуальних ефектів, що використовуються для забезпечення зворотного зв'язку (Feedback) гравцеві під час взаємодії з фішками (Chips) на ігровому полі.

## Base System

### `EffectConsts.cs`
Централізований реєстр цілочисельних констант-ідентифікаторів для всіх ефектів у системі. Замість зберігання посилань на окремі поля ефектів, система використовує словник `Dictionary<int, IEffect>` з ключами-константами.

**Базові ефекти (1–7)**:

| Константа | ID | Опис |
|---|---|---|
| [`MergeAvailable`](#1-merge-available) | 1 | Активується коли фішка придатна для злиття |
| [`CellHighlight`](#2-cell-highlight) | 2 | Підсвітка клітин під фішкою |
| [`ContainerRequirements`](#3-container-requirements) | 3 | Відображає іконки предметів, що потрібні контейнеру |
| [`GeneratorCharged`](#4-generator-charged) | 4 | Активується коли генератор повністю зарядився |
| [`GeneratorCharging`](#5-generator-charging) | 5 | Показує прогрес зарядки генератора |
| [`PBoosterConnectorCells`](#6-power-booster-connector-highlight) | 6 | Підсвічує сусідні чіпи, які посилюються бустером |
| [`PBoosterJoin`](#7-power-booster-join-links) | 7 | Динамічні лінії між бустером і цілями підсилення |
| [`ShadowEffect`](#8-shadow-effect) | 8 | Постійна тінь під фішкою, що реагує на рух |

**Blocker-ефекти (101+)** — вкладений клас `EffectConsts.Blockers`:

| Константа | ID | Опис |
|---|---|---|
| [`BoxEffect`](#101-box-effect) | 101 | Ефект коробки |
| [`ChainsEffect`](#102-chains-effect) | 102 | Ефект ланцюгів |
| [`MoveLockedEffect`](#103-move-locked-effect) | 103 | Ефект блокування переміщення |

**Утиліти**:
- **`nameToId`** (`Dictionary<string, int>`): Словник для резолвінгу рядкових імен ефектів у цілочисельні ID. Використовується `ExtraEffectData.EffectId` та `EffectBlockingSettings.UpdateHideEffectIds`.
- **`GetIdByName(string name)`**: Повертає ID ефекту за його ім'ям або `-1`, якщо ім'я не знайдено.

Доступ до ефектів відбувається через методи `GetEffect(hash)` та `GetEffect<T>(hash)` у базовому класі `Chip`.

### `IEffect`
Основний контракт для всіх ефектів:
- **`GetId()`**: Повертає унікальний ID ефекту (з `EffectConsts`).
- **`Init(Chip chip, int effectId)`**: Ініціалізація з посиланням на чіп та ідентифікатором ефекту.
- **`Activate(Chip chip) → bool`**: Активація ефекту. Повертає `false`, якщо `effectId` міститься в `chip.BlockingState.HideEffectIds` (ефект приховано іншим блокуючим ефектом). При успішній активації викликає `chip.BlockingState.ApplyBlock(BlockingSettings)`.
- **`Deactivate(Chip chip, bool force)`**: Деактивація ефекту.
- **`SendTrigger(string triggerName, bool allowRepeat)`**: Відправка довільного тригеру в Animator.
- **`OnChangedCell` / `OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`**: Обробка зміни клітин (`ICell`).
- **`OnMovingStateChanged(Chip chip, bool isMoving)`**: Обробка зміни стану руху (початок перетягування або системне переміщення).
- **`TryDestroyEffect(Chip, EffectDestroyingSettings, EffectDestroyingRuntimeData) → bool`**: Перевіряє, чи досяг ефект порогу руйнування.
- **`BlockingSettings`** (`EffectBlockingSettings`): Конфігурація блокувань чіпа при активації ефекту. Детальніше: [Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocking-system).
- **`DestroyingSettings`** (`EffectDestroyingSettings`): Конфігурація руйнування ефекту при сусідніх злиттях. Детальніше: [Effect Destroying System](../Features/ChipEffectBlockers.md#effect-destroying-system).

### 
Докладніше про систему блокувань (Blocking Settings, Combined Blocking State) див. у документі **[Chip Effect Blockers](../Features/ChipEffectBlockers.md)**.

---

### `Effect.cs`
Базовий клас для всіх ефектів. Реалізує `IEffect` та надає віртуальні методи для керування життєвим циклом ефекту:
- **`Init(Chip chip, int effectId)`**: Ініціалізує ефект, зберігає `effectId`, налаштовує позицію залежно від розміру чіпа, застосовує `AutoSizeType`, деактивує за замовчуванням.
- **`Activate(Chip chip) → bool`**: Вмикає ефект. Якщо `effectId` є в `HideEffectIds`, викликає `Deactivate` та повертає `false`. При активації викликає `chip.BlockingState.ApplyBlock(BlockingSettings)`.
- **`Deactivate(Chip chip, bool force)`**: Вимикає ефект. При `force = true` — негайна зміна стану через `animator.Play`.
- **`GetId()`**: Повертає збережений `effectId`.
- **`OnChangedCell(ICell sourceCell, ICell targetCell)`**: Викликається при переміщенні фішки. Якщо `parentType` встановлено в `ParentCell`, ефект переприв'язується до нової клітинки.
- **`OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`**: Обробка зміни клітин (`ICell`) під час Drag-and-Drop.
- **`OnMovingStateChanged(Chip chip, bool isMoving)`**: Автоматично приховує ефект при початку руху, якщо встановлено `deactivateOnMove = true`, та відновлює стан при зупинці.
- **`TryDestroyEffect(Chip, EffectDestroyingSettings, EffectDestroyingRuntimeData) → bool`**: Якщо `NeighboringMergeCount` менше порогу, відправляє прогресивний тригер (наприклад, `"Hit_1"`, `"Hit_2"`); якщо досяг порогу — деактивує ефект і повертає `true`.

**EffectParentType** (enum):
- `ParentChip`: Ефект прикріплений до трансформу фішки.
- `ParentChipAnimationNode`: Ефект прикріплений до дочірнього об'єкта `AnimationNode` (використовується для анімацій вильоту).
- `ParentCell`: Ефект прикріплений до трансформу клітинки (слідує за клітинкою, а не фішкою).

**Налаштування руху**:
- `deactivateOnMove`: Якщо `true`, ефект автоматично деактивується під час перетягування фішки, щоб зменшити візуальний шум.
- `restoreStateAfterMove`: Внутрішній прапорець для відновлення стану після завершення руху.

**AutoSizeType** (enum):
- `None`: Без автомасштабування.
- `ScaleByChipSize`: `localScale = (chipSize.x, chipSize.y, 1)`.
- `ScaleByMaxChipSize`: `localScale = (max, max, 1)`.
- `ScaleByMinChipSize`: `localScale = (min, min, 1)`.
- `ScaleByAverageValue`: `localScale = (avg, avg, 1)`.
- `ScaleByAverageValueOnlyMinValue`: Масштабує тільки менший вимір до середнього значення.

**Додаткові можливості**:
- **Animator Integration**: Якщо налаштовано `sendAnimatorTrigger`, методи `Activate` та `Deactivate` автоматично відправляють тригери `"Activate"` та `"Deactivate"` в компонент `Animator`, а також скидають протилежні тригери для запобігання анімаційним артефактам.
- **`SendTrigger(string triggerName, bool allowRepeat = false)`**: Дозволяє відправити довільний тригер в `Animator` ефекту. Використовується для спеціальних взаємодій, таких як анімація при спробі перетягнути заблокований чіп (`MoveLocked`). Параметр `allowRepeat` дозволяє ігнорувати налаштування `dontRepeatTrigger` для конкретних викликів.
- **`ResetTrigger(string triggerName)`**: Скидає вказаний анімаційний тригер в `Animator` ефекту. Викликається при переключенні анімаційних станів, щоб запобігти конфліктам між протилежними тригерами (наприклад, скидає `"Deactivate"` перед надсиланням `"Activate"`). Безпечно обробляє випадок, коли `Animator` значення `null`.

---

## Effect Destroying System

Система руйнування ефектів дозволяє ефектам поступово руйнуватися при сусідніх злиттях. 
Докладніше див. у документі **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#effect-destroying-system)**.

---

## Implemented Effects

### 1. Merge Available
**Клас**: `ChipMergeAvailableEffect.cs`
**Використовується в**: [Chip](../Chips/Chip.md)

Активується на фішці, яка знаходиться "знизу", коли гравець заносить над нею іншу фішку, з якою можливе злиття.
- **Особливості**:
  - `autoPosition`: Центрує ефект відносно фішки.
  - Використовує `Animator` з тригерами `Activate` та `Deactivate`.
  - Масштабування визначається через `AutoSizeType` у базовому `Effect`.

### 2. Cell Highlight
**Клас**: `CellHighlightEffect.cs`
**Використовується в**: [Chip](../Chips/Chip.md)

Динамічно створює та відображає підсвітку под фішкою. Підтримує багатокоміркові фішки, створюючи сітку хайлайтів відповідно до розміру (`Size`) фішки.
- **Параметри**:
  - `highlightPrefab`: Префаб візуального елемента підсвітки.
  - `color`: Колір підсвітки.
  - `order`: Зсув по осі Z для правильного рендерингу над полем.
- **Shared Material**: Перший створений хайлайт генерує `Material` (через `SpriteRenderer.material`), всі наступні використовують `sharedMaterial` для спільного кольору.
- **Адаптація до архітектури**:
  - Використовує інтерфейс `ICell` для визначення цільових клітинок.
  - Впроваджено перевірку заблокованих зон через `IFieldGrid.HasBlockedCells`: якщо хоча б одна клітинка під фішкою заблокована, підсвітка приховується для візуалізації неможливості розміщення.
- **Extensibility**: Клас спроектований для наслідування та перевизначення:
  - `CreateHighlights()`: Віртуальний — базова реалізація створює сітку за `chipSize`. Може бути перевизначений для іншої геометрії (наприклад, [§ 6 Power Booster Connector Highlight](#6-power-booster-connector-highlight)).
  - `DestroyHighlights()`: Віртуальний — очищує `highlights` список та `sharedMaterial`.
  - `CreateHighlight(Vector3)`: Віртуальний — створює один елемент підсвітки з префабу.
  - `OnChangedCell(ICell, ICell)`: Віртуальний — реагує на зміну клітинки (активація/деактивація `gameObject`).

### 3. Container Requirements
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

### 4. Generator Charged
**Використовується в**: [ChipGenerator](../Chips/ChipGenerator.md)

Візуальний ефект, що активується, коли генератор повністю зарядився і готовий до створення нової фішки.
- **Активація**: Коли `IsCharged` стає `true` (через поле `chargedEffect`).
- **Деактивація**: Після успішної генерації фішки або під час процесу перезарядки.
- **Тип**: Використовує базовий клас `Effect`. Зазвичай це cyclic idle-анімація (світіння, пульсація), що показує гравцеві готовність об'єкта до взаємодії.

### 5. Generator Charging
**Клас**: `ChipGeneratorRechargeEffect.cs`
**Реалізує**: `IEffectGeneratorCharging`
**Використовується в**: [ChipGenerator](../Chips/ChipGenerator.md)

Візуалізує процес перезарядки генератора. Зазвичай реалізовано через зміну локальної позиції маски (`maskRectTransform`), що створює ефект заповнення іконки знизу вгору.
- **Метод `OnCharging(float progress)`**: Отримує значення від 0 до 1 та оновлює візуалізацію.

### 6. Power Booster Connector Highlight
**Клас**: `PowerBoosterConnectorCellsHighlightEffect.cs` (наслідує `CellHighlightEffect`)
**Використовується в**: [ChipPowerBooster](../Chips/ChipPowerBooster.md)

Підсвічує клітинки, за якими спостерігає бустер (його `ObservedCellPositions`), показуючи гравцеві зону впливу Power Booster.
- **Параметри**:
  - `globalAlpha`: Глобальна прозорість ефекту (шейдерний параметр `_Alpha`).
  - `distractionAmount`: Інтенсивність візуального спотворення (шейдерний параметр `_DistractionAmount`).
  - `waitTimeBeforePowerEffect`: Час очікування перед першим запуском анімації Power Effect.
- **Power Effect**: Корутіна `StartPowerEffect` після `waitTimeBeforePowerEffect` секунд відправляє анімаційний тригер `"PowerBooster"` на чіп та ефект, потім перезапускається з подвоєним інтервалом.
- **CreateHighlights()**: Перевизначає базовий метод — створює хайлайти за позиціями `connectorCellPositions` (зміщення відносно `originCellPosition` бустера), а не за сіткою `chipSize`.
- **OnChangedCell**: Перевизначає базовий — оновлює `originCellPosition` та `connectorCellPositions` з `CellSubscriber.ObservedCellPositions`, потім перестворює хайлайти.
- **Deactivate**: Зупиняє корутіну Power Effect та скидає `globalAlpha` до 0.
- **Update**: Кожен кадр оновлює шейдерні параметри `_Alpha` та `_DistractionAmount` на `sharedMaterial`.

### 7. Power Booster Join Links
**Клас**: `PowerBoosterJoinEffect.cs` (наслідує `Effect`, реалізує `IEffectPowerBoosterJoin`)
**Використовується в**: [ChipPowerBooster](../Chips/ChipPowerBooster.md)

Відповідає за динамічні particle-лінки між бустером і кожною активною ціллю `IPowerBoosterTarget`.
- **Join API**: `ChipPowerBooster` викликає `OnJoin`/`OnLeave` через контракт `IEffectPowerBoosterJoin` (`EffectPowerBoosterJoinRef`) під час додавання/зняття підсилення.
- **JoinPoints**: Ефект використовує `JoinPoints` бустера та цілі, обирає найближчі кандидати, а далі періодично перебіндовує активні лінки через корутіну `ChangeJoinPointsCoroutine` (`changeJoinPointsTime`).
- **Cleanup**: `OnLeave` і `Deactivate` зупиняють particle systems, планують `Destroy` по `startLifetime` і очищують runtime-словник лінків; `SetMoving(true)` у бустері також викликає `joinEffect.Deactivate(...)`.

### 8. Shadow Effect
**Клас**: `ShadowEffect.cs`
**Використовується в**: [Chip](../Chips/Chip.md)

Забезпечує постійну тінь під фішкою, яка динамічно реагує на стан руху.
- **Особливості**:
  - `autoShadowSprite`: Автоматично копіює спрайт з основного рендерера фішки (якщо він один).
  - `autoScale`: Адаптує масштаб тіні під масштаб основного рендерера.
  - `autoSortingLayer`: Використовує налаштування `AdditionallyWhenMoving` для підняття тіні в шарах сортування під час руху.
- **Movement Reaction**: На відміну від звичайних ефектів, тінь не деактивується при русі. Замість цього вона відправляє тригери `Activate` (при початку руху) та `Deactivate` (при зупинці) у свій `Animator` для плавного візуального переходу (наприклад, збільшення зміщення тіні).
- **Сортування**: Під час руху тінь збільшує свій `sortingOrder`, щоб залишатися візуально синхронізованою з "піднятою" фішкою.

### 101. Box Effect
Ефект коробки. Докладніше про налаштування та використання див. у докуметі **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocker-effects-move-locked-chains-box)**.

### 102. Chains Effect
Ефект ланцюгів. Докладніше про налаштування та використання див. у докуметі **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocker-effects-move-locked-chains-box)**.

### 103. Move Locked Effect
Ефект блокування переміщення. Докладніше про налаштування та використання див. у докуметі **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocker-effects-move-locked-chains-box)**.
