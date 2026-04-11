# ChipPowerBooster

[← На Головну](../Main.md)

Чіп-підсилювач (Power Booster) — спеціальний чіп, який модифікує параметри сусідніх чіпів. Наприклад, прискорює зарядку генераторів, що знаходяться поруч.

## Architecture and Responsibility

### 1. `ChipPowerBooster.cs`
Наслідує `Chip`. Основний клас бустера.
- **Дані (Configuration)**: Зберігає `ChipPowerBoosterData` з параметром `Power` (множник підсилення), читаючи його через `data.GetSpecialData<ChipPowerBoosterData>()`.
- **Підписки**: Вимагає компонент `PowerBoosterCellSubscriber` (`[RequireComponent]`), який відстежує сусідні клітинки та збирає активні модифікатори.
- **ModifiedEntities**: Делегує колекцію `HashSet<IPowerBoosterModifier>` до `PowerBoosterCellSubscriber.ModifiedEntities`.
- **Fail-fast перевірки**: В `Init(ChipData)` перевіряє наявність `PowerBoosterCellSubscriber` і `ChipPowerBoosterData`; при відсутності логіка бустера зупиняється з `Debug.LogError`.
- **Ефекти**: Підтримує два опційні ефекти: `connectorCellsHighlightEffect` (підсвітка зони, див. [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-connector-highlight)) і `joinEffect` (`EffectPowerBoosterJoinRef`) для лінків між бустером та модифікаторами (див. [Visual Effects § 8](../Visuals/Effects.md#8-power-booster-join-links)).
- **Apply/Remove API**: `ApplyPowerBoosterModifier(IPowerBoosterModifier, bool reapply)` і `RemovePowerBoosterModifier(IPowerBoosterModifier)` оновлюють геймплейний модифікатор та викликають `joinEffect.Value.OnJoin(...)` / `OnLeave(...)`. При застосуванні модифікатора перевіряється `BlockingState.CanApplyModifiers`.
- **`RemoveEffect(int effectId)`** override: Перевіряє, чи `CanApplyModifiers` змінився після `base.RemoveEffect` — якщо так, викликає `OnChangedCell` для re-subscribe і reapply модифікаторів.
- **`OnTargetChipEffectRemoved(IPowerBoosterModifier, int effectId)`**: Викликається через `IPowerBoosterModifier.NotifyEffectRemoved`, коли у модифікатора видаляється ефект. Якщо `chipTarget.BlockingState.CanReceiveModifiers` стає `true`, reapply модифікатор і join-ефект.
- **Move lifecycle**: `SetMoving(true)` одразу деактивує обидва booster-ефекти, щоб уникати stale-візуалізації під час drag/relocation.
- **Destroy lifecycle**: `Destroy(Cell mainCell)` спочатку викликає `cellSubscriber.OnChipDestroy(mainCell)`, і лише потім делегує у `base.Destroy(...)`, щоб гарантовано зняти всі модифікатори до фінального очищення чіпа.

### 2. `ChipPowerBoosterData`
Серіалізований об'єкт налаштувань бустера.
- Реалізує `IChipSpecialData` і зберігається в `ChipData.specialDatas`.
- **`Power`** (`float`): Множник підсилення, який передається через `IPowerBoosterModifier.ApplyPowerBoosterModifier`.

### 3. `IPowerBoosterModifier` (Interface)
Інтерфейс для чіпів, які можуть отримувати підсилення від бустера.
- **`JoinPoints`** (`IReadOnlyList<Transform>`): Якірні точки для join-візуалізації бустера.
- **`PowerBoosterModifiers`** (`HashSet<ChipPowerBooster>`): Колекція активних бустерів.
- **`BlockingState`** (`CombinedBlockingState`): Агрегований стан блокувань модифікатора. Перевіряється бустером через `CanReceiveModifiers` перед застосуванням/reapply.
- **`ApplyPowerBoosterModifier(ChipPowerBooster, bool reapply = false)`**: Застосувати підсилення. Повертає `false`, якщо модифікатор вже активний (без `reapply`). При `reapply = true` перераховує множник з урахуванням поточного `BlockingState`.
- **`RemovePowerBoosterModifier(ChipPowerBooster)`**: Зняти підсилення.
- **`NotifyEffectRemoved(int effectId)`**: Сповіщає всі активні бустери про видалення ефекту. Викликається з `RemoveEffect` модифікатора, щоб бустери могли reapply, якщо `CanReceiveModifiers` став `true`.

### 4. `ChipGenerator.PowerBoosterModifier.cs` (Реалізація)
Часткова реалізація `IPowerBoosterModifier` у `ChipGenerator` (partial class).
- **`powerMultiplier`** (`float`): Runtime-множник швидкості зарядки (`Update()` використовує `Time.deltaTime * powerMultiplier`).
- **`joinPoints`** (`Transform[]`): Серіалізовані точки прив'язки на генераторі.
- **`PowerBoosterModifiers`** (`HashSet<ChipPowerBooster>`): Набір активних бустерів для цього генератора.
- **Логіка**: `RecalculatePowerMultiplier` спочатку перевіряє `BlockingState.CanReceiveModifiers` — якщо `false`, скидає множник до `1f`. При кількох бустерах використовується `Mathf.Max` — береться найбільше значення `Power`. При видаленні останнього бустера `powerMultiplier` скидається до `1f`.
- **`RemoveEffect(int effectId)`** override: Після `base.RemoveEffect` викликає `NotifyEffectRemoved(effectId)`, щоб усі активні бустери могли reapply модифікатори (якщо `CanReceiveModifiers` змінився).
- **`NotifyEffectRemoved(int effectId)`**: Ітерує `powerBoosterModifiers` і викликає `booster.OnTargetChipEffectRemoved(this, effectId)` для кожного.

### 5. `IEffectPowerBoosterJoin` + `PowerBoosterJoinEffect.cs`
- **`IEffectPowerBoosterJoin`**: Контракт для join-візуалізації (`OnJoin`, `OnLeave`, `Show`) з серіалізованою обгорткою `EffectPowerBoosterJoinRef`.
- **`PowerBoosterJoinEffect`**: Реалізація, яка спавнить particle-лінки між бустером і кожним активним `IPowerBoosterModifier`.
- **Вибір точок**: Ефект працює з `JoinPoints` обох сторін (booster/modifier), обирає найближчі кандидати та періодично перебіндовує активні лінки через корутіну (`changeJoinPointsTime`).
- **Cleanup**: `OnLeave` і `Deactivate` зупиняють particle systems, планують їх знищення по lifetime і очищують внутрішній словник активних лінків.

## Subscriber System

Бустер використовує систему [CellSubscriber](../Features/CellObserverSystem.md#cellsubscriber) для спостереження за сусідніми клітинками.

Спеціалізована реалізація `PowerBoosterCellSubscriber`:
- Трекає набір `modifiedEntities: HashSet<IPowerBoosterModifier>` — усі сусідні чіпи, що реалізують `IPowerBoosterModifier`.
- При появі нового чіпа (`OnObservedCellChipChanged`) — додає до `modifiedEntities` і викликає `ApplyPowerBoosterModifier` тільки якщо `HashSet.Add(...)` повернув `true` (idempotent захист від дублювання).
- При зникненні чіпа — видаляє з `modifiedEntities` і викликає `RemovePowerBoosterModifier` тільки якщо `HashSet.Remove(...)` повернув `true`.
- При переміщенні бустера (`OnChipChangedCell`) — спочатку знімає всі модифікатори, перепідписується на нові клітинки, заповнює наявний `modifiedEntities` через `GetAllChipsByType<IPowerBoosterModifier>(..., ref modifiedEntities)` (без додаткових алокацій) та повторно застосовує модифікатори.
- При знищенні (`OnChipDestroy`) — гарантовано знімає всі модифікатори, очищує `modifiedEntities`, після чого викликає базову відписку від `CellObserverManager`.

## Effect Management

Бустер керує двома ефектами, які зберігаються в `effects` словнику з ключами від `EffectConsts`:

- **`EffectConsts.PBoosterConnectorCells`**: Звертання через `GetEffect(EffectConsts.PBoosterConnectorCells)`
  - Ефект: `PowerBoosterConnectorCellsHighlightEffect` (див. [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-connector-highlight))
  - Підсвічує клітинки, за якими спостерігає бустер. Показує гравцеві зону впливу.
  - Ініціалізується в `InitEffects()` і деактивується під час руху чіпа.

- **`EffectConsts.PBoosterJoin`**: Звертання через `GetEffect<IEffectPowerBoosterJoin>(EffectConsts.PBoosterJoin)`
  - Ефект: `PowerBoosterJoinEffect` (реалізує `IEffectPowerBoosterJoin`)
  - Візуалізує динамічні лінії між бустером та активними модифікаторами.
  - Викликає `OnJoin(IPowerBoosterModifier)` / `OnLeave(IPowerBoosterModifier)` при apply/remove.
  - Деактивується при переміщенні чіпа.

- **Visual State**:
  - `UpdateVisual()` — деактивує обидва ефекти під час руху (`IsMoving()`), активує коли бустер стоїть.
  - `SetMoving(true)` — негайно деактивує обидва ефекти на початку перетягування.
  - `ApplyPowerBoosterModifier/RemovePowerBoosterModifier` — синхронізують gameplay-модифікатори з join-візуалізацією через `GetEffect<IEffectPowerBoosterJoin>(...)?OnJoin/OnLeave`.

Детальніше: [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-connector-highlight) і [Visual Effects § 8](../Visuals/Effects.md#8-power-booster-join-links).

1. **Init**: `ChipFactory` створює бустер → `Init(ChipData)` → `InitEffects()` → ініціалізація ефектів з ключами `EffectConsts.PBoosterConnectorCells` та `EffectConsts.PBoosterJoin` у словнику `effects`.
2. **Placement**: `FieldGrid.SetChipInCell` → `CellSubscriber.OnChipChangedCell` → `SubscribeToNeighbors` (обчислення bounding box + neighbors).
3. **Observation**: `CellObserverManager` нотифікує `PowerBoosterCellSubscriber.OnObservedCellChipChanged` → idempotent Apply/Remove модифікаторів + `GetEffect<IEffectPowerBoosterJoin>(...)?OnJoin/OnLeave`.
4. **Move**: `SetMoving(true)` → обидва booster-ефекти деактивуються через `GetEffect(EffectConsts.PBoosterConnectorCells)?.Deactivate(...)` та `GetEffect(EffectConsts.PBoosterJoin)?.Deactivate(...)` → після drop: `OnChipChangedCell` → re-subscribe → новий набір модифікаторів і join-лінків.
5. **Destroy**: 
   - Викликається `ChipPowerBooster.Destroy(Cell mainCell)` override.
   - Спочатку `cellSubscriber.OnChipDestroy(mainCell)` викликає `RemovePowerBoosterModifier()` для **всіх** елементів в `modifiedEntities` та зупиняє пов'язані join-ефекти через `GetEffect<IEffectPowerBoosterJoin>(...)?OnLeave`.
   - Потім викликається `base.Destroy(mainCell)`, який очищає occupancy на полі, викликає базовий `ICellSubscriber` cleanup і видаляє об'єкти ефектів.
   - **Результат**: Всі пов'язані генератори отримують очищення модифікаторів перед знищенням бустера, без "завислих" join-лінків або застарілих станів при merge/еволюції.
