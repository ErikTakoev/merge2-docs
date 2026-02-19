# Chip (Base Chip)

[← Back to Main](../Main.md)

The base class `Chip` is a visual representation and interaction component for objects on the game field. It is responsible for displaying state, effects, and handling Unity events (Input).

The merge logic and movement are extracted into respective logic classes.

## Architecture and Responsibility

### 1. `Chip.cs` (Base Class)
The `Chip` class is a visual representation and basic component.
- **Data (Configuration - `ChipData`)**: Stores a reference to `ChipData`, which contains settings:
  - **Type**: Chip type identifier (string).
  - **PrefabLink**: Reference to the chip prefab.
  - **Size**: Chip size in cells (Vector2Int).
  - **MergeData**: Optional merge settings (`ChipMergeData`). For more details about the merge process, see **[MergeableChipLogic](../Interactions/MergeableChipLogic.md)**.
  - **ChipContainerData**: Optional container settings.
  - **ChipGeneratorData**: Optional generator settings.
  - **OtherData**: Additional data (ScriptableObject).
- **Runtime Properties**:
  - **CellPosition**: Current position of the chip on the field grid (Vector2Int). Updated by the system when moved.
  - **RuntimeData**: Current state (see below).
  - **LogEnable**: Flag to enable logging of chip events to console.
- **Effects**: Manages visual effects, for details see [Visual Effects](../Visuals/Effects.md):
  - `MergeAvailableEffect`: Highlight when merge is possible ([ChipMergeAvailableEffect](../Visuals/Effects.md#2-merge-available-merge-available)).
  - `CellHighlightEffect`: Highlight of the cell under the chip ([CellHighlightEffect](../Visuals/Effects.md#1-cell-highlight-cell-highlighting)).
  - `MoveLockedEffect`: Visual indicator of movement blocking ([Move Locked](../Visuals/Effects.md#6-move-locked-move-locking)).
- **Animation**: Has a reference to `Animator` for playing states (e.g., `Merge`, `Generate`, `MoveLocked`).

## Effect Management

The base `Chip` class automatically tracks all visual effects belonging to it for proper event distribution and cleanup on destruction.

### Effect Initialization
- **`InitEffects()`**: Virtual method called from `Init(ChipData)` to initialize all effects. The base implementation creates and adds standard effects (CellHighlight, MergeAvailable, MoveLocked). This method is designed for overriding in derived classes that may add their own specialized effects. Example: `ChipGenerator` overrides this method to add `ChargedEffect` and `RechargeEffect`.

### Effect List Management
- **`effects` (List<Effect>)**: List of all active chip effects. Used for iteration when changing cell state or interaction.
- **`InstantiateEffect<T>(GameObject prefab)`**: Helper method for creating effects from prefabs. It automatically instantiates the object and calls `Init(this)`.

### Effect Lifecycle
- All effects added to the `effects` list automatically receive notifications through methods `OnChangedCell`, `OnInteractionOverCellChanged`, and `OnInteractionUnderCellChanged`.
- When `Destroy(Cell)` is called, all effects in the `effects` list are also destroyed.

## Runtime Data (In-Game State)

### `ChipRuntimeData`
Base class for storing runtime state of chips. Located in the `RuntimeData` folder. Contains dynamic properties that change during gameplay:
- **`IsMoveLocked`**: Determines whether the chip's movement is blocked. When set to `true`, the player cannot drag this chip.

### Runtime Data Methods
- **`UpdateVisual()`**: Virtual method for synchronizing the chip's visual state with its `RuntimeData`. Automatically activates/deactivates `MoveLockedEffect` based on the `IsMoveLocked` property.
- **`RuntimeData` (Property)**: Provides access to the `ChipRuntimeData` object for reading and modifying dynamic properties.
- **`CanMoving()`**: Virtual method that checks whether the chip can be moved. Returns `false` if `IsMoveLocked` is set to `true`.
- **`ChipData.Clone()`**: Allows creating a copy of the chip's configuration at runtime if you need to change parameters for a specific instance.

### Input and Interaction List Management
`Chip` contains virtual methods for handling events called by control logic (e.g., `DraggableChipLogic`):
- **`OnTap(Vector2 position)`**: Called on a short press.
- **`OnDragStart(Vector2 position)`**: Start of dragging.
- **`OnDrag(Vector2 position, Cell anchorCell)`**: Dragging process.
- **`OnDragEnd(Vector2 position)`**: End of dragging.

### Movement State Management
The system distinguishes between **user dragging state** and **visual movement state**:

#### User Drag State
- **`SetDragging(bool)`**: Sets the user dragging state. Called by `DraggableChipLogic` at the start/end of dragging. Automatically calls `SetMoving(true)` if needed.
- **`IsDragging()`**: Checks whether the chip is being dragged by the user. Tracks user interaction specifically, not just visual movement.

#### Visual Movement State
- **`SetMoving(bool)`**: Manages the visual movement state (changes `sortingOrder`). Called both when dragged by the user and when moved automatically by the system. Calls `UpdateVisual()` on completion.
- **`IsMoving()`**: Checks the visual movement state (by `sortingOrder`). Returns `true` for both user dragging and system movement.

### Other Methods
- **`OnDraggingChipWithMoveLocked()`**: Virtual method called when attempting to drag a locked chip. Plays the `MoveLocked` animation on the chip and its lock effect, providing visual feedback to the player.

### Extensions for Specialized Chips
- **`ChipGeneratorRuntimeData`**: Adds charging state, timers, recharge counters.
- **`ChipContainerRuntimeData`**: Adds a dictionary for container fill progress (`containers`).


### 2. `MergeableChipLogic.cs` (Merge Logic)
Implements the `IChipInteractionLogic` interface. Responsible for checking merge possibility and executing the operation.
- **Validation (`CanInteract`)**:
  - Uses `MergeData` of the initiating chip (source) to check via `CanMerge` whether the target chip is in the list of allowed partners.
- **Execution (`ExecuteInteraction`)**:
  - Determines the merge result using the `GetNextChip` method of the `MergeData` object.
  - Takes into account weights (Weighted Random) if multiple results are provided for a given pair.
  - Handles situations where the new chip is larger than the originals (shifting neighboring chips via `IChipMovingLogic`).
  - Destroys both original chips.
  - Creates a new chip via `ChipFactory`.
  - Starts the `Merge` animation.
  - **Extra Chips**: If `MergeResult` contains `ExtraChip`, the system checks the chance of its drop and spawns an additional chip (using `FieldChipData` for identification) on the nearest free cell.

### 3. `DraggableChipLogic.cs` (Control)
Handles player Input (Drag-and-Drop). Coordinates interaction between chips by calling `IChipInteractionLogic` (e.g., `MergeableChipLogic`) to check and execute actions.

## Merge Rules (ChipMergeData)

`ChipMergeData` is a settings object that defines who and how a chip can merge with.

### Data Structure

1. **`MergeCombination`**: Defines rules for a specific pair of chips. Contains a reference to the **`TargetChip`** (partner) and a list of possible results (**`Results`**).
2. **`MergeResult`**: Describes the merge result (**`Result`**), its **Weight**, and possible **`ExtraChip`**.
3. **`ExtraChip`**: Describes an additional chip (via `FieldChipData`) and its drop chance (**Chance**).

### Result Calculation

During merging, the system uses the **Weighted Random** algorithm:
- Collects all possible results for a given partner.
- Based on their weights, selects one final chip.
- If there is only one variant with a weight of 100, it is guaranteed.

### Automation in the Editor

For convenience, `ChipCreatorWindow` automatically initializes a basic rule when Merge is enabled: creates a record of the chip merging with itself, where the result is itself.

## Merge Flow

1. **Input**: Player drags a chip (`DraggableChipLogic`).
2. **Move**: When moving over another chip, `CanInteract` is called.
3. **Feedback**: If `CanInteract` = true, the merge available effect is activated on the lower chip (`mergeAvailableEffect`).
4. **Drop**: When the chip is released, `ExecuteInteraction` is called.
5. **Result**: The system selects the result, destroys old chips, and creates a new one via `ChipFactory`.
