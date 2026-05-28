# Namespace: Expecto.MergeBase

## Table of Contents
- [BlockedCell](#blockedcell)
- [Cell](#cell)
- [CellHighlightEffect](#cellhighlighteffect)
- [CellObserverManager](#cellobservermanager)
- [CellSubscriber](#cellsubscriber)
- [Chip](#chip)
- [ChipChangedEvent](#chipchangedevent)
- [ChipCollections](#chipcollections)
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
- [ChipSelectorAttribute](#chipselectorattribute)
- [ChipSelectorDrawer](#chipselectordrawer)
- [ChipSortingLayer](#chipsortinglayer)
- [CombinedBlockingState](#combinedblockingstate)
- [ContainerInfo](#containerinfo)
- [DeferredCell](#deferredcell)
- [DeferredChipChangeNotifier](#deferredchipchangenotifier)
- [DraggableChipLogic](#draggablechiplogic)
- [Effect](#effect)
- [EffectBlockerData](#effectblockerdata)
- [EffectBlockerDefinitionAttribute](#effectblockerdefinitionattribute)
- [EffectBlockerSelectorAttribute](#effectblockerselectorattribute)
- [EffectBlockerSelectorDrawer](#effectblockerselectordrawer)
- [EffectBlockingSettings](#effectblockingsettings)
- [EffectConsts](#effectconsts)
- [EffectContainerRef](#effectcontainerref)
- [EffectDefinitionAttribute](#effectdefinitionattribute)
- [EffectDestroyingRuntimeData](#effectdestroyingruntimedata)
- [EffectDestroyingSettings](#effectdestroyingsettings)
- [EffectExtraData](#effectextradata)
- [EffectGeneratorChargingRef](#effectgeneratorchargingref)
- [EffectPowerBoosterJoinRef](#effectpowerboosterjoinref)
- [EffectRef](#effectref)
- [EffectSelectorAttribute](#effectselectorattribute)
- [EffectSelectorDrawer](#effectselectordrawer)
- [ExtraChip](#extrachip)
- [FieldChipData](#fieldchipdata)
- [FieldData](#fielddata)
- [FieldEventHandler](#fieldeventhandler)
- [FieldGrid](#fieldgrid)
- [FieldInitializeCommand](#fieldinitializecommand)
- [FillContainerLogic](#fillcontainerlogic)
- [FreeCellFinder](#freecellfinder)
- [ICell](#icell)
- [ICellSubscriber](#icellsubscriber)
- [IChipChangeNotifier](#ichipchangenotifier)
- [IChipCollections](#ichipcollections)
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
- [ILockedAreaManager](#ilockedareamanager)
- [IMergeCamera](#imergecamera)
- [IMergeLifetimeScope](#imergelifetimescope)
- [IPowerBoosterTarget](#ipowerboostertarget)
- [IVisualField](#ivisualfield)
- [LockedAreaEffect](#lockedareaeffect)
- [LockedAreaManager](#lockedareamanager)
- [Merge2Initializer](#merge2initializer)
- [Merge2LifetimeScope](#merge2lifetimescope)
- [MergeableChipLogic](#mergeablechiplogic)
- [MergeCamera](#mergecamera)
- [MergeCombination](#mergecombination)
- [MergeResult](#mergeresult)
- [NeighborChipFinder](#neighborchipfinder)
- [PowerBoosterCellSubscriber](#powerboostercellsubscriber)
- [PowerBoosterConnectorCellsHighlightEffect](#powerboosterconnectorcellshighlighteffect)
- [PowerBoosterJoinEffect](#powerboosterjoineffect)
- [ShadowEffect](#shadoweffect)
- [SortingLayerData](#sortinglayerdata)
- [VisualField](#visualfield)

---

## BlockedCell
#### Fields
- `++ Chip: Chip`
- `++ IsBlocked: bool`
- `++ MainCell: ICell`
- `+- CellPosition: Vector2Int`
- `+- Transform: Transform`
#### Methods
- `+ GetColorForLevelEditor(): Nullable<Color>`
- `+ GetLocalPositionForChip(Vector2Int chipSize): Vector3`
- `+ Init(Vector2Int cellPos): void`
- `+ OnDrag(Vector2 position, bool isValidPosition): void`
- `+ OnDragEnd(Vector2 position): void`
- `+ OnDragStart(Vector2 position): void`
- `+ OnTap(Vector2 position): void`
---

## Cell
**Inherits**: `MonoBehaviour`

> - **Purpose**: Represents a single cell in the game grid that can hold a chip.
> - **Usage**: Attach to grid cell GameObjects
> - used to manage chip placement and interaction.
> - **Notes**: Acts as a container for a chip and handles chip movement/interaction logic.
#### Fields
- `++ Chip: Chip`
- `++ IsBlocked: bool`
- `++ MainCell: ICell`
- `+- CellPosition: Vector2Int`
- `+- Transform: Transform`
- `~ field: IFieldEventHandler`
- `~ flyAnimation: IChipFlyAnimation`
- `~ logEnable: bool`
- `~ movingTime: float`
#### Methods
- `+ GetLocalPositionForChip(Vector2Int chipSize): Vector3`
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
- `- Expecto.MergeBase.ICell.GetColorForLevelEditor(): Nullable<Color>`
- `- Update(): void`
- `- WaitAndStartFlyAnimation(Chip chip, Transform tr, Vector3 localTarget, float movingTime, float delay): IEnumerator`
---

## CellHighlightEffect
**Inherits**: `Effect`

> - **Purpose**: Visual effect that highlights the cell under a chip
> - **Usage**: Attached to chip using ChipData.CellHighlightPrefab
> - Managed by Chip to show/hide highlights
> - **Notes**: Handles multiple cells if chip size > 1x1
> - updates position during drag interaction
#### Fields
- `~ alphaId: int`
- `~ chipSize: Vector2Int`
- `~ color: Color`
- `~ fieldGrid: IFieldGrid`
- `~ globalAlpha: float`
- `~ highlightPrefab: GameObject`
- `~ highlights: List<GameObject>`
- `~ order: float`
- `~ sharedMaterial: Material`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnInteractionOverCellChanged(ICell sourceCell, ICell targetCell, ICell interactableCell): void`
- `~ CreateHighlight(Vector3 localPosition): GameObject`
- `~ CreateHighlights(): void`
- `~ DestroyHighlights(): void`
- `- SetHighlightsVisible(bool value): void`
- `~ Update(): void`
---

## CellObserverManager
**Inherits**: `MonoBehaviour`

> - **Purpose**: Manages cell observation subscriptions with a physical-cell index.
> - **Usage**: Subscribers are stored per physical cell. On flush, notifications are expanded by old/new chip footprint so secondary-cell observers are notified without fold/promote state.
> - **Notes**: Optimized for low churn: no topology migration, O(1) subscribe/unsubscribe operations, and reusable snapshot list for callback-safe iteration.
#### Fields
- `- cellToSubscribers: Dictionary<ICell, HashSet<ICellSubscriber>>`
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
- `- subscriberToCells: Dictionary<ICellSubscriber, HashSet<ICell>>`
    - **Purpose**: Reverse index: subscriber to all watched physical cells.
    - **Usage**: Used for O(k) unsubscribe and API data extraction without scanning all cells.
#### Methods
- `+ GetSubscriptionData(ICellSubscriber subscriber): List<SubscriptionData>`
    - **Purpose**: Returns subscription state for diagnostics/tests with backward-compatible direct/virtual representation.
    - **Usage**: Secondary watched cells are grouped under current MainCell as virtual positions to preserve old API contract.
- `+ LogSubscriptions(): void`
- `+ Subscribe(ICellSubscriber subscriber, List<Vector2Int> cellPositions): void`
- `+ Unsubscribe(ICellSubscriber subscriber): void`
- `- AddSubscription(ICellSubscriber subscriber, ICell cell): void`
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
- `+ OnChipChangedCell(ICell sourceCell, ICell targetCell): void`
    - **Purpose**: Re-subscribes to neighbors whenever the chip is placed or moved to a new cell.
    - **Usage**: Called by Cell.Chip setter
    - sourceCell == null means initial placement.
    - **Params**: sourceCell - previous cell (null on initial placement)
    - targetCell - new cell with correct CellPosition
    - **Notes**: Unsubscribes from old neighbors first to avoid stale subscriptions after a move.
- `+ OnChipDestroy(ICell mainCell): void`
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
- `+- AnimationNode: Transform`
- `+- Animator: Animator`
- `+- AppearanceDelay: float`
- `+- BlockingState: CombinedBlockingState`
- `+- CellSubscriber: ICellSubscriber`
- `+- Data: ChipData`
- `+- IsMoving: bool`
- `+- MergeData: ChipMergeData`
- `+- RuntimeData: ChipRuntimeData`
- `+- SortingLayer: IChipSortingLayer`
- `~ chipChangeNotifier: IChipChangeNotifier`
- `~ chipCollections: IChipCollections`
- `~ dontRepeatTrigger: bool`
- `~ effectOfPrioritizingDestroying: IEffect`
- `~ effects: Dictionary<int, IEffect>`
- `~ extraEffectsData: ChipExtraEffectsData`
- `~ fieldGrid: IFieldGrid`
- `~ invertTransformNode: Transform`
- `~ isDragging: bool`
    - **Purpose**: Tracks whether the chip is currently being dragged by the user
    - **Usage**: Set via SetDragging
    - queried via IsDragging
    - used to distinguish user drag from automated movement
    - **Notes**: Separate from IsMoving which tracks visual/sorting state
    - allows detection of user-initiated drag vs system movement
- `- lastTrigger: string`
- `~ lastTriggerName: string`
- `~ resolver: IObjectResolver`
- `~ transformNode: Transform`
#### Methods
- `+ CanMoving(): bool`
    - **Purpose**: Checks whether the chip can currently be moved by the player
    - **Usage**: Called by DraggableChipLogic before allowing drag operations
    - returns false if chip is locked
    - **Returns**: True if chip can be moved
    - false if movement is locked
    - **Notes**: Based on runtimeData.IsMoveLocked
    - prevents drag-and-drop when locked
- `+ Destroy(ICell mainCell, bool force): void`
    - **Purpose**: Destroys the chip and all its attached effects
    - **Usage**: Call to remove the chip from the field and scene
    - override in derived classes for custom teardown before or after base destruction
    - **Params**: mainCell - the chip's main occupied cell on the field grid
    - force - if true, destruction happens faster (0.1s), otherwise slower (0.3s) to allow for animations
    - **Notes**: Clears grid occupancy via FieldGrid first (which enqueues chip-change notifications), then invokes ICellSubscriber cleanup while cell context is still valid, destroys spawned effect objects, and finally schedules GameObject destruction with a delay based on force parameter
- `+ GetEffect(int effectHash): IEffect`
    - **Purpose**: Retrieves an effect from the effects dictionary by its EffectConsts hash key
    - **Usage**: Call in methods that need to access a specific effect without type casting
    - **Params**: effectHash - One of EffectConsts values (e.g., EffectConsts.MoveLocked)
    - **Returns**: The IEffect instance or null if not found
    - **Notes**: Use GetEffect<T>() for type-safe access when specialized interfaces are needed
- `+ GetEffect(int effectHash): T`
    - **Purpose**: Retrieves and casts an effect from the effects dictionary by its EffectConsts hash key
    - **Usage**: Call when you need a specialized effect interface (e.g., IEffectGeneratorCharging)
    - **Params**: effectHash - One of EffectConsts values (e.g., EffectConsts.GeneratorCharging)
    - T - Target effect interface type
    - **Returns**: The effect cast to type T, or null if not found or cannot be cast
    - **Notes**: Handles null safely with null-conditional operator (?.) at callsite
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
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
    - **Purpose**: Called when the chip is moved to a new cell
    - updates all attached effects accordingly
    - **Usage**: Call after the chip's parent cell has changed
    - propagates cell change to all effects
    - **Params**: sourceCell - the cell the chip was previously in
    - targetCell - the cell the chip is moved to
- `+ OnDrag(Vector2 position, ICell anchorCell): void`
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
- `+ OnInteractionOverCellChanged(ICell prevCell, ICell currentCell, ICell underCell): void`
    - **Purpose**: Notifies all chip effects when the dragged chip moves between cells
    - **Usage**: Called by DraggableChipLogic when chip position changes during drag to update visual effects
    - **Params**: prevCell - previous cell position
    - currentCell - new cell position
    - underCell - cell currently under the chip
    - **Notes**: Propagates cell change event to all attached effects like highlights, merge indicators, etc.
- `+ OnInteractionUnderCellChanged(ICell underCell, ICell overCell): void`
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
- `+ ResetTrigger(string triggerName): void`
- `+ SendTrigger(AnimatorTrigger trigger, bool allowRepeat): void`
- `+ SendTrigger(string trigger, bool allowRepeat): void`
    - **Purpose**: Sends a trigger to the animator and synchronizes the 'Little' state.
    - **Usage**: Called when a chip state change requires an animation trigger (e.g., Generate, Recharge).
    - **Params**: trigger - the animator trigger name to fire
    - allowRepeat - if true, bypasses the dontRepeatTrigger check
    - **Notes**: Automatically updates the 'Little' boolean in the animator based on the chip's blocking state. Prevents redundant triggers if dontRepeatTrigger is enabled.
- `+ SetAppearanceDelay(float delay): void`
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
- `+ SetRotationZ(float zAngle): void`
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
- `- AppearanceDelayCoroutine(float delay): IEnumerator`
- `~ DestroyEffects(float destroyDelay): void`
    - **Purpose**: Destroys all attached effects with a specified delay
    - **Usage**: Called from Destroy() to clean up visual effects with a timing synchronized with GameObject destruction
    - **Params**: destroyDelay - delay in seconds before GameObjects are destroyed
- `- HandleDestroyingEffects(): void`
- `~ HasTrigger(string name): bool`
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
- `~ NotifyEffectsOnChangedCell(ICell sourceCell, ICell targetCell): void`
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
- `+- Cell: ICell`
- `+- ChipAdded: bool`
- `+- ChipRemoved: bool`
- `+- ChipReplaced: bool`
- `+- NewChip: Chip`
- `+- OldChip: Chip`
#### Methods
- `+ ToString(): string`
---

## ChipCollections

> - **Purpose**: Provides a centralized collection of all active chips on the field, categorized by their data type
> - **Usage**: Injected as IChipCollections
> - used to find all chips of a specific type or to iterate over all active chips
> - **Notes**: Updated by ChipFactory during creation and by Chip during destruction
#### Fields
- `+- AllChipsByData: Dictionary<ChipData, List<Chip>>`
#### Methods
- `+ AddChip(Chip chip): void`
    - **Purpose**: Adds a chip to the collection, grouping it by its ChipData
    - **Usage**: Called by ChipFactory immediately after a new chip is initialized
    - **Params**: chip - the chip instance to add
- `+ RemoveChip(Chip chip): void`
    - **Purpose**: Removes a chip from the collection and cleans up empty data entries
    - **Usage**: Called by Chip.Destroy just before the chip GameObject is destroyed
    - **Params**: chip - the chip instance to remove
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
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
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
- `+- Blockers: EffectBlockerData[]`
- `+- BlockersDict: Dictionary<int, EffectBlockerData>`
- `+- OtherEffects: EffectExtraData[]`
- `+- OtherEffectsDict: Dictionary<int, EffectExtraData>`
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
- `~ chipCollections: IChipCollections`
- `~ fieldGrid: IFieldGrid`
- `~ resolver: IObjectResolver`
#### Methods
- `+ CreateChip(ICell cell, ChipData chipData, Nullable<Vector3> parentWorldPosition, Action<ChipRuntimeData> runtimeDataInitializer, Nullable<float> appearanceDelay): Chip`
- `+ CreateChip(Vector2Int cellPosition, ChipData chipData, Nullable<Vector3> parentWorldPosition, Action<ChipRuntimeData> runtimeDataInitializer, Nullable<float> appearanceDelay): Chip`
- `+ Init(IObjectResolver resolver, IFieldGrid fieldGrid, IChipCollections chipCollections): void`
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
- `+- AppliedBoosters: HashSet<ChipPowerBooster>`
- `+- JoinPoints: IReadOnlyList<Transform>`
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
- `+ ApplyPowerBooster(ChipPowerBooster chipPowerBooster, bool reapply): bool`
    - **Purpose**: Applies a booster to the generator and recalculates effective multiplier.
    - **Usage**: Called when booster subscriber detects this generator entering observed range.
    - **Params**: chipPowerBooster - booster to register as active
    - **Returns**: True when the booster was newly added
    - false when it was already present.
    - **Notes**: Multiplier becomes the maximum power among active boosters to prevent stacking by sum.
- `+ Destroy(ICell mainCell, bool force): void`
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
    - **Purpose**: Notifies all active boosters that an effect has been removed from this generator.
    - **Usage**: Called from RemoveEffect
    - allows boosters to re-evaluate their influence if they were previously blocked.
    - **Params**: effectId - the ID of the effect that was removed
- `+ OnTap(Vector2 position): void`
    - **Purpose**: Handles tap input for manual chip generation.
    - **Usage**: Call when the player taps the generator in manual mode.
    - **Params**: position - Tap position in world coordinates.
    - **Notes**: No effect in auto mode or if not charged. Uses TryGenerateChip for logic.
- `+ RemovePowerBooster(ChipPowerBooster chipPowerBooster): void`
    - **Purpose**: Removes a booster and recalculates multiplier from remaining active boosters.
    - **Usage**: Called when booster subscriber detects this generator leaving observed range or booster destruction.
    - **Params**: chipPowerBooster - booster to remove
    - **Notes**: Resets multiplier to 1f when no boosters remain
    - logs warning if removal is requested for a non-registered booster.
- `+ SetMoving(bool value): void`
    - **Purpose**: Updates movement state and notifies all active boosters of the movement change.
    - **Usage**: Called by the movement system when the generator starts or stops moving.
    - **Params**: value - true if starting movement, false if stopping
    - **Notes**: Ensures boosters can update their visual links during generator relocation.
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
    - **Usage**: Called from ApplyPowerBooster after adding or reapplying a booster
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
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
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
- `+ OnInteractionUnderCellChanged(ICell underCell, ICell overCell): void`
---

## ChipMergeData

> - **Purpose**: Defines merge logic and potential outcomes for a chip
> - **Usage**: Component of ChipData
> - used to determine merge compatibility and resulting chips
#### Fields
- `++ Combinations: MergeCombination[]`
- `+- MergeableChips: IEnumerable<ChipData>`
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
- `+ CanChipMoving(ICell leftTopCell, IEnumerable<Chip> chipsToExclude, Vector2Int chipSize, List`1& plannedRelocations): bool`
    - **Purpose**: Checks if a chip can move to a target and if conflicting chips can be relocated
    - **Usage**: Internal check before performing actual relocation
    - also used by merge logic for growing chips
    - **Params**: leftTopCell - target top-left cell
    - chipsToExclude - chips to ignore during collision checks
    - chipSize - dimensions of the chip
    - plannedRelocations - output list of moves to perform
    - **Returns**: True if a valid state is possible for all chips involved
    - **Notes**: Sorted relocation: larger chips are relocated first to minimize fragmentation
- `+ ChipMoving(ICell overCell, ICell leftTopCell, ICell sourceCell): void`
    - **Purpose**: Orchestrates the move of a chip to a new position, potentially relocating others
    - **Usage**: Call when a drag operation ends at a specific cell
    - **Params**: overCell - the cell the mouse is over
    - leftTopCell - the target top-left cell for the chip
    - sourceCell - the original cell of the chip
    - **Notes**: If relocation is impossible, the chip snaps back to its source position
- `+ ChipsRelocate(ICell leftTopCell, ICell sourceCell, List<ChipMoveAction> plannedRelocations): void`
    - **Purpose**: Executes the planned relocations of chips on the field
    - **Usage**: Final step of the relocation process
    - performs atomic field updates
    - **Params**: leftTopCell - destination for the primary chip
    - sourceCell - original spot of the primary chip
    - plannedRelocations - list of pre-calculated chip moves
    - **Notes**: Temporarily sets dragging state during relocation for visual consistency
- `- GetOccupiedCellsInArea(Vector2Int cellPos, Vector2Int chipSize, IEnumerable<Chip> chipsToExclude): List<ICell>`
---

## ChipPowerBooster
**Inherits**: `Chip`

> - **Purpose**: Chip that boosts nearby IPowerBoosterTarget entities and controls linked booster-specific effects.
> - **Usage**: Attach to booster chip prefabs together with PowerBoosterCellSubscriber to auto-apply/remove boosters as neighbor composition changes.
> - **Notes**: Coordinates numeric power boost state and optional join/highlight effects for movement and destruction lifecycles.
#### Fields
- `+- BoostedTargets: HashSet<IPowerBoosterTarget>`
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
- `+ ApplyPowerBooster(IPowerBoosterTarget target, bool reapply): void`
    - **Purpose**: Applies booster influence to a target entity and notifies join effect.
    - **Usage**: Called by PowerBoosterCellSubscriber when a matching target enters observed cells.
    - **Params**: target - chip/entity receiving this booster
- `+ Destroy(ICell mainCell, bool force): void`
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
- `+ OnTargetChipEffectRemoved(IPowerBoosterTarget chipTarget, int effectId): void`
    - **Purpose**: Reapplies booster influence when a target's blocking state changes due to effect removal
    - **Usage**: Called by IPowerBoosterTarget.NotifyEffectRemoved on each active booster when a target effect is removed
    - **Params**: chipTarget - target entity whose effect was removed
    - effectId - ID of the removed effect
    - **Notes**: Only reapplies if the target now has CanReceiveModifiers true
    - triggers joinEffect.OnJoin if reapply succeeds
- `+ OnTargetChipMoved(IPowerBoosterTarget chipTarget, bool value): void`
    - **Purpose**: Relays target movement notifications to the active join effect.
    - **Usage**: Called by target entities when they start or stop moving.
    - **Params**: chipTarget - the moving target
    - value - true if move began, false if it ended
    - **Notes**: Allows join link visuals to stay synchronized or hide during target relocation.
- `+ RemovePowerBooster(IPowerBoosterTarget target): void`
    - **Purpose**: Removes booster influence from a target entity and notifies join effect.
    - **Usage**: Called by PowerBoosterCellSubscriber when a matching target leaves observed cells or gets removed.
    - **Params**: target - chip/entity losing this booster
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
    - **Purpose**: Set of blocker effect IDs (from EffectConsts/EffectConsts.Blockers) that should be active on this chip
    - **Usage**: Populated from FieldChipData.ExtraEffectIds during level load or at runtime
    - queried by Chip.InitEffects and UpdateVisual to determine which extra effects to instantiate and activate
    - **Notes**: Serialized via effectEnablesArray for save/load support
- `- effectDestroyingDataArray: EffectDestroyingRuntimeData[]`
- `- effectEnablesArray: int[]`
#### Methods
- `+ OnAfterDeserialize(): void`
- `+ OnBeforeSerialize(): void`
---

## ChipSelectorAttribute
**Inherits**: `PropertyAttribute`

> - **Purpose**: Transforms a string field in the inspector into a dropdown selecting available ChipData asset names
> - **Usage**: Apply [ChipSelector] to a serialized string field
> - **Notes**: Requires ChipSelectorDrawer in an Editor assembly. Performs an AssetDatabase search for all objects of type ChipData.
---

## ChipSelectorDrawer
**Inherits**: `PropertyDrawer`
#### Methods
- `+ OnGUI(Rect position, SerializedProperty property, GUIContent label): void`
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

## DeferredCell
**Inherits**: `MonoBehaviour`

> - **Purpose**: Add-on component that stores initial chip data until its locked area becomes available
> - **Usage**: Added automatically by FieldGrid to regular Cell or IsoCell prefabs for locked-area deferred coordinates
> - **Notes**: Keeps deferred loading independent from the concrete cell class
> - requires an ICell component on the same GameObject
#### Fields
- `- chipFactory: ChipFactory`
- `- deferredCellData: CellData`
- `- deferredChipData: ChipData`
- `- hasDeferredChip: bool`
- `- ownerCell: ICell`
#### Methods
- `+ Init(ICell cell): void`
    - **Purpose**: Binds this deferred loader to the concrete grid cell it augments
    - **Usage**: Called by FieldGrid immediately after adding this component to a spawned cell GameObject
    - **Params**: cell - real Cell or IsoCell implementation that should receive the deferred chip
    - **Notes**: The component does not implement ICell itself and always delegates chip placement to this owner cell
- `+ SetupDeferredChip(CellData cellData, ChipData chipData): void`
    - **Purpose**: Stores level chip configuration without creating the chip immediately
    - **Usage**: Called by FieldInitializeCommand.LoadChips when the target cell is deferred
    - **Params**: cellData - serialized cell placement and runtime effect state
    - chipData - chip definition resolved from the collection
    - **Notes**: A later call to SpawnDeferredChip consumes this data exactly once
- `+ SpawnDeferredChip(): Chip`
    - **Purpose**: Creates the delayed chip when the owning locked area opens
    - **Usage**: Called by LockedAreaManager.UnlockArea after the cell is unblocked
    - **Returns**: Created chip instance, or null when there is no stored chip data or creation fails
    - **Notes**: Copies blocker effect state from FieldData so deferred chips match normal LoadChips initialization
- `+ TryGet(ICell cell, DeferredCell& deferredCell): bool`
- `- EnsureOwnerCell(): void`
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
- `- pendingIndexByCell: Dictionary<ICell, int>`
    - **Purpose**: Maps changed cell to its index in pending list.
    - **Usage**: Allows O(1) collapse of repeated updates for the same cell within one frame.
#### Methods
- `+ Enqueue(ICell cell, Chip oldChip, Chip newChip): void`
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
- `~ chipInteractionLogics: List<IChipInteractionLogic>`
    - **Purpose**: Collection of all chip interaction logic handlers
    - **Usage**: Internal field
    - populated in Awake and queried during drag operations
    - **Notes**: Populated in Awake from attached components
    - used to check and execute interactions like merge or container fill
- `~ chipMovingLogic: IChipMovingLogic`
- `~ currentMergableCell: ICell`
- `~ currentMergableLogic: IChipInteractionLogic`
- `~ draggableChip: Chip`
- `~ draggableTransform: Transform`
    - **Purpose**: Cached transform of the chip being dragged
    - **Usage**: Internal field
    - cached for performance during drag operations
    - **Notes**: Set in OnDragStart for performance optimization during drag
- `~ fieldGrid: IFieldGrid`
- `~ prevCell: ICell`
- `~ sourceCell: ICell`
    - **Purpose**: Stores the cell from which the chip was originally dragged
    - **Usage**: Internal field
    - automatically set in OnDragStart and used throughout drag operations
    - **Notes**: Set in OnDragStart
    - used to track the chip's origin during drag operations
- `~ sourceCellTransform: Transform`
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
- `+ OnDrag(Vector3 worldPosition, ICell anchorCell): void`
    - **Purpose**: Handles chip movement and merge effect during drag.
    - **Usage**: Call continuously while dragging, passing current mouse/cell info.
    - **Params**: worldPosition - mouse position
    - anchorCell - cell under chip
    - **Notes**: Activates/deactivates merge effect on target chips
    - chip position is constrained to field boundaries.
- `+ OnDragEnd(ICell overCell, ICell leftTopCell): void`
    - **Purpose**: Handles logic when drag ends, including merge, swap, and cleanup.
    - **Usage**: Call when drag operation finishes, passing the cell under the chip and the anchor cell.
    - **Params**: overCell - cell under chip on drop
    - leftTopCell - anchor cell for placement
    - **Notes**: Cleans up visual interaction indicators
    - invokes merge handlers, performs swap if not handled, resets state.
- `+ OnDragStart(ICell sourceCell, Vector3 worldPosition): void`
    - **Purpose**: Initializes drag state for a chip.
    - **Usage**: Call when drag starts, passing the source cell and mouse position.
    - **Params**: sourceCell - cell chip is dragged from
    - worldPosition - mouse position in world space
    - **Notes**: Sets up internal references and marks chip as dragging.
- `~ Awake(): void`
- `- GetFilterCells(ICell targetCell): List<ICell>`
- `- MoveToWorldPosition(Vector3 worldPosition): void`
- `- ResetCurrentMergable(): void`
- `- ResetDragState(): void`
- `- UpdateInteractionState(ICell sourceCell, ICell targetCell): void`
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
    - **Usage**: Call when the chip or non-chip visual owner is activated
    - trigger 'Activate' animation if configured
    - **Params**: chip - the chip this effect belongs to
    - **Returns**: True if the effect was activated
    - false if skipped because effectId is in chip.BlockingState.HideEffectIds
    - **Notes**: Pass null only for non-chip visuals
    - when chip is provided, applies BlockingSettings and respects HideEffectIds
- `+ Deactivate(Chip chip, bool force): void`
    - **Purpose**: Deactivates the effect on the specified chip
    - **Usage**: Call when the chip is deactivated or disabled
    - trigger 'Deactivate' animation if configured
    - **Params**: chip - the chip this effect belongs to
    - force - if true, forces immediate animation state change (plays 'Deactivate' state at end time)
- `+ GetId(): int`
    - **Purpose**: Returns the unique hash/ID of this effect for identification in the chip's effects dictionary
    - **Usage**: Called by external code to retrieve the effect's ID for lookup or removal
    - **Returns**: The hash code that identifies this effect
- `+ Init(Chip chip, int effectId): void`
    - **Purpose**: Initializes the effect with chip reference and its effect hash ID
    - **Usage**: Called from Chip.AddEffect after instantiation to set up the effect's runtime state
    - **Params**: chip - the chip this effect belongs to
    - effectId - unique hash for identifying this effect
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
    - **Purpose**: Handles logic when a chip is moved from one cell to another
    - **Usage**: Called after the chip's parent cell has changed
    - updates the effect's transform to match the new cell if effectForCell is true
    - override in derived classes for custom behavior
    - **Params**: sourceCell - the cell the chip was previously in
    - targetCell - the cell the chip is moved to
    - **Notes**: Restores local position and scale after changing parent transform
    - only applies if effectForCell is true
- `+ OnInteractionOverCellChanged(ICell prevCell, ICell currentCell, ICell underCell): void`
    - **Purpose**: Called when the chip's cell changes during drag
    - **Usage**: Handle logic when a chip is dragged from one cell to another during drag-and-drop
    - Override in derived classes for custom behavior
    - **Params**: prevCell - the cell the chip was in before the drag cell change
    - currentCell - the cell the chip is currently over during drag
    - **Notes**: Should only be called as part of drag operations
    - no default implementation in base class
- `+ OnInteractionUnderCellChanged(ICell underCell, ICell overCell): void`
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
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
- `~ HasTrigger(string name): bool`
- `~ ResetTrigger(string triggerName): void`
    - **Purpose**: Resets an animation trigger on the effect's animator
    - **Usage**: Internal helper to clear opposing triggers when switching states
    - **Params**: triggerName - name of the trigger to reset
    - **Notes**: Safely handles null animator
---

## EffectBlockerData

> - **Purpose**: Serializable entry mapping an effect ID to its prefab for extra blocker/overlay effects
> - **Usage**: Configured in ChipExtraEffectsData.blockers array
> - integrates directly with EffectConsts IDs
> - **Notes**: effectId is updated via inspector combobox using [EffectSelector]
#### Fields
- `+- EffectId: int`
- `+- Prefab: GameObject`
---

## EffectBlockerDefinitionAttribute
**Inherits**: `EffectDefinitionAttribute`

> - **Purpose**: Marks a constant field as a custom Blocker effect definition
> - **Usage**: Apply this attribute to 'const int' fields that are considered blockers/extras
> - **Params**: name - The string name of the effect identifier
> - **Notes**: Inherits from EffectDefinitionAttribute. Collected at startup to populate blocker-specific dictionaries
---

## EffectBlockerSelectorAttribute
**Inherits**: `PropertyAttribute`

> - **Purpose**: Transforms an integer field in the inspector into a dropdown selecting available blocker effect names
> - **Usage**: Apply [EffectBlockerSelector] to a serialized int or int[] field
> - **Notes**: Requires EffectBlockerSelectorDrawer
> - filters to effects with ID > 100
---

## EffectBlockerSelectorDrawer
**Inherits**: `PropertyDrawer`
#### Methods
- `+ OnGUI(Rect position, SerializedProperty property, GUIContent label): void`
---

## EffectBlockingSettings
**Inherits**: `ScriptableObject`

> - **Purpose**: ScriptableObject asset that configures which chip actions are blocked when an effect is active
> - **Usage**: Assigned to Effect.blockingSettings in Inspector
> - applied to CombinedBlockingState via ApplyBlock during Effect.Activate
> - **Notes**: hideEffects array is directly configured in the Inspector using integer [EffectSelector]. Cached to hideEffectIds HashSet during deserialization.
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
- `- cachedHideEffectIds: HashSet<int>`
---

## EffectConsts

> - **Purpose**: Centralized registry of integer effect IDs used as keys in the chip effects dictionary
> - **Usage**: Reference these constants when adding, retrieving, or removing effects via Chip.AddEffect/GetEffect/RemoveEffect
> - **Notes**: Base IDs are for built-in effects
> - Extra subclass (101+) is for optional blocker/overlay effects configured through ChipExtraEffectsData
> - nameToId dictionary enables serialized string-to-int resolution. Supports extension via EffectDefinitionAttribute.
#### Fields
- `+ CellHighlight: int`
- `+ ContainerRequirements: int`
- `+ GeneratorCharged: int`
- `+ GeneratorCharging: int`
- `+ MergeAvailable: int`
- `+ MergeLight: int`
- `+ PBoosterConnectorCells: int`
- `+ PBoosterJoin: int`
- `+ ShadowEffect: int`
- `- blockerNameToId: Dictionary<string, int>`
- `- nameToId: Dictionary<string, int>`
#### Methods
- `+ GetAllEffectsEditorOnly(): Dictionary<string, int>`
- `+ GetBlockerEffectsEditorOnly(): Dictionary<string, int>`
- `+ GetNameByIdEditorOnly(int id): string`
---

## EffectContainerRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## EffectDefinitionAttribute
**Inherits**: `Attribute`

> - **Purpose**: Marks a constant field as a custom effect definition to be collected by EffectConsts
> - **Usage**: Apply this attribute to 'const int' fields in external projects to register custom effects automatically
> - **Params**: name - The string name of the effect identifier
> - **Notes**: Collected at startup via reflection to populate EffectConsts.nameToId
#### Fields
- `+- Name: string`
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

## EffectExtraData

> - **Purpose**: Extended effect data include activation/deactivation triggers
> - **Usage**: Used in OtherEffects list for chips that need effects with specific lifecycle control
> - **Notes**: Supports custom activation/deactivation logic during chip initialization
#### Fields
- `+- ActivateOnStart: bool`
- `+- DeactivateOnStart: bool`
- `+- EffectId: int`
- `+- Prefab: GameObject`
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

## EffectSelectorAttribute
**Inherits**: `PropertyAttribute`

> - **Purpose**: Transforms a string field in the inspector into a dropdown selecting available effect names
> - **Usage**: Apply [EffectSelector] to a serialized string field
> - **Notes**: Requires EffectSelectorDrawer in an Editor assembly
#### Fields
- `+- IgnoreBlockers: bool`
---

## EffectSelectorDrawer
**Inherits**: `PropertyDrawer`
#### Methods
- `+ OnGUI(Rect position, SerializedProperty property, GUIContent label): void`
---

## ExtraChip

> - **Purpose**: Represents an extra chip that can be generated or rewarded with a certain chance
> - **Usage**: Used in merge outcomes or loot tables to provide bonus chips
#### Fields
- `++ Chance: int`
- `++ ChipData: FieldChipData`
---

## FieldChipData

> - **Purpose**: Contains data for a chip specifically placed on the field, including its ID and current state/effects
> - **Usage**: Used within FieldData.CellData to manage chip instances on the game board
#### Fields
- `+ BlockerEffectIds: int[]`
    - **Purpose**: Array of blocker effect IDs to activate on this chip when loaded from level data
    - **Usage**: Populated by Level Editor
    - consumed by FieldInitializeCommand to populate ChipRuntimeData.EffectEnables
    - **Notes**: Values correspond to EffectConsts.Blockers constants (e.g. MoveLockedEffect=103)
- `+ ChipId: string`
    - **Purpose**: Unique identifier for the chip type
    - **Usage**: Should correspond to a chip name in the ChipDataCollection
---

## FieldData
**Inherits**: `ScriptableObject`
#### Fields
- `+- BlockedCells: Vector2Int[]`
- `+- Cells: CellData[]`
- `+- ChipDataCollection: ChipDataCollection`
- `+- FieldSize: Vector2Int`
- `+- LevelVisualPrefab: GameObject`
- `+- LockedAreas: LockedAreaData[]`
#### Methods
- `+ SetBlockedCells(Vector2Int[] newBlockedCells): void`
    - **Purpose**: Editor-only setter for blocked cells array
    - **Usage**: Called by LevelEditorWindow when saving level data to assign blocked cells array
    - **Params**: newBlockedCells - array of blocked cell positions to persist
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
- `+ SetLevelVisualPrefab(GameObject prefab): void`
    - **Purpose**: Editor-only setter for the level visual prefab reference
    - **Usage**: Called by level editing tools when assigning visuals that contain LockedAreaEffect components
    - **Params**: prefab - prefab instantiated during runtime locked-area initialization
- `+ SetLockedAreas(LockedAreaData[] newLockedAreas): void`
    - **Purpose**: Editor-only setter for the lockedAreas array
    - **Usage**: Called by LevelEditorWindow when saving level data after editing locked areas
    - **Params**: newLockedAreas - array of locked area definitions to persist
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
- `+- IsChipDragging: bool`
- `- chipChangeNotifier: IChipChangeNotifier`
- `~ draggableChip: DraggableChipLogic`
- `~ fieldGrid: IFieldGrid`
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
- `+- Cells: ICell[]`
- `+- FieldSize: Vector2Int`
- `- chipChangeNotifier: IChipChangeNotifier`
- `- mergeSettings: MergeSettings`
- `- resolver: IObjectResolver`
#### Methods
- `+ CreateCells(FieldData fieldData): void`
    - **Purpose**: Creates and initializes the grid of cells based on field size.
    - **Usage**: Call during field initialization to create all cells.
    - **Params**: fieldData - configuration for the field including size and cell data.
    - **Notes**: Centers cells around origin
    - creates Cell GameObjects with RectTransform.
- `+ GetCells(Vector2Int cellPos, Vector2Int size): List<ICell>`
    - **Purpose**: Retrieves all cells in a rectangular area.
    - **Usage**: Call to get cells occupied by a multi-cell chip.
    - **Params**: cellPos - top-left position
    - size - area dimensions (width, height).
    - **Returns**: List of all cells in the specified area.
- `+ HasBlockedCells(Vector2Int cellPos, Vector2Int size): bool`
    - **Purpose**: Checks whether a rectangular grid area contains cells that cannot receive chips
    - **Usage**: Use before movement, relocation, and drag highlight display for multi-cell chips
    - **Params**: cellPos - top-left position of the area
    - size - area dimensions
    - **Returns**: True if the area is outside the field or any covered cell is blocked
    - otherwise false
    - **Notes**: Runs the bounds check first to avoid invalid Cells indexing
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
- `+ SetChipInCell(ICell cell, Chip chip): void`
    - **Purpose**: Places a chip into a target cell and updates observer-visible state for placement/removal.
    - **Usage**: Call to place, move, or remove a chip in grid coordinates while keeping chip-change notifications and multi-cell occupancy metadata in sync.
    - **Params**: cell - target main cell for placement or clearing
    - chip - chip instance to place (null means clear).
    - **Notes**: When placing, CellPosition is assigned before chipChangeNotifier.Enqueue so observers handling the event read the chip at its new logical coordinates
    - when clearing, all occupied cells are reset via ClearCells before enqueueing old/new chip state.
- `- AddDeferredCell(GameObject cellGO, ICell cell): void`
    - **Purpose**: Attaches the deferred chip loader to a spawned real cell
    - **Usage**: Called during CreateCells for coordinates listed in CellsToLockAndDeferred
    - **Params**: cellGO - spawned cell GameObject
    - cell - ICell implementation on that GameObject
    - **Notes**: Supports both base Cell and derived cells such as IsoCell without requiring a DeferredCell prefab
- `- BuildDeferredCellsSet(FieldData fieldData): HashSet<Vector2Int>`
    - **Purpose**: Collects grid coordinates that need deferred chip loading during field creation
    - **Usage**: Called once from CreateCells before prefab selection begins
    - **Params**: fieldData - level configuration containing locked-area definitions
    - **Returns**: Set of coordinates whose chips should be loaded only after area unlock
    - **Notes**: Coordinates keep their normal cell prefab
    - FieldGrid adds a DeferredCell component after spawning
- `- ClearCells(ICell cell): void`
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
- `- lockedAreaManager: ILockedAreaManager`
- `- mergeCamera: IMergeCamera`
- `- resolver: IObjectResolver`
- `- visualFieldInstance: GameObject`
#### Methods
- `+ CreateField(): void`
    - **Purpose**: Creates the field grid and sets up camera based on field data.
    - **Usage**: Call first during initialization to create the field structure.
    - **Notes**: Validates FieldData before proceeding
    - sets camera size and creates cells.
- `+ CreateLevelVisual(): void`
    - **Purpose**: Creates the level visual prefab and initializes field-wide visual components
    - **Usage**: Call after field and locked-area runtime state are initialized
    - **Notes**: Only initializes the root IVisualField
    - concrete visual fields are responsible for their child visual components
- `+ GetFieldData(): FieldData`
    - **Purpose**: Returns the current FieldData used by this field.
    - **Usage**: Used in tests to access ChipDataCollection and other field configurations.
- `+ InitCamera(): void`
    - **Purpose**: Initializes the merge camera's orthographic size and position based on the field size.
    - **Usage**: Called after visual field creation to ensure correct camera framing.
    - **Notes**: Retrieves Camera component from the injected IMergeCamera and computes bounds multiplier.
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
---

## FillContainerLogic
**Inherits**: `MonoBehaviour`

> - **Purpose**: Handles chip container filling logic when chips are added to container chips.
> - **Usage**: Implements IChipInteractionLogic to provide unified interface for container operations.
> - **Notes**: Validates container compatibility and adds chips to containers
> - integrates with unified interaction system.
#### Methods
- `+ CanInteract(ICell sourceCell, ICell targetCell): bool`
    - **Purpose**: Validates if a chip can be added to a container chip.
    - **Usage**: Called by unified interaction system to check container fill possibility.
    - **Params**: sourceCell - cell with chip being dragged
    - targetCell - cell with container chip
    - **Returns**: True if chip can be added to container
    - false otherwise
    - **Notes**: Checks if target is a container and if source chip is compatible.
- `+ ExecuteInteraction(ICell sourceCell, ICell targetCell): bool`
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
- `+ FindNearestFreeCell(Vector2Int parentPos, Vector2Int parentSize, Vector2Int childChipSize, Chip chipToPlace, HashSet<ICell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove, bool onlyAround): ICell`
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
- `- IsAreaCompletelyFree(Vector2Int cellPos, Vector2Int chipSize, Chip chipToPlace, HashSet<ICell> excludedCellsSet, HashSet<Chip> movingChipsSet): bool`
---

## ICell

> - **Purpose**: Contract for a single cell in the game grid that can hold a chip.
> - **Usage**: Use instead of the concrete Cell type in all logic, interaction, and effect code. Enables testability and implementation swapping.
> - **Notes**: Implemented by Cell : MonoBehaviour. Expose Transform so effects can re-parent without casting to MonoBehaviour.
#### Fields
- `++ Chip: Chip`
- `++ IsBlocked: bool`
- `++ MainCell: ICell`
- `+- CellPosition: Vector2Int`
- `+- Transform: Transform`
#### Methods
- `+ GetColorForLevelEditor(): Nullable<Color>`
- `+ GetLocalPositionForChip(Vector2Int chipSize): Vector3`
- `+ Init(Vector2Int cellPos): void`
    - **Purpose**: Initializes the cell with its grid position.
    - **Usage**: Call once after instantiating the cell to set its logical position in the grid.
    - **Params**: cellPos - grid coordinates for this cell.
- `+ OnDrag(Vector2 position, bool isValidPosition): void`
- `+ OnDragEnd(Vector2 position): void`
- `+ OnDragStart(Vector2 position): void`
- `+ OnTap(Vector2 position): void`
---

## ICellSubscriber

> - **Purpose**: Implemented by any object that wants to be notified when a chip on a watched cell changes.
> - **Usage**: Deliver via CellObserverManager.Subscribe.
#### Fields
- `+- ObservedCellPositions: IReadOnlyList<Vector2Int>`
#### Methods
- `+ OnChipChangedCell(ICell sourceCell, ICell targetCell): void`
    - **Purpose**: Rebinds neighbor observation when the owning chip changes its main cell.
    - **Usage**: Called by chip movement/placement flow after field occupancy is updated to the target cell.
    - **Params**: sourceCell - previous main cell (may be null on first placement)
    - targetCell - current main cell after relocation
- `+ OnChipDestroy(ICell mainCell): void`
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
- `+ Enqueue(ICell cell, Chip oldChip, Chip newChip): void`
    - **Purpose**: Record a chip change.
    - **Usage**: Called by FieldGrid.SetChipInCell before mutating state.
    - **Params**: cell - the location of change
    - oldChip - previous chip
    - newChip - current chip
- `+ Flush(): void`
    - **Purpose**: Fire OnFlush with all pending events and clear the queue.
    - **Usage**: Called from FieldEventHandler.LateUpdate.
---

## IChipCollections
#### Fields
- `+- AllChipsByData: Dictionary<ChipData, List<Chip>>`
#### Methods
- `+ AddChip(Chip chip): void`
- `+ RemoveChip(Chip chip): void`
---

## IChipFinder

> - **Purpose**: Contract for chip finding/discovery implementations
> - **Usage**: Implement this interface for different strategies of locating chips on the field
> - **Notes**: Allows for dependency injection of various chip discovery mechanisms
#### Methods
- `+ FindChips(ICell cell, Vector2Int chipSize): HashSet<Chip>`
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
- `+ CanInteract(ICell sourceCell, ICell targetCell): bool`
    - **Purpose**: Validates whether a chip interaction can be performed between source and target cells.
    - **Usage**: Call before attempting to execute an interaction to ensure it's valid.
    - **Params**: sourceCell - cell with the chip being moved
    - targetCell - destination cell for the interaction
    - **Returns**: Boolean indicating if the interaction is allowed
    - **Notes**: Should perform all necessary validation checks without modifying game state.
- `+ ExecuteInteraction(ICell sourceCell, ICell targetCell): bool`
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
- `+ CanChipMoving(ICell leftTopCell, IEnumerable<Chip> chipsToExclude, Vector2Int chipSize, List`1& plannedRelocations): bool`
    - **Purpose**: Checks if a chip can move to a target and plans relocations if necessary.
    - **Usage**: Call before moving a chip to validate if the move is possible.
    - **Params**: leftTopCell - target anchor cell
    - chipsToExclude - chips to ignore during collision checks
    - chipSize - size of the chip being moved
    - plannedRelocations - output list of moves to perform
    - **Returns**: True if the move and all necessary relocations are possible.
- `+ ChipMoving(ICell overCell, ICell leftTopCell, ICell sourceCell): void`
    - **Purpose**: Moves a chip to a new cell position, handling potential relocations.
    - **Usage**: Call when a chip is dropped or needs to be moved programmatically.
    - **Params**: overCell - cell under the chip
    - leftTopCell - target anchor cell
    - sourceCell - original anchor cell
- `+ ChipsRelocate(ICell leftTopCell, ICell sourceCell, List<ChipMoveAction> plannedRelocations): void`
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
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnInteractionOverCellChanged(ICell prevCell, ICell currentCell, ICell underCell): void`
- `+ OnInteractionUnderCellChanged(ICell underCell, ICell overCell): void`
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

> - **Purpose**: Effect contract for visualizing dynamic join links between a power booster and targets.
> - **Usage**: Implemented by effects that react to booster OnJoin/OnLeave lifecycle and follow IEffect activation rules.
#### Methods
- `+ OnJoin(IPowerBoosterTarget target): void`
    - **Purpose**: Registers a target and creates/updates visual links for it.
    - **Usage**: Called when a booster starts affecting the provided target.
    - **Params**: target - entity receiving booster influence
- `+ OnLeave(IPowerBoosterTarget target): void`
    - **Purpose**: Removes visual links associated with the provided target.
    - **Usage**: Called when a booster stops affecting the target or the target is removed.
    - **Params**: target - entity losing booster influence
- `+ OnTargetChipMoved(IPowerBoosterTarget chipTarget, bool value): void`
    - **Purpose**: Notifies that a target chip has started or stopped moving.
    - **Usage**: Called by ChipPowerBooster when a target chip's IsMoving state changes.
    - **Params**: chipTarget - the target chip that moved, value - true if chip started moving, false if it stopped
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
- `+- Cells: ICell[]`
- `+- FieldSize: Vector2Int`
#### Methods
- `+ CreateCells(FieldData fieldData): void`
    - **Purpose**: Initializes the grid
    - **Usage**: Call during initialization for level setup
    - **Params**: fieldData - configuration for the field
- `+ GetCells(Vector2Int cellPos, Vector2Int size): List<ICell>`
    - **Purpose**: Retrieves a list of cells within a specified area
    - **Usage**: Query grid for multiple cells, e.g. for multi-cell chips
    - **Params**: cellPos - top-left position
    - size - area dimensions
    - **Returns**: List of cells in the specified area
- `+ HasBlockedCells(Vector2Int cellPos, Vector2Int size): bool`
    - **Purpose**: Checks whether any cell in a rectangular grid area blocks chip placement
    - **Usage**: Call before placement, relocation, or drag highlight updates for multi-cell chips
    - **Params**: cellPos - top-left position of the area
    - size - area dimensions
    - **Returns**: True if the area is out of bounds or contains a blocked cell
    - otherwise false
    - **Notes**: Treats invalid bounds as blocked so callers can safely use the result before indexing Cells
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
- `+ SetChipInCell(ICell cell, Chip chip): void`
    - **Purpose**: Assigns or removes a chip from a specific cell
    - **Usage**: Primary method for modifying chip placement on the grid
    - **Params**: cell - target cell
    - chip - chip to place (null to clear)
---

## IFieldInitializeCommand

> - **Purpose**: Interface for field initialization commands that set up the board structure, load chips, and prepare cameras.
> - **Usage**: Injected and called by the field initialization orchestrator (e.g., Merge2Initializer). Implementations should handle both 2D and Isometric layouts.
#### Methods
- `+ CreateField(): void`
    - **Purpose**: Creates the cells and logical field grid structure.
    - **Usage**: Called first during initialization.
- `+ CreateLevelVisual(): void`
    - **Purpose**: Creates the level visual prefab and initializes field-wide visual components.
    - **Usage**: Called after logical cell states are ready.
- `+ InitCamera(): void`
    - **Purpose**: Initializes the merge camera parameters (position, bounds, viewport scale) relative to the field bounds.
    - **Usage**: Called as the final step of field setup to ensure correct camera framing.
- `+ LoadChips(): void`
    - **Purpose**: Populates the cells on the field grid with initial chips from cell data.
    - **Usage**: Called after logical and visual field structures are set up.
---

## IFreeCellFinder

> - **Purpose**: Interface for finding free cells on the field grid.
> - **Usage**: Injected into components that need to spawn or relocate chips.
> - **Notes**: Provides logic for nearest free cell search with spiral expansion.
#### Methods
- `+ FindNearestFreeCell(Vector2Int parentPos, Vector2Int parentSize, Vector2Int childChipSize, Chip chipToPlace, HashSet<ICell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove, bool onlyAround): ICell`
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

## ILockedAreaManager

> - **Purpose**: Contract for coordinating runtime locked-area state, visuals, and deferred chip spawning
> - **Usage**: Inject to initialize locked areas, register visual effects, and unlock areas during gameplay
> - **Notes**: Implemented by LockedAreaManager
#### Methods
- `+ Initialize(): void`
    - **Purpose**: Applies initial locked-area state to field cells
    - **Usage**: Call once during scene startup after field cells exist and before LoadChips
    - **Notes**: Blocks both normal locked cells and deferred cells
    - level visuals are created by FieldInitializeCommand
- `+ RegisterEffect(LockedAreaEffect effect): void`
    - **Purpose**: Associates a spawned visual effect with its locked-area state
    - **Usage**: Called by LockedAreaEffect.Init after the level visual prefab is instantiated
    - **Params**: effect - effect component representing a single locked area visual
    - **Notes**: Immediately synchronizes visual state so late registrations still match current lock state
- `+ UnlockArea(int areaId, bool force): void`
    - **Purpose**: Opens a locked area and materializes any deferred chips inside it
    - **Usage**: Call from gameplay systems when the unlock condition for an area is satisfied
    - **Params**: areaId - unique identifier from FieldData.LockedAreas
    - force - true repeats unlock side effects even if the area is already open
    - **Notes**: Unblocks cells first so spawned chips can behave as normal cells immediately after creation
---

## IMergeCamera

> - **Purpose**: Defines camera controls used by the merge field input bridge.
> - **Usage**: Register a MergeCamera or IsoMergeCamera as this interface in the LifetimeScope.
> - **Notes**: All methods are driven by InputManager and FieldEventHandler events.
#### Fields
- `+- Camera: Camera`
#### Methods
- `+ Init(Vector3 startPosition, float orthographicSize): void`
- `+ OnDrag(Vector3 worldPosition, bool isChipDragging): Nullable<Vector3>`
- `+ OnDragEnd(bool wasChipDragging): void`
- `+ OnDragStart(Vector3 worldPosition, bool isChipDragging): void`
- `+ OnPointerMoved(Vector2 screenPosition): void`
- `+ OnZoom(float delta, Vector2 screenPosition): void`
- `+ SetBounds(Vector3 leftBottom, Vector3 rightTop): void`
---

## IMergeLifetimeScope

> - **Purpose**: Interface for LifetimeScope components in Merge2 and IsoMerge to expose configuration
> - **Usage**: Implemented by Merge2LifetimeScope and IsoMergeLifetimeScope to allow Editor and Runtime tools to query and set properties without tight coupling
#### Fields
- `++ FieldData: FieldData`
- `++ MergeSettings: MergeSettings`
- `+- Container: IObjectResolver`
---

## IPowerBoosterTarget

> - **Purpose**: Contract for entities that can be influenced by ChipPowerBooster instances.
> - **Usage**: Implemented by chips (for example ChipGenerator) that expose join points and manage active boosters.
#### Fields
- `+- AppliedBoosters: HashSet<ChipPowerBooster>`
- `+- BlockingState: CombinedBlockingState`
- `+- IsMoving: bool`
- `+- JoinPoints: IReadOnlyList<Transform>`
#### Methods
- `+ ApplyPowerBooster(ChipPowerBooster chipPowerBooster, bool reapply): bool`
    - **Purpose**: Adds a booster to this entity.
    - **Usage**: Called when booster observation detects this entity in range.
    - **Params**: chipPowerBooster - booster to apply
    - **Returns**: True if the booster was added
    - false if it was already active.
- `+ NotifyEffectRemoved(int effectId): void`
    - **Purpose**: Notifies that an effect has been removed from this entity.
    - **Usage**: Called by Chip.Effects when an effect is removed
    - allows boosters to re-evaluate influence.
    - **Params**: effectId - identifier of the removed effect
- `+ RemovePowerBooster(ChipPowerBooster chipPowerBooster): void`
    - **Purpose**: Removes a booster from this entity.
    - **Usage**: Called when booster observation no longer includes this entity or booster is removed.
    - **Params**: chipPowerBooster - booster to remove
---

## IVisualField

> - **Purpose**: Contract for the root visual field component created from FieldData.LevelVisualPrefab
> - **Usage**: Implement on the level visual prefab root so FieldInitializeCommand can initialize field-wide visual services
> - **Notes**: Use for visual field concerns such as camera bounds without coupling MergeBase to a concrete view implementation
#### Methods
- `+ InitVisualField(FieldData fieldData): void`
- `+ OnFieldSizeChanged(Vector2Int newSize): void`
---

## LockedAreaEffect
**Inherits**: `Effect`

> - **Purpose**: Runtime visual controller for one locked area overlay or gate
> - **Usage**: Attach to level visual prefab children and assign the matching LockedAreaId
> - FieldInitializeCommand calls Init after instantiation
> - **Notes**: Inherits Effect for shared animator trigger behavior, but does not call base.Init because this visual is not owned by a chip
#### Fields
- `+- LockedAreaId: int`
- `- lockedAreaManager: ILockedAreaManager`
#### Methods
- `+ Init(Chip chip, int effectId): void`
    - **Purpose**: Registers this level visual effect with the locked-area runtime manager
    - **Usage**: Called by FieldInitializeCommand after the level visual prefab is instantiated and injected
    - **Params**: chip - ignored because locked-area visuals are not chip-owned
    - effectId - stored for inherited Effect identity
    - **Notes**: Intentionally skips base.Init because the base implementation reads chip.Data and deactivates chip effects
---

## LockedAreaManager

> - **Purpose**: Coordinates runtime locked-area state, visuals, and deferred chip spawning
> - **Usage**: Registered as a VContainer singleton and initialized after FieldGrid creates cells but before chips are loaded
> - **Notes**: Cells remain blocked until UnlockArea clears them
> - deferred chips are created only during unlock
#### Fields
- `- effectsByAreaId: Dictionary<int, List<LockedAreaEffect>>`
- `- fieldData: FieldData`
- `- fieldGrid: IFieldGrid`
- `- lockedAreaIds: HashSet<int>`
#### Methods
- `+ Initialize(): void`
    - **Purpose**: Applies initial locked-area state to field cells
    - **Usage**: Call once during scene startup after field cells exist and before LoadChips
    - **Notes**: Blocks both normal locked cells and deferred cells
    - level visuals are created by FieldInitializeCommand
- `+ RegisterEffect(LockedAreaEffect effect): void`
    - **Purpose**: Associates a spawned visual effect with its locked-area state
    - **Usage**: Called by LockedAreaEffect.Init after the level visual prefab is instantiated
    - **Params**: effect - effect component representing a single locked area visual
    - **Notes**: Immediately synchronizes visual state so late registrations still match current lock state
- `+ UnlockArea(int areaId, bool force): void`
    - **Purpose**: Opens a locked area and materializes any deferred chips inside it
    - **Usage**: Call from gameplay systems when the unlock condition for an area is satisfied
    - **Params**: areaId - unique identifier from FieldData.LockedAreas
    - force - true repeats unlock side effects even if the area is already open
    - **Notes**: Unblocks cells first so spawned chips can behave as normal cells immediately after creation
- `- DeactivateEffects(int areaId, bool immediate): void`
- `- SetAreaBlocked(LockedAreaData area, bool isBlocked): void`
- `- SetCellsBlocked(Vector2Int[] cellPositions, bool isBlocked): void`
- `- SpawnDeferredChips(LockedAreaData area): void`
- `- TryGetArea(int areaId, LockedAreaData& area): bool`
---

## Merge2Initializer

> - **Purpose**: Main initializer for the Merge2 game module using VContainer.
> - **Usage**: Registered in VContainer lifetime scope
> - automatically called on game start.
> - **Notes**: Wires up dependencies, connects input events, and initializes field and chips.
#### Fields
- `- chipCollections: IChipCollections`
- `- chipFactory: ChipFactory`
- `- field: IFieldEventHandler`
- `- fieldGrid: IFieldGrid`
- `- fieldInitializeCommand: IFieldInitializeCommand`
- `- inputManager: InputManager`
- `- lockedAreaManager: ILockedAreaManager`
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
- `++ MergeSettings: MergeSettings`
#### Methods
- `~ Awake(): void`
- `~ Configure(IContainerBuilder builder): void`
- `- Expecto.MergeBase.IMergeLifetimeScope.get_Container(): IObjectResolver`
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
- `+ CanInteract(ICell sourceCell, ICell targetCell): bool`
    - **Purpose**: Validates if two cells can perform a merge interaction.
    - **Usage**: Called by unified interaction system to check merge possibility.
    - **Params**: sourceCell - cell with chip being dragged
    - targetCell - destination cell
    - **Returns**: True if chips can be merged
    - false otherwise
    - **Notes**: Checks chip compatibility and merge data availability.
- `+ ExecuteInteraction(ICell sourceCell, ICell targetCell): bool`
    - **Purpose**: Executes merge interaction between two cells, handling potential resizing
    - **Usage**: Called by unified interaction system after CanInteract validation passes
    - **Params**: sourceCell - cell with chip being dragged
    - targetCell - destination cell
    - **Notes**: If the merged chip is larger than the parent, it uses IChipMovingLogic to relocate neighboring chips if needed. If relocation at the primary position fails, it tries all 8 neighboring offsets before giving up.
- `- HandleExtraChip(MergeResult mergeResult, ICell mergedCell, Vector2Int mergedChipSize): void`
- `- NotifyNeighborsOfMerge(ICell targetCell): void`
    - **Purpose**: Notifies neighboring chips that merges have occurred nearby
    - **Usage**: Called from ExecuteInteraction before destroying the merging chips
    - **Params**: targetCell - the main cell of the target chip whose neighbors will be notified
- `- TryResolveCellPosition(ICell primaryCell, Chip[] chipsToExclude, Vector2Int chipSize, ICell& resolvedCell, List`1& plannedRelocations): bool`
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

## MergeCamera
**Inherits**: `MonoBehaviour`

> - **Purpose**: Provides the default 2D orthographic camera implementation for the merge field, managing size and initial position.
> - **Usage**: Attach to the camera GameObject in 2D scenes and register as IMergeCamera in Merge2LifetimeScope.
#### Fields
- `+- Camera: Camera`
- `- cam: Camera`
#### Methods
- `+ Init(Vector3 startPosition, float orthographicSize): void`
    - **Purpose**: Sets the camera orthographic size and position based on the field constraints.
    - **Usage**: Called during initialization by FieldInitializeCommand.
    - **Params**: startPosition - the initial camera coordinates
    - orthographicSize - the calculated viewport scale.
- `+ OnDrag(Vector3 worldPosition, bool isChipDragging): Nullable<Vector3>`
- `+ OnDragEnd(bool wasChipDragging): void`
- `+ OnDragStart(Vector3 worldPosition, bool isChipDragging): void`
- `+ OnPointerMoved(Vector2 screenPosition): void`
- `+ OnZoom(float delta, Vector2 screenPosition): void`
- `+ SetBounds(Vector3 leftBottom, Vector3 rightTop): void`
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
- `+ FindChips(ICell cell, Vector2Int chipSize): HashSet<Chip>`
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

> - **Purpose**: Cell subscriber that maintains power booster links to nearby IPowerBoosterTarget entities.
> - **Usage**: Attached to ChipPowerBooster
> - reacts to observed chip changes and chip movement to keep booster application and removal consistent.
> - **Notes**: Tracks unique booster targets in boostedTargets to prevent duplicate apply/remove calls.
#### Fields
- `+- BoostedTargets: HashSet<IPowerBoosterTarget>`
- `~ chipPowerBooster: ChipPowerBooster`
#### Methods
- `+ OnChipChangedCell(ICell sourceCell, ICell targetCell): void`
    - **Purpose**: Re-evaluates all booster links after the booster itself is relocated.
    - **Usage**: Called when the owner booster chip changes its grid position.
    - **Params**: sourceCell - original cell
    - targetCell - result cell
    - **Notes**: Clears all previous links, re-subscribes to new neighbors, and rebuilds links from the new location.
- `+ OnChipDestroy(ICell mainCell): void`
    - **Purpose**: Cleans up all active booster links before the chip is destroyed.
    - **Usage**: Called during the booster's teardown lifecycle.
    - **Params**: mainCell - the booster's current cell
    - **Notes**: Ensures all targets are notified of booster removal to prevent stale visual links or modifiers.
- `+ OnObservedCellChipChanged(ChipChangedEvent evt): void`
    - **Purpose**: Updates booster links when neighboring chips change, ensuring consistent application/removal.
    - **Usage**: Invoked by CellObserverManager
    - uses boostedTargets set for idempotency.
    - **Params**: evt - event containing old/new chips on the observed cell
- `~ Awake(): void`
- `- ClearAllTargets(): void`
---

## PowerBoosterConnectorCellsHighlightEffect
**Inherits**: `CellHighlightEffect`

> - **Purpose**: Visualizes the booster's observation range by highlighting neighbor cells and periodically triggering booster animations.
> - **Usage**: Used by ChipPowerBooster to show coverage
> - activated when the booster cell subscriber is updated.
> - **Notes**: Uses powerEffectCoroutine to repeat a secondary animation trigger while active.
#### Fields
- `+ distractionAmount: float`
- `~ connectorCellPositions: IReadOnlyList<Vector2Int>`
- `~ distractionAmountId: int`
- `~ originCellPosition: Vector2Int`
- `~ powerEffectCoroutine: Coroutine`
- `~ waitTimeBeforePowerEffect: float`
#### Methods
- `+ Activate(Chip chip): bool`
    - **Purpose**: Starts the highlight effect and initiates the periodic animation coroutine.
    - **Usage**: Called by Chip when activating effects
    - triggers the recurring PowerBooster animation via StartPowerEffect.
    - **Returns**: True if the effect was successfully activated.
    - **Notes**: Restarts powerEffectCoroutine to ensure periodic visual feedback begins immediately.
- `+ Deactivate(Chip chip, bool force): void`
    - **Purpose**: Stops the highlight effect and cleans up the animation coroutine.
    - **Usage**: Called when deactivating effects
    - stops the repetitive powerEffectCoroutine.
    - **Notes**: Ensures the coroutine is stopped and the animator state is reset to prevent stuck animations.
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
    - **Purpose**: Reacts to cell changes by triggering the PowerBooster animation
    - **Usage**: Called automatically via CellHighlightEffect when the chip is moved
    - **Params**: sourceCell - original cell
    - targetCell - result cell
- `+ OnInteractionOverCellChanged(ICell sourceCell, ICell targetCell, ICell interactableCell): void`
- `- <Activate>g__StartPowerEffect|6_0(Chip chip, float waitTime): IEnumerator`
- `~ CreateHighlights(): void`
- `~ Update(): void`
---

## PowerBoosterJoinEffect
**Inherits**: `Effect`

> - **Purpose**: Effect that spawns and maintains animated particle links between a booster and each active power-booster target.
> - **Usage**: Used by ChipPowerBooster via IEffectPowerBoosterJoin.OnJoin/OnLeave
> - periodically rebinds endpoints to create dynamic join visuals.
> - **Notes**: Owns instantiated particle systems per target and stops/destroys them on leave/deactivate.
#### Fields
- `- changeJoinPointsCoroutine: Coroutine`
- `- changeJoinPointsTime: float`
- `- chipPowerBooster: Chip`
- `- effectPowerDistance: float`
- `~ effects: Dictionary<IPowerBoosterTarget, List<JoinEffectData>>`
- `~ joinEffectPrefab: ParticleSystem`
- `~ joinPoints: Transform[]`
- `- minMaxEffectsForOneModifier: Vector2Int`
- `- pendingWaitCoroutines: Dictionary<IPowerBoosterTarget, Coroutine>`
#### Methods
- `+ Deactivate(Chip chip, bool force): void`
    - **Purpose**: Stops all running join visuals and clears tracked modifier links.
    - **Usage**: Called when owning booster starts moving or effect is forcefully deactivated.
    - **Params**: chip - owning chip passed by effect lifecycle
    - force - optional force flag from IEffect contract
    - **Notes**: Stops coroutine, stops particles, schedules particle GameObject destruction using each particle lifetime, then clears state dictionary.
- `+ Init(Chip chip, int effectId): void`
- `+ OnJoin(IPowerBoosterTarget target): void`
    - **Purpose**: Registers a new target and creates one or more visual join effects for it.
    - **Usage**: Called when a booster starts affecting a target.
    - **Params**: target - target entity that should receive join visuals
    - **Notes**: Starts the join-point reshuffle coroutine on first active target.
- `+ OnLeave(IPowerBoosterTarget target): void`
    - **Purpose**: Removes and schedules cleanup of all join effects associated with a target.
    - **Usage**: Called when booster influence on a target ends.
    - **Params**: target - target entity to remove
    - **Notes**: Stops reshuffle coroutine when no targets remain.
- `+ OnTargetChipMoved(IPowerBoosterTarget chipTarget, bool value): void`
    - **Purpose**: Toggles the looping state of join effects based on target movement.
    - **Usage**: Called when a target chip's movement state changes
    - stops effects while moving to avoid visual jitter.
    - **Params**: chipTarget - the affected target
    - value - true if move started, false if stopped
    - **Notes**: Uses dictionary lookup to find and modify all particle systems related to the target.
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
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
- `- GetTopClosest(IReadOnlyList<Transform> sources, IReadOnlyList<Transform> targets, int n, List`1& distances): List<Transform>`
    - **Purpose**: Selects up to N source transforms that are closest to any target transform.
    - **Usage**: Used to prefilter candidate join points on both booster and modifier sides.
    - **Params**: sources - candidate points to rank
    - targets - points to measure distance against
    - n - max number of closest sources to return
    - distances - reusable scratch list for distance calculations
    - **Returns**: List containing the N closest source transforms (or fewer when sources are limited).
    - **Notes**: Uses a reusable distance list to reduce temporary allocations during repeated effect updates.
- `- ShowEffect(IPowerBoosterTarget target, JoinEffectData& joinEffectData): void`
    - **Purpose**: Creates or rebinds a single join particle effect between booster and target join points.
    - **Usage**: Called for initial spawn and periodic endpoint reshuffles.
    - **Params**: target - target entity for this link
    - joinEffectData - existing effect data to update (null creates a new record)
    - **Returns**: Effect data instance containing selected endpoints and the particle system instance.
- `- WaitStopMovingAndShowEffects(IPowerBoosterTarget target): IEnumerator`
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

## VisualField
**Inherits**: `MonoBehaviour`

> - **Purpose**: Provides visual presentation of the 2D merge grid, managing field sprite size and injecting sub-effects like locked areas.
> - **Usage**: Attach to visual field prefabs
> - instantiated dynamically via field initialization.
#### Fields
- `- fieldSpriteRenderer: SpriteRenderer`
- `- resolver: IObjectResolver`
#### Methods
- `+ InitVisualField(FieldData fieldData): void`
    - **Purpose**: Initializes the 2D visual field using field configuration data.
    - **Usage**: Called dynamically after instantiating the prefab during field initialization.
    - **Params**: fieldData - configuration specifying grid size and other settings.
- `+ OnFieldSizeChanged(Vector2Int newSize): void`
- `- ApplyEffects(): void`
---

