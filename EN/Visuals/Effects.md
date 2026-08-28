# Visual Effects

[← Back to Main](../Main.md)

This document describes the visual feedback system used to provide player feedback during interaction with chips on the game field.

## Base System

### `EffectConsts.cs`
Centralized registry of integer constant identifiers for all effects in the system. Instead of storing references to individual effect fields, the system uses a `Dictionary<int, IEffect>` with constant keys.

**Base Effects (1–7)**:

| Constant | ID | Description |
|---|---|---|
| [`MergeAvailable`](#1-merge-available) | 1 | Activated when chip is eligible for merge |
| [`CellHighlight`](#2-cell-highlight) | 2 | Highlights cells under the chip |
| [`ContainerRequirements`](#3-container-requirements) | 3 | Displays item icons required by the container |
| [`GeneratorCharged`](#4-generator-charged) | 4 | Activated when generator is fully charged |
| [`GeneratorCharging`](#5-generator-charging) | 5 | Shows generator charging progress |
| [`PBoosterConnectorCells`](#6-power-booster-connector-highlight) | 6 | Highlights neighboring chips boosted by the booster |
| [`PBoosterJoin`](#7-power-booster-join-links) | 7 | Dynamic lines between booster and boost targets |
| [`ShadowEffect`](#8-shadow-effect) | 8 | Permanent shadow under chip that reacts to movement |

**Blocker Effects (101+)** — nested class `EffectConsts.Blockers`:

| Constant | ID | Description |
|---|---|---|
| [`BoxEffect`](#101-box-effect) | 101 | Box effect |
| [`ChainsEffect`](#102-chains-effect) | 102 | Chains effect |
| [`MoveLockedEffect`](#103-move-locked-effect) | 103 | Move lock effect |

**Utilities**:
- **`nameToId`** (`Dictionary<string, int>`): Dictionary for resolving string effect names into integer IDs. Used by `ExtraEffectData.EffectId` and `EffectBlockingSettings.UpdateHideEffectIds`.
- **`GetIdByName(string name)`**: Returns effect ID by its name or `-1` if name not found.

Access to effects is provided via `GetEffect(hash)` and `GetEffect<T>(hash)` methods in the base `Chip` class.

### `IEffect`
Main contract for all effects:
- **`GetId()`**: Returns unique effect ID (from `EffectConsts`).
- **`Init(Chip chip, int effectId)`**: Initialization with chip reference and effect identifier.
- **`Activate(Chip chip) → bool`**: Activates the effect. Returns `false` if `effectId` is in `chip.BlockingState.HideEffectIds` (effect hidden by another blocking effect). On success calls `chip.BlockingState.ApplyBlock(BlockingSettings)`.
- **`Deactivate(Chip chip, bool force)`**: Deactivates the effect. If `force = true` forces immediate state change (useful for instant cleanup).
- **`SendTrigger(string triggerName, bool allowRepeat)`**: Sends an arbitrary trigger to Animator.
- **`OnChangedCell` / `OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`**: Handling cell (`ICell`) changes.
- **`OnMovingStateChanged(Chip chip, bool isMoving)`**: Handling movement state changes (start of drag or system move).
- **`TryDestroyEffect(Chip, EffectDestroyingSettings, EffectDestroyingRuntimeData) → bool`**: Checks if effect has reached destruction threshold.
- **`BlockingSettings`** (`EffectBlockingSettings`): Chip blocking configuration when effect is active. More details: [Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocking-system).
- **`DestroyingSettings`** (`EffectDestroyingSettings`): Effect destruction configuration on neighboring merges. More details: [Effect Destroying System](../Features/ChipEffectBlockers.md#effect-destroying-system).

###
For more details on blocking system (Blocking Settings, Combined Blocking State) see **[Chip Effect Blockers](../Features/ChipEffectBlockers.md)** document.

---

### `Effect.cs`
Base class for all effects. Implements `IEffect` and provides virtual methods for managing effect lifecycle:
- **`Init(Chip chip, int effectId)`**: Initializes effect, stores `effectId`, sets position depending on chip size, applies `AutoSizeType`, deactivates by default.
- **`Activate(Chip chip) → bool`**: Enables effect. If `effectId` is in `HideEffectIds`, calls `Deactivate` and returns `false`. On activation calls `chip.BlockingState.ApplyBlock(BlockingSettings)`.
- **`Deactivate(Chip chip, bool force = false)`**: Disables effect. If `force = true` — immediate state change via `animator.Play("Deactivate", -1, 1f)`.
- **`GetId()`**: Returns stored `effectId`.
- **`OnChangedCell(ICell sourceCell, ICell targetCell)`**: Called when chip moves. If `parentType` is set to `ParentCell`, effect rebinds to new cell.
- **`OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`**: Handling cell (`ICell`) changes during Drag-and-Drop.
- **`OnMovingStateChanged(Chip chip, bool isMoving)`**: Automatically hides effect when movement starts if `deactivateOnMove = true` is set, and restores state on stop.
- **`TryDestroyEffect(Chip, EffectDestroyingSettings, EffectDestroyingRuntimeData) → bool`**: If `NeighboringMergeCount` does not exceed the number of states in `AdditionalStates`, sends the corresponding additional state trigger; when all additional states are exhausted (or the array is empty) — deactivates the effect and returns `true`.

**EffectParentType** (enum):
- `ParentChip`: Effect is attached to chip transform.
- `ParentChipAnimationNode`: Effect is attached to `AnimationNode` child object (used for fly animations).
- `ParentCell`: Effect is attached to cell transform (follows cell, not chip).
- `ParentCellWithoutRotation`: Effect is attached to cell transform, but ignores its rotation (useful for UI elements above the cell).

**Movement Settings**:
- `deactivateOnMove`: If `true`, effect automatically deactivates during chip drag to reduce visual noise.
- `restoreStateAfterMove`: Internal flag for state restoration after movement completion.

**AutoSizeType** (enum):
- `None`: No autoscaling.
- `ScaleByChipSize`: `localScale = (chipSize.x, chipSize.y, 1)`.
- `ScaleByMaxChipSize`: `localScale = (max, max, 1)`.
- `ScaleByMinChipSize`: `localScale = (min, min, 1)`.
- `ScaleByAverageValue`: `localScale = (avg, avg, 1)`.
- `ScaleByAverageValueOnlyMinValue`: Scales only smaller dimension to average value.

**Additional Features**:
- **Animator Integration**: If `sendAnimatorTrigger` is configured, `Activate` and `Deactivate` methods automatically send `"Activate"` and `"Deactivate"` triggers to `Animator` component, and reset opposite triggers to prevent animation artifacts.
- **`SendTrigger(string triggerName, bool allowRepeat = false)`**: Allows sending arbitrary trigger to effect `Animator`. Used for special interactions, like animation when trying to drag a blocked chip (`MoveLocked`). `allowRepeat` parameter allows ignoring `dontRepeatTrigger` setting for specific calls.
- **`ResetTrigger(string triggerName)`**: Resets specified animation trigger in effect `Animator`. Called when switching animation states to prevent conflicts between opposite triggers (e.g., resets `"Deactivate"` before sending `"Activate"`). Safely handles case when `Animator` is `null`.

---

## Effect Destroying System

Effect destruction system allows effects to progressively break during neighboring merges.
For more details see **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#effect-destroying-system)** document.

---

## Implemented Effects

### 1. Merge Available
**Class**: `ChipMergeAvailableEffect.cs`
**Used in**: [Chip](../Chips/Chip.md)

Activated on "bottom" chip when player brings another compatible chip over it for merge.
- **Features**:
  - `autoPosition`: Centers effect relative to chip.
  - Uses `Animator` with `Activate` and `Deactivate` triggers.
  - Scaling is defined via `AutoSizeType` in base `Effect`.

### 2. Cell Highlight
**Class**: `CellHighlightEffect.cs`
**Used in**: [Chip](../Chips/Chip.md)

Dynamically creates and displays highlight under chip. Supports multi-cell chips by creating a highlight grid according to chip `Size`.
- **Parameters**:
  - `highlightPrefab`: Visual highlight element prefab.
  - `color`: Highlight color.
  - `order`: Z-axis offset for correct rendering over the field.
- **Shared Material**: First created highlight generates `Material` (via `SpriteRenderer.material`), all subsequent ones use `sharedMaterial` for shared color.
- **Architecture Adaptation**:
  - Uses `ICell` interface to determine target cells.
  - Implements blocked zone check via `IFieldGrid.HasBlockedCells`: if at least one cell under chip is blocked, highlight is hidden to visualize placement impossibility.
- **Extensibility**: Class is designed for inheritance and overriding:
  - `CreateHighlights()`: Virtual — base implementation creates grid by `chipSize`. Can be overridden for different geometry (e.g., [§ 6 Power Booster Connector Highlight](#6-power-booster-connector-highlight)).
  - `DestroyHighlights()`: Virtual — clears `highlights` list and `sharedMaterial`.
  - `CreateHighlight(Vector3)`: Virtual — creates one highlight element from prefab.
  - `OnChangedCell(ICell, ICell)`: Virtual — reacts to cell change (`gameObject` activation/deactivation).

### 3. Container Requirements
**Class**: `ChipContainerEffect.cs`
**Implements**: `IEffectContainer`
**Used in**: [ChipContainer](../Chips/ChipContainer.md)

Displays Panel with item icons required by the container for quest completion.

- **Components**:
  - `Animator`: Manages panel appearance (`Activate`) and disappearance (`Deactivate`) animation.
  - `LayoutForElements`: Container for item icons.
  - `PanelSpriteRenderer`: Background whose size adapts to number of elements.
- **Configuration (`ElementInfo`)**:
  - Effect contains an array of settings for different element counts (e.g., for 1 or 2 items).
  - Each entry configures panel width, layout group width, and icon scale.
- **Logic (`UpdateElements`)**:
  1. Gets current requirement list (`containers`).
  2. Finds matching `ElementInfo` based on requirement count.
  3. Adapts background and group size.
  4. Clears old elements and creates new prefabs (`ContainerElementPrefab`) for each requirement.
  5. If container is full (`isFull`), calls `ClearElements` and deactivates effect.

### 4. Generator Charged
**Used in**: [ChipGenerator](../Chips/ChipGenerator.md)

Visual effect activated when generator is fully charged and ready to create a new chip.
- **Activation**: When `IsCharged` becomes `true` (via `chargedEffect` field).
- **Deactivation**: After successful chip generation or during recharge process.
- **Type**: Uses base `Effect` class. Usually a cyclic idle animation (glow, pulse) showing object readiness for interaction.

### 5. Generator Charging
**Class**: `ChipGeneratorRechargeEffect.cs`
**Implements**: `IEffectGeneratorCharging`
**Used in**: [ChipGenerator](../Chips/ChipGenerator.md)

Visualizes generator recharge process. Usually implemented via `maskRectTransform` local position change, creating a fill effect from bottom to top.
- **`OnCharging(float progress)` method**: Receives value from 0 to 1 and updates visualization.

### 6. Power Booster Connector Highlight
**Class**: `PowerBoosterConnectorCellsHighlightEffect.cs` (inherits from `CellHighlightEffect`)
**Used in**: [ChipPowerBooster](../Chips/ChipPowerBooster.md)

Highlights cells observed by the booster (its `ObservedCellPositions`), showing the player the Power Booster influence zone.
- **Parameters**:
  - `globalAlpha`: Global effect transparency (`_Alpha` shader parameter).
  - `distractionAmount`: Visual distortion intensity (`_DistractionAmount` shader parameter).
  - `waitTimeBeforePowerEffect`: Wait time before first Power Effect animation launch.
- **Power Effect**: `StartPowerEffect` coroutine after `waitTimeBeforePowerEffect` seconds sends `"PowerBooster"` animation trigger to chip and effect, then restarts with doubled interval.
- **CreateHighlights()**: Overrides base method — creates highlights by `connectorCellPositions` (offsets relative to booster `originCellPosition`), not by `chipSize` grid.
- **OnChangedCell**: Overrides base — updates `originCellPosition` and `connectorCellPositions` from `CellSubscriber.ObservedCellPositions`, then recreates highlights.
- **Deactivate**: Stops Power Effect coroutine and resets `globalAlpha` to 0.
- **Update**: Every frame updates `_Alpha` and `_DistractionAmount` shader parameters on `sharedMaterial`.

### 7. Power Booster Join Links
**Class**: `PowerBoosterJoinEffect.cs` (inherits from `Effect`, implements `IEffectPowerBoosterJoin`)
**Used in**: [ChipPowerBooster](../Chips/ChipPowerBooster.md)

Responsible for dynamic particle links between booster and each active `IPowerBoosterTarget`.
- **Join API**: `ChipPowerBooster` calls `OnJoin`/`OnLeave` via `IEffectPowerBoosterJoin` contract (`EffectPowerBoosterJoinRef`) during boost add/remove.
- **JoinPoints**: Effect uses booster and target `JoinPoints`, selects nearest candidates, and then periodically rebinds active links via `ChangeJoinPointsCoroutine` (`changeJoinPointsTime`).
- **Cleanup**: `OnLeave` and `Deactivate` stop particle systems, schedule `Destroy` by `startLifetime`, and clear runtime links dictionary; `SetMoving(true)` in booster also calls `joinEffect.Deactivate(...)`.

### 8. Shadow Effect
**Class**: `ShadowEffect.cs`
**Used in**: [Chip](../Chips/Chip.md)

Provides permanent shadow under chip that dynamically reacts to movement state.
- **Features**:
  - `autoShadowSprite`: Automatically copies sprite from chip main renderer (if it is one).
  - `autoScale`: Adapts shadow scale to main renderer scale.
  - `autoSortingLayer`: Uses `AdditionallyWhenMoving` settings to raise shadow in sorting layers during movement.
- **Movement Reaction**: Unlike regular effects, shadow is not deactivated on movement. Instead, it sends `Activate` (on movement start) and `Deactivate` (on stop) triggers to its `Animator` for smooth visual transition (for example, increasing shadow offset).
- **Sorting**: During movement, shadow increases its `sortingOrder` to stay visually synchronized with the "raised" chip.

### 101. Box Effect
Box effect. For more details on setup and usage, see **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocker-effects-move-locked-chains-box)** document.

### 102. Chains Effect
Chains effect. For more details on setup and usage, see **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocker-effects-move-locked-chains-box)** document.

### 103. Move Locked Effect
Move lock effect. For more details on setup and usage, see **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocker-effects-move-locked-chains-box)** document.
