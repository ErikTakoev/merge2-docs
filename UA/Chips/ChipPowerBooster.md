# ChipPowerBooster

[← На Головну](../Main.md)

Чіп-підсилювач (Power Booster) — спеціальний чіп, який модифікує параметри сусідніх чіпів. Наприклад, прискорює зарядку генераторів, що знаходяться поруч.

## Architecture and Responsibility

### 1. `ChipPowerBooster.cs`
Наслідує `Chip`. Основний клас бустера.
- **Дані (Configuration)**: Зберігає `ChipPowerBoosterData` з параметром `Power` (множник підсилення), читаючи його через `data.GetSpecialData<ChipPowerBoosterData>()`.
- **Підписки**: Вимагає компонент `PowerBoosterCellSubscriber` (`[RequireComponent]`), який відстежує сусідні клітинки та збирає активні модифікатори.
- **BoostedTargets**: Делегує колекцію `HashSet<IPowerBoosterTarget>` до `PowerBoosterCellSubscriber.BoostedTargets`.
- **Fail-fast перевірки**: В `Init(ChipData)` перевіряє наявність `PowerBoosterCellSubscriber` і `ChipPowerBoosterData`; при відсутності логіка бустера зупиняється з `Debug.LogError`.
- **Ефекти**: Підтримує два опційні ефекти: `connectorCellsHighlightEffect` (підсвітка зони, див. [Visual Effects § 6](../Visuals/Effects.md#6-power-booster-connector-highlight)) і `joinEffect` (`EffectPowerBoosterJoinRef`) для лінків між бустером та цілями (див. [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-join-links)).
- **Apply/Remove API**: `ApplyPowerBooster(IPowerBoosterTarget, bool reapply)` і `RemovePowerBooster(IPowerBoosterTarget)` оновлюють геймплейний вплив та викликають `joinEffect.Value.OnJoin(...)` / `OnLeave(...)`. При застосуванні перевіряється `BlockingState.CanApplyModifiers`.
- **`RemoveEffect(int effectId)`** override: Перевіряє, чи `CanApplyModifiers` змінився після `base.RemoveEffect` — якщо так, викликає `OnChangedCell` для re-subscribe і reapply бустерів до сусідів.
- **`OnTargetChipEffectRemoved(IPowerBoosterTarget, int effectId)`**: Викликається через `IPowerBoosterTarget.NotifyEffectRemoved`, коли у цілі видаляється ефект. Якщо `chipTarget.BlockingState.CanReceiveModifiers` стає `true`, reapply бустер і join-ефект.
- **Move lifecycle**: `SetMoving(true)` одразу деактивує обидва booster-ефекти, щоб уникати stale-візуалізації під час drag/relocation.
- **Destroy lifecycle**: `Destroy(Cell mainCell)` спочатку викликає `cellSubscriber.OnChipDestroy(mainCell)`, і лише потім делегує у `base.Destroy(...)`, щоб гарантовано зняти всі бустери до фінального очищення чіпа.

### 2. `ChipPowerBoosterData`
Серіалізований об'єкт налаштувань бустера.
- Реалізує `IChipSpecialData` і зберігається в `ChipData.specialDatas`.
- **`Power`** (`float`): Множник підсилення, який передається через `IPowerBoosterTarget.ApplyPowerBooster`.

### 3. `IPowerBoosterTarget` (Interface)
Інтерфейс для чіпів, які можуть отримувати підсилення від бустера.
- **`JoinPoints`** (`IReadOnlyList<Transform>`): Якірні точки для join-візуалізації бустера.
- **`AppliedBoosters`** (`HashSet<ChipPowerBooster>`): Колекція активних бустерів.
- **`BlockingState`** (`CombinedBlockingState`): Агрегований стан блокувань цілі. Перевіряється бустером через `CanReceiveModifiers` перед застосуванням/reapply.
- **`ApplyPowerBooster(ChipPowerBooster, bool reapply = false)`**: Застосувати підсилення. Повертає `false`, якщо бустер вже активний (без `reapply`). При `reapply = true` перераховує множник з уврахуванням поточного `BlockingState`.
- **`RemovePowerBooster(ChipPowerBooster)`**: Зняти підсилення.
- **`NotifyEffectRemoved(int effectId)`**: Сповіщає всі активні бустери про видалення ефекту. Викликається з `RemoveEffect` цілі, щоб бустери могли reapply, якщо `CanReceiveModifiers` став `true`.

### 4. `ChipGenerator.PowerBoosterTarget.cs` (Реалізація)
Часткова реалізація `IPowerBoosterTarget` у `ChipGenerator` (partial class).
- **`powerMultiplier`** (`float`): Runtime-множник швидкості зарядки (`Update()` використовує `Time.deltaTime * powerMultiplier`).
- **`joinPoints`** (`Transform[]`): Серіалізовані точки прив'язки на цілі.
- **`AppliedBoosters`** (`HashSet<ChipPowerBooster>`): Набір активних бустерів для цієї цілі.
- **Логіка**: `RecalculatePowerMultiplier` спочатку перевіряє `BlockingState.CanReceiveModifiers` — якщо `false`, скидає множник до `1f`. При кількох бустерах використовується `Mathf.Max` — береться найбільше значення `Power`. При видаленні останнього бустера `powerMultiplier` скидається до `1f`.
- **`RemoveEffect(int effectId)`** override: Після `base.RemoveEffect` викликає `NotifyEffectRemoved(effectId)`, щоб усі активні бустери могли reapply (якщо `CanReceiveModifiers` змінився).
- **`NotifyEffectRemoved(int effectId)`**: Ітерує `appliedBoosters` і викликає `booster.OnTargetChipEffectRemoved(this, effectId)` для кожного.

### 5. `IEffectPowerBoosterJoin` + `PowerBoosterJoinEffect.cs`
- **`IEffectPowerBoosterJoin`**: Контракт для join-візуалізації (`OnJoin`, `OnLeave`, `Show`) з серіалізованою обгорткою `EffectPowerBoosterJoinRef`.
- **`PowerBoosterJoinEffect`**: Реалізація, яка спавнить particle-лінки між бустером і кожним активним `IPowerBoosterTarget`.
- **Вибір точок**: Ефект працює з `JoinPoints` обох сторін (booster/target), обирає найближчі кандидати та періодично перебіндовує активні лінки через корутіну (`changeJoinPointsTime`).
- **Cleanup**: `OnLeave` і `Deactivate` зупиняють particle systems, планують їх знищення по lifetime і очищують внутрішній словник активних лінків.

## Subscriber System

Бустер використовує систему [CellSubscriber](../Features/CellObserverSystem.md#cellsubscriber) для спостереження за сусідніми клітинками.

Спеціалізована реалізація `PowerBoosterCellSubscriber`:
- Трекає набір `boostedTargets: HashSet<IPowerBoosterTarget>` — усі сусідні чіпи, що реалізують `IPowerBoosterTarget`.
- При появі нового чіпа (`OnObservedCellChipChanged`) — додає до `boostedTargets` і викликає `ApplyPowerBooster` тільки якщо `HashSet.Add(...)` повернув `true` (idempotent захист від дублювання).
- При зникненні чіпа — видаляє з `boostedTargets` і викликає `RemovePowerBooster` тільки якщо `HashSet.Remove(...)` повернув `true`.
- При переміщенні бустера (`OnChipChangedCell`) — спочатку знімає всі бустери, перепідписується на нові клітинки, заповнює наявний `boostedTargets` через `GetAllChipsByType<IPowerBoosterTarget>(..., ref boostedTargets)` (без додаткових алокацій) та повторно застосовує підсилення.
- При знищенні (`OnChipDestroy`) — гарантовано знімає всі бустери, очищує `boostedTargets`, після чого викликає базову відписку від `CellObserverManager`.

## Effect Management

Бустер керує двома ефектами, які зберігаються в `effects` словнику з ключами від `EffectConsts`:

- **`EffectConsts.PBoosterConnectorCells`**: Звертання через `GetEffect(EffectConsts.PBoosterConnectorCells)`
  - Ефект: `PowerBoosterConnectorCellsHighlightEffect` (див. [Visual Effects § 6](../Visuals/Effects.md#6-power-booster-connector-highlight))
  - Підсвічує клітинки, за якими спостерігає бустер. Показує гравцеві зону впливу.
  - Ініціалізується в `InitEffects()` і деактивується під час руху чіпа.

- **`EffectConsts.PBoosterJoin`**: Звертання через `GetEffect<IEffectPowerBoosterJoin>(EffectConsts.PBoosterJoin)`
  - Ефект: `PowerBoosterJoinEffect` (реалізує `IEffectPowerBoosterJoin`)
  - Візуалізує динамічні лінії між бустером та активними цілями.
  - Викликає `OnJoin(IPowerBoosterTarget)` / `OnLeave(IPowerBoosterTarget)` при apply/remove.
  - Деактивується при переміщенні чіпа.

- **Visual State**:
  - `UpdateVisual()` — деактивує обидва ефекти під час руху (`IsMoving()`), активує коли бустер стоїть.
  - `SetMoving(true)` — негайно деактивує обидва ефекти на початку перетягування.
  - `ApplyPowerBooster/RemovePowerBooster` — синхронізують gameplay-вплив з join-візуалізацією через `GetEffect<IEffectPowerBoosterJoin>(...)?OnJoin/OnLeave`.

Детальніше: [Visual Effects § 6](../Visuals/Effects.md#6-power-booster-connector-highlight) і [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-join-links).

1. **Init**: `ChipFactory` створює бустер → `Init(ChipData)` → `InitEffects()` → ініціалізація ефектів з ключами `EffectConsts.PBoosterConnectorCells` та `EffectConsts.PBoosterJoin` у словнику `effects`.
2. **Placement**: `FieldGrid.SetChipInCell` → `CellSubscriber.OnChipChangedCell` → `SubscribeToNeighbors` (обчислення bounding box + neighbors).
3. **Observation**: `CellObserverManager` нотифікує `PowerBoosterCellSubscriber.OnObservedCellChipChanged` → idempotent Apply/Remove бустерів + `GetEffect<IEffectPowerBoosterJoin>(...)?OnJoin/OnLeave`.
4. **Move**: `SetMoving(true)` → обидва booster-ефекти деактивуються через `GetEffect(EffectConsts.PBoosterConnectorCells)?.Deactivate(...)` та `GetEffect(EffectConsts.PBoosterJoin)?.Deactivate(...)` → після drop: `OnChipChangedCell` → re-subscribe → новий набір цілей і join-лінків.
5. **Destroy**: 
   - Викликається `ChipPowerBooster.Destroy(Cell mainCell)` override.
   - Спочатку `cellSubscriber.OnChipDestroy(mainCell)` викликає `RemovePowerBooster()` для **всіх** елементів в `boostedTargets` та зупиняє пов'язані join-ефекти через `GetEffect<IEffectPowerBoosterJoin>(...)?OnLeave`.
   - Потім викликається `base.Destroy(mainCell)`, який очищає occupancy на полі, викликає базовий `ICellSubscriber` cleanup і видаляє об'єкти ефектів.
   - **Результат**: Всі пов'язані цілі отримують очищення підсилення перед знищенням бустера, без "завислих" join-лінків або застарілих станів при merge/еволюції.
