[← На Головну](../Main.md)

# CellObserverManager

## Description
Система спостереження за клітинками складається з двох компонентів:
- [`CellObserverManager.cs`](../../../Core/Scripts/Field/CellObserver/CellObserverManager.cs) керує підписками `ICellSubscriber` на фізичні клітинки поля та розсилає події.
- [`DeferredChipChangeNotifier.cs`](../../../Core/Scripts/Field/CellObserver/DeferredChipChangeNotifier.cs) накопичує зміни клітинок протягом кадру і віддає їх одним `Flush`.

Такий підхід дає стабільний зріз змін наприкінці кадру і зменшує кількість дубльованих callback-викликів.

## Key Properties

### CellObserverManager.cs
- Підтримує двосторонній індекс підписок:
  - `cellToSubscribers`: фізична клітинка -> підписники.
  - `subscriberToCells`: підписник -> всі клітинки, які він відстежує.
- `Subscribe` додає зв'язок у обидва індекси; невалідні позиції ігнорує з warning.
- `Unsubscribe` працює за `subscriberToCells`, тому видалення виконується за `O(k)` від числа відстежуваних клітинок.
- Під час обробки `OnFlush` застосовує `eventNotifiedSubscribers`, щоб один підписник не отримував дубльовані виклики для однієї події.
- Для безпечної ітерації callback-ів використовує `subscriberSnapshot`, щоб відписка під час callback не ламала перебір колекції.
- Розширює зону нотифікації на область старого/нового розміру чіпа (`OldChip.Data.Size` / `NewChip.Data.Size`), тому secondary-клітинки multi-cell чіпів також покриваються.
- Має `GetSubscriptionData` і `LogSubscriptions` для діагностики підписок.

#### Core Data Structures
- `Dictionary<Cell, HashSet<ICellSubscriber>> cellToSubscribers`
  - Призначення: прямий індекс `клітинка -> підписники`.
  - Де використовується: під час проходу по області змін у `NotifySubscribersInArea`.
  - Чому так: `Dictionary` дає швидкий доступ за `Cell`, а `HashSet` не допускає дубльованих підписок одного і того ж `ICellSubscriber` на ту саму клітинку.

- `Dictionary<ICellSubscriber, HashSet<Cell>> subscriberToCells`
  - Призначення: зворотний індекс `підписник -> всі його клітинки`.
  - Де використовується: в `Unsubscribe` і `GetSubscriptionData`.
  - Чому так: відписка виконується за списком власних клітинок підписника (`O(k)`), без сканування всієї сітки.

- `HashSet<ICellSubscriber> eventNotifiedSubscribers`
  - Призначення: тимчасовий dedupe-набір на одну подію `ChipChangedEvent`.
  - Де використовується: у `HandleFlush`/`NotifySubscribersForEvent`; очищується перед кожною подією.
  - Чому так: якщо підписник відстежує кілька клітинок в межах однієї області змін, callback `OnObservedCellChipChanged` все одно викликається тільки один раз.

- `List<ICellSubscriber> subscriberSnapshot`
  - Призначення: реюзабельний snapshot поточного `HashSet` підписників клітинки.
  - Де використовується: у `NotifySubscribersInArea` перед callback-викликами.
  - Чому так: дозволяє безпечно ітерувати, навіть якщо під час callback хтось відпишеться; також зменшує алокації, бо список перевикористовується.

### DeferredChipChangeNotifier.cs
- Зберігає відкладені події у `pending` в порядку додавання.
- Має `pendingIndexByCell`, щоб за `O(1)` зливати повторні зміни тієї ж клітинки в межах кадру.
- При повторному `Enqueue` для тієї ж клітинки зберігає перший `OldChip`, а `NewChip` оновлює до останнього стану.
- `Flush` викликає `OnFlush` один раз за кадр і очищає обидві структури.

## Interactions
- [`FieldGrid.cs`](../../../Core/Scripts/Field/FieldGrid.cs) у `SetChipInCell` викликає `IChipChangeNotifier.Enqueue` при кожній зміні клітинки: для очищення після `ClearCells(...)`, для встановлення чіпа після оновлення `cell.Chip` і `chip.CellPosition`.
- [`FieldEventHandler.cs`](../../../Core/Scripts/Field/FieldEventHandler.cs) у `LateUpdate` викликає `Flush`, щоб завершити кадр консистентним набором змін.
- `CellObserverManager` підписується на `IChipChangeNotifier.OnFlush` у `Start` і відписується в `OnDestroy`.
- Споживачами є реалізації [`ICellSubscriber`](../../../Core/Scripts/VContainer/Interfaces/ICellSubscriber.cs) (наприклад, `CellSubscriber`), які отримують `OnObservedCellChipChanged(ChipChangedEvent evt)`.
- У DI-конфігурації [`Merge2LifetimeScope.cs`](../../../Core/Scripts/VContainer/Merge2LifetimeScope.cs) `DeferredChipChangeNotifier` реєструється як `IChipChangeNotifier` (Singleton), а `CellObserverManager` — як компонент сцени.

## Process (Flow)
Ланцюг обробки: `SetChipInCell -> Enqueue -> LateUpdate/Flush -> HandleFlush -> OnObservedCellChipChanged`.

1. Зміна клітинки проходить через `FieldGrid.SetChipInCell`.
2. Якщо чіп прибирається (`chip == null`), `SetChipInCell` спочатку очищає зайняті клітинки через `ClearCells`, після чого додає `Enqueue(cell, oldChip, null)`.
3. Якщо чіп встановлюється, `SetChipInCell` призначає `cell.Chip`, оновлює `chip.CellPosition`, і тільки потім додає `Enqueue(cell, oldChip, chip)`, щоб підписники бачили актуальну позицію.
4. Наприкінці кадру `FieldEventHandler.LateUpdate` викликає `IChipChangeNotifier.Flush()`.
5. `DeferredChipChangeNotifier.Flush` публікує пакет `ChipChangedEvent` через `OnFlush`.
6. `CellObserverManager.HandleFlush` проходить по подіях і для кожної події обчислює релевантну область клітинок.
7. Для знайдених підписників викликається `ICellSubscriber.OnObservedCellChipChanged(evt)` без дублювання викликів у межах однієї події.

## Subscribers (CellSubscriber)

Базовий компонент для чіпів, що потребують спостереження за сусідніми клітинками. Знаходиться в `Chips/Subscribers/`.

### `CellSubscriber.cs`
`MonoBehaviour`, реалізує `ICellSubscriber`. Потребує `[RequireComponent(typeof(Chip))]`.

- **DI**: Отримує `CellObserverManager` та `IFieldGrid` через `[Inject]`.
- **`ObservedCellPositions`** (`IReadOnlyList<Vector2Int>`): Публічний доступ до списку позицій, за якими підписник спостерігає.
- **`SubscribeToNeighbors(Vector2Int origin)`**: Обчислює всі клітинки навколо зайнятої чіпом області (bounding box `origin-1 .. origin+size`), виключає власні клітинки чіпа, перевіряє валідність позицій через `IFieldGrid.IsValidCellPos` та підписується через `CellObserverManager.Subscribe`.
- **`OnChipChangedCell(Cell source, Cell target)`**: Відписується від старих клітинок, підписується на нових сусідів навколо `target`.
- **`OnChipDestroy(Cell)`**: Відписується від `CellObserverManager`.
- **`OnObservedCellChipChanged(ChipChangedEvent)`**: Базова реалізація — логування (якщо `chip.LogEnable`).
- **`GetAllChipsByType<T>(List<Vector2Int>, ref HashSet<T>)`**: Утиліта — збирає всі чіпи певного типу з вказаних позицій поля в caller-owned `HashSet` (очищає і перевикористовує колекцію без додаткових алокацій).

### `PowerBoosterCellSubscriber.cs`
Спеціалізація `CellSubscriber` для [ChipPowerBooster](../Chips/ChipPowerBooster.md). Потребує `[RequireComponent(typeof(ChipPowerBooster))]`.

- **`ModifiedEntities`** (`HashSet<IPowerBoosterModifier>`): Набір активних модифікаторів (сусідніх чіпів, що реалізують `IPowerBoosterModifier`).
- **`OnObservedCellChipChanged(ChipChangedEvent)`**: 
  - Якщо `NewChip` реалізує `IPowerBoosterModifier` → викликає `modifiedEntities.Add(...)`; модифікатор застосовується лише при `true` (idempotent захист).
  - Якщо `OldChip` реалізує `IPowerBoosterModifier` → викликає `modifiedEntities.Remove(...)`; модифікатор знімається лише при `true`.
- **`OnChipChangedCell(Cell, Cell)`**: Знімає всі існуючі модифікатори → `base.OnChipChangedCell` (re-subscribe) → оновлює набір через `GetAllChipsByType<IPowerBoosterModifier>(..., ref modifiedEntities)` → застосовує нові модифікатори.
- **`OnChipDestroy(Cell)`**: Знімає всі активні модифікатори, очищує `modifiedEntities`, після чого викликає базову відписку від `CellObserverManager`.
