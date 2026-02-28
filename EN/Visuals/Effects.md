# Visual Effects

[← Back to Main](../Main.md)

This document describes the visual effect system used to provide feedback to the player during interaction with chips on the game field.

## Base System

### `Effect.cs`
Base class for all effects. Implements `IEffect` and provides virtual methods for managing effect lifecycle:
- **`Init(Chip chip)`**: Initializes the effect, configures position based on chip size, deactivates by default.
- **`Activate(Chip chip)`**: Enables the effect object.
- **`Deactivate(Chip chip)`**: Disables the effect object.
- **`OnChangedCell(Cell sourceCell, Cell targetCell)`**: Called when a chip moves.
- **`OnInteractionOverCellChanged` / `OnInteractionUnderCellChanged`**: Handles cell change during Drag-and-Drop.

**Interfaces**:
- **`IEffect`**: Core contract for all effects (`Activate`, `Deactivate`, `Init`, etc.).
- **`IEffectContainer`**: Extends `IEffect` for container-specific visuals.
- **`IEffectGeneratorCharging`**: Extends `IEffect` for charging progress visuals.

**Additional Features**:
- **Animator Integration**: If `sendAnimatorTrigger` is configured, `Activate` and `Deactivate` methods automatically send `"Activate"` and `"Deactivate"` triggers to the `Animator` component, and reset opposite triggers to prevent animation artifacts.
- **`SendTrigger(string triggerName, bool allowRepeat = false)`**: Allows sending an arbitrary trigger to the effect's `Animator`. Used for special interactions, such as animation when attempting to drag a locked chip (`MoveLocked`). The `allowRepeat` parameter allows bypassing the `dontRepeatTrigger` setting for specific calls.
- **`ResetTrigger(string triggerName)`**: Resets the specified animation trigger in the effect's `Animator`. Called when switching animation states to prevent conflicts between opposite triggers (e.g., resets `"Deactivate"` before sending `"Activate"`). Safely handles the case when `Animator` is `null`.

---

## Implemented Effects

### 1. Cell Highlight (Cell Highlighting)
**Class**: `CellHighlightEffect.cs`
**Used in**: [Chip](../Chips/Chip.md)

Dynamically creates and displays highlighting under the chip. Supports multi-cell chips by creating a grid of highlights corresponding to the chip's size.
- **Parameters**:
  - `highlightPrefab`: Prefab of the visual highlight element.
  - `color`: Highlight color.
  - `order`: Z-offset for correct rendering above the field.

### 2. Merge Available (Merge Available)
**Class**: `ChipMergeAvailableEffect.cs`
**Used in**: [Chip](../Chips/Chip.md)

Activated on the chip that is "below" when the player brings another chip with which merge is possible over it.
- **Features**:
  - `autoSize`: Automatically scales the effect to the chip's size.
  - `autoPosition`: Centers the effect relative to the chip.
  - Uses `Animator` with `Activate` and `Deactivate` triggers.

### 3. Chip Generator Recharge (Progress Generator)
**Class**: `ChipGeneratorRechargeEffect.cs`
**Implements**: `IEffectGeneratorCharging`
**Used in**: [ChipGenerator](../Chips/ChipGenerator.md)

Visualizes generator recharge progress. Usually implemented through changes to the local position of a mask (`maskRectTransform`), creating a fill-from-bottom effect.
- **Method `OnCharging(float progress)`**: Receives a value from 0 to 1 and updates the visualization.

### 4. Chip Container (Container Requirements)
**Class**: `ChipContainerEffect.cs`
**Implements**: `IEffectContainer`
**Used in**: [ChipContainer](../Chips/ChipContainer.md)

Displays a Panel with icons of items needed for the container to complete a quest.

- **Components**:
  - `Animator`: Controls animation of panel appearance (`Activate`) and disappearance (`Deactivate`).
  - `LayoutForElements`: Container for item icons.
  - `PanelSpriteRenderer`: Background that adapts in size to the number of elements.
- **Configuration (`ElementInfo`)**:
  - The effect contains an array of settings for different element counts (e.g., for 1 or 2 items).
  - Each record sets panel width, layout group width, and icon scale.
- **Logic (`UpdateElements`)**:
  1. Gets the current list of requirements (`containers`).
  2. Finds the matching `ElementInfo` based on the number of requirements.
  3. Adapts background size and group size.
  4. Clears old elements and creates new `ContainerElementPrefab` for each requirement.
  5. If the container is full (`isFull`), calls `ClearElements` and deactivates the effect.

### 5. Generator Charged (Generator Readiness)
**Used in**: [ChipGenerator](../Chips/ChipGenerator.md)

Visual effect activated when the generator is fully charged and ready to create a new chip.
- **Activation**: When `IsCharged` becomes `true` (via `chargedEffect` field).
- **Deactivation**: After successful chip generation or during the recharge process.
- **Type**: Uses the base `Effect` class. Usually a cyclic idle animation (glow, pulsation) that shows the player the object is ready to interact.

### 6. Move Locked (Move Locking)
**Class**: `Effect` (base)
**Used in**: [Chip](../Chips/Chip.md)

Visual effect displayed on a chip when its movement is blocked.
- **Activation**: 
  - When `ChipRuntimeData.IsMoveLocked` is set to `true` via `UpdateVisual()`.
  - Automatically handled in `Chip.UpdateVisual()`.
- **Deactivation**: 
  - When `IsMoveLocked` becomes `false`.
  - Automatically handled in `Chip.UpdateVisual()`.
- **Behavior**:
  - Uses the base `Effect` class with Animator triggers support (`Activate`/`Deactivate`).
  - Usually implemented as a lock icon or other visual indicator showing the player the chip cannot be moved.
  - Movement possibility checking is performed via the `Chip.CanMoving()` method, used in `DraggableChipLogic`.
  - **Interactive Feedback**: When attempting to drag a locked chip, `Chip.OnDraggingChipWithMoveLocked()` is called, which plays the `MoveLocked` animation via the `Effect.SendTrigger()` method. This allows showing feedback to the player even if they try to move the locked chip.
- **Settings**:
  - Effect prefab is set in `ChipData.MoveLockedEffectPrefab`.
  - Created automatically in `Chip.InitEffects()` if the prefab is specified.
