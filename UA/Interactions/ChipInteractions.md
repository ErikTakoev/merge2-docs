# ChipInteractions

[← На Головну](../Main.md)

Цей розділ описує механізм, за допомогою якого система перетягування фішок ([DraggableChipLogic](../../../Core/Scripts/Field/Logics/DraggableChipLogic.cs)) делегує перевірку та виконання специфічних взаємодій (таких як злиття або наповнення контейнера) підключеним модулям фішок через контракт [IChipModule](../../../Core/Scripts/Chips/Interfaces/IChipModule.cs), а також керує візуальним зворотним зв'язком через інтерфейс `IDragFeedback`.

## DragInteractionResult

`DragInteractionResult` — перечислення, що описує результат взаємодії після завершення перетягування. Повертається методом `IChipModule.OnInteract` і передається у `IDragFeedback.OnDragEndFeedback` для диференціації реакції feedback-компонентів:

| Значення | Опис |
|---|---|
| `None` | Взаємодії не відбулось; фішка повернулась на місце або перемістилась через `IChipMovingLogic`. |
| `Partial` | Взаємодія прийнята, але не повністю завершена (наприклад, фішку додано до контейнера, але в ньому ще залишилися невиконані вимоги). |
| `Full` | Взаємодія повністю завершена (злиття або повне заповнення контейнера). |

## Architecture Overview

Система взаємодій побудована навколо композиції модулів на об'єкті [Chip](../../../Core/Scripts/Chips/Chip.cs):
- `DraggableChipLogic` відповідає за фізичне переміщення фішки, пошук зайнятих клітинок у зоні накладання та оркестрацію drag-життєвого циклу, не містячи жорстко прив'язаної логіки конкретних правил.
- Логіка конкретних взаємодій реалізується модулями `IChipModule`, розміщеними на префабі самої фішки (`Chip`), через методи `CanInteract`, `OnInteract` та `CollectHintTargets`.
- `IDragFeedback` — компоненти на об'єкті `DraggableChipLogic` для візуального супроводу перетягування (підказки `MergeHintDragFeedback`, спалах `MergeLightDragFeedback`, лічильники груп тощо).

## Transition Process

### 1. Initialization
При старті (`Awake`) `DraggableChipLogic` збирає всі компоненти візуального зворотного зв'язку на своєму GameObject:
```csharp
IDragFeedback[] feedbacks = GetComponents<IDragFeedback>();
```
Самі модулі взаємодій (`IChipModule[]`) кешуються безпосередньо в кожному екземплярі `Chip` під час `Chip.Init` (`modules = GetComponents<IChipModule>()`).

### 2. Drag Lifecycle & Feedback Callbacks
Під час життєвого циклу перетягування `DraggableChipLogic` сповіщає всі компоненти `IDragFeedback`:
- **`OnDragStart`**: Викликається `feedback.OnDragStartFeedback(Chip)`. Наприклад, `MergeHintDragFeedback` запускає корутину `ActivateHintsGradually`, яка збирає сумісні цілі через `Chip.CollectHintTargets(chipCollections, hintTargetChips)` та додає контейнери з `chipCollections.FillableChipsByData` без дублікатів.
- **`OnDrag` (при зміні якоря)**: Коли фішка переміщується на нову клітинку, оновлюється стан взаємодії (`UpdateInteractionState`), надсилаються сповіщення ефектам (`OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`) та викликається `feedback.OnDragFeedback(Chip, prevCell, newCell)`.
- **`OnDragEnd`**: Очищає hover-ефекти, виконує активну взаємодію (якщо вона є та не заблокована `IInputBlocker`), після чого викликає `feedback.OnDragEndFeedback(targetChipBeforeInteraction, interactionResult)`. Наприклад, `MergeLightDragFeedback` активує ефект `MergeLight` лише при `DragInteractionResult.Full`.

### 3. Continuous Interaction Checking (`OnDrag`)
Під час перетягування метод `UpdateInteractionState(sourceCell, anchorCell)` перевіряє допустимі взаємодії:
- Якщо `targetCell.IsBlocked`, стан взаємодії скидається (`ResetCurrentMergable`).
- `GetFilterCells(targetCell)` збирає унікальні зайняті клітинки під зоною розміщення перетягуваної фішки (із використанням кешованих буферів без GC-алокацій).
- Для кожної клітинки `cell` із `filterCells` перевірка виконується у строгому пріоритеті:
  1. Спочатку опитується перетягувана фішка: `Chip.CanInteract(sourceCell, cell)` (активний модуль на фішці, яку тягнуть, наприклад `Merge2Module`). Якщо повертає `true`, встановлюється `currentInteractionChip = Chip`.
  2. Інакше опитується цільова фішка під курсором: `targetChip.CanInteract(sourceCell, cell)` (модуль на цільовій фішці, наприклад `ContainerModule`). Якщо повертає `true`, встановлюється `currentInteractionChip = targetChip`.
- У обох випадках `currentMergableCell` зберігає головну клітинку цілі (`cell.MainCell ?? cell`).

### 4. Interaction Completion (`OnDragEnd`)
Коли гравець відпускає фішку:
- Якщо `currentInteractionChip != null`, `targetChipBeforeInteraction != null` та взаємодія не ігнорується туторіал-блокером (`ignoreInteraction == false`):
  - Викликається `currentInteractionChip.OnInteract(sourceCell, currentMergableCell, out interactionResult)`.
  - Усередині `Chip.OnInteract` перебираються кешовані модулі `GetModules()` у порядку компонентів в інспекторі: виконується перший модуль, для якого `module.CanInteract(...) && module.OnInteract(..., out result)` повертає `true`.
  - Якщо `OnInteract` повернув `true`, `DraggableChipLogic` сповіщає сусідні фішки через `NotifyNeighborsOfInteraction(currentMergableCell, targetChipSize)`.
  - Якщо `OnInteract` повернув `false`, знімається стан перетягування і фішка повертається на початкову позицію через `chipMovingLogic.ChipMoving(overCell, null, sourceCell, ChipFlightSettings.ChipSwap)`.
- Якщо активної взаємодії немає (або її заблоковано), виконується переміщення/обмін (`ChipMoving`) на `targetAnchorCell`.
- На завершення кожен `IDragFeedback` отримує `OnDragEndFeedback(targetChipBeforeInteraction, interactionResult)`.

## `IChipModule` Interaction Contract

Кожен модуль фішки може за потреби реалізувати методи взаємодії інтерфейсу `IChipModule` (усі мають реалізації за замовчуванням у базовому інтерфейсі):
- `bool CanInteract(ICell sourceCell, ICell targetCell)` — валідує можливість взаємодії без зміни стану поля; за замовчуванням повертає `false`.
- `bool OnInteract(ICell sourceCell, ICell targetCell, out DragInteractionResult result)` — виконує взаємодію та класифікує результат через `DragInteractionResult`; за замовчуванням повертає `false` із `DragInteractionResult.None`.
- `void CollectHintTargets(IChipCollections collections, ICollection<Chip> targets)` — додає цільові фішки поля, які повинні підсвічуватися через `MergeHint` на старті перетягування.

## `IDragFeedback` Interface

Кожен компонент візуального фідбеку реалізує три методи життєвого циклу:
- `OnDragStartFeedback(Chip chip)`: Початок перетягування.
- `OnDragFeedback(Chip chip, ICell prevCell, ICell newCell)`: Зміна клітинки-якоря під час перетягування.
- `OnDragEndFeedback(Chip chip, DragInteractionResult interaction)`: Завершення перетягування з переданим підсумком `interaction` (`None`, `Partial` або `Full`).

## Implementations
- **[Merge2Module](../Chips/Merge2Module.md)** — Модуль злиття двох сумісних фішок із делегуванням планування і спавну в `IChipEvolutionService`.
- **[ContainerModule](../Chips/ContainerModule.md#drag-and-drop-interaction-caninteract--oninteract)** — Модуль наповнення контейнера через `CanInteract`/`OnInteract`.
- **`MergeHintDragFeedback`** — Компонент фідбеку, що активує ефекти `MergeHint` на цілях, зібраних через `Chip.CollectHintTargets`, та на контейнерах із `FillableChipsByData`.
- **`MergeLightDragFeedback`** — Компонент фідбеку, що активує ефект `MergeLight` на цільовій фішці при `DragInteractionResult.Full`.
