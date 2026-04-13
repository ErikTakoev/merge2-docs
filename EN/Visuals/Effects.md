# Visual Effects

[← Back to Main](../Main.md)

This document describes the visual effects system used to provide feedback to the player during interactions with chips on the game field.

## Base System

### `EffectConsts.cs`
Centralized registry of integer identifier constants for all effects in the system. Instead of storing references in separate effect fields, the system uses `Dictionary<int, IEffect>` with constant keys.

**Base effects**:

| Constant | ID | Description |
|---|---|---|
| [`MergeAvailable`](#1-merge-available) | 1 | Activates when chip is valid for merge |
| [`CellHighlight`](#2-cell-highlight) | 2 | Highlights cells under chip |
| [`ContainerRequirements`](#3-container-requirements) | 3 | Displays item icons required by container |
| [`GeneratorCharged`](#4-generator-charged) | 4 | Activates when generator is fully charged |
| [`GeneratorCharging`](#5-generator-charging) | 5 | Shows generator charging progress |
| [`PBoosterConnectorCells`](#6-power-booster-connector-highlight) | 6 | Highlights neighboring chips boosted by booster |
| [`PBoosterJoin`](#7-power-booster-join-links) | 7 | Dynamic links between booster and boost targets |
| [`ShadowEffect`](#8-shadow-effect) | 8 | Permanent shadow under chip that reacts to movement |

**Blocker effects (101+)** — nested class `EffectConsts.Blockers`:

| Constant | ID | Description |
|---|---|---|
| [`BoxEffect`](#101-box-effect) | 101 | Box effect |
| [`ChainsEffect`](#102-chains-effect) | 102 | Chains effect |
| [`MoveLockedEffect`](#103-move-locked-effect) | 103 | Movement lock effect |

**Utilities**:
- **`nameToId`** (`Dictionary<string, int>`): Dictionary for resolving string effect names into integer IDs. Used by `ExtraEffectData.EffectId` and `EffectBlockingSettings.UpdateHideEffectIds`.
- **`GetIdByName(string name)`**: Returns effect ID by name or `-1` if name not found.

Effects are accessed through `GetEffect(hash)` and `GetEffect<T>(hash)` methods in base `Chip` class.

### `IEffect`
Main contract for all effects:
- **`GetId()`**: Returns unique effect ID (from `EffectConsts`).
- **`Init(Chip chip, int effectId)`**: Initialization with reference to chip and effect identifier.
- **`Activate(Chip chip) -> bool`**: Effect activation. Returns `false` if `effectId` is in `chip.BlockingState.HideEffectIds` (effect is hidden by another blocking effect). On successful activation, calls `chip.BlockingState.ApplyBlock(BlockingSettings)`.
- **`Deactivate(Chip chip, bool force)`**: Effect deactivation.
- **`SendTrigger(string triggerName, bool allowRepeat)`**: Sends arbitrary trigger to Animator.
- **`OnChangedCell` / `OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`**: Cell change handling.
- **`OnMovingStateChanged(Chip chip, bool isMoving)`**: Handles movement state changes (drag start or system movement).
- **`TryDestroyEffect(Chip, EffectDestroyingSettings, EffectDestroyingRuntimeData) -> bool`**: Checks if effect reached destruction threshold.
- **`BlockingSettings`** (`EffectBlockingSettings`): Chip blocking configuration when effect is active. More details: [Chip Effect Blockers](../Features/ChipEffectBlockers.md#blocking-system).
- **`DestroyingSettings`** (`EffectDestroyingSettings`): Effect destruction configuration during neighboring merges. More details: [Effect Destroying System](../Features/ChipEffectBlockers.md#effect-destroying-system).

### 
For more details about blocking system (Blocking Settings, Combined Blocking State), see **[Chip Effect Blockers](../Features/ChipEffectBlockers.md)** document.

---

### `Effect.cs`
Base class for all effects. Implements `IEffect` and provides virtual methods for effect lifecycle management:
- **`Init(Chip chip, int effectId)`**: Initializes effect, stores `effectId`, configures position based on chip size, applies `AutoSizeType`, deactivates by default.
- **`Activate(Chip chip) -> bool`**: Enables effect. If `effectId` is in `HideEffectIds`, calls `Deactivate` and returns `false`. On activation, calls `chip.BlockingState.ApplyBlock(BlockingSettings)`.
- **`Deactivate(Chip chip, bool force)`**: Disables effect. With `force = true` - immediate state change via `animator.Play`.
- **`GetId()`**: Returns stored `effectId`.
- **`OnChangedCell(Cell sourceCell, Cell targetCell)`**: Called when chip moves. If `parentType` is set to `ParentCell`, effect is rebound to new cell.
- **`OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`**: Handling cell changes during Drag-and-Drop.
- **`OnMovingStateChanged(Chip chip, bool isMoving)`**: Automatically hides effect on movement start if `deactivateOnMove = true`, and restores state on stop.
- **`TryDestroyEffect(Chip, EffectDestroyingSettings, EffectDestroyingRuntimeData) -> bool`**: If `NeighboringMergeCount` is below threshold, sends progressive trigger (for example, `"Hit_1"`, `"Hit_2"`); if threshold is reached - deactivates effect and returns `true`.

**EffectParentType** (enum):
- `ParentChip`: Effect is attached to chip transform.
- `ParentChipAnimationNode`: Effect is attached to child object `AnimationNode` (used for fly animations).
- `ParentCell`: Effect is attached to cell transform (follows cell, not chip).

**Movement settings**:
- `deactivateOnMove`: If `true`, effect is automatically deactivated while chip is dragged to reduce visual noise.
- `restoreStateAfterMove`: Internal flag for state restoration after movement ends.

**AutoSizeType** (enum):
- `None`: No auto scaling.
- `ScaleByChipSize`: `localScale = (chipSize.x, chipSize.y, 1)`.
- `ScaleByMaxChipSize`: `localScale = (max, max, 1)`.
- `ScaleByMinChipSize`: `localScale = (min, min, 1)`.
- `ScaleByAverageValue`: `localScale = (avg, avg, 1)`.
- `ScaleByAverageValueOnlyMinValue`: Scales only smaller dimension to average value.

**Additional capabilities**:
- **Animator Integration**: If `sendAnimatorTrigger` is configured, `Activate` and `Deactivate` methods automatically send `"Activate"` and `"Deactivate"` triggers to `Animator` component, and also reset opposite triggers to prevent animation artifacts.
- **`SendTrigger(string triggerName, bool allowRepeat = false)`**: Allows sending an arbitrary trigger to effect `Animator`. Used for special interactions such as animation when trying to drag a locked chip (`MoveLocked`). `allowRepeat` parameter allows ignoring `dontRepeatTrigger` setting for specific calls.
- **`ResetTrigger(string triggerName)`**: Resets specified animation trigger in effect `Animator`. Called while switching animation states to prevent conflicts between opposite triggers (for example, resets `"Deactivate"` before sending `"Activate"`). Safely handles case when `Animator` is `null`.

---

## Effect Destroying System

Effect destroying system allows effects to be progressively destroyed during neighboring merges. 
For more details, see **[Chip Effect Blockers](../Features/ChipEffectBlockers.md#effect-destroying-system)** document.

---

## Implemented Effects

### 1. Merge Available
**Class**: `ChipMergeAvailableEffect.cs`
**Used in**: [Chip](../Chips/Chip.md)

Activates on the chip that is "below" when player moves another chip above it, and merge is possible.
- **Features**:
  - `autoPosition`: Centers effect relative to chip.
  - Uses `Animator` with `Activate` and `Deactivate` triggers.
  - Scaling is defined via `AutoSizeType` in base `Effect`.

### 2. Cell Highlight
**Class**: `CellHighlightEffect.cs`
**Used in**: [Chip](../Chips/Chip.md)

Dynamically creates and displays highlight under chip. Supports multi-cell chips by creating a highlight grid according to chip `Size`.
- **Parameters**:
  - `highlightPrefab`: Prefab of visual highlight element.
  - `color`: Highlight color.
  - `order`: Z-axis offset for correct rendering above field.
- **Shared Material**: First created highlight generates `Material` (via `SpriteRenderer.material`), all subsequent ones use `sharedMaterial` for shared color.
- **Extensibility**: Class is designed for inheritance and override:
  - `CreateHighlights()`: Virtual - base implementation creates grid by `chipSize`. Can be overridden for different geometry (for example, [§ 6 Power Booster Connector Highlight](#6-power-booster-connector-highlight)).
  - `DestroyHighlights()`: Virtual - clears `highlights` list and `sharedMaterial`.
  - `CreateHighlight(Vector3)`: Virtual - creates one highlight element from prefab.
  - `OnChangedCell(Cell, Cell)`: Virtual - reacts to cell change (`gameObject` activation/deactivation).

### 3. Container Requirements
**Class**: `ChipContainerEffect.cs`
**Implements**: `IEffectContainer`
**Used in**: [ChipContainer](../Chips/ChipContainer.md)

Displays a panel with icons of items required by container to complete quest.

- **Components**:
  - `Animator`: Controls panel appearance (`Activate`) and disappearance (`Deactivate`) animation.
  - `LayoutForElements`: Container for item icons.
  - `PanelSpriteRenderer`: Background whose size adapts to number of elements.
- **Configuration (`ElementInfo`)**:
  - Effect contains settings array for different element counts (for example, for 1 or 2 items).
  - Each entry configures panel width, layout group width, and icon scale.
- **Logic (`UpdateElements`)**:
  1. Gets current requirements list (`containers`).
  2. Finds matching `ElementInfo` based on requirements count.
  3. Adapts background and group size.
  4. Clears old elements and creates new prefabs (`ContainerElementPrefab`) for each requirement.
  5. If container is full (`isFull`), calls `ClearElements` and deactivates effect.

### 4. Generator Charged
**Used in**: [ChipGenerator](../Chips/ChipGenerator.md)

Visual effect that activates when generator is fully charged and ready to create a new chip.
- **Activation**: When `IsCharged` becomes `true` (through `chargedEffect` field).
- **Deactivation**: After successful chip generation or during recharge process.
- **Type**: Uses base `Effect` class. Usually this is a cyclic idle animation (glow, pulse) that shows player object readiness for interaction.

### 5. Generator Charging
**Class**: `ChipGeneratorRechargeEffect.cs`
**Implements**: `IEffectGeneratorCharging`
**Used in**: [ChipGenerator](../Chips/ChipGenerator.md)

Visualizes generator recharge process. Usually implemented through changing local position of mask (`maskRectTransform`), creating effect of icon filling from bottom to top.
- **`OnCharging(float progress)` method**: Receives value from 0 to 1 and updates visualization.

### 6. Power Booster Connector Highlight
**Class**: `PowerBoosterConnectorCellsHighlightEffect.cs` (inherits `CellHighlightEffect`)
**Used in**: [ChipPowerBooster](../Chips/ChipPowerBooster.md)

Highlights cells observed by booster (`ObservedCellPositions`), showing player Power Booster influence zone.
- **Parameters**:
  - `globalAlpha`: Global effect transparency (shader parameter `_Alpha`).
  - `distractionAmount`: Visual distortion intensity (shader parameter `_DistractionAmount`).
  - `waitTimeBeforePowerEffect`: Wait time before first Power Effect animation run.
- **Power Effect**: `StartPowerEffect` coroutine, after `waitTimeBeforePowerEffect` seconds, sends `"PowerBooster"` animation trigger to chip and effect, then restarts with doubled interval.
- **CreateHighlights()**: Overrides base method - creates highlights by `connectorCellPositions` (offset relative to booster `originCellPosition`), not by `chipSize` grid.
- **OnChangedCell**: Overrides base - updates `originCellPosition` and `connectorCellPositions` from `CellSubscriber.ObservedCellPositions`, then recreates highlights.
- **Deactivate**: Stops Power Effect coroutine and resets `globalAlpha` to 0.
- **Update**: Every frame updates shader parameters `_Alpha` and `_DistractionAmount` on `sharedMaterial`.

### 7. Power Booster Join Links
**Class**: `PowerBoosterJoinEffect.cs` (inherits `Effect`, implements `IEffectPowerBoosterJoin`)
**Used in**: [ChipPowerBooster](../Chips/ChipPowerBooster.md)

Responsible for dynamic particle links between booster and each active `IPowerBoosterTarget` target.
- **Join API**: `ChipPowerBooster` calls `OnJoin`/`OnLeave` through `IEffectPowerBoosterJoin` contract (`EffectPowerBoosterJoinRef`) while adding/removing boost.
- **JoinPoints**: Effect uses booster and target `JoinPoints`, selects nearest candidates, then periodically rebinds active links through `ChangeJoinPointsCoroutine` coroutine (`changeJoinPointsTime`).
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
