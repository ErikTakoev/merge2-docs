# Chip Effect Blockers & Destroying

[← Back to Main](../Main.md)

This document describes the action blocking system on chips (Blocking) and the mechanic of their progressive destruction (Effect Destroying). These systems allow creating puzzle elements where player has to interact with the field to unlock specific objects.

## Overview

Blocking in Merge2 is a chip state that forbids certain gameplay actions (movement, merge, generation, etc.). Most often, blocking is applied through **Blocker effects** (chains, boxes), which have their own destruction rules.

---

## Blocking System

### `IEffectBlockingSettings`
Blocking configuration contract that defines allowed actions on chip:
- **`CanBeMergedAsSource` / `CanBeMergedAsTarget`**: Permission to merge as source/target.
- **`CanBeFilled`**: Fill permission (for `ChipContainer`).
- **`CanGenerate`**: Generation permission (for `ChipGenerator`).
- **`CanBeTaped`**: Tap permission.
- **`CanApplyModifiers`**: Permission to affect others (for `ChipPowerBooster`).
- **`CanReceiveModifiers`**: Permission to receive boosts (for `ChipGenerator`, `ChipPowerBooster`).
- **`CanBeMoved`**: Movement permission.
- **`IsLittleChip`**: Reduces chip visual size when effect is active.
- **`HideEffectIds`** (`HashSet<int>`): Set of effect IDs that must be hidden when blocking is active.

**Implementations**:
- **`EffectBlockingSettings`** (`ScriptableObject`): Serialized configuration set in Inspector. `hideEffectIds` field (int[]) is directly configured via `[EffectSelector]` attribute and cached in `HideEffectIds`.
- **`CombinedBlockingState`**: Runtime aggregate of all active `IEffectBlockingSettings` on chip. `ApplyBlock` uses AND logic for bool flags and OR for `IsLittleChip`. `RemoveBlock` triggers full `Recalculate` from all remaining blocks. `HideEffectIds` is union of all active ones.

---

## Effect Destroying System

Effect destruction system allows effects to be progressively destroyed during neighboring merges.

### Configuration
- **`EffectDestroyingSettings`** (`ScriptableObject`): Effect destruction settings.
  - `AdditionalStates` (`string[]`): Array of additional state/trigger names played sequentially on neighboring merges before full effect destruction.
  - `Priority` (int): Priority among active destroying effects; highest is processed first.

### Runtime
- **`EffectDestroyingRuntimeData`**: Stores current `NeighboringMergeCount` for each destroying effect.
- **`ChipRuntimeData.EffectDestroyingData`** (`Dictionary<int, EffectDestroyingRuntimeData>`): Per-effect destruction progress dictionary.

### Lifecycle
1. `Chip.InitDestroyingEffectsData()` — scans all effects with `DestroyingSettings`, creates `EffectDestroyingRuntimeData` entries.
2. `Chip.UpdatePrioritizingDestroyingEffect()` — selects effect with highest `Priority` as `effectOfPrioritizingDestroying`.
3. When neighboring chip merges (`OnNeighborsChipOfMerged`), `HandleDestroyingEffects` increments `NeighboringMergeCount` and calls `TryDestroyEffect`.
4. If `TryDestroyEffect` returns `true`, `RemoveEffect` deactivates effect, removes it from dictionary, removes block from `BlockingState`, and selects next priority effect.

---

## Blocker Effects (Move Locked, Chains, Box)

Blocker effects are a set of optional visual effects configured through `ChipExtraEffectsData` (implements `IChipSpecialData`).

- **Configuration**: `ChipExtraEffectsData` contains `ExtraEffectData[]` array where each item defines `effectName` (string resolved to ID via `EffectConsts.GetIdByName`) and `Prefab` (effect prefab).
- **Activation**:
  - Effect activates when its ID is in `ChipRuntimeData.EffectEnables`.
  - During initialization (`Chip.InitEffects`), `ChipExtraEffectsData.Blockers` array is iterated. For each item whose `EffectId` is in `runtimeData.EffectEnables`, prefab is instantiated and added to effects dictionary.
  - `Chip.UpdateVisual()` method activates effects whose IDs are in `EffectEnables`.
- **Deactivation**: When ID is removed from `EffectEnables`.
- **Blocking**: Each blocker effect has `EffectBlockingSettings`, which are passed into chip `CombinedBlockingState` on activation.
- **Destruction**: Blocker effects support destruction system (see [Effect Destroying System](#effect-destroying-system)).

---

## Technical Implementation (Chip Locking)

**Chip Locking** (Move Locked) mechanic is the most common usage of blocking system.

### Data (FieldData)
Lock state is stored in `FieldChipData` structure inside `FieldData`:

```csharp
public struct FieldChipData
{
    [ChipSelector]
    public string ChipId;
    [EffectBlockerSelector]
    public int[] BlockerEffectIds; // Contains effect IDs (for example, EffectConsts.Blockers.MoveLockedEffect = 103)
}
```

Lock-effect configuration is stored in `ChipData.specialDatas` via `ChipExtraEffectsData`.

### Runtime (Chip)
`Chip` class contains `ChipRuntimeData` synchronized with level data:
- **`runtimeData.EffectEnables`**: Set of active blocker effects.
- **`CombinedBlockingState`**: Aggregated blocking state, checked through `BlockingState.CanBeMoved`.
- **`OnDraggingChipWithMoveLocked()`**: Virtual method for feedback on attempt to drag locked chip. First tries sending `"MoveLocked"` trigger to `effectOfPrioritizingDestroying`; if absent — to effect with key `EffectConsts.Blockers.MoveLockedEffect`.

### Level Editor
Blocking system is fully integrated into visual level editor:
1. **Context Menu**: Right-click on chip in grid opens menu with list of all available blocker effects. Effect names are retrieved via `EffectConsts.GetNameByIdEditorOnly`. Currently active effects are marked with checkmark "✓".
2. **Visual indication**: In editor mode, active effects are shown as icons (previews of corresponding effect prefabs) in bottom-right corner of cell. This allows instantly seeing all applied blocks.
3. **Saving**: State is saved through `BlockerEffectIds` array in `FieldChipData` when level asset is saved.

### Use in Gameplay
Blocking through `EffectBlockingSettings` directly affects gameplay mechanics:
- **Movement**: If `CanBeMoved = false`, player cannot start dragging chip. On drag attempt, visual feedback is triggered (`OnDraggingChipWithMoveLocked`).
- **Boost slot**: If `CanReceiveModifiers = false`, chip ignores boosts from Power Boosters.
- **Relocation**: Locked chips act as **immovable obstacles**. Automatic relocation system cannot move such chip. If freeing space for an action (for example, merge or movement of another large chip) requires moving a locked object, the whole action is canceled and player object returns to place.
