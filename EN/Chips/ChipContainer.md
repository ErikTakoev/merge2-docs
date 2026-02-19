# ChipContainer (Container)

[← Back to Main](../Main.md)

`ChipContainer` is an interactive chip that accepts other chips to complete quests or orders. It inherits from the base `Chip` class and extends it with the ability to accumulate items.

## Architecture and Responsibility

### 1. `ChipContainer.cs` (Container)
The `ChipContainer` class stores fill state and manages visual effects.
- **Properties (Data)**:
  - `ChipContainerData`: Container configuration (list of required items `containers`, reward `NextChipData`).
- **Events and Delegates**:
  - `FillContainerDelegate`: Delegate for container state update event.
  - `OnFillContainer`: Event called on initialization, item addition, or complete fill.
- **Effects**:
  - **[ChipContainerEffect](../Visuals/Effects.md#4-chip-container-container-requirements)**: Special effect that visualizes required items ("bubbles") above the container.

### 1.1. `ChipContainerRuntimeData.cs` (Runtime State)
Extends base `ChipRuntimeData` to store dynamic container state.

**Inherited properties** (from `ChipRuntimeData`):
- **`IsMoveLocked`**: Determines whether the container's movement is blocked. For details, see [Chip Runtime Data](Chip.md#runtime-data-in-game-state).

**Own properties**:
- **`containers`**: Dictionary (`Dictionary<ContainerInfo, int>`) tracking the current fill progress for each requirement.
  - **Key (`ContainerInfo`)**: Requirement definition (item type, ID, required quantity).
  - **Value (`int`)**: Current number of collected items for this requirement.
  - **Completion**: When a requirement is fully met (counter reaches `ContainerInfo.Count`), it is removed from the dictionary. When the dictionary is empty — the container is considered full.

**Initialization**: Created in `ChipContainer.Init()` based on `ChipContainerData`. Initial counter values are set to 0.

### 2. `FillContainerLogic.cs` (Logic)
Manages the interaction process between player, chip, and container. For more details on the general interaction system, see **[FillContainerLogic](../Interactions/FillContainerLogic.md)**.
- **Validation (`CanInteract`)**:
  - Checks whether the target object is a container (`ChipContainer`).
  - Calls `chipContainer.IsChipCompatible(sourceChip)` to determine if the chip suits current needs.
- **Execution (`ExecuteInteraction`)**:
  - Calls `chipContainer.TryAddChip(sourceChip)`.
  - On success, calls `sourceCell.Chip.Destroy(sourceCell)` to remove the added chip.

## Key Methods (`ChipContainer.cs`)

### `IsChipCompatible(Chip chip)`
Checks whether the chip's `Type` or `Name` matches at least one active requirement in `containerRuntimeData.containers`. Does not change state.

### `TryAddChip(Chip chip)`
Main chip addition method:
1. Finds the matching requirement in the `containers` dictionary.
2. If the counter reaches the required quantity:
   - Removes the requirement from the dictionary.
   - Sends the `Recharge` animation trigger.
   - Calls `OnFillContainer` with the `isFull` flag.
   - **Completion**: If all requirements are met, the container is destroyed via `this.Destroy(cell)`, and `NextChipData` is created in its place using `ChipFactory`.
3. If the counter is not yet full — simply increments its value.
4. **Result**: Returns `true` if the chip was successfully accepted.

## Visual Effects

### ChipContainerEffect.cs
Specialized effect for visualizing container requirements. Manages a Panel above the chip where required items are displayed.

For a detailed implementation description, see [Effects.md](../Visuals/Effects.md#4-chip-container-container-requirements).

## Flow

1. **Input**: Player starts dragging a chip.
2. **Move**: System constantly checks compatibility through `FillContainerLogic.CanInteract` when hovering over containers.
3. **Check**: Container confirms through `IsChipCompatible` that it needs the chip.
4. **Drop**: On release, `ExecuteInteraction` is called -> `TryAddChip`.
5. **Update**: Container removes/changes requirements, `ChipContainerEffect` updates "bubbles" (or hides them if requirement is met).
6. **Complete**: If no requirements remain, the container is replaced with a new chip (result).
