# Visual Effects

[← На Головну](../Main.md)

Цей документ описує систему візуальних ефектів, що використовуються для забезпечення зворотного зв'язку (Feedback) гравцеві під час взаємодії з фішками (Chips) на ігровому полі.

## Base System

### `EffectConsts.cs`
Централізований реєстр цілочисельних констант-ідентифікаторів для всіх ефектів у системі. Замість зберігання посилань на окремі поля ефектів, система використовує словник `Dictionary<int, IEffect>` з ключами-константами.

**Базові ефекти (1–10)**:

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
| [`MergeLight`](#9-merge-light) | 9 | Спалах підсвічування на результуючій фішці після злиття |
| [`MergeHint`](#10-merge-hint) | 10 | Підсвічує сумісні фішки та контейнери під час перетягування |
| [`TapHint`](#11-tap-hint) | 11 | Візуальний ефект підказки тапу (OnTap) на фішці |

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
- **`Deactivate(Chip chip, bool force)`**: Деактивація ефекту. При `force = true` форсує негайну зміну стану (корисно для миттєвого очищення).
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
- **`Deactivate(Chip chip, bool force = false)`**: Вимикає ефект. При `force = true` — негайна зміна стану через `animator.Play("Deactivate", -1, 1f)`.
- **`GetId()`**: Повертає збережений `effectId`.
- **`OnChangedCell(ICell sourceCell, ICell targetCell)`**: Викликається при переміщенні фішки. Якщо `parentType` встановлено в `ParentCell`, ефект переприв'язується до нової клітинки.
- **`OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`**: Обробка зміни клітин (`ICell`) під час Drag-and-Drop.
- **`OnMovingStateChanged(Chip chip, bool isMoving)`**: Автоматично приховує ефект при початку руху, якщо встановлено `deactivateOnMove = true`, та відновлює стан при зупинці.
- **`TryDestroyEffect(Chip, EffectDestroyingSettings, EffectDestroyingRuntimeData) → bool`**: Якщо `NeighboringMergeCount` менше порогу, відправляє прогресивний тригер (наприклад, `"Hit_1"`, `"Hit_2"`); якщо досяг порогу — деактивує ефект і повертає `true`.

**EffectParentType** (enum):
- `ParentChip`: Ефект прикріплений до трансформу фішки.
- `ParentChipAnimationNode`: Ефект прикріплений до дочірнього об'єкта `AnimationNode` (використовується для анімацій вильоту).
- `ParentCell`: Ефект прикріплений до трансформу клітинки (слідує за клітинкою, а не фішкою).
- `ParentCellWithoutRotation`: Ефект прикріплений до трансформу клітинки, але ігнорує її поворот.

**Налаштування руху**:
- `deactivateOnMove`: Якщо `true`, ефект автоматично деактивується під час перетягування фішки, щоб зменшити візуальний шум.
- `restoreStateAfterMove`: Внутрішній прапорець для відновлення стану після завершення руху.

**Налаштування життєвого циклу (Lifecycle Settings)**:
- `destroyAfterActivate` (bool): Якщо `true`, ефект автоматично запускає самознищення (Destroy) свого `gameObject` після активації.
- `destroyDelayAfterActivate` (float): Затримка в секундах перед самознищенням після активації.
- `SkipDestroy()`: Метод відв'язує ефект від його власника-чіпа (`transform.SetParent(null)`) та встановлює прапорець `isSkipDestroy = true`. Це дозволяє ефекту дограти свою анімацію та самознищитися незалежно від життєвого циклу чіпа (наприклад, для ефекту злиття `MergeLight` на знищуваному чіпі).
- `IsSkipDestroy` (bool): Властивість, яка вказує, чи потрібно пропустити автоматичне знищення цього ефекту разом із власником чіпа в методі `DestroyEffects`.

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
**Клас**: `ContainerHintEffect.cs`
**Реалізує**: `IEffectContainerHint`
**Використовується в**: [ContainerModule](../Chips/ContainerModule.md)

Відображає Panel з іконками предметів, які необхідні контейнеру для виконання квесту, а також показує візуальні підказки (Hint).

- **Компоненти**:
  - `Animator`: Керує анімацією появи (`Activate`), зникнення (`Deactivate`) панелі та відтворення підказки (`Hint`).
  - `LayoutForElements`: Контейнер для іконок предметів.
- **Логіка (`UpdateElements` / `Hint`)**:
  1. Отримує поточний список вимог (`containers`).
  2. Очищає старі елементи та створює нові префаби (`ContainerElementPrefab`) для кожної вимоги в `layoutForElements`.
  3. Якщо контейнер заповнений (`isFull`), викликає `ClearElements` та деактивує ефект.
  4. При спавні, зміні клітинки або тапі запускає підказку `Hint(bool force)` (через Animator-тригер `"Hint"` з налаштованою затримкою або миттєво).

### 4. Generator Charged
**Використовується в**: [GeneratorModule](../Chips/GeneratorModule.md)

Візуальний ефект, що активується, коли генератор повністю зарядився і готовий до створення нової фішки.
- **Активація**: Коли `IsCharged` стає `true` (через поле `chargedEffect`).
- **Деактивація**: Після успішної генерації фішки або під час процесу перезарядки.
- **Тип**: Використовує базовий клас `Effect`. Зазвичай це cyclic idle-анімація (світіння, пульсація), що показує гравцеві готовність об'єкта до взаємодії.

### 5. Generator Charging
**Клас**: `ChargingEffect.cs`
**Реалізує**: `IChargingEffect`
**Використовується в**: [GeneratorModule](../Chips/GeneratorModule.md), [WaitEvolutionModule](../Chips/WaitEvolutionModule.md)

Візуалізує процес заряджання таймера (генератора або модуля очікування еволюції). Відображає прогрес у вигляді таймера-секундоміра (через зміну параметра матеріалу `_GrayProgress` та обертання стрілки-вказівника `arrowTransform`).
- **Метод `OnCharging(float progress)`**: Отримує значення від 0 до 1 та оновлює візуалізацію.

### 6. Power Booster Connector Highlight
**Клас**: `PowerBoosterConnectorCellsHighlightEffect.cs` (наслідує `CellHighlightEffect`)
**Використовується в**: [PowerBoosterModule](../Chips/PowerBoosterModule.md)

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
**Використовується в**: [PowerBoosterModule](../Chips/PowerBoosterModule.md)

Відповідає за динамічні particle-лінки між бустером і кожною активною ціллю `IPowerBoosterTarget`.
- **Join API**: `PowerBoosterModule` викликає `OnJoin`/`OnLeave` через контракт `IEffectPowerBoosterJoin` (`EffectPowerBoosterJoinRef`) під час додавання/зняття підсилення.
- **JoinPoints**: Ефект використовує `JoinPoints` бустера та цілі, обирає найближчі кандидати, а далі періодично перебіндовує активні лінки через корутіну `ChangeJoinPointsCoroutine` (`changeJoinPointsTime`).
- **Cleanup**: `OnLeave` і `Deactivate` зупиняють particle systems, планують `Destroy` по `startLifetime` і очищують runtime-словник лінків; `SetMoving(true)` у бустері також викликає `joinEffect.Deactivate(...)`.

### 8. Shadow Effect
**Клас**: `ShadowEffect.cs`
**Реалізує**: `IShadowEffect`
**Використовується в**: [Chip](../Chips/Chip.md)

Забезпечує постійну тінь під фішкою, яка динамічно реагує на стан руху та зміну висоти підйому чіпа.
- **Особливості**:
  - **Автоматичне копіювання спрайту**: Якщо спрайт `shadowRenderer.sprite` не заданий, ефект автоматично копіює спрайт та параметри відображення (flipX, flipY) з першого рендерера основного чіпа під час `Init`.
  - **Масштабування та позиціонування при ініціалізації**: Автоматично копіює масштаб з основного рендерера та зберігає початкові локальні координати і масштаб тіні для подальших трансформацій.
- **Динамічна висота (`OnHeightChanged`)**: Реалізує інтерфейс `IShadowEffect`. Метод `OnHeightChanged(float height)` викликається під час перетягування фішки або польоту та динамічно змінює:
  - `transform.localPosition` на основі `shadowOffsetPerOneHeight * height`.
  - `transform.localScale` на основі `shadowScalePerOneHeight * height * Vector3.one`.
- **Сортування**: Під час руху тінь збільшує свій `sortingOrder` на величину `AdditionallyWhenMoving` основного чіпа, щоб залишатися візуально під чіпом, але над полем.

### 9. Merge Light
**Константа**: `EffectConsts.MergeLight` (ID 9)  
**Клас**: `MergeLightEffect.cs`  
**Реалізує**: `IEffectMergeLight`  
**Використовується в**: `MergeLightDragFeedback`, [TapEvolutionModule](../Chips/Chip.md#modular-architecture-and-composition-ichipmodule), [WaitEvolutionModule](../Chips/Chip.md#modular-architecture-and-composition-ichipmodule)

Візуальний ефект спалаху або підсвічування, який відтворюється на результуючій фішці після успішного злиття або при еволюції (Tap/Wait Evolution).
- **Особливості**:
  - **Класичний варіант (Merge)**: Активація відбувається автоматично після успішного виконання злиття через систему фідбеку `MergeLightDragFeedback` за допомогою методу `Activate(chip)`.
  - **Малий спалах (`IEffectMergeLight.Little`)**: Підтримує спеціалізовану властивість `bool Little { get; set; }`. При її встановленні (`mergeLight.Little = true`) значення миттєво передається в Animator-параметр `"Little"`, спрямовуючи анімацію в стан малої спалаху (використовується при еволюціях в `TapEvolutionModule` та `WaitEvolutionModule`).
  - Оскільки попередній чіп знищується, а новий з'являється на його місці, ефект допомогає зробити перехід візуально безшовним. Прапорець `destroyAfterActivate` зазвичай дорівнює `true`, що дозволяє йому автоматично самоочиститися з поля після завершення анімації спалаху.

### 10. Merge Hint
**Константа**: `EffectConsts.MergeHint` (ID 10)  
**Використовується в**: `MergeHintDragFeedback`

Візуальний підказка (наприклад, анімоване сяйво або пульсація), яка активується на всіх сумісних фішках-цілях та контейнерах під час початку перетягування (Drag Start).
- **Особливості**:
  - Активація керується через `MergeHintDragFeedback`, який підписується на lifecycle перетягування через `IDragFeedback`.
  - Для уникнення просідання FPS при великій кількості об'єктів на полі, активація ефектів розподіляється по кадрах (порціями через корутіну).
  - Швидкий пошук сумісних об'єктів виконується без алокацій за допомогою словників `MergeableChipsByData` та `FillableChipsByData` у `ChipCollections`.
  - При завершенні перетягування (Drag End) всі активовані ефекти `MergeHint` деактивовуються.

### 11. Tap Hint
**Клас**: `HintEffect.cs`
**Реалізує**: `IEffectHint`
**Використовується в**: [TapEvolutionModule](../Chips/Chip.md#modular-architecture-and-composition-ichipmodule) (або на інших фішках як загальна підказка тапу)

Візуальна підказка для фішок, що реагують на тап (наприклад, що еволюціонують).
- **Особливості**:
  - Реалізує `IEffectHint` для відтворення візуальної підказки `Hint(bool force)`.
  - Відтворює тригер `"Hint"` в Animator із затримкою (`delay`) або миттєво (`force = true`).
  - Спрацьовує автоматично при спавні фішки, зміні клітинки (`OnChangedCell`), або при тапі користувача.

### 101. Box Effect
Ефект коробки. Докладніше про налаштування та використання див. у докуметі **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocker-effects-move-locked-chains-box)**.

### 102. Chains Effect
Ефект ланцюгів. Докладніше про налаштування та використання див. у докуметі **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocker-effects-move-locked-chains-box)**.

### 103. Move Locked Effect
Ефект блокування переміщення. Докладніше про налаштування та використання див. у докуметі **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocker-effects-move-locked-chains-box)**.
