# Namespace: Merge2

## Table of Contents
- [Cell](#cell)
- [CellHighlightEffect](#cellhighlighteffect)
- [Chip](#chip)
- [ChipContainer](#chipcontainer)
- [ChipContainerData](#chipcontainerdata)
- [ChipContainerEffect](#chipcontainereffect)
- [ChipData](#chipdata)
- [ChipDataCollection](#chipdatacollection)
- [ChipFactory](#chipfactory)
- [ChipFlyAnimation](#chipflyanimation)
- [ChipGenerator](#chipgenerator)
- [ChipGeneratorData](#chipgeneratordata)
- [ChipGeneratorEffect](#chipgeneratoreffect)
- [ChipGeneratorRuntimeData](#chipgeneratorruntimedata)
- [ChipMergeAvailableEffect](#chipmergeavailableeffect)
- [ChipMergeData](#chipmergedata)
- [ChipMovingLogic](#chipmovinglogic)
- [ContainerInfo](#containerinfo)
- [DraggableChipLogic](#draggablechiplogic)
- [Effect](#effect)
- [FieldData](#fielddata)
- [FieldEventHandler](#fieldeventhandler)
- [FieldGrid](#fieldgrid)
- [FieldInitializeCommand](#fieldinitializecommand)
- [FillContainerLogic](#fillcontainerlogic)
- [FreeCellFinder](#freecellfinder)
- [IChipFlyAnimation](#ichipflyanimation)
- [IChipInteractionLogic](#ichipinteractionlogic)
- [IChipMovingLogic](#ichipmovinglogic)
- [IFieldEventHandler](#ifieldeventhandler)
- [IFieldGrid](#ifieldgrid)
- [IFieldInitializeCommand](#ifieldinitializecommand)
- [IFreeCellFinder](#ifreecellfinder)
- [Merge2Initializer](#merge2initializer)
- [Merge2LifetimeScope](#merge2lifetimescope)
- [MergeableChipLogic](#mergeablechiplogic)
- [MergeData](#mergedata)
- [MergeNextChipData](#mergenextchipdata)

---

## Cell
**Inherits**: `MonoBehaviour`

> - **Purpose**: Represents a single cell in the game grid that can hold a chip.
> - **Usage**: Attach to grid cell GameObjects
> - used to manage chip placement and interaction.
> - **Notes**: Acts as a container for a chip and handles chip movement/interaction logic.
#### Fields
- `++ Chip: Chip`
- `+ MainCell: Cell`
- `+- CellPosition: Vector2Int`
- `- field: IFieldEventHandler`
- `- flyAnimation: IChipFlyAnimation`
- `- logEnable: bool`
- `- movingTime: float`
#### Methods
- `+ GetLocalPositionForChip(): Vector3`
    - **Purpose**: Gets the center position of the chip in this cell.
    - **Usage**: Call to determine where to place the chip inside the cell.
    - **Returns**: Vector3 representing the local center position for the chip.
- `+ Init(Vector2Int cellPos): void`
    - **Purpose**: Initializes the cell with its grid position.
    - **Usage**: Call once after instantiating the cell to set its logical position in the grid.
    - **Params**: cellPos - grid coordinates for this cell.
- `+ OnDrag(Vector2 position, bool isValidPosition): void`
    - **Purpose**: Handles chip movement during dragging from this cell.
    - **Usage**: Call continuously while a chip is being dragged from this cell.
    - **Params**: position - current drag position
    - isValidPosition - whether the position is valid for dropping.
- `+ OnDragEnd(Vector2 position): void`
    - **Purpose**: Handles the end of a drag interaction for the chip from this cell.
    - **Usage**: Call when the user releases a dragged chip from this cell.
    - **Params**: position - screen or world position where drag ended.
- `+ OnDragStart(Vector2 position): void`
    - **Purpose**: Handles the start of a drag interaction on the chip in this cell.
    - **Usage**: Call when the user begins dragging a chip from this cell.
    - **Params**: position - screen or world position where drag started.
- `+ OnTap(Vector2 position): void`
    - **Purpose**: Handles tap interaction on the chip in this cell.
    - **Usage**: Call when the user taps on this cell
    - delegates to the chip if present.
    - **Params**: position - screen or world position of the tap.
- `- Update(): void`
---

## CellHighlightEffect
**Inherits**: `Effect`
#### Fields
- `- color: Color`
- `- highlightPrefab: GameObject`
- `- highlights: GameObject[]`
- `- order: float`
#### Methods
- `+ Activate(Chip chip): void`
- `+ Deactivate(Chip chip): void`
- `+ OnChangedCell(Cell sourceCell, Cell targetCell): void`
- `+ OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell interactableCell): void`
- `+ UpdateCell(Cell sourceCell, Cell targetCell): void`
    - **Purpose**: Updates the currently highlighted cell and shows all highlights
    - **Usage**: Call when the highlighted cell changes to update the highlight effect
    - **Params**: sourceCell - previous cell
    - targetCell - new cell to highlight
    - **Notes**: Shows all highlights without any cutting/clipping logic
- `- CreateHighlights(Vector2Int chipSize): void`
- `- DestroyHighlights(): void`
- `- ShowAllHighlights(): void`
---

## Chip
**Inherits**: `MonoBehaviour`

> - **Purpose**: Base class for all chips in the game, handles core chip logic including dragging, highlighting, and effect management
> - **Usage**: Inherit for all chip types
> - provides virtual methods for interaction and effect handling
> - attach to chip GameObjects
> - **Notes**: Extend and override virtual methods for custom chip behavior
> - manages chip effects and animation triggers
#### Fields
- `++ CellPosition: Vector2Int`
- `+- Data: ChipData`
- `+- LogEnable: bool`
- `- animator: Animator`
- `- effects: Effect[]`
- `~ fieldGrid: IFieldGrid`
- `- mergeAvailableEffect: Effect`
- `- sorting: SortingGroup`
#### Methods
- `+ Destroy(Cell mainCell): void`
    - **Purpose**: Destroys the chip and all its attached effects
    - **Usage**: Call to remove the chip from the scene and clean up its effects
    - override in derived classes for custom destruction logic
- `+ Init(ChipData data): void`
    - **Purpose**: Initializes the chip with data and sets up all required components and effects
    - **Usage**: Call after creating a chip instance to assign data and prepare effects
    - override in derived classes for custom initialization
    - **Params**: data - the data object containing chip configuration and effect prefabs
    - **Notes**: Creates highlight and merge-available effects if prefabs are provided
    - logs errors if required components are missing
- `+ IsDragging(): bool`
- `+ OnChangedCell(Cell sourceCell, Cell targetCell): void`
    - **Purpose**: Called when the chip is moved to a new cell
    - updates all attached effects accordingly
    - **Usage**: Call after the chip's parent cell has changed
    - propagates cell change to all effects
    - **Params**: sourceCell - the cell the chip was previously in
    - targetCell - the cell the chip is moved to
- `+ OnDrag(Vector2 position, Cell anchorCell): void`
    - **Purpose**: Called during dragging to update chip's highlight or position
    - **Usage**: Override in derived classes to implement custom drag behavior
    - called continuously during drag
    - **Params**: position - current world position
    - anchorCell - the cell used as anchor during drag
- `+ OnDragEnd(Vector2 position): void`
    - **Purpose**: Called when drag ends on this chip
    - **Usage**: Override in derived classes to implement custom drag end behavior
    - receives drag end position in world coordinates
    - deactivates merge available effect by default
    - **Params**: position - the world position where the drag ended
    - **Notes**: Deactivates merge available effect after drag ends
- `+ OnDragStart(Vector2 position): void`
    - **Purpose**: Called when drag starts on this chip
    - **Usage**: Override in derived classes to implement custom drag start behavior
    - receives drag start position in world coordinates
    - **Params**: position - the world position where the drag started
- `+ OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell underCell): void`
    - **Purpose**: Notifies all chip effects when the dragged chip moves between cells
    - **Usage**: Called by DraggableChipLogic when chip position changes during drag to update visual effects
    - **Params**: prevCell - previous cell position
    - currentCell - new cell position
    - underCell - cell currently under the chip
    - **Notes**: Propagates cell change event to all attached effects like highlights, merge indicators, etc.
- `+ OnInteractionUnderCellChanged(Cell underCell, Cell overCell): void`
- `+ OnTap(Vector2 position): void`
    - **Purpose**: Called when the chip is tapped by the user
    - **Usage**: Override in derived classes to implement custom tap behavior
    - receives tap position in world coordinates
    - **Params**: position - the world position where the chip was tapped
- `+ SendTrigger(AnimatorTrigger trigger): void`
- `+ SendTrigger(string trigger): void`
- `+ SetDragging(bool value): void`
---

## ChipContainer
**Inherits**: `Chip`

> - **Purpose**: Represents a chip that can contain other chips, managing their addition and triggering effects when filled.
> - **Usage**: Use as a base class for chips that act as containers for other chips
> - Call Init with ChipData to initialize
> - Use TryAddChip to attempt to add a chip.
> - **Params**: data - ChipData for initialization
> - chip - Chip to check or add.
> - **Returns**: IsChipCompatible and TryAddChip return true if the chip can be added or is compatible, false otherwise.
> - **Notes**: Handles container fill logic and triggers effects when full
> - Removes itself and spawns a new chip if all containers are filled
> - Uses event OnFillContainer for notification
> - Requires proper ChipData with ChipContainerData.
#### Fields
- `- chipFactory: ChipFactory`
- `- containerEffect: ChipContainerEffect`
- `~ containers: Dictionary<ContainerInfo, int>`
- `- OnFillContainer: FillContainerDelegate`
#### Methods
- `+ Init(ChipData data): void`
- `+ IsChipCompatible(Chip chip): bool`
- `+ TryAddChip(Chip chip): bool`
---

## ChipContainerData
#### Fields
- `+ containers: ContainerInfo[]`
- `+ NextChipData: ChipData`
---

## ChipContainerEffect
**Inherits**: `Effect`
#### Fields
- `- animator: Animator`
- `- cloudTransform: Transform`
- `- elements: ElementInfo[]`
- `- rootForElements: GameObject`
#### Methods
- `+ Activate(Chip chip): void`
- `+ Deactivate(Chip chip): void`
- `+ UpdateElements(Chip chip, Dictionary<ContainerInfo, int> containers, bool isFull): void`
---

## ChipData
**Inherits**: `ScriptableObject`
#### Fields
- `++ Size: Vector2Int`
- `+ CellHighlightPrefab: GameObject`
- `+ ChipContainerData: Optional<ChipContainerData>`
- `+ ChipGeneratorData: Optional<ChipGeneratorData>`
- `+ MergeAvailableEffectPrefab: GameObject`
- `+ MergeData: Optional<ChipMergeData>`
- `+ OtherData: ScriptableObject`
- `+ PrefabLink: GameObject`
- `+ Type: string`
#### Methods
- `- OnEnable(): void`
---

## ChipDataCollection
**Inherits**: `ScriptableObject`
#### Fields
- `++ Data: ChipData[]`
- `+- ChipDataFolder: string`
- `+- ChipPrefabFolder: string`
#### Methods
- `+ GetChipData(string chipName): ChipData`
---

## ChipFactory

> - **Purpose**: Factory for creating Chip objects.
> - **Usage**: Call CreateChip to instantiate a Chip from a ChipData.
> - **Notes**: Handles object pooling and initialization
> - requires ChipData with valid PrefabLink.
#### Fields
- `~ fieldGrid: IFieldGrid`
- `~ resolver: IObjectResolver`
#### Methods
- `+ CreateChip(Cell cell, ChipData chipData, Nullable<Vector3> parentWorldPosition): Chip`
- `+ CreateChip(Vector2Int cellPosition, ChipData chipData, Nullable<Vector3> parentWorldPosition): Chip`
- `+ Init(IObjectResolver resolver, IFieldGrid fieldGrid): void`
---

## ChipFlyAnimation

> - **Purpose**: Handles the logic for animating a chip flying to a target position
> - **Usage**: Instance per animation context. Call Update per frame.
> - **Notes**: Uses Vector3.Lerp for simple linear movement
#### Fields
- `+- IsAnimating: bool`
- `- animationDuration: float`
- `- elapsedTime: float`
- `- onCompleteCallback: Action`
- `- startPosition: Vector3`
- `- targetPosition: Vector3`
- `- targetTransform: Transform`
#### Methods
- `+ StartAnimation(Transform target, Vector3 startPos, Vector3 endPos, float duration, Action onComplete): void`
    - **Purpose**: Starts the fly animation for a chip
    - **Usage**: Call when a chip needs to fly back to its cell (e.g. after invalid drag or swap)
    - **Params**: target - the transform to animate
    - startPos - local start position
    - endPos - local target position
    - duration - time in seconds
    - onComplete - callback when finished
- `+ StopAnimation(): void`
- `+ Update(float deltaTime): void`
    - **Purpose**: Updates the animation state and applies movement to the target transform
    - **Usage**: Call in Update loop with deltaTime
    - **Params**: deltaTime - time since last frame
---

## ChipGenerator
**Inherits**: `Chip`

> - **Purpose**: Represents a chip that can generate other chips, supporting automatic/manual modes and recharge cycles.
> - **Usage**: Attach to a cell. Handles generation, charging, recharges, evolution to NextChipData, or destruction.
> - **Notes**: Manages recharge counts. If TotalRecharges > 0, it recharges N times before evolution/destruction.
#### Fields
- `+- RuntimeDataOnlyEditor: ChipGeneratorRuntimeData`
- `- chipFactory: ChipFactory`
- `- field: IFieldEventHandler`
- `- findFreePlaceForChip: IFreeCellFinder`
- `- generatorData: ChipGeneratorData`
    - **Purpose**: Stores static configuration for the chip generator.
    - **Usage**: Initialized in Init from ChipData. Used for generation logic.
    - **Notes**: Should not be null. Affects generator mode and chip creation.
- `- generatorEffect: ChipGeneratorEffect`
    - **Purpose**: Effect component for visual feedback during charging and activation.
    - **Usage**: Assigned via inspector. Used in Init, Update, and generation logic.
    - **Notes**: Must not be null. Handles visual state changes.
- `~ generatorRuntimeData: ChipGeneratorRuntimeData`
    - **Purpose**: Stores runtime state for the chip generator.
    - **Usage**: Initialized in Init. Tracks charge, waiting state, and charge count.
    - **Notes**: Critical for generator logic and state transitions.
- `- isAutoGeneration: bool`
    - **Purpose**: Indicates if the generator operates in auto mode.
    - **Usage**: Set in Init from generatorData. Controls event subscriptions and tap behavior.
    - **Notes**: Affects event handling and chip generation triggers.
- `- OnCharging: Action<float>`
#### Methods
- `+ Destroy(Cell mainCell): void`
    - **Purpose**: Cleans up the chip generator, removing event subscriptions and clearing delegates.
    - **Usage**: Call when destroying or removing a ChipGenerator instance.
    - **Notes**: Ensures no memory leaks or dangling event handlers.
- `+ Init(ChipData data): void`
    - **Purpose**: Initializes the chip generator with provided data and sets up event subscriptions.
    - **Usage**: Call when creating or resetting a ChipGenerator instance.
    - **Params**: data - ChipData for initialization, must contain valid ChipGeneratorData.
    - **Notes**: Handles event subscriptions, effect activation, and runtime state setup.
- `+ OnTap(Vector2 position): void`
    - **Purpose**: Handles tap input for manual chip generation.
    - **Usage**: Call when the player taps the generator in manual mode.
    - **Params**: position - Tap position in world coordinates.
    - **Notes**: No effect in auto mode or if not charged. Uses TryGenerateChip for logic.
- `- OnFieldChanged(): void`
    - **Purpose**: Handles field change events for auto-generation mode.
    - **Usage**: Automatically called when the field changes if in auto mode.
    - **Notes**: Triggers chip generation if waiting for space.
- `- TryGenerateChip(): void`
    - **Purpose**: Attempts to generate a new chip, managing charge state, recharges, and generator lifecycle.
    - **Usage**: Call when generator is charged. Triggered by tap (manual) or field change (auto).
    - **Notes**: Decrements ChargeCount. If reaching 0, handles recharge cycle decrement or evolution/destruction if out of recharges.
- `- Update(): void`
    - **Purpose**: Manages charging, state transitions, and triggers chip generation in auto mode.
    - **Usage**: Called every frame by Unity. Handles charging timer and triggers effects.
    - **Notes**: Handles both auto and manual modes. Triggers animation and effect changes.
---

## ChipGeneratorData

> - **Purpose**: Configuration data for chip generators, defining generation weights, charge settings, and recharge cycles.
> - **Usage**: Used in ChipData to define generator behavior. Can be edited via Inspector or ScriptableObject.
> - **Notes**: TotalRecharges = 0 means infinite recharges. ChargeCount defines charges per cycle.
#### Fields
- `+ ChargeCount: int`
- `+ ChargingTime: float`
- `+ Data: GeneratorData[]`
- `+ IsAutoGeneration: bool`
- `+ IsStartCharged: bool`
- `+ NextChipData: ChipData`
- `+ TotalRecharges: int`
#### Methods
- `+ GenerateChipData(): ChipData`
---

## ChipGeneratorEffect
**Inherits**: `Effect`
#### Fields
- `- maskRectTransform: RectTransform`
#### Methods
- `+ OnCharging(float progress): void`
---

## ChipGeneratorRuntimeData

> - **Purpose**: Stores runtime state for a chip generator, tracking its charge status and remaining recharges.
> - **Usage**: Created and managed by ChipGenerator. Used to persist state during gameplay.
> - **Notes**: Updated during generation and charging cycles.
#### Fields
- `+ ChargeCount: int`
- `+ ChargingTimeLeft: float`
- `+ IsCharged: bool`
- `+ IsWaitingForSpace: bool`
- `+ RechargesLeft: int`
---

## ChipMergeAvailableEffect
**Inherits**: `Effect`
#### Fields
- `- animator: Animator`
- `- autoPosition: bool`
- `- autoSize: bool`
- `- isActive: bool`
#### Methods
- `+ Activate(Chip chip): void`
- `+ Deactivate(Chip chip): void`
- `+ OnInteractionUnderCellChanged(Cell underCell, Cell overCell): void`
- `- Awake(): void`
---

## ChipMergeData
#### Fields
- `++ MergeData: MergeData[]`
#### Methods
- `+ CanMerge(ChipData otherChip): bool`
- `+ GetNextChip(ChipData otherChip): ChipData`
---

## ChipMovingLogic

> - **Purpose**: Handles logic for moving chips and relocating conflicting chips to make room
> - **Usage**: Called when a chip is dragged or when a merged chip grows and needs more space
> - **Notes**: Uses IFreeCellFinder to find new valid spots for chips that are in the way
#### Fields
- `- fieldGrid: IFieldGrid`
- `- freeCellFinder: IFreeCellFinder`
#### Methods
- `+ CanChipMoving(Cell leftTopCell, IEnumerable<Chip> chipsToExclude, Vector2Int chipSize, List`1& plannedRelocations): bool`
    - **Purpose**: Checks if a chip can move to a target and if conflicting chips can be relocated
    - **Usage**: Internal check before performing actual relocation
    - also used by merge logic for growing chips
    - **Params**: leftTopCell - target top-left cell
    - chip - the chip being moved (can be null for new chips)
    - chipSize - dimensions of the chip
    - plannedRelocations - output list of moves to perform
    - **Returns**: True if a valid state is possible for all chips involved
    - **Notes**: Sorted relocation: larger chips are relocated first to minimize fragmentation
- `+ ChipMoving(Cell overCell, Cell leftTopCell, Cell sourceCell): void`
    - **Purpose**: Orchestrates the move of a chip to a new position, potentially relocating others
    - **Usage**: Call when a drag operation ends at a specific cell
    - **Params**: overCell - the cell the mouse is over
    - leftTopCell - the target top-left cell for the chip
    - sourceCell - the original cell of the chip
    - **Notes**: If relocation is impossible, the chip snaps back to its source position
- `+ ChipsRelocate(Cell leftTopCell, Cell sourceCell, List<ChipMoveAction> plannedRelocations): void`
    - **Purpose**: Executes the planned relocations of chips on the field
    - **Usage**: Final step of the relocation process
    - performs atomic field updates
    - **Params**: leftTopCell - destination for the primary chip
    - sourceCell - original spot of the primary chip
    - plannedRelocations - list of pre-calculated chip moves
    - **Notes**: Temporarily sets dragging state during relocation for visual consistency
- `- GetOccupiedCellsInArea(Vector2Int cellPos, Vector2Int chipSize, IEnumerable<Chip> chipsToExclude): List<Cell>`
---

## ContainerInfo
#### Fields
- `+ Count: int`
- `+ Type: ContainerType`
- `+ TypeOrId: string`
- `+- ContainerElementPrefab: GameObject`
---

## DraggableChipLogic
**Inherits**: `MonoBehaviour`

> - **Purpose**: Handles the logic for dragging, merging, and swapping chips on the field.
> - **Usage**: Attach to a GameObject that manages chip drag-and-drop interactions.
> - **Notes**: Coordinates chip movement, merge checks, and swap logic. Relies on GameManager for cell/chip operations.
#### Fields
- `+- Chip: Chip`
- `+- IsDragging: bool`
- `- chipInteractionLogics: List<IChipInteractionLogic>`
    - **Purpose**: Collection of all chip interaction logic handlers
    - **Usage**: Internal field
    - populated in Awake and queried during drag operations
    - **Notes**: Populated in Awake from attached components
    - used to check and execute interactions like merge or container fill
- `- chipMovingLogic: IChipMovingLogic`
- `- currentMergableCell: Cell`
- `- currentMergableLogic: IChipInteractionLogic`
- `- draggableChip: Chip`
- `- draggableTransform: Transform`
    - **Purpose**: Cached transform of the chip being dragged
    - **Usage**: Internal field
    - cached for performance during drag operations
    - **Notes**: Set in OnDragStart for performance optimization during drag
- `- fieldGrid: IFieldGrid`
- `- prevCell: Cell`
- `- sourceCell: Cell`
    - **Purpose**: Stores the cell from which the chip was originally dragged
    - **Usage**: Internal field
    - automatically set in OnDragStart and used throughout drag operations
    - **Notes**: Set in OnDragStart
    - used to track the chip's origin during drag operations
- `- sourceCellTransform: Transform`
    - **Purpose**: Cached transform of the source cell
    - **Usage**: Internal field
    - used for coordinate transformations during drag
    - **Notes**: Used for coordinate transformations during drag
    - set in OnDragStart
#### Methods
- `+ GetAnchorPosition(Vector2 originalPosition): Vector2`
    - **Purpose**: Calculates anchor position for chip placement based on its size.
    - **Usage**: Call to get the correct anchor for multi-cell chips.
    - **Params**: originalPosition - initial position of the chip
    - **Returns**: Adjusted anchor position for the chip.
- `+ OnDrag(Vector3 worldPosition, Cell anchorCell): void`
    - **Purpose**: Handles chip movement and merge effect during drag.
    - **Usage**: Call continuously while dragging, passing current mouse/cell info.
    - **Params**: worldPosition - mouse position
    - anchorCell - cell under chip
    - **Notes**: Activates/deactivates merge effect on target chips
    - chip position is constrained to field boundaries.
- `+ OnDragEnd(Cell overCell, Cell leftTopCell): void`
    - **Purpose**: Handles logic when drag ends, including merge, swap, and cleanup.
    - **Usage**: Call when drag operation finishes, passing the cell under the chip and the anchor cell.
    - **Params**: overCell - cell under chip on drop
    - leftTopCell - anchor cell for placement
    - **Notes**: Cleans up visual interaction indicators
    - invokes merge handlers, performs swap if not handled, resets state.
- `+ OnDragStart(Cell sourceCell, Vector3 worldPosition): void`
    - **Purpose**: Initializes drag state for a chip.
    - **Usage**: Call when drag starts, passing the source cell and mouse position.
    - **Params**: sourceCell - cell chip is dragged from
    - worldPosition - mouse position in world space
    - **Notes**: Sets up internal references and marks chip as dragging.
- `- Awake(): void`
- `- GetFilterCells(Cell targetCell): List<Cell>`
- `- MoveToWorldPosition(Vector3 worldPosition): void`
- `- ResetCurrentMergable(): void`
- `- ResetDragState(): void`
- `- UpdateInteractionState(Cell sourceCell, Cell targetCell): void`
    - **Purpose**: Checks if merge or container fill is allowed between two cells.
    - **Usage**: Call before attempting merge or placement.
    - **Params**: sourceCell - cell chip is dragged from
    - targetCell - cell chip is hovered over
    - **Returns**: True if merge or fill is allowed
    - false otherwise.
    - **Notes**: Returns false if no handlers are subscribed.
---

## Effect
**Inherits**: `MonoBehaviour`

> - **Purpose**: Base class for all effects that can be activated or deactivated on chips
> - **Usage**: Attach to chip GameObjects to provide reusable effect logic
> - provides virtual methods for activation, deactivation, and cell change handling
> - intended to be inherited by specific effect implementations
> - **Notes**: Designed for extension
> - override virtual methods to implement custom effect behavior
> - supports both chip-based and cell-based effects
#### Fields
- `~ effectForCell: bool`
    - **Purpose**: Determines whether the effect should be attached to the cell's transform instead of the chip's
    - **Usage**: Set to true if the effect must follow the cell when the chip moves between cells
    - used in OnChangedCell to update the effect's parent transform
    - **Notes**: If false, the effect remains attached to the chip's transform regardless of cell changes
#### Methods
- `+ Activate(Chip chip): void`
- `+ Deactivate(Chip chip): void`
- `+ OnChangedCell(Cell sourceCell, Cell targetCell): void`
    - **Purpose**: Handles logic when a chip is moved from one cell to another
    - **Usage**: Called after the chip's parent cell has changed
    - updates the effect's transform to match the new cell if effectForCell is true
    - override in derived classes for custom behavior
    - **Params**: sourceCell - the cell the chip was previously in
    - targetCell - the cell the chip is moved to
    - **Notes**: Restores local position and scale after changing parent transform
    - only applies if effectForCell is true
- `+ OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell underCell): void`
    - **Purpose**: Called when the chip's cell changes during drag
    - **Usage**: Handle logic when a chip is dragged from one cell to another during drag-and-drop
    - Override in derived classes for custom behavior
    - **Params**: prevCell - the cell the chip was in before the drag cell change
    - currentCell - the cell the chip is currently over during drag
    - **Notes**: Should only be called as part of drag operations
    - no default implementation in base class
- `+ OnInteractionUnderCellChanged(Cell underCell, Cell overCell): void`
---

## FieldData
**Inherits**: `ScriptableObject`
#### Fields
- `+- Cells: CellData[]`
- `+- ChipDataCollection: ChipDataCollection`
- `+- FieldSize: Vector2Int`
#### Methods
- `+ SetCells(CellData[] newCells): void`
    - **Purpose**: Editor-only setter for cells array
    - **Usage**: Called by LevelEditorWindow when saving level data
    - **Params**: newCells - array of chip placements to save
- `+ SetChipDataCollection(ChipDataCollection collection): void`
    - **Purpose**: Editor-only setter for ChipDataCollection reference
    - **Usage**: Called by LevelEditorWindow to assign chip collection to level
- `+ SetFieldSize(Vector2Int size): void`
    - **Purpose**: Editor-only helper methods for level editor to modify FieldData
    - **Usage**: Called by LevelEditorWindow to set field properties during editing
    - **Notes**: Only available in editor
    - allows modification of private serialized fields
- `+ ValidateLevel(List`1& errors): bool`
    - **Purpose**: Validates level data for errors and inconsistencies
    - **Usage**: Call before saving to check for overlapping chips and out-of-bounds placements
    - **Params**: errors - output list of validation error messages
    - **Returns**: True if level is valid, false if errors were found
---

## FieldEventHandler
**Inherits**: `MonoBehaviour`

> - **Purpose**: Main game field that manages cells, chips, and their interactions.
> - **Usage**: Attach to the main field GameObject
> - coordinates chip placement, movement, and merging.
> - **Notes**: Central controller for grid logic and chip management.
#### Fields
- `~ draggableChip: DraggableChipLogic`
- `- fieldGrid: IFieldGrid`
- `- OnChangeField: Action`
- `~ shouldNotifyFieldChanged: bool`
#### Methods
- `+ NotifyFieldChanged(): void`
    - **Purpose**: Notifies subscribers that the field state has changed.
    - **Usage**: Call after any change to the field (chip move, merge, etc.) to trigger update events.
- `+ OnDrag(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Handles chip dragging with field boundary constraints
    - **Usage**: Called continuously during chip drag to update position and prevent overflow
    - **Params**: position - screen position
    - worldPosition - world position
    - **Notes**: Clamps chip position to field boundaries to prevent chips from going outside the field
- `+ OnDragEnd(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Handles chip drop with the same boundary constraints as OnDrag
    - **Usage**: Called when drag ends to finalize chip placement
    - **Params**: position - screen position
    - worldPosition - world position
    - **Notes**: Uses identical clamping logic as OnDrag to ensure consistency between drag and drop positions
- `+ OnDragStart(Vector2 position, Vector3 worldPosition): void`
- `+ OnTap(Vector2 position, Vector3 worldPosition): void`
- `~ GetClampedAnchorPosition(Vector2 position): Vector2Int`
    - **Purpose**: Calculates clamped anchor position for dragged chip within field boundaries
    - **Usage**: Used by OnDrag and OnDragEnd to ensure consistent position clamping
    - **Params**: position - screen position to clamp
    - **Returns**: Clamped anchor position that keeps the chip within field boundaries
---

## FieldGrid
**Inherits**: `MonoBehaviour`

> - **Purpose**: Manages the grid of cells for the game field.
> - **Usage**: Attach to field GameObject
> - handles cell creation, validation, and chip placement.
> - **Notes**: Implements IFieldGrid interface
> - central component for grid-based operations.
#### Fields
- `+- Cells: Cell[]`
- `+- FieldSize: Vector2Int`
- `- resolver: IObjectResolver`
#### Methods
- `+ CreateCells(Vector2Int fieldSize): void`
    - **Purpose**: Creates and initializes the grid of cells based on field size.
    - **Usage**: Call during field initialization to create all cells.
    - **Params**: fieldSize - dimensions of the field (width, height).
    - **Notes**: Centers cells around origin
    - creates Cell GameObjects with RectTransform.
- `+ GetCells(Vector2Int cellPos, Vector2Int size): List<Cell>`
    - **Purpose**: Retrieves all cells in a rectangular area.
    - **Usage**: Call to get cells occupied by a multi-cell chip.
    - **Params**: cellPos - top-left position
    - size - area dimensions (width, height).
    - **Returns**: List of all cells in the specified area.
- `+ IsValidCellPos(Vector2Int cellPos): bool`
    - **Purpose**: Validates if a single cell position is within field boundaries.
    - **Usage**: Call to check if coordinates are valid before accessing cells array.
    - **Params**: cellPos - cell position to validate.
    - **Returns**: True if position is within bounds
    - otherwise false.
- `+ IsValidCellPos(Vector2Int cellPos, Vector2Int size): bool`
    - **Purpose**: Checks if the given cell position and size are valid within field boundaries.
    - **Usage**: Call to validate if a chip of given size can be placed at the specified position.
    - **Params**: cellPos - top-left cell position
    - size - chip size (width, height).
    - **Returns**: True if the area is fully within field bounds
    - otherwise false.
- `+ SetChipInCell(Cell cell, Chip chip): void`
    - **Purpose**: Places a chip in the specified cell.
    - **Usage**: Call to assign a chip to a cell and update multi-cell occupancy if needed.
    - **Params**: cell - the target cell
    - chip - the chip to place.
- `- ClearCells(Cell cell): void`
    - **Purpose**: Clears all chips from cells occupied by the specified cell's chip.
    - **Usage**: Call to remove a chip and its multi-cell occupancy from the field.
    - **Params**: cell - the main or secondary cell to clear.
---

## FieldInitializeCommand
**Inherits**: `MonoBehaviour`

> - **Purpose**: Handles field initialization including cell creation and chip loading.
> - **Usage**: Attach to field GameObject
> - orchestrates field setup from FieldData.
> - **Notes**: Uses VContainer dependency injection
> - requires FieldData and ChipDataCollection.
#### Fields
- `- chipFactory: ChipFactory`
- `- fieldData: FieldData`
- `- fieldGrid: IFieldGrid`
- `~ fieldSpriteRenderer: SpriteRenderer`
- `~ mergeCamera: Camera`
#### Methods
- `+ CreateField(): void`
    - **Purpose**: Creates the field grid and sets up camera based on field data.
    - **Usage**: Call first during initialization to create the field structure.
    - **Notes**: Validates FieldData before proceeding
    - sets camera size and creates cells.
- `+ GetFieldData(): FieldData`
- `+ LoadChips(): void`
    - **Purpose**: Loads and creates chips from field data.
    - **Usage**: Call after CreateField to populate the field with initial chips.
    - **Notes**: Validates data sources
    - looks up chip definitions from ChipDataCollection
    - triggers spawn animations.
- `+ SetFieldData(FieldData fieldData): void`
- `- CreateChip(Vector2Int cellPosition, ChipData chipData): void`
    - **Purpose**: Creates a chip at the specified position and triggers spawn animation.
    - **Usage**: Internal helper method called by LoadChips for each chip.
    - **Params**: cellPosition - grid position for the chip
    - chipData - chip configuration data.
- `- SetupCameraAndFieldSize(Vector2Int fieldSize): void`
---

## FillContainerLogic
**Inherits**: `MonoBehaviour`

> - **Purpose**: Handles chip container filling logic when chips are added to container chips.
> - **Usage**: Implements IChipInteractionLogic to provide unified interface for container operations.
> - **Notes**: Validates container compatibility and adds chips to containers
> - integrates with unified interaction system.
#### Methods
- `+ CanInteract(Cell sourceCell, Cell targetCell): bool`
    - **Purpose**: Validates if a chip can be added to a container chip.
    - **Usage**: Called by unified interaction system to check container fill possibility.
    - **Params**: sourceCell - cell with chip being dragged
    - targetCell - cell with container chip
    - **Returns**: True if chip can be added to container
    - false otherwise
    - **Notes**: Checks if target is a container and if source chip is compatible.
- `+ ExecuteInteraction(Cell sourceCell, Cell targetCell): bool`
    - **Purpose**: Executes container fill interaction between chip and container.
    - **Usage**: Called by unified interaction system after CanInteract validation passes.
    - **Params**: sourceCell - cell with chip being dragged
    - targetCell - cell with container chip
    - **Notes**: Adds chip to container and destroys source chip if successful.
---

## FreeCellFinder
#### Fields
- `- fieldGrid: IFieldGrid`
#### Methods
- `+ FindNearestFreeCell(Vector2Int parentPos, Vector2Int parentSize, Vector2Int childChipSize, Chip chipToPlace, HashSet<Cell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove): Cell`
    - **Purpose**: Finds the nearest free space for placing a chip, can exclude cells and potentially move other chips.
    - **Usage**: Call to find a suitable cell for a chip, considering exclusions and movable chips.
    - **Params**: parentPos - area start
    - parentSize - area size
    - childChipSize - chip size
    - chipToPlace - chip to place
    - cellsToExclude - cells to skip
    - chipsToPotentiallyMove - chips allowed to move.
    - **Returns**: The nearest free Cell or null if none found.
---

## IChipFlyAnimation
#### Fields
- `+- IsAnimating: bool`
#### Methods
- `+ StartAnimation(Transform target, Vector3 startPos, Vector3 endPos, float duration, Action onComplete): void`
    - **Purpose**: Starts the fly animation for a chip
    - **Usage**: Call when a chip needs to fly back to its cell (e.g. after invalid drag)
    - **Params**: target - the transform to animate
    - startPos - local start position
    - endPos - local target position
    - duration - time in seconds
    - onComplete - callback when finished
- `+ StopAnimation(): void`
    - **Purpose**: Stops the fly animation
    - **Usage**: Call when a chip needs to stop the animation
- `+ Update(float deltaTime): void`
    - **Purpose**: Updates the animation state
    - **Usage**: Call in Update loop
    - **Params**: deltaTime - time since last frame
---

## IChipInteractionLogic
#### Methods
- `+ CanInteract(Cell sourceCell, Cell targetCell): bool`
    - **Purpose**: Validates whether a chip interaction can be performed between source and target cells.
    - **Usage**: Call before attempting to execute an interaction to ensure it's valid.
    - **Params**: sourceCell - cell with the chip being moved
    - targetCell - destination cell for the interaction
    - **Returns**: Boolean indicating if the interaction is allowed
    - **Notes**: Should perform all necessary validation checks without modifying game state.
- `+ ExecuteInteraction(Cell sourceCell, Cell targetCell): bool`
    - **Purpose**: Executes the actual chip interaction between source and target cells.
    - **Usage**: Call after CanInteract validation passes to perform the interaction.
    - **Params**: sourceCell - cell with the chip being moved
    - targetCell - destination cell for the interaction
    - **Notes**: Modifies game state
    - should only be called after successful CanInteract validation.
---

## IChipMovingLogic

> - **Purpose**: Interface for chip movement and relocation logic.
> - **Usage**: Use to manage chip movement on the field, including relocation of conflicting chips.
> - **Notes**: Implemented by ChipMovingLogic.
#### Methods
- `+ CanChipMoving(Cell leftTopCell, IEnumerable<Chip> chipsToExclude, Vector2Int chipSize, List`1& plannedRelocations): bool`
    - **Purpose**: Checks if a chip can move to a target and plans relocations if necessary.
    - **Usage**: Call before moving a chip to validate if the move is possible.
    - **Params**: leftTopCell - target anchor cell
    - chipsToExclude - chips to ignore during collision checks
    - chipSize - size of the chip being moved
    - plannedRelocations - output list of moves to perform
    - **Returns**: True if the move and all necessary relocations are possible.
- `+ ChipMoving(Cell overCell, Cell leftTopCell, Cell sourceCell): void`
    - **Purpose**: Moves a chip to a new cell position, handling potential relocations.
    - **Usage**: Call when a chip is dropped or needs to be moved programmatically.
    - **Params**: overCell - cell under the chip
    - leftTopCell - target anchor cell
    - sourceCell - original anchor cell
- `+ ChipsRelocate(Cell leftTopCell, Cell sourceCell, List<ChipMoveAction> plannedRelocations): void`
    - **Purpose**: Executes pre-planned chip relocations.
    - **Usage**: Call after CanChipMoving returns true to apply the relocations.
    - **Params**: leftTopCell - destination for the primary chip
    - sourceCell - original spot of the primary chip
    - plannedRelocations - list of calculated moves
---

## IFieldEventHandler
#### Methods
- `+ NotifyFieldChanged(): void`
- `+ OnDrag(Vector2 position, Vector3 worldPosition): void`
- `+ OnDragEnd(Vector2 position, Vector3 worldPosition): void`
- `+ OnDragStart(Vector2 position, Vector3 worldPosition): void`
- `+ OnTap(Vector2 position, Vector3 worldPosition): void`
---

## IFieldGrid

> - **Purpose**: Interface for the grid system that manages cells and chip placement.
> - **Usage**: Use to access cells, validate positions, and manage chip placement on the grid.
> - **Notes**: Implemented by FieldGrid.
#### Fields
- `+- Cells: Cell[]`
- `+- FieldSize: Vector2Int`
#### Methods
- `+ CreateCells(Vector2Int vector2Int): void`
    - **Purpose**: Initializes the grid with a specific size
    - **Usage**: Call during initialization for level setup
    - **Params**: vector2Int - dimensions of the field
- `+ GetCells(Vector2Int cellPos, Vector2Int size): List<Cell>`
    - **Purpose**: Retrieves a list of cells within a specified area
    - **Usage**: Query grid for multiple cells, e.g. for multi-cell chips
    - **Params**: cellPos - top-left position
    - size - area dimensions
    - **Returns**: List of cells in the specified area
- `+ IsValidCellPos(Vector2Int cellPos): bool`
    - **Purpose**: Checks if a position is within field bounds
    - **Usage**: Validation before any grid-based operation
    - **Params**: cellPos - position to check
- `+ IsValidCellPos(Vector2Int cellPos, Vector2Int size): bool`
    - **Purpose**: Checks if a multi-cell area is within field bounds
    - **Usage**: Validation for multi-cell chip placement
    - **Params**: cellPos - top-left position
    - size - area dimensions
- `+ SetChipInCell(Cell cell, Chip chip): void`
    - **Purpose**: Assigns or removes a chip from a specific cell
    - **Usage**: Primary method for modifying chip placement on the grid
    - **Params**: cell - target cell
    - chip - chip to place (null to clear)
---

## IFieldInitializeCommand
#### Methods
- `+ CreateField(): void`
- `+ LoadChips(): void`
---

## IFreeCellFinder
#### Methods
- `+ FindNearestFreeCell(Vector2Int parentPos, Vector2Int parentSize, Vector2Int childChipSize, Chip chipToPlace, HashSet<Cell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove): Cell`
---

## Merge2Initializer

> - **Purpose**: Main initializer for the Merge2 game module using VContainer.
> - **Usage**: Registered in VContainer lifetime scope
> - automatically called on game start.
> - **Notes**: Wires up dependencies, connects input events, and initializes field and chips.
#### Fields
- `- chipFactory: ChipFactory`
- `- field: IFieldEventHandler`
- `- fieldGrid: IFieldGrid`
- `- fieldInitializeCommand: IFieldInitializeCommand`
- `- inputManager: InputManager`
- `- resolver: IObjectResolver`
#### Methods
- `+ Initialize(): void`
    - **Purpose**: Initializes the Merge2 module by setting up all dependencies and creating the field.
    - **Usage**: Called automatically by VContainer on application start.
    - **Notes**: Connects input events to field handlers
    - initializes factory, field, and loads initial chips.
---

## Merge2LifetimeScope
**Inherits**: `LifetimeScope`
#### Methods
- `~ Awake(): void`
- `~ Configure(IContainerBuilder builder): void`
---

## MergeableChipLogic
**Inherits**: `MonoBehaviour`

> - **Purpose**: Handles chip merging logic when two compatible chips are combined.
> - **Usage**: Implements IChipInteractionLogic to provide unified interface for merge operations.
> - **Notes**: Validates chip compatibility and creates new merged chips
> - integrates with unified interaction system.
#### Fields
- `- chipFactory: ChipFactory`
- `- chipMovingLogic: IChipMovingLogic`
- `- fieldGrid: IFieldGrid`
#### Methods
- `+ CanInteract(Cell sourceCell, Cell targetCell): bool`
    - **Purpose**: Validates if two cells can perform a merge interaction.
    - **Usage**: Called by unified interaction system to check merge possibility.
    - **Params**: sourceCell - cell with chip being dragged
    - targetCell - destination cell
    - **Returns**: True if chips can be merged
    - false otherwise
    - **Notes**: Checks chip compatibility and merge data availability.
- `+ ExecuteInteraction(Cell sourceCell, Cell targetCell): bool`
    - **Purpose**: Executes merge interaction between two cells, handling potential resizing
    - **Usage**: Called by unified interaction system after CanInteract validation passes
    - **Params**: sourceCell - cell with chip being dragged
    - targetCell - destination cell
    - **Notes**: If the merged chip is larger than the parent, it uses IChipMovingLogic to relocate neighboring chips if needed.
---

## MergeData
#### Fields
- `++ MergeableChipData: ChipData`
- `++ NextChipData: MergeNextChipData[]`
#### Methods
- `+ GetRandomNextChip(): ChipData`
    - **Purpose**: Selects a random next chip based on defined weights
    - **Usage**: Called internally to determine the output chip when multiple outcomes are possible
    - **Params**: none
    - **Returns**: The selected ChipData
---

## MergeNextChipData
#### Fields
- `++ NextChipData: ChipData`
- `++ Weight: int`
---

