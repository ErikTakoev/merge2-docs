# ChipPowerBooster

[← Back to Main](../Main.md)

Power Booster chip is a special chip that modifies parameters of neighboring chips. For example, it speeds up charging of nearby generators.

## Architecture and Responsibility

### 1. `ChipPowerBooster.cs`
Inherits `Chip`. Main booster class.
- **Data (Configuration)**: Stores `ChipPowerBoosterData` with `Power` parameter (boost multiplier), reading it via `data.GetSpecialData<ChipPowerBoosterData>()`.
- **Subscriptions**: Requires `PowerBoosterCellSubscriber` component (`[RequireComponent]`), which tracks neighboring cells and gathers active modifiers.
- **BoostedTargets**: Delegates `HashSet<IPowerBoosterTarget>` collection to `PowerBoosterCellSubscriber.BoostedTargets`.
- **Fail-fast checks**: In `Init(ChipData)`, checks availability of `PowerBoosterCellSubscriber` and `ChipPowerBoosterData`; if missing, booster logic stops with `Debug.LogError`.
- **Effects**: Supports two optional effects: `connectorCellsHighlightEffect` (zone highlight, see [Visual Effects § 6](../Visuals/Effects.md#6-power-booster-connector-highlight)) and `joinEffect` (`EffectPowerBoosterJoinRef`) for links between booster and targets (see [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-join-links)).
- **Apply/Remove API**: `ApplyPowerBooster(IPowerBoosterTarget, bool reapply)` and `RemovePowerBooster(IPowerBoosterTarget)` update gameplay impact and call `joinEffect.Value.OnJoin(...)` / `OnLeave(...)`. During application, `BlockingState.CanApplyModifiers` is checked.
- **`RemoveEffect(int effectId)`** override: Checks whether `CanApplyModifiers` changed after `base.RemoveEffect` — if yes, calls `OnChangedCell` for re-subscribe and booster reapply to neighbors.
- **`OnTargetChipEffectRemoved(IPowerBoosterTarget, int effectId)`**: Called via `IPowerBoosterTarget.NotifyEffectRemoved` when an effect is removed from target. If `chipTarget.BlockingState.CanReceiveModifiers` becomes `true`, reapply booster and join effect.
- **Move lifecycle**: `SetMoving(true)` immediately deactivates both booster effects to avoid stale visualization during drag/relocation.
- **Destroy lifecycle**: `Destroy(Cell mainCell)` first calls `cellSubscriber.OnChipDestroy(mainCell)`, and only then delegates to `base.Destroy(...)` to guarantee all boosters are removed before final chip cleanup.

### 2. `ChipPowerBoosterData`
Serialized booster settings object.
- Implements `IChipSpecialData` and is stored in `ChipData.specialDatas`.
- **`Power`** (`float`): Boost multiplier passed through `IPowerBoosterTarget.ApplyPowerBooster`.

### 3. `IPowerBoosterTarget` (Interface)
Interface for chips that can receive boost from booster.
- **`JoinPoints`** (`IReadOnlyList<Transform>`): Anchor points for booster join visualization.
- **`AppliedBoosters`** (`HashSet<ChipPowerBooster>`): Collection of active boosters.
- **`BlockingState`** (`CombinedBlockingState`): Aggregated target blocking state. Checked by booster through `CanReceiveModifiers` before apply/reapply.
- **`ApplyPowerBooster(ChipPowerBooster, bool reapply = false)`**: Apply boost. Returns `false` if booster is already active (without `reapply`). With `reapply = true`, recalculates multiplier considering current `BlockingState`.
- **`RemovePowerBooster(ChipPowerBooster)`**: Remove boost.
- **`NotifyEffectRemoved(int effectId)`**: Notifies all active boosters about effect removal. Called from target `RemoveEffect` so boosters can reapply if `CanReceiveModifiers` became `true`.

### 4. `ChipGenerator.PowerBoosterTarget.cs` (Implementation)
Partial implementation of `IPowerBoosterTarget` in `ChipGenerator` (partial class).
- **`powerMultiplier`** (`float`): Runtime charging speed multiplier (`Update()` uses `Time.deltaTime * powerMultiplier`).
- **`joinPoints`** (`Transform[]`): Serialized anchor points on target.
- **`AppliedBoosters`** (`HashSet<ChipPowerBooster>`): Set of active boosters for this target.
- **Logic**: `RecalculatePowerMultiplier` first checks `BlockingState.CanReceiveModifiers` — if `false`, resets multiplier to `1f`. With multiple boosters, `Mathf.Max` is used — highest `Power` value is taken. On removing last booster, `powerMultiplier` resets to `1f`.
- **`RemoveEffect(int effectId)`** override: After `base.RemoveEffect`, calls `NotifyEffectRemoved(effectId)` so all active boosters can reapply (if `CanReceiveModifiers` changed).
- **`NotifyEffectRemoved(int effectId)`**: Iterates `appliedBoosters` and calls `booster.OnTargetChipEffectRemoved(this, effectId)` for each.

### 5. `IEffectPowerBoosterJoin` + `PowerBoosterJoinEffect.cs`
- **`IEffectPowerBoosterJoin`**: Contract for join visualization (`OnJoin`, `OnLeave`, `Show`) with serialized wrapper `EffectPowerBoosterJoinRef`.
- **`PowerBoosterJoinEffect`**: Implementation that spawns particle links between booster and each active `IPowerBoosterTarget`.
- **Point selection**: Effect works with both sides' `JoinPoints` (booster/target), selects nearest candidates, and periodically rebinds active links through coroutine (`changeJoinPointsTime`).
- **Cleanup**: `OnLeave` and `Deactivate` stop particle systems, schedule their destruction by lifetime, and clear internal active links dictionary.

## Subscriber System

Booster uses [CellSubscriber](../Features/CellObserverSystem.md#cellsubscriber) system to observe neighboring cells.

Specialized implementation `PowerBoosterCellSubscriber`:
- Tracks `boostedTargets: HashSet<IPowerBoosterTarget>` set — all neighboring chips implementing `IPowerBoosterTarget`.
- On new chip appearance (`OnObservedCellChipChanged`) — adds to `boostedTargets` and calls `ApplyPowerBooster` only if `HashSet.Add(...)` returned `true` (idempotent duplicate protection).
- On chip disappearance — removes from `boostedTargets` and calls `RemovePowerBooster` only if `HashSet.Remove(...)` returned `true`.
- On booster move (`OnChipChangedCell`) — first removes all boosters, re-subscribes to new cells, fills existing `boostedTargets` via `GetAllChipsByType<IPowerBoosterTarget>(..., ref boostedTargets)` (without extra allocations), and reapplies boost.
- On destroy (`OnChipDestroy`) — guaranteed to remove all boosters, clear `boostedTargets`, then call base unsubscribe from `CellObserverManager`.

## Effect Management

Booster controls two effects stored in `effects` dictionary with keys from `EffectConsts`:

- **`EffectConsts.PBoosterConnectorCells`**: Accessed through `GetEffect(EffectConsts.PBoosterConnectorCells)`
  - Effect: `PowerBoosterConnectorCellsHighlightEffect` (see [Visual Effects § 6](../Visuals/Effects.md#6-power-booster-connector-highlight))
  - Highlights cells observed by booster. Shows player the influence zone.
  - Initialized in `InitEffects()` and deactivated while chip is moving.

- **`EffectConsts.PBoosterJoin`**: Accessed through `GetEffect<IEffectPowerBoosterJoin>(EffectConsts.PBoosterJoin)`
  - Effect: `PowerBoosterJoinEffect` (implements `IEffectPowerBoosterJoin`)
  - Visualizes dynamic lines between booster and active targets.
  - Calls `OnJoin(IPowerBoosterTarget)` / `OnLeave(IPowerBoosterTarget)` on apply/remove.
  - Deactivates when chip moves.

- **Visual State**:
  - `UpdateVisual()` — deactivates both effects while moving (`IsMoving()`), activates when booster is standing.
  - `SetMoving(true)` — immediately deactivates both effects at drag start.
  - `ApplyPowerBooster/RemovePowerBooster` — synchronize gameplay impact with join visualization through `GetEffect<IEffectPowerBoosterJoin>(...)?OnJoin/OnLeave`.

More details: [Visual Effects § 6](../Visuals/Effects.md#6-power-booster-connector-highlight) and [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-join-links).

1. **Init**: `ChipFactory` creates booster -> `Init(ChipData)` -> `InitEffects()` -> initialization of effects with keys `EffectConsts.PBoosterConnectorCells` and `EffectConsts.PBoosterJoin` in `effects` dictionary.
2. **Placement**: `FieldGrid.SetChipInCell` -> `CellSubscriber.OnChipChangedCell` -> `SubscribeToNeighbors` (bounding box + neighbors calculation).
3. **Observation**: `CellObserverManager` notifies `PowerBoosterCellSubscriber.OnObservedCellChipChanged` -> idempotent booster Apply/Remove + `GetEffect<IEffectPowerBoosterJoin>(...)?OnJoin/OnLeave`.
4. **Move**: `SetMoving(true)` -> both booster effects are deactivated through `GetEffect(EffectConsts.PBoosterConnectorCells)?.Deactivate(...)` and `GetEffect(EffectConsts.PBoosterJoin)?.Deactivate(...)` -> after drop: `OnChipChangedCell` -> re-subscribe -> new target set and join links.
5. **Destroy**: 
   - `ChipPowerBooster.Destroy(Cell mainCell)` override is called.
   - First `cellSubscriber.OnChipDestroy(mainCell)` calls `RemovePowerBooster()` for **all** elements in `boostedTargets` and stops related join effects through `GetEffect<IEffectPowerBoosterJoin>(...)?OnLeave`.
   - Then `base.Destroy(mainCell)` is called, which clears field occupancy, calls base `ICellSubscriber` cleanup, and removes effect objects.
   - **Result**: All related targets receive boost cleanup before booster destruction, without "hanging" join links or stale states during merge/evolution.
