# Namespace: Merge2;Merge2.Input

## Table of Contents
- [Cell](#cell)
- [CellHighlightEffect](#cellhighlighteffect)
- [CellObserverManager](#cellobservermanager)
- [CellSubscriber](#cellsubscriber)
- [Chip](#chip)
- [ChipChangedEvent](#chipchangedevent)
- [ChipContainer](#chipcontainer)
- [ChipContainerData](#chipcontainerdata)
- [ChipContainerEffect](#chipcontainereffect)
- [ChipContainerRuntimeData](#chipcontainerruntimedata)
- [ChipData](#chipdata)
- [ChipDataCollection](#chipdatacollection)
- [ChipExtraEffectsData](#chipextraeffectsdata)
- [ChipFactory](#chipfactory)
- [ChipFlyAnimation](#chipflyanimation)
- [ChipGenerator](#chipgenerator)
- [ChipGeneratorData](#chipgeneratordata)
- [ChipGeneratorRechargeEffect](#chipgeneratorrechargeeffect)
- [ChipGeneratorRuntimeData](#chipgeneratorruntimedata)
- [ChipMergeAvailableEffect](#chipmergeavailableeffect)
- [ChipMergeData](#chipmergedata)
- [ChipMovingLogic](#chipmovinglogic)
- [ChipPowerBooster](#chippowerbooster)
- [ChipPowerBoosterData](#chippowerboosterdata)
- [ChipRuntimeData](#chipruntimedata)
- [ChipSortingLayer](#chipsortinglayer)
- [CombinedBlockingState](#combinedblockingstate)
- [ContainerInfo](#containerinfo)
- [DeferredChipChangeNotifier](#deferredchipchangenotifier)
- [DraggableChipLogic](#draggablechiplogic)
- [Effect](#effect)
- [EffectBlockingSettings](#effectblockingsettings)
- [EffectConsts](#effectconsts)
- [EffectContainerRef](#effectcontainerref)
- [EffectDestroyingRuntimeData](#effectdestroyingruntimedata)
- [EffectDestroyingSettings](#effectdestroyingsettings)
- [EffectGeneratorChargingRef](#effectgeneratorchargingref)
- [EffectPowerBoosterJoinRef](#effectpowerboosterjoinref)
- [EffectRef](#effectref)
- [ExtraChip](#extrachip)
- [ExtraEffectData](#extraeffectdata)
- [ExtraEffectDataEx](#extraeffectdataex)
- [FieldChipData](#fieldchipdata)
- [FieldData](#fielddata)
- [FieldEventHandler](#fieldeventhandler)
- [FieldGrid](#fieldgrid)
- [FieldInitializeCommand](#fieldinitializecommand)
- [FillContainerLogic](#fillcontainerlogic)
- [FreeCellFinder](#freecellfinder)
- [ICellSubscriber](#icellsubscriber)
- [IChipChangeNotifier](#ichipchangenotifier)
- [IChipFinder](#ichipfinder)
- [IChipFlyAnimation](#ichipflyanimation)
- [IChipInteractionLogic](#ichipinteractionlogic)
- [IChipMovingLogic](#ichipmovinglogic)
- [IChipSortingLayer](#ichipsortinglayer)
- [IChipSpecialData](#ichipspecialdata)
- [IEffect](#ieffect)
- [IEffectBlockingSettings](#ieffectblockingsettings)
- [IEffectContainer](#ieffectcontainer)
- [IEffectGeneratorCharging](#ieffectgeneratorcharging)
- [IEffectPowerBoosterJoin](#ieffectpowerboosterjoin)
- [IFieldEventHandler](#ifieldeventhandler)
- [IFieldGrid](#ifieldgrid)
- [IFieldInitializeCommand](#ifieldinitializecommand)
- [IFreeCellFinder](#ifreecellfinder)
- [IPowerBoosterModifier](#ipowerboostermodifier)
- [Merge2Initializer](#merge2initializer)
- [Merge2LifetimeScope](#merge2lifetimescope)
- [MergeableChipLogic](#mergeablechiplogic)
- [MergeCombination](#mergecombination)
- [MergeResult](#mergeresult)
- [NeighborChipFinder](#neighborchipfinder)
- [PowerBoosterCellSubscriber](#powerboostercellsubscriber)
- [PowerBoosterConnectorCellsHighlightEffect](#powerboosterconnectorcellshighlighteffect)
- [PowerBoosterJoinEffect](#powerboosterjoineffect)
- [ShadowEffect](#shadoweffect)
- [SortingLayerData](#sortinglayerdata)
- [IInputManager](#iinputmanager)
- [InputManager](#inputmanager)
- [Merge2Input](#merge2input)

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

> - **Purpose**: Visual effect that highlights the cell under a chip
> - **Usage**: Attached to chip using ChipData.CellHighlightPrefab
> - Managed by Chip to show/hide highlights
> - **Notes**: Handles multiple cells if chip size > 1x1
> - updates position during drag interaction
#### Fields
- `~ chipSize: Vector2Int`
- `~ color: Color`
- `~ highlightPrefab: GameObject`
- `~ highlights: List<GameObject>`
- `~ order: float`
- `~ sharedMaterial: Material`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ OnChangedCell(Cell sourceCell, Cell targetCell): void`
- `+ OnInteractionOverCellChanged(Cell sourceCell, Cell targetCell, Cell interactableCell): void`
- `~ CreateHighlight(Vector3 localPosition): GameObject`
- `~ CreateHighlights(): void`
- `~ DestroyHighlights(): void`
---

## CellObserverManager
**Inherits**: `MonoBehaviour`

> - **Purpose**: Manages cell observation subscriptions with a physical-cell index.
> - **Usage**: Subscribers are stored per physical cell. On flush, notifications are expanded by old/new chip footprint so secondary-cell observers are notified without fold/promote state.
> - **Notes**: Optimized for low churn: no topology migration, O(1) subscribe/unsubscribe operations, and reusable snapshot list for callback-safe iteration.
#### Fields
- `- cellToSubscribers: Dictionary<Cell, HashSet<ICellSubscriber>>`
    - **Purpose**: Primary forward index: physical cell to subscribers.
    - **Usage**: Used by event-area notification for O(1) lookup of observers per cell.
    - **Notes**: HashSet prevents duplicate subscriptions for the same subscriber-cell pair.
- `- eventNotifiedSubscribers: HashSet<ICellSubscriber>`
    - **Purpose**: Per-event dedupe set reused across flush loop iterations.
    - **Usage**: Prevents duplicate callbacks when an event area touches multiple watched cells.
- `- fieldGrid: IFieldGrid`
- `- logEnable: bool`
- `- notifier: IChipChangeNotifier`
- `- subscriberSnapshot: List<ICellSubscriber>`
    - **Purpose**: Reusable snapshot list for safe callback iteration.
    - **Usage**: Copies current subscriber set before callbacks to avoid collection-modified issues when unsubscribe happens during callback.
- `- subscriberToCells: Dictionary<ICellSubscriber, HashSet<Cell>>`
    - **Purpose**: Reverse index: subscriber to all watched physical cells.
    - **Usage**: Used for O(k) unsubscribe and API data extraction without scanning all cells.
#### Methods
- `+ GetSubscriptionData(ICellSubscriber subscriber): List<SubscriptionData>`
    - **Purpose**: Returns subscription state for diagnostics/tests with backward-compatible direct/virtual representation.
    - **Usage**: Secondary watched cells are grouped under current MainCell as virtual positions to preserve old API contract.
- `+ LogSubscriptions(): void`
- `+ Subscribe(ICellSubscriber subscriber, List<Vector2Int> cellPositions): void`
- `+ Unsubscribe(ICellSubscriber subscriber): void`
- `- AddSubscription(ICellSubscriber subscriber, Cell cell): void`
    - **Purpose**: Adds a subscriber-cell relation to both forward and reverse indexes.
    - **Usage**: Used by Subscribe and internal resubscription flows.
    - **Notes**: Duplicate relations are ignored by HashSet.Add to avoid redundant reverse-index writes.
- `- HandleFlush(IReadOnlyList<ChipChangedEvent> events): void`
    - **Purpose**: Dispatches notifications for all events in the current flush batch.
    - **Usage**: Reuses one dedupe HashSet per event to minimize allocations.
- `- NotifySubscribersForEvent(ChipChangedEvent evt, HashSet<ICellSubscriber> alreadyNotified): void`
    - **Purpose**: Expands a single cell change event to old/new occupied areas and notifies affected subscribers once.
    - **Usage**: Fast path handles same-size replace in one area pass
    - fallback handles old-only/new-only and defensive empty-empty event.
- `- NotifySubscribersInArea(Vector2Int origin, Vector2Int size, ChipChangedEvent evt, HashSet<ICellSubscriber> alreadyNotified): void`
- `- OnDestroy(): void`
- `- OnValidate(): void`
- `- Start(): void`
---

## CellSubscriber
**Inherits**: `MonoBehaviour`

> - **Purpose**: Base component for chips that need batched notifications about changes in neighboring cells.
> - **Usage**: Inherit for domain-specific subscribers (for example boosters) and override virtual callbacks to react when observed chips appear, disappear, or the owner chip changes cell.
> - **Notes**: Computes a border around the chip occupied area (supports multi-cell chips), subscribes through CellObserverManager, and guarantees unsubscription on destroy.
#### Fields
- `+- ObservedCellPositions: IReadOnlyList<Vector2Int>`
- `~ cellObserverManager: CellObserverManager`
- `~ chip: Chip`
- `~ fieldGrid: IFieldGrid`
#### Methods
- `+ OnChipChangedCell(Cell sourceCell, Cell targetCell): void`
    - **Purpose**: Re-subscribes to neighbors whenever the chip is placed or moved to a new cell.
    - **Usage**: Called by Cell.Chip setter
    - sourceCell == null means initial placement.
    - **Params**: sourceCell - previous cell (null on initial placement)
    - targetCell - new cell with correct CellPosition
    - **Notes**: Unsubscribes from old neighbors first to avoid stale subscriptions after a move.
- `+ OnChipDestroy(Cell mainCell): void`
    - **Purpose**: Unsubscribes from CellObserverManager and cleans up before the chip is removed.
    - **Usage**: Called by the base Destroy path or directly when the chip is cleared from the field.
    - **Params**: mainCell - the cell this chip occupies
- `+ OnObservedCellChipChanged(ChipChangedEvent evt): void`
    - **Purpose**: Receives chip-change notifications for any watched neighboring cell.
    - **Usage**: Invoked by CellObserverManager once per frame flush
    - base implementation only logs debug information when chip.LogEnable is true.
    - **Params**: evt - event payload containing the changed cell and old/new chip references
- `~ Awake(): void`
- `~ GetAllChipsByType(List<Vector2Int> cellPositions, HashSet`1& chips): void`
    - **Purpose**: Collects unique neighboring chips matching a target reference type into a reusable set.
    - **Usage**: Call from derived subscribers when recomputing tracked neighbors after movement or subscription changes.
    - **Params**: cellPositions - observed grid positions to scan
    - chips - destination set passed by ref and cleared before population
    - **Notes**: Uses caller-owned HashSet to avoid per-call allocations
    - only main chip references present on scanned cells are added.
- `~ SubscribeToNeighbors(Vector2Int origin): void`
    - **Purpose**: Computes all cells that border the chip's occupied area and subscribes to them via CellObserverManager.
    - **Usage**: Called from OnChipChangedCell with the target cell position after previous subscriptions are removed.
    - **Params**: origin - top-left position of the chip on the grid
    - **Notes**: Supports multi-cell chips: iterates the expanded bounding box (origin-1 .. origin+size) and excludes cells owned by the chip itself. Skips out-of-bounds positions.
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
- `++ LogEnable: bool`
- `+- BlockingState: CombinedBlockingState`
- `+- CellSubscriber: ICellSubscriber`
- `+- Data: ChipData`
- `+- MergeData: ChipMergeData`
- `+- RuntimeData: ChipRuntimeData`
- `+- SortingLayer: IChipSortingLayer`
- `~ animator: Animator`
- `~ chipChangeNotifier: IChipChangeNotifier`
- `~ effectOfPrioritizingDestroying: IEffect`
- `~ effects: Dictionary<int, IEffect>`
- `~ extraEffectsData: ChipExtraEffectsData`
- `~ fieldGrid: IFieldGrid`
- `~ isDragging: bool`
    - **Purpose**: Tracks whether the chip is currently being dragged by the user
    - **Usage**: Set via SetDragging
    - queried via IsDragging
    - used to distinguish user drag from automated movement
    - **Notes**: Separate from IsMoving which tracks visual/sorting state
    - allows detection of user-initiated drag vs system movement
- `~ isMoving: bool`
    - **Purpose**: Tracks whether the chip is currently in a visual movement state (e.g., being dragged or relocated)
    - **Usage**: Set via SetMoving
    - used to adjust sorting layers and notify effects that visual position is changing
    - **Notes**: When true, sortingLayer.SetMoving(true) is called to lift the chip visually
- `- lastTrigger: string`
#### Methods
- `+ CanMoving(): bool`
    - **Purpose**: Checks whether the chip can currently be moved by the player
    - **Usage**: Called by DraggableChipLogic before allowing drag operations
    - returns false if chip is locked
    - **Returns**: True if chip can be moved
    - false if movement is locked
    - **Notes**: Based on runtimeData.IsMoveLocked
    - prevents drag-and-drop when locked
- `+ Destroy(Cell mainCell): void`
    - **Purpose**: Destroys the chip and all its attached effects
    - **Usage**: Call to remove the chip from the field and scene
    - override in derived classes for custom teardown before or after base destruction
    - **Params**: mainCell - the chip's main occupied cell on the field grid
    - **Notes**: Clears grid occupancy via FieldGrid first (which enqueues chip-change notifications), then invokes ICellSubscriber cleanup while cell context is still valid, destroys spawned effect objects, and finally schedules GameObject destruction with a short delay (0.1s)
- `+ Init(ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the chip with data and sets up all required components and effects
    - **Usage**: Call after creating a chip instance to assign data and prepare effects
    - override in derived classes for custom initialization
    - **Params**: data - the data object containing chip configuration and effect prefabs
    - **Notes**: Creates highlight and merge-available effects if prefabs are provided
    - logs errors if required components are missing
- `+ InitRuntimeData(ChipData data, ChipRuntimeData& runtimeData): void`
- `+ IsDragging(): bool`
    - **Purpose**: Checks if the chip is currently being dragged by the user
    - **Usage**: Use to determine if the chip is in a user-initiated drag state
    - **Returns**: True if the chip is being dragged by user input
    - **Notes**: Separate from IsMoving
    - tracks user drag state specifically, not just visual movement
- `+ IsMoving(): bool`
    - **Purpose**: Checks if the chip is currently in a moving state based on its sorting order
    - **Usage**: Use to determine if the chip is visually moving (either by user drag or system relocation)
    - **Returns**: True if the chip's sorting order indicates it is moving
    - **Notes**: Depends on sorting order being set to 2 in SetMoving
    - returns true for both user drag and automated movement
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
- `+ OnDraggingChipWithMoveLocked(): void`
    - **Purpose**: Provides visual feedback when user attempts to drag a chip that is locked and cannot be moved
    - **Usage**: Called by DraggableChipLogic.OnDragStart when CanMoving() returns false
    - triggers 'MoveLocked' animation on both chip and effect
    - **Notes**: First tries effectOfPrioritizingDestroying (highest-priority destroying effect), then falls back to EffectConsts.Extra.MoveLockedEffect
    - uses allowRepeat=true to ensure feedback on every attempt
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
    - **Purpose**: Notifies all chip effects when the cell under the dragged chip changes
    - **Usage**: Called by DraggableChipLogic to update effects that react to being held over a specific cell (e.g., potential interaction targets)
    - **Params**: underCell - The cell currently directly under the chip
    - overCell - The cell above which the chip is currently positioned
    - **Notes**: Broadcasts event to all effects in the dictionary
- `+ OnNeighborsChipOfMerged(): void`
    - **Purpose**: Called when a neighboring chip merges with another chip, notifying this chip of the merge event
    - **Usage**: Override in derived classes to react to neighboring merges
    - default implementation handles effect destruction logic
    - **Notes**: Called from MergeableChipLogic.ExecuteInteraction before destroying neighboring chips
- `+ OnTap(Vector2 position): void`
    - **Purpose**: Called when the chip is tapped by the user
    - **Usage**: Override in derived classes to implement custom tap behavior
    - receives tap position in world coordinates
    - **Params**: position - the world position where the chip was tapped
- `+ SendTrigger(AnimatorTrigger trigger): void`
- `+ SendTrigger(string trigger): void`
- `+ SetDragging(bool value): void`
    - **Purpose**: Sets the drag state of the chip and ensures visual consistency
    - **Usage**: Called by DraggableChipLogic when user drag starts (true) or ends (false)
    - **Params**: value - true when user starts dragging, false when drag ends
    - **Notes**: Automatically calls SetMoving(true) if chip is not already moving
    - separates user drag state from movement state
- `+ SetMoving(bool value): void`
    - **Purpose**: Updates the sorting order of the chip during movement (user drag or system relocation)
    - **Usage**: Called when movement starts (true) or ends (false)
    - used for both user dragging and automated chip movement
    - **Params**: value - true if starting movement, false if ending movement
    - **Notes**: Sets sorting order to 2 for moving chips to ensure they're on top
    - on move-start it enqueues a chip-change event with NewChip=null for the current cell so observer-based systems can immediately react to temporary chip departure
    - calls UpdateVisual when movement ends
- `+ UpdateVisual(): void`
    - **Purpose**: Updates the visual state of the chip based on its runtime data
    - **Usage**: Call after modifying runtimeData.EffectEnables to synchronize visual effects
    - **Notes**: Iterates blockersData.Blockers and activates effects whose EffectId is in EffectEnables
    - also activates CellHighlight unless hidden by BlockingState.HideEffectIds
- `~ AddEffect(IEffect effect, int effectHash, bool activate, bool deactivate): void`
    - **Purpose**: Adds an effect to the effects dictionary and optionally activates it
    - **Usage**: Call from InitEffects to register effects with their EffectConsts keys
    - **Params**: effect - The effect instance to add
    - effectHash - EffectConsts hash value
    - activate - Whether to immediately activate the effect
    - **Notes**: Null-safe: effect is only added if not null
    - handles activation before storing
- `~ DestroyEffects(): void`
- `~ GetEffect(int effectHash): IEffect`
    - **Purpose**: Retrieves an effect from the effects dictionary by its EffectConsts hash key
    - **Usage**: Call in methods that need to access a specific effect without type casting
    - **Params**: effectHash - One of EffectConsts values (e.g., EffectConsts.MoveLocked)
    - **Returns**: The IEffect instance or null if not found
    - **Notes**: Use GetEffect<T>() for type-safe access when specialized interfaces are needed
- `~ GetEffect(int effectHash): T`
    - **Purpose**: Retrieves and casts an effect from the effects dictionary by its EffectConsts hash key
    - **Usage**: Call when you need a specialized effect interface (e.g., IEffectGeneratorCharging)
    - **Params**: effectHash - One of EffectConsts values (e.g., EffectConsts.GeneratorCharging)
    - T - Target effect interface type
    - **Returns**: The effect cast to type T, or null if not found or cannot be cast
    - **Notes**: Handles null safely with null-conditional operator (?.) at callsite
- `- HandleDestroyingEffects(): void`
- `~ InitDestroyingEffectsData(): void`
    - **Purpose**: Scans all registered effects for DestroyingSettings and initializes their runtime destroying data
    - **Usage**: Called from PostInitEffects after all effects are added to the dictionary
    - **Notes**: Creates EffectDestroyingRuntimeData entries in runtimeData.EffectDestroyingData for effects with non-null DestroyingSettings
    - then selects the highest-priority destroying effect via UpdatePrioritizingDestroyingEffect
- `~ InitEffects(): void`
    - **Purpose**: Initializes all effects for the chip by instantiating effect prefabs and adding them to the effects dictionary
    - **Usage**: Called from Init
    - override in derived classes to add custom effects while maintaining base effect initialization
    - **Notes**: First iterates ChipExtraEffectsData.Blockers and instantiates only effects whose EffectId is present in runtimeData.EffectEnables
    - then specifically looks for ShadowEffect in blockersData.BlockersDict and instantiates it if found
    - finally creates CellHighlightEffect and ChipMergeAvailableEffect from ChipData prefabs
    - designed for virtual extension pattern
- `~ InstantiateEffect(GameObject prefab): T`
    - **Purpose**: Instantiates an effect prefab and initializes it with the current chip
    - **Usage**: Call during Init or whenever a new visual effect needs to be added to the chip
    - **Params**: prefab - The GameObject prefab containing the effect component
    - T - The type of the effect component (must implement IEffect)
    - **Returns**: The instantiated effect component of type T, or null if prefab is null
    - **Notes**: Automatically calls Init(this) on the instantiated effect
    - return value can be added to the effects list
    - T must be a class and implement IEffect
- `~ NotifyEffectsOnChangedCell(Cell sourceCell, Cell targetCell): void`
- `~ NotifyEffectsOnMovingStateChanged(bool isMoving): void`
    - **Purpose**: Broadcasts movement state changes to all attached effects
    - **Usage**: Called from SetMoving when a chip starts or ends visual movement
    - **Params**: isMoving - true if movement started, false if it ended
- `~ PostInitEffects(): void`
- `~ RemoveEffect(int effectId): void`
    - **Purpose**: Removes a destroying effect from the chip after it reaches destruction threshold
    - **Usage**: Called from HandleDestroyingEffects when TryDestroyEffect returns true
    - **Params**: effectId - The unique hash/ID of the effect to remove
    - **Notes**: Deactivates the effect, removes it from dictionary, and updates the priority selection
- `- UpdatePrioritizingDestroyingEffect(): void`
    - **Purpose**: Selects the effect with the highest DestroyingSettings.Priority as the active destroying target
    - **Usage**: Called after InitDestroyingEffectsData and after RemoveEffect to reselect the next priority
    - **Notes**: Sets effectOfPrioritizingDestroying to null if no effects with DestroyingSettings remain
---

## ChipChangedEvent

> - **Purpose**: Describes a chip change on a single cell.
> - **Usage**: Produced by FieldGrid.SetChipInCell and delivered once per frame via DeferredChipChangeNotifier.
#### Fields
- `+- Cell: Cell`
- `+- ChipAdded: bool`
- `+- ChipRemoved: bool`
- `+- ChipReplaced: bool`
- `+- NewChip: Chip`
- `+- OldChip: Chip`
#### Methods
- `+ ToString(): string`
---

## ChipContainer
**Inherits**: `Chip`

> - **Purpose**: Represents a chip that can contain other chips, managing their addition and triggering effects when filled
> - **Usage**: Use as a base class for chips that act as containers for other chips
> - Call Init with ChipData to initialize
> - Use TryAddChip to attempt to add a chip
> - **Params**: data - ChipData for initialization
> - chip - Chip to check or add
> - containers - runtime state of container requirements
> - **Returns**: IsChipCompatible and TryAddChip return true if the chip can be added or is compatible, false otherwise
> - **Notes**: Handles container fill logic and triggers effects when full
> - Removes itself and spawns a new chip if all containers are filled
> - Uses event OnFillContainer for notification
> - Requires proper ChipData with ChipContainerData
#### Fields
- `~ chipContainerData: ChipContainerData`
- `- chipFactory: ChipFactory`
- `- containerEffect: EffectContainerRef`
    - **Purpose**: Visual effect reference for container item display
    - obtained from Inspector via InterfaceRef
    - **Usage**: Used in InitEffects to instantiate and add the effect to the effects dictionary with EffectConsts.ContainerRequirements key
    - **Notes**: Assigned via inspector
    - effect is stored in dictionary for lifetime management
- `~ containerRuntimeData: ChipContainerRuntimeData`
- `- OnFillContainer: FillContainerDelegate`
#### Methods
- `+ Init(ChipData data, ChipRuntimeData runtimeData): void`
- `+ InitRuntimeData(ChipData data, ChipRuntimeData& runtimeData): void`
- `+ IsChipCompatible(Chip chip): bool`
    - **Purpose**: Checks if a given chip is compatible with any of the container's remaining requirements
    - **Usage**: Called by interaction logic to determine if a chip can be dropped into this container
    - **Params**: chip - The chip to check for compatibility
    - **Returns**: True if the chip matches a required ChipType or ChipId
    - false otherwise
    - **Notes**: Does not modify the container's state
    - only performs a check against current requirements
- `+ SetMoving(bool value): void`
    - **Purpose**: Updates sorting order and visual state during movement
    - **Usage**: Called from SetMoving(true/false) when movement starts or ends
    - **Params**: value - true if starting movement, false if ending
    - **Notes**: Delegates to base.SetMoving then refreshes visuals when stopping
- `+ TryAddChip(Chip chip): bool`
    - **Purpose**: Attempts to add a chip to the container, updating progress and handling completion logic.
    - **Usage**: Called by interaction logic when a chip is dropped onto the container.
    - **Params**: chip - The chip being added.
    - **Returns**: True if the chip was successfully added
    - otherwise False.
    - **Notes**: Side effects: Updates internal container state
    - Triggers OnFillContainer event
    - If full, destroys parent Cell content and spawns NextChipData result.
- `+ UpdateVisual(): void`
- `~ InitEffects(): void`
---

## ChipContainerData
#### Fields
- `+ containers: ContainerInfo[]`
- `+ NextChipData: ChipData`
---

## ChipContainerEffect
**Inherits**: `Effect`

> - **Purpose**: Visual effect controller for ChipContainer, managing spawned elements inside the container
> - **Usage**: Attached to ChipContainer prefab
> - calls UpdateElements when container state changes
> - **Params**: chip - owner chip
> - containers - current state of containers
> - isFull - completion flag
> - **Notes**: Instantiates and positions element prefabs based on ContainerInfo
> - Handles activation/deactivation animations
#### Fields
- `- elementSortingOrder: int`
- `- layoutConfigs: LayoutConfig[]`
- `- layoutForElements: Transform`
- `- panelSpriteRenderer: SpriteRenderer`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ UpdateElements(Chip chip, Dictionary<ContainerInfo, int> containers, bool isFull): void`
    - **Purpose**: Updates the visual representation of container requirements based on current state.
    - **Usage**: Called by ChipContainer when an item is added or the container initializes.
    - **Params**: chip - owner chip
    - containers - current remaining requirements
    - isFull - true if all requirements are met.
    - **Notes**: Dynamically instantiates UI elements (bubbles) and resizes the background panel. Deactivates effect if isFull is true.
- `- ClearElements(): void`
- `- Merge2.IEffect.get_gameObject(): GameObject`
---

## ChipContainerRuntimeData
**Inherits**: `ChipRuntimeData`

> - **Purpose**: Extends ChipRuntimeData to store runtime state specific to container chips, tracking fill progress for each container requirement
> - **Usage**: Used by ChipContainer to track which items have been added and how many remain
> - initialized in ChipContainer.Init
> - updated in TryAddChip
> - **Notes**: Inherits IsMoveLocked from ChipRuntimeData
> - containers dictionary maps requirements to current count
#### Fields
- `+ containers: Dictionary<ContainerInfo, int>`
    - **Purpose**: Tracks the current fill progress for each container requirement
    - **Usage**: Key is ContainerInfo (requirement definition)
    - Value is current count of items added
    - updated when chips are added via TryAddChip
    - **Notes**: When a container requirement is fully met, it is removed from this dictionary
    - empty dictionary means container is full
---

## ChipData
**Inherits**: `ScriptableObject`
#### Fields
- `++ Size: Vector2Int`
- `+ CellHighlightPrefab: GameObject`
- `+ MergeAvailableEffectPrefab: GameObject`
- `+ PrefabLink: GameObject`
- `+ Type: string`
- `- specialDatas: List<IChipSpecialData>`
#### Methods
- `+ AddSpecialData(IChipSpecialData specialData): void`
- `+ Clone(): ChipData`
    - **Purpose**: Creates a deep copy of this ChipData asset
    - **Usage**: Call at runtime to create a unique instance of ChipData that can be modified without affecting the original asset
    - **Returns**: A new ChipData instance with identical field values
- `+ CreateSpecialData(): T`
- `+ GetSpecialData(): T`
- `- OnEnable(): void`
---

## ChipDataCollection
**Inherits**: `ScriptableObject`
#### Fields
- `++ Data: ChipData[]`
#### Methods
- `+ GetChipData(string chipName): ChipData`
---

## ChipExtraEffectsData

> - **Purpose**: Configures available extra blocker/overlay effects for a chip type via name-to-prefab mappings
> - **Usage**: Added to ChipData.specialDatas
> - Chip.InitEffects iterates Blockers and instantiates only those whose EffectId is in runtimeData.EffectEnables
> - **Notes**: BlockersDict provides O(1) lookup by EffectId
> - built during serialization callbacks
#### Fields
- `+- Blockers: ExtraEffectData[]`
- `+- BlockersDict: Dictionary<int, ExtraEffectData>`
- `+- OtherEffects: ExtraEffectDataEx[]`
- `+- OtherEffectsDict: Dictionary<int, ExtraEffectDataEx>`
#### Methods
- `+ OnAfterDeserialize(): void`
- `+ OnBeforeSerialize(): void`
- `- UpdateDict(): void`
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
- `+ CreateChip(Cell cell, ChipData chipData, Nullable<Vector3> parentWorldPosition, Action<ChipRuntimeData> runtimeDataInitializer): Chip`
- `+ CreateChip(Vector2Int cellPosition, ChipData chipData, Nullable<Vector3> parentWorldPosition, Action<ChipRuntimeData> runtimeDataInitializer): Chip`
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
- `+- JoinPoints: IReadOnlyList<Transform>`
- `+- PowerBoosterModifiers: HashSet<ChipPowerBooster>`
- `+- RuntimeDataOnlyEditor: ChipGeneratorRuntimeData`
- `- chargedEffect: EffectRef`
    - **Purpose**: Visual effect active when the generator is fully charged and ready to generate chips.
    - **Usage**: Assigned via inspector as EffectRef
    - used in InitEffects to add to effects dictionary
    - accessed via GetEffect(EffectConsts.GeneratorCharged)
    - **Notes**: Optional
    - targets IEffect interface
    - provides visual feedback for readiness.
- `- chipFactory: ChipFactory`
- `- field: IFieldEventHandler`
- `- findFreePlaceForChip: IFreeCellFinder`
- `- generatorData: ChipGeneratorData`
    - **Purpose**: Stores static configuration for the chip generator.
    - **Usage**: Initialized in Init from ChipData. Used for generation logic.
    - **Notes**: Should not be null. Affects generator mode and chip creation.
- `~ generatorRuntimeData: ChipGeneratorRuntimeData`
    - **Purpose**: Stores runtime state for the chip generator.
    - **Usage**: Initialized in Init. Tracks charge, waiting state, and charge count.
    - **Notes**: Critical for generator logic and state transitions.
- `- isAutoGeneration: bool`
    - **Purpose**: Indicates if the generator operates in auto mode.
    - **Usage**: Set in Init from generatorData. Controls event subscriptions and tap behavior.
    - **Notes**: Affects event handling and chip generation triggers.
- `- OnCharging: Action<float>`
- `~ powerMultiplier: float`
    - **Purpose**: Current generation speed multiplier contributed by nearby power boosters.
    - **Usage**: Updated when boosters are applied/removed
    - consumed by Update() charging logic to scale Time.deltaTime.
    - **Notes**: Defaults to 1f (no boost) and tracks the highest active booster power.
- `- rechargeEffect: EffectGeneratorChargingRef`
    - **Purpose**: Effect component for visual feedback during charging (recharge cycle).
    - **Usage**: Assigned via inspector as EffectGeneratorChargingRef
    - used in InitEffects to add to effects dictionary
    - accessed via GetEffect(EffectConsts.GeneratorCharging)
    - **Notes**: Must not be null if charging visualization is required
    - targets IEffectGeneratorCharging interface.
#### Methods
- `+ ApplyPowerBoosterModifier(ChipPowerBooster chipPowerBooster, bool reapply): bool`
    - **Purpose**: Applies a booster to the generator and recalculates effective multiplier.
    - **Usage**: Called when booster subscriber detects this generator entering observed range.
    - **Params**: chipPowerBooster - booster to register as active modifier
    - **Returns**: True when the modifier was newly added
    - false when it was already present.
    - **Notes**: Multiplier becomes the maximum power among active boosters to prevent stacking by sum.
- `+ Destroy(Cell mainCell): void`
    - **Purpose**: Cleans up generator-owned callbacks and subscriptions before the base chip destruction flow runs.
    - **Usage**: Call when removing a ChipGenerator from the field
    - this override must execute before base.Destroy so auto-mode callbacks cannot fire against a chip that is already being removed.
    - **Params**: mainCell - the generator main cell used by base destruction to clear occupancy
    - **Notes**: Resets OnCharging delegates and unsubscribes from field.OnChangeField in auto mode, then delegates to base.Destroy for grid/effect/GameObject cleanup.
- `+ Init(ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the chip generator with provided data and sets up event subscriptions.
    - **Usage**: Call when creating or resetting a ChipGenerator instance.
    - **Params**: data - ChipData for initialization, must contain valid ChipGeneratorData.
    - **Notes**: Handles event subscriptions, effect activation, and runtime state setup.
- `+ InitRuntimeData(ChipData data, ChipRuntimeData& runtimeData): void`
- `+ NotifyEffectRemoved(int effectId): void`
- `+ OnTap(Vector2 position): void`
    - **Purpose**: Handles tap input for manual chip generation.
    - **Usage**: Call when the player taps the generator in manual mode.
    - **Params**: position - Tap position in world coordinates.
    - **Notes**: No effect in auto mode or if not charged. Uses TryGenerateChip for logic.
- `+ RemovePowerBoosterModifier(ChipPowerBooster chipPowerBooster): void`
    - **Purpose**: Removes a booster modifier and recalculates multiplier from remaining active boosters.
    - **Usage**: Called when booster subscriber detects this generator leaving observed range or booster destruction.
    - **Params**: chipPowerBooster - booster to remove from active modifier set
    - **Notes**: Resets multiplier to 1f when no boosters remain
    - logs warning if removal is requested for a non-registered booster.
- `+ SetMoving(bool value): void`
    - **Purpose**: Updates dragging state and deactivates charged effect during drag
    - **Usage**: Called when drag starts or ends
    - deactivates charged effect to prevent visual clutter
    - **Params**: value - true if starting drag, false if ending drag
    - **Notes**: Ensures the 'charged' visual doesn't obscure the field while the user is positioning the generator
- `+ UpdateVisual(): void`
    - **Purpose**: Updates the visual state of the generator, including charge and move-locked effects
    - **Usage**: Overridden to manage generator-specific effects (charging vs. charged)
    - called by base or when charging state changes
    - **Notes**: Synchronizes activation state of charging and charged effects based on generatorRuntimeData.IsCharged
- `~ InitEffects(): void`
    - **Purpose**: Initializes generator-specific effects including charged and recharge visual feedback
    - **Usage**: Called from Init via InitEffects chain
    - adds generator-specific effects to the effects dictionary
    - **Notes**: Calls base.InitEffects() first to ensure standard effects are initialized before adding generator effects
- `- OnFieldChanged(): void`
    - **Purpose**: Handles field change events for auto-generation mode.
    - **Usage**: Automatically called when the field changes if in auto mode.
    - **Notes**: Triggers chip generation if waiting for space.
- `- RecalculatePowerMultiplier(ChipPowerBooster chipPowerBooster): bool`
    - **Purpose**: Recalculates the effective power multiplier based on active boosters and blocking state
    - **Usage**: Called from ApplyPowerBoosterModifier after adding or reapplying a booster
    - **Params**: chipPowerBooster - the booster that triggered the recalculation
    - **Returns**: True if the multiplier was applied
    - false if CanReceiveModifiers is blocked
    - **Notes**: When CanReceiveModifiers is false, resets multiplier to 1f regardless of active boosters
- `~ RemoveEffect(int effectId): void`
    - **Purpose**: Extends base effect removal to notify all active boosters about the change
    - **Usage**: Called when a destroying effect reaches its threshold on this generator
    - **Notes**: After base removal and blocking state update, calls NotifyEffectRemoved so boosters can reapply if previously blocked
- `- TryGenerateChip(): void`
    - **Purpose**: Attempts to generate a new chip, managing charge state, recharges, and generator lifecycle.
    - **Usage**: Call when generator is charged. Triggered by tap (manual) or field change (auto).
    - **Notes**: Decrements ChargeCount. Checks GenerationInterval if ChargeCount > 0. Uses 'onlyAround' search only if IsAutoGeneration is true.
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
- `+ GenerationInterval: float`
    - **Purpose**: Defines the cooldown time between consecutive generations in a single charge cycle
    - **Usage**: Used by ChipGenerator to set delay between spawns when multiple charges are available
    - **Notes**: Only applies if ChargeCount > 1
- `+ IsAutoGeneration: bool`
- `+ IsStartCharged: bool`
- `+ NextChipData: ChipData`
- `+ TotalRecharges: int`
#### Methods
- `+ GenerateChipData(): ChipData`
---

## ChipGeneratorRechargeEffect
**Inherits**: `Effect`

> - **Purpose**: Visual effect handler for ChipGenerator, managing charging progress and activation states.
> - **Usage**: Attached to ChipGenerator prefab
> - referenced by ChipGenerator to visualize charging.
> - **Notes**: Updates maskRectTransform based on charging progress.
#### Fields
- `- EraseLevelId: int`
- `- maskMaterial: Material`
- `- maskSpriteRenderer: SpriteRenderer`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ Init(Chip chip, int effectHash): void`
- `+ OnCharging(float progress): void`
    - **Purpose**: Updates the visual state of charging based on progress
    - **Usage**: Called via event from ChipGenerator during update loop
    - **Params**: progress - value between 0 and 1 indicating charge percentage
- `- Merge2.IEffect.get_gameObject(): GameObject`
---

## ChipGeneratorRuntimeData
**Inherits**: `ChipRuntimeData`

> - **Purpose**: Stores runtime state for a chip generator, tracking its charge status and remaining recharges.
> - **Usage**: Created and managed by ChipGenerator. Used to persist state during gameplay.
> - **Notes**: Updated during generation and charging cycles.
#### Fields
- `+ ChargeCount: int`
- `+ ChargingTimeLeft: float`
- `+ CurrentTargetChargingTime: float`
    - **Purpose**: The current target duration for the charging phase
    - **Usage**: Set based on either ChargingTime (full recharge) or GenerationInterval (between generations)
    - **Notes**: Modified by ChipGenerator to control timing dynamically
- `+ IsCharged: bool`
- `+ IsWaitingForSpace: bool`
- `+ RechargesLeft: int`
---

## ChipMergeAvailableEffect
**Inherits**: `Effect`

> - **Purpose**: Visual effect indicating that a merge is available with the chip below
> - **Usage**: Attached to chip using ChipData.MergeAvailableEffectPrefab
> - Activated by Chip when another compatible chip is dragged over it
> - **Notes**: Handles auto-sizing and positioning based on chip size
#### Fields
- `- autoPosition: bool`
- `- isActive: bool`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ OnInteractionUnderCellChanged(Cell underCell, Cell overCell): void`
---

## ChipMergeData

> - **Purpose**: Defines merge logic and potential outcomes for a chip
> - **Usage**: Component of ChipData
> - used to determine merge compatibility and resulting chips
#### Fields
- `++ Combinations: MergeCombination[]`
#### Methods
- `+ CanMerge(ChipData otherChip): bool`
    - **Purpose**: Checks if this chip can be merged with another specific chip
    - **Usage**: Used by interaction systems to validate possible merge actions
    - **Params**: otherChip - the potential merge partner
    - **Returns**: True if merge is possible
- `+ GetNextChip(ChipData otherChip): MergeResult`
    - **Purpose**: Retrieves the result of merging this chip with another specific chip
    - **Usage**: Called during merge process to determine outcome
    - returns null if chips are incompatible
    - **Params**: otherChip - the chip to merge with
    - **Returns**: New ChipData or null
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
    - chipsToExclude - chips to ignore during collision checks
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

## ChipPowerBooster
**Inherits**: `Chip`

> - **Purpose**: Chip that boosts nearby IPowerBoosterModifier entities and controls linked booster-specific effects.
> - **Usage**: Attach to booster chip prefabs together with PowerBoosterCellSubscriber to auto-apply/remove modifiers as neighbor composition changes.
> - **Notes**: Coordinates numeric power boost state and optional join/highlight effects for movement and destruction lifecycles.
#### Fields
- `+- ModifiedEntities: HashSet<IPowerBoosterModifier>`
- `+- Power: float`
- `~ cellSubscriber: PowerBoosterCellSubscriber`
- `~ chipPowerBoosterData: ChipPowerBoosterData`
- `~ connectorCellsHighlightEffect: EffectRef`
    - **Purpose**: Optional effect reference that visualizes currently observed booster coverage cells.
    - **Usage**: Initialized in InitEffects and toggled by movement/visual updates
    - **Notes**: When missing, booster still functions but no connector cell highlighting is shown.
- `~ joinEffect: EffectPowerBoosterJoinRef`
    - **Purpose**: Optional effect reference that draws dynamic join links between booster and modified generators.
    - **Usage**: Receives OnJoin/OnLeave callbacks when modifiers are applied or removed
    - **Notes**: Deactivated when booster starts moving to prevent stale visual links.
#### Methods
- `+ ApplyPowerBoosterModifier(IPowerBoosterModifier generator, bool reapply): void`
    - **Purpose**: Applies booster influence to a modifier target and notifies join effect.
    - **Usage**: Called by PowerBoosterCellSubscriber when a matching modifier enters observed cells.
    - **Params**: generator - modifier-capable chip/entity receiving this booster
- `+ Destroy(Cell mainCell): void`
    - **Purpose**: Ensures booster subscriptions/modifiers are cleared before base chip destruction.
    - **Usage**: Called by chip lifecycle when booster is removed from field.
    - **Params**: mainCell - booster main grid cell at destruction time
    - **Notes**: Explicitly invokes subscriber cleanup before base.Destroy to release modifier links deterministically.
- `+ Init(ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes booster-specific dependencies and validates booster data payload.
    - **Usage**: Called after creation
    - must run before boost application logic and effect usage.
    - **Params**: data - chip data expected to contain ChipPowerBoosterData
    - **Notes**: Caches PowerBoosterCellSubscriber and ChipPowerBoosterData
    - logs errors and exits early when required components/data are missing.
- `+ OnTargetChipEffectRemoved(IPowerBoosterModifier chipTarget, int effectId): void`
    - **Purpose**: Reapplies booster influence when a target modifier's blocking state changes due to effect removal
    - **Usage**: Called by IPowerBoosterModifier.NotifyEffectRemoved on each active booster when a modifier effect is removed
    - **Params**: chipTarget - modifier entity whose effect was removed
    - effectId - ID of the removed effect
    - **Notes**: Only reapplies if the target now has CanReceiveModifiers true
    - triggers joinEffect.OnJoin if reapply succeeds
- `+ RemovePowerBoosterModifier(IPowerBoosterModifier generator): void`
    - **Purpose**: Removes booster influence from a modifier target and notifies join effect.
    - **Usage**: Called by PowerBoosterCellSubscriber when a matching modifier leaves observed cells or gets removed.
    - **Params**: generator - modifier-capable chip/entity losing this booster
- `+ SetMoving(bool value): void`
    - **Purpose**: Handles movement-state transitions and immediately deactivates booster-only effects on move start.
    - **Usage**: Called by drag/movement flows with true on start and false on stop.
    - **Params**: value - true to start moving, false to stop
    - **Notes**: Always delegates sorting/notification behavior to base.SetMoving first.
- `+ UpdateVisual(): void`
    - **Purpose**: Updates booster visual state while preserving base chip visuals.
    - **Usage**: Called when runtime state changes and when movement state toggles.
    - **Notes**: Connector cell highlight is active only while the booster is stationary.
- `~ InitEffects(): void`
    - **Purpose**: Initializes optional booster effects in addition to base chip effects.
    - **Usage**: Called from base Init flow.
    - **Notes**: Adds connectorCellsHighlightEffect and joinEffect to effects dictionary via AddEffect with their respective EffectConsts keys.
- `~ RemoveEffect(int effectId): void`
    - **Purpose**: Re-evaluates booster influence when an effect is removed, potentially re-enabling blocked modifiers
    - **Usage**: Called from base.RemoveEffect after an extra effect is destroyed and blocking state changes
    - **Notes**: If CanApplyModifiers becomes true after removal, triggers OnChangedCell to re-subscribe and reapply modifiers to neighbors
---

## ChipPowerBoosterData
#### Fields
- `++ Power: float`
---

## ChipRuntimeData

> - **Purpose**: Stores runtime state for chips during gameplay, including dynamic properties that change during game execution
> - **Usage**: Base class for all chip runtime data
> - extend for specific chip types
> - initialized in Chip.Init and updated during gameplay
> - **Notes**: Serializable for save/load support
> - contains only runtime state, not configuration data
#### Fields
- `+ EffectDestroyingData: Dictionary<int, EffectDestroyingRuntimeData>`
    - **Purpose**: Tracks per-effect destruction progress for effects that have DestroyingSettings configured
    - **Usage**: Key is EffectId
    - value contains neighboring merge count that increments each merge event
    - initialized by Chip.InitDestroyingEffectsData
    - **Notes**: When NeighboringMergeCount reaches threshold, the effect is removed via Chip.RemoveEffect
- `+ EffectEnables: HashSet<int>`
    - **Purpose**: Set of extra effect IDs (from EffectConsts/EffectConsts.Extra) that should be active on this chip
    - **Usage**: Populated from FieldChipData.ExtraEffectIds during level load or at runtime
    - queried by Chip.InitEffects and UpdateVisual to determine which extra effects to instantiate and activate
    - **Notes**: Serialized via effectEnablesArray for save/load support
- `- effectDestroyingDataArray: EffectDestroyingRuntimeData[]`
- `- effectEnablesArray: int[]`
#### Methods
- `+ OnAfterDeserialize(): void`
- `+ OnBeforeSerialize(): void`
---

## ChipSortingLayer
**Inherits**: `MonoBehaviour`

> - **Purpose**: Implements movement-based sorting order adjustments for chip renderers
> - **Usage**: Attach to chip prefab
> - initialized by Chip.Init
> - handles visual depth changes during dragging
> - **Notes**: Supports multiple renderers, ensuring each one maintains its relative depth while moving
#### Fields
- `+- SortingLayers: SortingLayerData[]`
#### Methods
- `+ Init(): void`
    - **Purpose**: Caches the initial sorting orders of all renderers
    - **Usage**: Called from Chip.Init before any movement occurs
    - **Notes**: Required for restoring correct sorting order after movement ends
- `+ SetMoving(bool value): void`
    - **Purpose**: Adjusts sorting orders of all renderers based on movement state
    - **Usage**: Called from Chip.SetMoving(true/false) when movement starts or ends
    - **Params**: value - true if starting movement (increases order), false if stopping (restores order)
    - **Notes**: Uses AdditionallyWhenMoving offset defined in inspector for each renderer
---

## CombinedBlockingState

> - **Purpose**: Runtime aggregate of all active effect blocking settings on a chip
> - **Usage**: Accessed via Chip.BlockingState
> - game systems check flags (CanBeMoved, CanBeMergedAsSource, etc.) before allowing actions
> - **Notes**: ApplyBlock uses AND logic for boolean flags and OR for IsLittleChip
> - RemoveBlock triggers full Recalculate from remaining blocks
> - HideEffectIds is the union of all active blocks
#### Fields
- `+~ CanApplyModifiers: bool`
- `+~ CanBeFilled: bool`
- `+~ CanBeMergedAsSource: bool`
- `+~ CanBeMergedAsTarget: bool`
- `+~ CanBeMoved: bool`
- `+~ CanBeTaped: bool`
- `+~ CanGenerate: bool`
- `+~ CanReceiveModifiers: bool`
- `+- HideEffectIds: HashSet<int>`
- `+~ IsLittleChip: bool`
- `- appliedBlocks: HashSet<IEffectBlockingSettings>`
#### Methods
- `+ ApplyBlock(IEffectBlockingSettings blockSettings): void`
- `+ RemoveBlock(IEffectBlockingSettings blockSettings): void`
- `- Recalculate(): void`
- `~ Reset(): void`
---

## ContainerInfo
#### Fields
- `+ Count: int`
- `+ Type: ContainerType`
- `+ TypeOrId: string`
- `+- ContainerElementPrefab: GameObject`
---

## DeferredChipChangeNotifier

> - **Purpose**: Plain-class implementation of IChipChangeNotifier.
> - **Usage**: Register as Singleton in VContainer
> - no MonoBehaviour needed.
#### Fields
- `- OnFlush: Action<IReadOnlyList<ChipChangedEvent>>`
- `- pending: List<ChipChangedEvent>`
    - **Purpose**: Stores pending per-frame chip change events in insertion order.
    - **Usage**: Read by Flush and then cleared.
- `- pendingIndexByCell: Dictionary<Cell, int>`
    - **Purpose**: Maps changed cell to its index in pending list.
    - **Usage**: Allows O(1) collapse of repeated updates for the same cell within one frame.
#### Methods
- `+ Enqueue(Cell cell, Chip oldChip, Chip newChip): void`
    - **Purpose**: Enqueues or merges chip change for a cell.
    - **Usage**: If the same cell changed earlier in the frame, keeps the first OldChip and updates only NewChip.
    - **Notes**: Uses pendingIndexByCell for O(1) merge instead of linear scan.
- `+ Flush(): void`
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
- `+- BlockingSettings: EffectBlockingSettings`
- `+- DestroyingSettings: EffectDestroyingSettings`
- `~ animator: Animator`
- `~ autoSize: AutoSizeType`
- `~ deactivateOnMove: bool`
- `~ dontRepeatTrigger: bool`
- `~ durationMovePositionDependingOnSize: Vector2`
- `~ effectId: int`
- `~ lastTriggerName: string`
- `~ movePositionDependingOnSize: Transform`
- `~ parentType: EffectParentType`
- `~ restoreStateAfterMove: bool`
- `~ sendAnimatorTrigger: bool`
#### Methods
- `+ Activate(Chip chip): bool`
    - **Purpose**: Activates the effect on the specified chip
    - **Usage**: Call when the chip is activated (e.g. created or enabled)
    - trigger 'Activate' animation if configured
    - **Params**: chip - the chip this effect belongs to
    - **Returns**: True if the effect was activated
    - false if skipped because effectId is in chip.BlockingState.HideEffectIds
    - **Notes**: When activated, calls chip.BlockingState.ApplyBlock(BlockingSettings) to apply this effect blocking rules
    - when hidden, calls Deactivate instead
- `+ Deactivate(Chip chip, bool force): void`
    - **Purpose**: Deactivates the effect on the specified chip
    - **Usage**: Call when the chip is deactivated or disabled
    - trigger 'Deactivate' animation if configured
    - **Params**: chip - the chip this effect belongs to
    - force - if true, forces immediate animation state change
- `+ GetId(): int`
    - **Purpose**: Returns the unique hash/ID of this effect for identification in the chip's effects dictionary
    - **Usage**: Called by external code to retrieve the effect's ID for lookup or removal
    - **Returns**: The hash code that identifies this effect
- `+ Init(Chip chip, int effectId): void`
    - **Purpose**: Initializes the effect with chip reference and its effect hash ID
    - **Usage**: Called from Chip.AddEffect after instantiation to set up the effect's runtime state
    - **Params**: chip - the chip this effect belongs to
    - effectId - unique hash for identifying this effect
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
    - **Purpose**: Called when the cell under the chip changes
    - **Usage**: Override to handle logic when the chip moves over a different cell without changing parent
    - **Params**: underCell - the new cell under the chip
    - overCell - the previous cell under the chip
- `+ OnMovingStateChanged(Chip chip, bool isMoving): void`
    - **Purpose**: Called when the chip's movement state changes (e.g., starting or ending a drag, or system-initiated movement)
    - **Usage**: Override in derived classes to react to movement (e.g., stopping animations, hiding visuals)
    - **Params**: chip - the owner chip
    - isMoving - true if the chip started moving, false if it stopped
    - **Notes**: Supports automatic hiding of effects during drag/move to prevent visual noise
    - uses deactivateOnMove and restoreStateAfterMove flags
- `+ SendTrigger(string triggerName, bool allowRepeat): void`
    - **Purpose**: Sends a custom animation trigger to the effect's animator
    - **Usage**: Call to trigger custom animations on the effect
    - used for special interactions like move-locked feedback
    - **Params**: triggerName - name of the animator trigger to activate
    - allowRepeat - if true, bypasses the dontRepeatTrigger check for this call
    - **Notes**: Safely handles null animator
    - allows effects to respond to chip-specific events beyond standard Activate/Deactivate
- `+ TryDestroyEffect(Chip chip, EffectDestroyingSettings settings, EffectDestroyingRuntimeData destroyingData): bool`
    - **Purpose**: Evaluates whether the effect has reached its destruction threshold based on neighboring merge count
    - **Usage**: Called from Chip.HandleDestroyingEffects when effectOfPrioritizingDestroying processes a neighboring merge event
    - **Params**: chip - owner chip
    - settings - destruction configuration (threshold, trigger prefix, priority)
    - destroyingData - runtime counter tracking merge events
    - **Returns**: True if the effect should be removed (merge count reached threshold)
    - false if still alive
    - **Notes**: When not yet destroyed, sends a progressive trigger (e.g. 'Hit1', 'Hit2') to the effect animator
    - when threshold reached, deactivates the effect
- `~ ApplyAutoSize(Vector2Int chipSize): void`
    - **Purpose**: Applies auto-sizing logic to the effect based on the autoSize setting and chip dimensions
    - **Usage**: Called internally during Init
    - scales the effect proportionally or non-proportionally
    - **Params**: chipSize - the grid dimensions of the chip (e.g., Data.Size)
- `~ HasTrigger(string name): bool`
- `- Merge2.IEffect.get_gameObject(): GameObject`
- `~ ResetTrigger(string triggerName): void`
    - **Purpose**: Resets an animation trigger on the effect's animator
    - **Usage**: Internal helper to clear opposing triggers when switching states
    - **Params**: triggerName - name of the trigger to reset
    - **Notes**: Safely handles null animator
---

## EffectBlockingSettings
**Inherits**: `ScriptableObject`

> - **Purpose**: ScriptableObject asset that configures which chip actions are blocked when an effect is active
> - **Usage**: Assigned to Effect.blockingSettings in Inspector
> - applied to CombinedBlockingState via ApplyBlock during Effect.Activate
> - **Notes**: hideEffectNames are resolved to integer IDs via EffectConsts.GetIdByName during serialization callbacks
#### Fields
- `+- CanApplyModifiers: bool`
- `+- CanBeFilled: bool`
- `+- CanBeMergedAsSource: bool`
- `+- CanBeMergedAsTarget: bool`
- `+- CanBeMoved: bool`
- `+- CanBeTaped: bool`
- `+- CanGenerate: bool`
- `+- CanReceiveModifiers: bool`
- `+- HideEffectIds: HashSet<int>`
- `+- IsLittleChip: bool`
- `- hideEffectNames: string[]`
#### Methods
- `+ OnAfterDeserialize(): void`
- `+ OnBeforeSerialize(): void`
- `- UpdateHideEffectIds(): void`
---

## EffectConsts

> - **Purpose**: Centralized registry of integer effect IDs used as keys in the chip effects dictionary
> - **Usage**: Reference these constants when adding, retrieving, or removing effects via Chip.AddEffect/GetEffect/RemoveEffect
> - **Notes**: Base IDs are for built-in effects
> - Extra subclass (101+) is for optional blocker/overlay effects configured through ChipExtraEffectsData
> - nameToId dictionary enables serialized string-to-int resolution
#### Fields
- `+ CellHighlight: int`
- `+ ContainerRequirements: int`
- `+ GeneratorCharged: int`
- `+ GeneratorCharging: int`
- `+ MergeAvailable: int`
- `+ PBoosterConnectorCells: int`
- `+ PBoosterJoin: int`
- `+ ShadowEffect: int`
- `- nameToId: Dictionary<string, int>`
#### Methods
- `+ GetIdByName(string name): int`
    - **Purpose**: Resolves a serialized effect name string to its integer ID constant
    - **Usage**: Called by ExtraEffectData.EffectId and EffectBlockingSettings.UpdateHideEffectIds to convert inspector-configured names
    - **Returns**: Integer effect ID or -1 if name not found in nameToId dictionary
---

## EffectContainerRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## EffectDestroyingRuntimeData
#### Fields
- `+ effectId: int`
- `+ NeighboringMergeCount: int`
---

## EffectDestroyingSettings
**Inherits**: `ScriptableObject`
#### Fields
- `+- NeighboringMergeCountToDestroy: float`
- `+- NeighboringMergeTriggerForEffect: string`
- `+- Priority: int`
---

## EffectGeneratorChargingRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## EffectPowerBoosterJoinRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## EffectRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## ExtraChip

> - **Purpose**: Represents an extra chip that can be generated or rewarded with a certain chance
> - **Usage**: Used in merge outcomes or loot tables to provide bonus chips
#### Fields
- `++ Chance: int`
- `++ ChipData: FieldChipData`
---

## ExtraEffectData

> - **Purpose**: Serializable entry mapping an effect name to its prefab for extra blocker/overlay effects
> - **Usage**: Configured in ChipExtraEffectsData.blockers array
> - EffectId is resolved at runtime via EffectConsts.GetIdByName
> - **Notes**: effectName must match a key in EffectConsts.nameToId dictionary
#### Fields
- `+- EffectId: int`
- `+- EffectName: string`
- `+- Prefab: GameObject`
---

## ExtraEffectDataEx
**Inherits**: `ExtraEffectData`

> - **Purpose**: Extended effect data include activation/deactivation triggers
> - **Usage**: Used in OtherEffects list for chips that need effects with specific lifecycle control
> - **Notes**: Supports custom activation/deactivation logic during chip initialization
#### Fields
- `+- ActivateOnStart: bool`
- `+- DeactivateOnStart: bool`
---

## FieldChipData

> - **Purpose**: Contains data for a chip specifically placed on the field, including its ID and current state/effects
> - **Usage**: Used within FieldData.CellData to manage chip instances on the game board
#### Fields
- `+ ChipId: string`
    - **Purpose**: Unique identifier for the chip type
    - **Usage**: Should correspond to a chip name in the ChipDataCollection
- `+ ExtraEffectIds: int[]`
    - **Purpose**: Array of extra effect IDs to activate on this chip when loaded from level data
    - **Usage**: Populated by Level Editor
    - consumed by FieldInitializeCommand to populate ChipRuntimeData.EffectEnables
    - **Notes**: Values correspond to EffectConsts.Extra constants (e.g. MoveLockedEffect=103)
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
- `- chipChangeNotifier: IChipChangeNotifier`
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
- `- chipChangeNotifier: IChipChangeNotifier`
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
- `+ IsValidCellPos(int x, int y): bool`
    - **Purpose**: High-performance bounds check using direct x, y coordinates (no Vector2Int allocation).
    - **Usage**: Use in tight loops to avoid temporary Vector2Int objects
    - preferred over IsValidCellPos(Vector2Int).
    - **Params**: x - column coordinate
    - y - row coordinate.
    - **Returns**: True if coordinates are within field bounds
    - otherwise false.
- `+ IsValidCellPos(Vector2Int cellPos, Vector2Int size): bool`
    - **Purpose**: Checks if the given cell position and size are valid within field boundaries.
    - **Usage**: Call to validate if a chip of given size can be placed at the specified position.
    - **Params**: cellPos - top-left cell position
    - size - chip size (width, height).
    - **Returns**: True if the area is fully within field bounds
    - otherwise false.
- `+ SetChipInCell(Cell cell, Chip chip): void`
    - **Purpose**: Places a chip into a target cell and updates observer-visible state for placement/removal.
    - **Usage**: Call to place, move, or remove a chip in grid coordinates while keeping chip-change notifications and multi-cell occupancy metadata in sync.
    - **Params**: cell - target main cell for placement or clearing
    - chip - chip instance to place (null means clear).
    - **Notes**: When placing, CellPosition is assigned before chipChangeNotifier.Enqueue so observers handling the event read the chip at its new logical coordinates
    - when clearing, all occupied cells are reset via ClearCells before enqueueing old/new chip state.
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
- `- chipDataCollection: ChipDataCollection`
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
    - **Purpose**: Returns the current FieldData used by this field.
    - **Usage**: Used in tests to access ChipDataCollection and other field configurations.
- `+ LoadChips(): void`
    - **Purpose**: Loads and creates chips from field data.
    - **Usage**: Call after CreateField to populate the field with initial chips.
    - **Notes**: Validates data sources
    - looks up chip definitions from ChipDataCollection
    - triggers spawn animations.
- `- CreateChip(CellData cellData, ChipData chipData): void`
    - **Purpose**: Creates a chip at the specified position and triggers spawn animation.
    - **Usage**: Internal helper method called by LoadChips for each chip.
    - **Params**: cellData - cell data for the chip
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
- `+ FindNearestFreeCell(Vector2Int parentPos, Vector2Int parentSize, Vector2Int childChipSize, Chip chipToPlace, HashSet<Cell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove, bool onlyAround): Cell`
    - **Purpose**: Finds the nearest free cell for placing a chip, starting the search from a rectangular area.
    - **Usage**: Call to find a suitable cell for a chip, considering exclusions and movable chips. The search is performed first within the specified parent area and then spirally expands around it.
    - **Params**: parentPos - area start
    - parentSize - area size
    - childChipSize - chip size
    - chipToPlace - chip to place
    - cellsToExclude - cells to skip
    - chipsToPotentiallyMove - chips allowed to move
    - onlyAround - if true, search only in the immediate neighborhood.
    - **Returns**: The nearest free Cell or null if none found.
- `- IsAreaCompletelyFree(Vector2Int cellPos, Vector2Int chipSize, Chip chipToPlace, HashSet<Cell> excludedCellsSet, HashSet<Chip> movingChipsSet): bool`
---

## ICellSubscriber

> - **Purpose**: Implemented by any object that wants to be notified when a chip on a watched cell changes.
> - **Usage**: Deliver via CellObserverManager.Subscribe.
#### Fields
- `+- ObservedCellPositions: IReadOnlyList<Vector2Int>`
#### Methods
- `+ OnChipChangedCell(Cell sourceCell, Cell targetCell): void`
    - **Purpose**: Rebinds neighbor observation when the owning chip changes its main cell.
    - **Usage**: Called by chip movement/placement flow after field occupancy is updated to the target cell.
    - **Params**: sourceCell - previous main cell (may be null on first placement)
    - targetCell - current main cell after relocation
- `+ OnChipDestroy(Cell mainCell): void`
    - **Purpose**: Performs subscriber cleanup before the owning chip is destroyed or removed from the field.
    - **Usage**: Called by chip destruction flow to release observer subscriptions and derived-state links.
    - **Params**: mainCell - chip main cell at destruction time
- `+ OnObservedCellChipChanged(ChipChangedEvent evt): void`
    - **Purpose**: Called once per changed cell after all frame changes have been collected and flushed.
    - **Usage**: Handle local chip/cell logic updates here.
    - **Params**: evt - event data containing cell and chip state
---

## IChipChangeNotifier

> - **Purpose**: Collects chip-change records from FieldGrid during the frame.
> - **Usage**: Flushes changes all at once (called from FieldEventHandler.LateUpdate) to ensure consistent end-of-frame snapshots.
#### Methods
- `+ Enqueue(Cell cell, Chip oldChip, Chip newChip): void`
    - **Purpose**: Record a chip change.
    - **Usage**: Called by FieldGrid.SetChipInCell before mutating state.
    - **Params**: cell - the location of change
    - oldChip - previous chip
    - newChip - current chip
- `+ Flush(): void`
    - **Purpose**: Fire OnFlush with all pending events and clear the queue.
    - **Usage**: Called from FieldEventHandler.LateUpdate.
---

## IChipFinder

> - **Purpose**: Contract for chip finding/discovery implementations
> - **Usage**: Implement this interface for different strategies of locating chips on the field
> - **Notes**: Allows for dependency injection of various chip discovery mechanisms
#### Methods
- `+ FindChips(Cell cell, Vector2Int chipSize): HashSet<Chip>`
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

> - **Purpose**: Unified interface for all chip interaction logic implementations.
> - **Usage**: Provides a standardized contract for validating and executing chip interactions.
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

## IChipSortingLayer

> - **Purpose**: Defines the contract for managing chip sorting layers and movement-based sorting adjustments
> - **Usage**: Implemented by ChipSortingLayer
> - used by Chip to adjust visual depth during drag/movement
> - **Notes**: Supports multiple renderers per chip
#### Fields
- `+- SortingLayers: SortingLayerData[]`
#### Methods
- `+ Init(): void`
- `+ SetMoving(bool value): void`
---

## IChipSpecialData
---

## IEffect

> - **Purpose**: Interface for all effects that can be activated or deactivated on chips
> - **Usage**: Implement this interface for custom chip effects. Used by Chip class to manage common effect lifecycle.
> - **Notes**: Includes core activation, deactivation, trigger, and cell change handling methods.
#### Fields
- `+- BlockingSettings: EffectBlockingSettings`
- `+- DestroyingSettings: EffectDestroyingSettings`
- `+- gameObject: GameObject`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ GetId(): int`
- `+ Init(Chip chip, int effectHash): void`
- `+ OnChangedCell(Cell sourceCell, Cell targetCell): void`
- `+ OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell underCell): void`
- `+ OnInteractionUnderCellChanged(Cell underCell, Cell overCell): void`
- `+ OnMovingStateChanged(Chip chip, bool isMoving): void`
- `+ SendTrigger(string triggerName, bool allowRepeat): void`
- `+ TryDestroyEffect(Chip chip, EffectDestroyingSettings settings, EffectDestroyingRuntimeData destroyingData): bool`
---

## IEffectBlockingSettings

> - **Purpose**: Contract for effect blocking configuration that controls what actions are allowed on a chip when an effect is active
> - **Usage**: Implemented by EffectBlockingSettings (ScriptableObject) and CombinedBlockingState (runtime aggregate)
> - consumed by game logic (DraggableChipLogic, MergeableChipLogic, ChipGenerator, ChipPowerBooster)
> - **Notes**: All boolean flags default to true (allowed)
> - false means the action is blocked
> - HideEffectIds specifies which other effects should be hidden when this blocking is active
#### Fields
- `+- CanApplyModifiers: bool`
- `+- CanBeFilled: bool`
- `+- CanBeMergedAsSource: bool`
- `+- CanBeMergedAsTarget: bool`
- `+- CanBeMoved: bool`
- `+- CanBeTaped: bool`
- `+- CanGenerate: bool`
- `+- CanReceiveModifiers: bool`
- `+- HideEffectIds: HashSet<int>`
- `+- IsLittleChip: bool`
---

## IEffectContainer

> - **Purpose**: Interface for ChipContainer specific visual effects
> - **Usage**: Implemented by ChipContainerEffect to handle bubble container visuals. Used by ChipContainer.
> - **Notes**: Extends IEffect with UpdateElements method.
#### Methods
- `+ UpdateElements(Chip chip, Dictionary<ContainerInfo, int> containers, bool isFull): void`
---

## IEffectGeneratorCharging

> - **Purpose**: Interface for ChipGenerator specific charging effects
> - **Usage**: Implemented by ChipGeneratorRechargeEffect to visualize charging progress. Used by ChipGenerator.
> - **Notes**: Extends IEffect with OnCharging method.
#### Methods
- `+ OnCharging(float progress): void`
---

## IEffectPowerBoosterJoin

> - **Purpose**: Effect contract for visualizing dynamic join links between a power booster and modifier-capable targets.
> - **Usage**: Implemented by effects that react to booster OnJoin/OnLeave lifecycle and follow IEffect activation rules.
#### Methods
- `+ OnJoin(IPowerBoosterModifier powerBoosterModifier): void`
    - **Purpose**: Registers a modifier target and creates/updates visual links for it.
    - **Usage**: Called when a booster starts affecting the provided modifier.
    - **Params**: powerBoosterModifier - target receiving booster influence
- `+ OnLeave(IPowerBoosterModifier powerBoosterModifier): void`
    - **Purpose**: Removes visual links associated with the provided modifier target.
    - **Usage**: Called when a booster stops affecting the modifier or the modifier is removed.
    - **Params**: powerBoosterModifier - target losing booster influence
- `+ Show(): void`
    - **Purpose**: Starts standalone display logic for this join effect implementation.
    - **Usage**: Optional explicit trigger for implementations that expose manual show behavior.
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
- `+ IsValidCellPos(int x, int y): bool`
    - **Purpose**: Checks if coordinates are within field bounds (direct x, y parameters)
    - **Usage**: High-performance validation eliminating Vector2Int allocation
    - used in tight loops
    - **Params**: x - column index
    - y - row index
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

> - **Purpose**: Interface for finding free cells on the field grid.
> - **Usage**: Injected into components that need to spawn or relocate chips.
> - **Notes**: Provides logic for nearest free cell search with spiral expansion.
#### Methods
- `+ FindNearestFreeCell(Vector2Int parentPos, Vector2Int parentSize, Vector2Int childChipSize, Chip chipToPlace, HashSet<Cell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove, bool onlyAround): Cell`
    - **Purpose**: Finds the nearest free cell for placing a chip.
    - **Usage**: Call to find a suitable cell for a chip, considering exclusions and movable chips.
    - **Params**: parentPos - search origin
    - parentSize - size of origin area
    - childChipSize - size of chip to place
    - chipToPlace - chip instance (optional)
    - cellsToExclude - set of cells to skip
    - chipsToPotentiallyMove - set of chips allowed to move
    - onlyAround - if true, limits search to the immediate neighborhood.
    - **Returns**: The nearest free Cell or null if none found.
---

## IPowerBoosterModifier

> - **Purpose**: Contract for entities that can be modified by ChipPowerBooster instances.
> - **Usage**: Implemented by chips (for example ChipGenerator) that expose join points and manage active booster modifiers.
#### Fields
- `+- BlockingState: CombinedBlockingState`
- `+- JoinPoints: IReadOnlyList<Transform>`
- `+- PowerBoosterModifiers: HashSet<ChipPowerBooster>`
#### Methods
- `+ ApplyPowerBoosterModifier(ChipPowerBooster chipPowerBooster, bool reapply): bool`
    - **Purpose**: Adds a booster modifier to this entity.
    - **Usage**: Called when booster observation detects this entity in range.
    - **Params**: chipPowerBooster - booster to apply
    - **Returns**: True if the modifier was added
    - false if it was already active.
- `+ NotifyEffectRemoved(int effectId): void`
    - **Purpose**: Notifies that an effect has been removed from this entity.
    - **Usage**: Called by Chip.Effects when an effect is removed.
    - **Params**: effectId - identifier of the removed effect
- `+ RemovePowerBoosterModifier(ChipPowerBooster chipPowerBooster): void`
    - **Purpose**: Removes a booster modifier from this entity.
    - **Usage**: Called when booster observation no longer includes this entity or booster is removed.
    - **Params**: chipPowerBooster - booster to remove
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
#### Fields
- `++ FieldData: FieldData`
#### Methods
- `~ Awake(): void`
- `~ Configure(IContainerBuilder builder): void`
---

## MergeableChipLogic
**Inherits**: `MonoBehaviour`

> - **Purpose**: Handles chip merging logic when two compatible chips are combined.
> - **Usage**: Implements IChipInteractionLogic to provide unified interface for merge operations.
> - **Notes**: Validates chip compatibility and creates new merged chips
> - integrates with unified interaction system
> - handles neighbor-searching logic if primary relocation fails.
#### Fields
- `- chipDataCollection: ChipDataCollection`
- `- chipFactory: ChipFactory`
- `- chipFinder: IChipFinder`
- `- chipMovingLogic: IChipMovingLogic`
- `- fieldGrid: IFieldGrid`
- `- freeCellFinder: IFreeCellFinder`
- `- NeighborOffsets: Vector2Int[]`
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
    - **Notes**: If the merged chip is larger than the parent, it uses IChipMovingLogic to relocate neighboring chips if needed. If relocation at the primary position fails, it tries all 8 neighboring offsets before giving up.
- `- HandleExtraChip(MergeResult mergeResult, Cell mergedCell, Vector2Int mergedChipSize): void`
- `- NotifyNeighborsOfMerge(Cell targetCell): void`
    - **Purpose**: Notifies neighboring chips that merges have occurred nearby
    - **Usage**: Called from ExecuteInteraction before destroying the merging chips
    - **Params**: targetCell - the main cell of the target chip whose neighbors will be notified
- `- TryResolveCellPosition(Cell primaryCell, Chip[] chipsToExclude, Vector2Int chipSize, Cell& resolvedCell, List`1& plannedRelocations): bool`
    - **Purpose**: Resolves the final cell position for a merged chip: tries the primary cell first, then all 8 neighbors
    - **Usage**: Called from ExecuteInteraction when relocation check is needed
    - **Params**: primaryCell - preferred cell to place the chip
    - chipsToExclude - chips to ignore during checks
    - chipSize - size of chip to place
    - **resolvedCell - output**: the cell that worked
    - **plannedRelocations - output**: relocations for the resolved cell
    - **Returns**: True if a valid position was found (primary or neighbor)
    - false if all options failed
---

## MergeCombination

> - **Purpose**: Holds information about a specific merge partnership
> - **Usage**: Member of ChipMergeData
> - defines which chip can be merged and what the results are
#### Fields
- `++ Results: MergeResult[]`
- `++ TargetChip: ChipData`
#### Methods
- `+ GetRandomResult(): MergeResult`
    - **Purpose**: Selects a random result result (including extra chip) based on defined weights
    - **Usage**: Called internally to determine the outcome when multiple outcomes are possible
    - **Params**: none
    - **Returns**: The selected MergeResult object
---

## MergeResult

> - **Purpose**: Describes a single potential outcome of a merge
> - **Usage**: Member of MergeCombination
> - used with weighted random selection
#### Fields
- `++ ExtraChip: Optional<ExtraChip>`
- `++ Result: ChipData`
- `++ Weight: int`
---

## NeighborChipFinder

> - **Purpose**: Finds all unique neighboring chips around a given cell, handling multi-cell chips correctly
> - **Usage**: Injected through VContainer with key 'Neighbors'
> - called from MergeableChipLogic to identify chips affected by merges
> - **Notes**: Returns a HashSet to automatically handle duplicates
> - **correctly excludes cells occupied by the source chip itself. Optimized**: caches grid dimensions and uses direct x,y bounds checks to eliminate allocations
#### Fields
- `- cachedGridHeight: int`
- `- cachedGridWidth: int`
- `- fieldGrid: IFieldGrid`
- `- neighbors: HashSet<Chip>`
#### Methods
- `+ FindChips(Cell cell, Vector2Int chipSize): HashSet<Chip>`
    - **Purpose**: Retrieves all unique neighboring chips around a specific cell, accounting for chip size
    - **Usage**: Call when you need to find all unique chip neighbors for a given position and size
    - **Params**: cell - the main cell of the chip
    - chipSize - the size (width, height) of the chip occupying the cell
    - **Returns**: HashSet<Chip> containing all unique neighboring chips (no duplicates)
    - empty set if no neighbors found
    - **Notes**: Optimized: Uses 4-border iteration to skip own-cell checking entirely
    - employs direct coordinates (int x, y) to eliminate Vector2Int allocations
    - searches 1 cell outward from chip boundary in all directions
- `- AddNeighborChip(int x, int y): void`
---

## PowerBoosterCellSubscriber
**Inherits**: `CellSubscriber`

> - **Purpose**: Cell subscriber that maintains power booster modifier links to nearby IPowerBoosterModifier entities.
> - **Usage**: Attached to ChipPowerBooster
> - reacts to observed chip changes and chip movement to keep modifier application and removal consistent.
> - **Notes**: Tracks unique modifier targets in modifiedEntities to prevent duplicate apply/remove calls.
#### Fields
- `+- ModifiedEntities: HashSet<IPowerBoosterModifier>`
- `~ chipPowerBooster: ChipPowerBooster`
#### Methods
- `+ OnChipChangedCell(Cell sourceCell, Cell targetCell): void`
    - **Purpose**: Recomputes modifier links after the booster itself changes cell.
    - **Usage**: Called during owner chip relocation
    - removes all existing links, re-subscribes via base logic, then rebuilds links from newly observed cells.
    - **Params**: sourceCell - previous booster main cell
    - targetCell - new booster main cell
- `+ OnChipDestroy(Cell mainCell): void`
    - **Purpose**: Removes all active modifier links before subscriber teardown.
    - **Usage**: Called during booster destruction to guarantee counterpart modifiers are reverted.
    - **Params**: mainCell - booster main cell at destruction time
- `+ OnObservedCellChipChanged(ChipChangedEvent evt): void`
    - **Purpose**: Applies/removes booster modifiers when observed neighboring chips change.
    - **Usage**: Invoked by CellObserverManager flush events
    - idempotent membership checks avoid duplicate callbacks.
    - **Params**: evt - neighbor chip change payload with old/new chip references
- `~ Awake(): void`
- `- ClearAllModifiers(): void`
---

## PowerBoosterConnectorCellsHighlightEffect
**Inherits**: `CellHighlightEffect`
#### Fields
- `+ distractionAmount: float`
- `+ globalAlpha: float`
- `~ alphaId: int`
- `~ connectorCellPositions: IReadOnlyList<Vector2Int>`
- `~ distractionAmountId: int`
- `~ originCellPosition: Vector2Int`
- `~ powerEffectCoroutine: Coroutine`
- `~ waitTimeBeforePowerEffect: float`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ OnChangedCell(Cell sourceCell, Cell targetCell): void`
    - **Purpose**: Reacts to cell changes by triggering the PowerBooster animation
    - **Usage**: Called automatically via CellHighlightEffect when the chip is moved
    - **Params**: sourceCell - original cell
    - targetCell - result cell
- `+ OnInteractionOverCellChanged(Cell sourceCell, Cell targetCell, Cell interactableCell): void`
- `~ CreateHighlights(): void`
- `- StartPowerEffect(Chip chip, float waitTime): IEnumerator`
- `- Update(): void`
---

## PowerBoosterJoinEffect
**Inherits**: `Effect`

> - **Purpose**: Effect that spawns and maintains animated particle links between a booster and each active power-booster modifier target.
> - **Usage**: Used by ChipPowerBooster via IEffectPowerBoosterJoin.OnJoin/OnLeave
> - periodically rebinds endpoints to create dynamic join visuals.
> - **Notes**: Owns instantiated particle systems per modifier and stops/destroys them on leave/deactivate.
#### Fields
- `- changeJoinPointsCoroutine: Coroutine`
- `- changeJoinPointsTime: float`
- `- effectPowerDistance: float`
- `~ effects: Dictionary<IPowerBoosterModifier, List<JoinEffectData>>`
- `~ joinEffectPrefab: ParticleSystem`
- `~ joinPoints: Transform[]`
- `- minMaxEffectsForOneModifier: Vector2Int`
#### Methods
- `+ Deactivate(Chip chip, bool force): void`
    - **Purpose**: Stops all running join visuals and clears tracked modifier links.
    - **Usage**: Called when owning booster starts moving or effect is forcefully deactivated.
    - **Params**: chip - owning chip passed by effect lifecycle
    - force - optional force flag from IEffect contract
    - **Notes**: Stops coroutine, stops particles, schedules particle GameObject destruction using each particle lifetime, then clears state dictionary.
- `+ OnJoin(IPowerBoosterModifier powerBoosterModifier): void`
    - **Purpose**: Registers a new modifier target and creates one or more visual join effects for it.
    - **Usage**: Called when a booster starts affecting a modifier target.
    - **Params**: powerBoosterModifier - modifier target that should receive join visuals
    - **Notes**: Starts the join-point reshuffle coroutine on first active modifier.
- `+ OnLeave(IPowerBoosterModifier powerBoosterModifier): void`
    - **Purpose**: Removes and schedules cleanup of all join effects associated with a modifier target.
    - **Usage**: Called when booster influence on a modifier ends.
    - **Params**: powerBoosterModifier - modifier target to remove
    - **Notes**: Stops reshuffle coroutine when no modifiers remain.
- `+ Show(): void`
    - **Purpose**: Optional explicit show entrypoint for IEffectPowerBoosterJoin contract.
    - **Usage**: Currently unused in this implementation.
- `- CalcRotation(Transform joinPoint, Transform entityTransformJoinPoint): Quaternion`
    - **Purpose**: Calculates particle orientation so link visuals point from booster join point toward modifier join point.
    - **Usage**: Called when (re)binding an effect to selected endpoints.
    - **Params**: joinPoint - selected booster-side transform
    - entityTransformJoinPoint - selected modifier-side transform
    - **Returns**: World-space rotation for the particle system.
- `- ChangeJoinPointsCoroutine(): IEnumerator`
    - **Purpose**: Periodically reassigns random join endpoints for active effects to keep links visually dynamic.
    - **Usage**: Started on first join and runs until all modifier links are removed.
    - **Notes**: Waits changeJoinPointsTime between updates and mutates one random active effect per tick.
- `- ChangeStartLifetime(ParticleSystem particleSystem, float distance): void`
    - **Purpose**: Adjusts particle lifetime so visual link length matches the current endpoint distance.
    - **Usage**: Called after endpoint selection or reassignment.
    - **Params**: particleSystem - effect instance to update
    - distance - world-space endpoint distance used for lifetime scaling
- `- GetTopClosest(IReadOnlyList<Transform> sources, IReadOnlyList<Transform> targets, int n, List`1& distances): List<Transform>`
    - **Purpose**: Selects up to N source transforms that are closest to any target transform.
    - **Usage**: Used to prefilter candidate join points on both booster and modifier sides.
    - **Params**: sources - candidate points to rank
    - targets - points to measure distance against
    - n - max number of closest sources to return
    - distances - reusable scratch list for distance calculations
    - **Returns**: List containing the N closest source transforms (or fewer when sources are limited).
    - **Notes**: Uses a reusable distance list to reduce temporary allocations during repeated effect updates.
- `- Merge2.IEffect.get_gameObject(): GameObject`
- `- ShowEffect(IPowerBoosterModifier powerBoosterModifier, JoinEffectData joinEffectData): JoinEffectData`
    - **Purpose**: Creates or rebinds a single join particle effect between booster and modifier join points.
    - **Usage**: Called for initial spawn and periodic endpoint reshuffles.
    - **Params**: powerBoosterModifier - modifier target for this link
    - joinEffectData - existing effect data to update (null creates a new record)
    - **Returns**: Effect data instance containing selected endpoints and the particle system instance.
---

## ShadowEffect
**Inherits**: `Effect`

> - **Purpose**: Provides a persistent shadow effect for chips that reacts to movement states
> - **Usage**: Instantiated by Chip.InitEffects
> - sends 'Move' and 'Stop' triggers to its animator when the chip starts or stops moving
> - **Notes**: Does not deactivate on move to ensure the shadow remains visible during dragging
#### Fields
- `- additionallyWhenMoving: int`
- `- autoScale: bool`
- `- autoShadowSprite: bool`
- `- autoSortingLayer: bool`
- `- cashedLayerOrder: int`
- `- shadowRenderer: SpriteRenderer`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ Init(Chip chip, int effectId): void`
- `+ OnMovingStateChanged(Chip chip, bool isMoving): void`
    - **Purpose**: Reacts to chip movement by sending appropriate animator triggers
    - **Usage**: Called by Chip.NotifyEffectsOnMovingStateChanged
    - sends 'Move' when isMoving is true, and 'Stop' otherwise
    - **Params**: chip - owner chip
    - isMoving - current movement state
---

## SortingLayerData

> - **Purpose**: Stores data for a single renderer's sorting layer configuration
> - **Usage**: Used by IChipSortingLayer to manage multiple renderers within a chip
> - **Notes**: CachedOrder is populated during Init and restored when movement ends
#### Fields
- `+ AdditionallyWhenMoving: int`
- `+ CachedOrder: int`
- `+ Renderer: Renderer`
---

## IInputManager

> - **Purpose**: Interface for input event handling system
> - defines events for tap and drag operations
> - **Usage**: Implemented by InputManager
> - used for dependency injection via VContainer to decouple input handling from consumers
> - **Notes**: Provides events (OnTap, OnDragStart, OnDrag, OnDragEnd) and editor-only simulation methods for testing
#### Methods
- `+ SimulateDrag(Vector2 position, Vector3 worldPosition): void`
- `+ SimulateDragEnd(Vector2 position, Vector3 worldPosition): void`
- `+ SimulateDragStart(Vector2 position, Vector3 worldPosition): void`
- `+ SimulateTap(Vector2 position, Vector3 worldPosition): void`
---

## InputManager
**Inherits**: `MonoBehaviour`

> - **Purpose**: Handles user input for the Merge2 game board, converting raw input into game events (Tap, Drag)
> - **Usage**: Attached to the board GameObject
> - raises events for other managers to subscribe to.
> - **Notes**: Uses the new Unity Input System via Merge2Input class. Preserves legacy thresholds for drag detection.
#### Fields
- `- dragging: bool`
- `- dragStartThreshold: float`
- `- input: Merge2Input`
- `- logEnable: bool`
- `- mainCamera: Camera`
- `- OnDrag: InputEvent`
- `- OnDragEnd: InputEvent`
- `- OnDragStart: InputEvent`
- `- OnTap: InputEvent`
- `- press: bool`
- `- remainingTapTime: float`
- `- spriteRenderer: SpriteRenderer`
- `- spriteTransform: Transform`
- `- startPos: Vector2`
- `- tapTimeThreshold: float`
#### Methods
- `+ SimulateDrag(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Simulates drag input for testing purposes in Unity Editor
    - **Usage**: Called from test helpers to simulate continuous drag without actual input
    - available only in editor
    - **Params**: position - local field position
    - worldPosition - world space position
- `+ SimulateDragEnd(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Simulates drag end input for testing purposes in Unity Editor
    - **Usage**: Called from test helpers to simulate drag end without actual input
    - available only in editor
    - **Params**: position - local field position
    - worldPosition - world space position
- `+ SimulateDragStart(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Simulates drag start input for testing purposes in Unity Editor
    - **Usage**: Called from test helpers to simulate drag start without actual input
    - available only in editor
    - **Params**: position - local field position
    - worldPosition - world space position
- `+ SimulateTap(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Simulates tap input for testing purposes in Unity Editor
    - **Usage**: Called from test helpers to simulate user tap without actual input
    - available only in editor
    - **Params**: position - local field position
    - worldPosition - world space position
- `- Awake(): void`
- `- OnDisable(): void`
- `- OnDragEndImpl(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Internal implementation for drag end event handling
    - **Usage**: Called internally when drag operation ends
    - logs and invokes OnDragEnd event. For testing, use SimulateDragEnd method instead.
    - **Params**: position - local field position
    - worldPosition - world space position
- `- OnDragImpl(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Internal implementation for drag event handling
    - **Usage**: Called internally continuously during drag
    - logs and invokes OnDrag event. For testing, use SimulateDrag method instead.
    - **Params**: position - local field position
    - worldPosition - world space position
- `- OnDragStartImpl(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Internal implementation for drag start event handling
    - **Usage**: Called internally when drag operation begins
    - logs and invokes OnDragStart event. For testing, use SimulateDragStart method instead.
    - **Params**: position - local field position
    - worldPosition - world space position
- `- OnEnable(): void`
- `- OnTapImpl(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Internal implementation for tap event handling
    - **Usage**: Called internally when a tap is detected
    - logs and invokes OnTap event. For testing, use SimulateTap method instead.
    - **Params**: position - local field position
    - worldPosition - world space position
- `- Start(): void`
- `- Update(): void`
---

## Merge2Input
#### Fields
- `++ bindingMask: Nullable<InputBinding>`
- `++ devices: Nullable<ReadOnlyArray<InputDevice>>`
- `+- asset: InputActionAsset`
- `+- bindings: IEnumerable<InputBinding>`
- `+- controlSchemes: ReadOnlyArray<InputControlScheme>`
- `+- Game: GameActions`
- `- m_Game: InputActionMap`
- `- m_Game_Click: InputAction`
- `- m_Game_Point: InputAction`
- `- m_GameActionsCallbackInterfaces: List<IGameActions>`
#### Methods
- `+ Contains(InputAction action): bool`
- `+ Disable(): void`
- `+ Dispose(): void`
- `+ Enable(): void`
- `+ FindAction(string actionNameOrId, bool throwIfNotFound): InputAction`
- `+ FindBinding(InputBinding bindingMask, InputAction& action): int`
- `+ GetEnumerator(): IEnumerator<InputAction>`
- `~ Finalize(): void`
- `- System.Collections.IEnumerable.GetEnumerator(): IEnumerator`
---

