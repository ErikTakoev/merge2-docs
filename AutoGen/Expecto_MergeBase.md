# Namespace: Expecto.MergeBase

## Table of Contents
- [AreaUnlockedEventArgs](#areaunlockedeventargs)
- [BlockedCell](#blockedcell)
- [Cell](#cell)
- [CellHighlightEffect](#cellhighlighteffect)
- [CellObserverManager](#cellobservermanager)
- [CellSubscriber](#cellsubscriber)
- [ChargingEffect](#chargingeffect)
- [ChargingEffectRef](#chargingeffectref)
- [Chip](#chip)
- [ChipChangedEvent](#chipchangedevent)
- [ChipCollections](#chipcollections)
- [ChipContainerData](#chipcontainerdata)
- [ChipContainerElementData](#chipcontainerelementdata)
- [ChipContainerRuntimeData](#chipcontainerruntimedata)
- [ChipCreatedEventArgs](#chipcreatedeventargs)
- [ChipData](#chipdata)
- [ChipDataCollection](#chipdatacollection)
- [ChipEffectUnlockedEventArgs](#chipeffectunlockedeventargs)
- [ChipExtraEffectsData](#chipextraeffectsdata)
- [ChipFactory](#chipfactory)
- [ChipFlightSettings](#chipflightsettings)
- [ChipFlyAnimation](#chipflyanimation)
- [ChipGeneratorData](#chipgeneratordata)
- [ChipGeneratorRuntimeData](#chipgeneratorruntimedata)
- [ChipLiftController](#chipliftcontroller)
- [ChipLiftControllerRef](#chipliftcontrollerref)
- [ChipMergeAvailableEffect](#chipmergeavailableeffect)
- [ChipMergeData](#chipmergedata)
- [ChipMovingLogic](#chipmovinglogic)
- [ChipPowerBoosterData](#chippowerboosterdata)
- [ChipRemovedEventArgs](#chipremovedeventargs)
- [ChipRuntimeData](#chipruntimedata)
- [ChipSelectorAttribute](#chipselectorattribute)
- [ChipSelectorDrawer](#chipselectordrawer)
- [ChipSortingLayer](#chipsortinglayer)
- [ChipTapEvolutionData](#chiptapevolutiondata)
- [ChipWaitEvolutionData](#chipwaitevolutiondata)
- [ChipWaitEvolutionRuntimeData](#chipwaitevolutionruntimedata)
- [CombinedBlockingState](#combinedblockingstate)
- [ContainerHintEffect](#containerhinteffect)
- [ContainerInfo](#containerinfo)
- [ContainerModule](#containermodule)
- [DeferredCell](#deferredcell)
- [DeferredChipChangeNotifier](#deferredchipchangenotifier)
- [DraggableChipLogic](#draggablechiplogic)
- [DragInteractionResult](#draginteractionresult)
- [Effect](#effect)
- [EffectBlockerData](#effectblockerdata)
- [EffectBlockerDefinitionAttribute](#effectblockerdefinitionattribute)
- [EffectBlockerSelectorAttribute](#effectblockerselectorattribute)
- [EffectBlockerSelectorDrawer](#effectblockerselectordrawer)
- [EffectBlockingSettings](#effectblockingsettings)
- [EffectConsts](#effectconsts)
- [EffectContainerHintRef](#effectcontainerhintref)
- [EffectDefinitionAttribute](#effectdefinitionattribute)
- [EffectDestroyingRuntimeData](#effectdestroyingruntimedata)
- [EffectDestroyingSettings](#effectdestroyingsettings)
- [EffectExtraData](#effectextradata)
- [EffectHintRef](#effecthintref)
- [EffectMergeLightRef](#effectmergelightref)
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
- [FlightType](#flighttype)
- [FreeCellFinder](#freecellfinder)
- [GeneratorModule](#generatormodule)
- [HintEffect](#hinteffect)
- [ICell](#icell)
- [ICellSubscriber](#icellsubscriber)
- [IChargingEffect](#ichargingeffect)
- [IChipChangeNotifier](#ichipchangenotifier)
- [IChipCollections](#ichipcollections)
- [IChipFinder](#ichipfinder)
- [IChipFlyAnimation](#ichipflyanimation)
- [IChipInteractionLogic](#ichipinteractionlogic)
- [IChipLiftController](#ichipliftcontroller)
- [IChipModule](#ichipmodule)
- [IChipMovingLogic](#ichipmovinglogic)
- [IChipSortingLayer](#ichipsortinglayer)
- [IChipSpecialData](#ichipspecialdata)
- [IChipSpecialRuntimeData](#ichipspecialruntimedata)
- [IDragFeedback](#idragfeedback)
- [IEffect](#ieffect)
- [IEffectBlockingSettings](#ieffectblockingsettings)
- [IEffectContainerHint](#ieffectcontainerhint)
- [IEffectHint](#ieffecthint)
- [IEffectMergeLight](#ieffectmergelight)
- [IEffectPowerBoosterJoin](#ieffectpowerboosterjoin)
- [IFieldEventHandler](#ifieldeventhandler)
- [IFieldGrid](#ifieldgrid)
- [IFieldInitializeCommand](#ifieldinitializecommand)
- [IFreeCellFinder](#ifreecellfinder)
- [ILockedAreaManager](#ilockedareamanager)
- [IMergeCamera](#imergecamera)
- [IMergeLifetimeScope](#imergelifetimescope)
- [IPowerBoosterTarget](#ipowerboostertarget)
- [IScenarioEventHandler](#iscenarioeventhandler)
- [IShadowEffect](#ishadoweffect)
- [IVisualField](#ivisualfield)
- [LockedAreaEffect](#lockedareaeffect)
- [LockedAreaManager](#lockedareamanager)
- [Merge2Initializer](#merge2initializer)
- [Merge2LifetimeScope](#merge2lifetimescope)
- [MergeableChipLogic](#mergeablechiplogic)
- [MergeCamera](#mergecamera)
- [MergeCombination](#mergecombination)
- [MergeHintDragFeedback](#mergehintdragfeedback)
- [MergeLightDragFeedback](#mergelightdragfeedback)
- [MergeLightEffect](#mergelighteffect)
- [MergeResult](#mergeresult)
- [NeighborChipFinder](#neighborchipfinder)
- [PowerBoosterCellSubscriber](#powerboostercellsubscriber)
- [PowerBoosterConnectorCellsHighlightEffect](#powerboosterconnectorcellshighlighteffect)
- [PowerBoosterJoinEffect](#powerboosterjoineffect)
- [PowerBoosterModule](#powerboostermodule)
- [ScenarioEventHandler](#scenarioeventhandler)
- [ShadowEffect](#shadoweffect)
- [ShadowEffectRef](#shadoweffectref)
- [SortingLayerData](#sortinglayerdata)
- [TapEvolutionModule](#tapevolutionmodule)
- [UnlockAreaNode](#unlockareanode)
- [VisualField](#visualfield)
- [WaitEvolutionModule](#waitevolutionmodule)
- [WaitForAreaUnlockedNode](#waitforareaunlockednode)
- [WaitForChipCreatedNode](#waitforchipcreatednode)
- [WaitForChipEffectUnlockedNode](#waitforchipeffectunlockednode)
- [WaitForChipRemovedNode](#waitforchipremovednode)

---

## AreaUnlockedEventArgs
#### Fields
- `+ AreaId: int`
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
- `- WaitAndStartFlyAnimation(Chip chip, Transform tr, Vector3 localTarget, float duration, FlightType flightType, float delay): IEnumerator`
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

## ChargingEffect
**Inherits**: `Effect`

> - **Purpose**: Visual effect handler for chip charging progress, displaying a rotating stopwatch hand and updating material progress.
> - **Usage**: Attached to chip prefabs that support charging (generators, wait-evolution) and referenced via IChargingEffect contract.
> - **Notes**: Updates the stopwatch material property _GrayProgress and rotates the arrow transform based on the charging progress.
#### Fields
- `- arrowTransform: Transform`
- `- EraseLevelId: int`
- `- stopwatchMaterial: Material`
- `- stopwatchSpriteRenderer: SpriteRenderer`
#### Methods
- `+ Init(Chip chip, int effectHash): void`
- `+ OnCharging(float progress): void`
    - **Purpose**: Updates the visual state of charging based on progress
    - **Usage**: Called via event from ChipGenerator or WaitEvolutionModule during update loop
    - **Params**: progress - value between 0 and 1 indicating charge percentage
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
---

## ChargingEffectRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
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
- `+- BlockingState: CombinedBlockingState`
- `+- CellSubscriber: ICellSubscriber`
- `+- Data: ChipData`
- `+- FlightSettings: ChipFlightSettings`
- `+- IsDestroying: bool`
- `+- IsMoving: bool`
- `+- IsSpawning: bool`
- `+- LiftController: IChipLiftController`
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
- `- liftControllerRef: ChipLiftControllerRef`
- `~ resolver: IObjectResolver`
- `~ scenarioEventHandler: IScenarioEventHandler`
- `~ transformNode: Transform`
#### Methods
- `+ AddEffect(IEffect effect, int effectHash, bool activate, bool deactivate): void`
    - **Purpose**: Adds an effect to the effects dictionary and optionally activates it
    - **Usage**: Call from InitEffects or from chip modules to register effects with their EffectConsts keys
    - **Params**: effect - The effect instance to add
    - effectHash - EffectConsts hash value
    - activate - Whether to immediately activate the effect
    - **Notes**: Null-safe: effect is only added if not null
    - handles activation before storing
- `+ CanMoving(): bool`
    - **Purpose**: Checks whether the chip can currently be moved by the player
    - **Usage**: Called by DraggableChipLogic before allowing drag operations
    - returns false if chip is locked
    - **Returns**: True if chip can be moved
    - false if movement is locked
    - **Notes**: Based on runtimeData.IsMoveLocked
    - prevents drag-and-drop when locked
- `+ Destroy(ICell mainCell, bool force, AnimatorTrigger destroyTrigger): void`
    - **Purpose**: Initiates the destruction of the chip by clearing grid/collection references and starting the specified destruction animation trigger.
    - **Usage**: Call to remove the chip from the field grid. The actual GameObject destruction happens in FinishDestroy, either immediately or triggered by animator event.
    - **Params**: mainCell - the chip's main occupied cell
    - force - if true, bypasses animation and destroys immediately
    - destroyTrigger - the animator trigger to use for the destruction animation
    - **Notes**: Clears grid occupancy and collections first. If force is true or animator is missing, FinishDestroy is called immediately. Otherwise, sends the specified animator trigger.
- `+ FinishDestroy(): void`
    - **Purpose**: Completes the destruction process by destroying attached effects and the GameObject itself.
    - **Usage**: Called immediately if force is true, or triggered via a Unity Animation Event at the end of the destroy animation.
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
- `+ OnNeighborsChipOfInteraction(): void`
    - **Purpose**: Called when a neighboring chip interacts with another chip, notifying this chip of the interaction event
    - **Usage**: Override in derived classes to react to neighboring interactions
    - default implementation handles effect destruction logic
    - **Notes**: Called from DraggableChipLogic.NotifyNeighborsOfInteraction after a successful interaction
- `+ OnTap(Vector2 position): void`
    - **Purpose**: Called when the chip is tapped by the user, invoking OnTap on all attached modules and effects.
    - **Usage**: Override in derived classes to implement custom tap behavior or call base to execute modules/effects
    - receives tap position in world coordinates
    - **Params**: position - the world position where the chip was tapped
- `+ RemoveEffect(int effectId): void`
    - **Purpose**: Removes a destroying effect from the chip after it reaches destruction threshold
    - **Usage**: Called from HandleDestroyingEffects when TryDestroyEffect returns true or from chip modules
    - **Params**: effectId - The unique hash/ID of the effect to remove
    - **Notes**: Deactivates the effect, removes it from dictionary, and updates the priority selection
- `+ ResetTrigger(string triggerName): void`
- `+ SendTrigger(AnimatorTrigger trigger, bool allowRepeat): void`
- `+ SendTrigger(string trigger, bool allowRepeat): void`
    - **Purpose**: Sends a trigger to the animator and synchronizes the 'Little' state.
    - **Usage**: Called when a chip state change requires an animation trigger (e.g., Generate, Recharge).
    - **Params**: trigger - the animator trigger name to fire
    - allowRepeat - if true, bypasses the dontRepeatTrigger check
    - **Notes**: Automatically updates the 'Little' boolean in the animator based on the chip's blocking state. Prevents redundant triggers if dontRepeatTrigger is enabled.
- `+ SetDragging(bool value): void`
    - **Purpose**: Sets the drag state of the chip and ensures visual consistency
    - **Usage**: Called by DraggableChipLogic when user drag starts (true) or ends (false)
    - **Params**: value - true when user starts dragging, false when drag ends
    - **Notes**: Automatically calls SetMoving(true) if chip is not already moving
    - separates user drag state from movement state
- `+ SetFlightSettings(ChipFlightSettings settings): void`
    - **Purpose**: Updates the flight animation configuration for the chip
    - **Usage**: Called when creating or relocating the chip to specify its next flight movement properties
    - **Params**: settings - the flight settings to apply
    - falls back to default settings if duration is invalid (<= 0)
- `+ SetMoving(bool value): void`
    - **Purpose**: Updates the sorting order of the chip during movement (user drag or system relocation)
    - **Usage**: Called when movement starts (true) or ends (false)
    - used for both user dragging and automated chip movement
    - **Params**: value - true if starting movement, false if ending movement
    - **Notes**: Sets sorting order to 2 for moving chips to ensure they're on top
    - on move-start it enqueues a chip-change event with NewChip=null for the current cell so observer-based systems can immediately react to temporary chip departure
    - calls UpdateVisual when movement ends
- `+ SetRotationZ(float zAngle): void`
- `+ StopSpawning(): void`
    - **Purpose**: Allows updating visual state after initial setup by toggling off the visual update deferral flag and forcing a visual refresh.
    - **Usage**: Call after completing initialization or state setup where initial rendering was deferred to prevent redundant visual updates.
    - **Notes**: Toggles deferredUpdateVisual to false and immediately calls UpdateVisual.
- `+ UpdateVisual(): void`
    - **Purpose**: Updates the visual state of the chip based on its runtime data
    - **Usage**: Call after modifying runtimeData.EffectEnables to synchronize visual effects
    - **Notes**: Iterates blockersData.Blockers and activates effects whose EffectId is in EffectEnables
    - also activates CellHighlight unless hidden by BlockingState.HideEffectIds
- `~ DeactivateAllEffects(): void`
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
- `~ RemoveAllEffects(): void`
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

> - **Purpose**: Provides a centralized collection of all active chips on the field, categorized by their data type. Also maintains two filtered sub-collections for fast merge-target and container-fill lookups.
> - **Usage**: Injected as IChipCollections
> - used to find all chips of a specific type or to iterate over all active chips. Call OnChipBlockingChanged when a chip's BlockingState or its cell's IsBlocked changes. Call OnContainerRequirementsChanged when a container's active requirements shrink.
> - **Notes**: Updated by ChipFactory during creation and by Chip during destruction. MergeableChipsByData and FillableChipsByData are kept in sync via invalidation callbacks
> - never stale-read by polling.
#### Fields
- `+- AllChipsByData: Dictionary<ChipData, List<Chip>>`
- `+- FillableChipsByData: Dictionary<ChipData, List<Chip>>`
- `+- MergeableChipsByData: Dictionary<ChipData, List<Chip>>`
- `- fieldGrid: IFieldGrid`
#### Methods
- `+ AddChip(Chip chip): void`
    - **Purpose**: Adds a chip to the collection, grouping it by its ChipData. Also inserts it into filtered sub-collections if it passes the merge/fill criteria.
    - **Usage**: Called by ChipFactory immediately after a new chip is initialized
    - **Params**: chip - the chip instance to add
- `+ OnChipBlockingChanged(Chip chip): void`
    - **Purpose**: Re-evaluates a chip's presence in MergeableChipsByData and FillableChipsByData after its BlockingState or cell's IsBlocked flag changes.
    - **Usage**: Called by Chip.RemoveEffect after a blocker effect is destroyed, and by LockedAreaManager when a cell's IsBlocked flag changes.
    - **Params**: chip - the chip whose eligibility may have changed
- `+ OnContainerRequirementsChanged(ContainerModule container, ChipData fulfilledRequirement): void`
    - **Purpose**: Removes a container from the specific key in FillableChipsByData that was just fulfilled.
    - **Usage**: Called by ChipContainer.TryAddChip when one requirement is completed but the container is not yet full. The fulfilled ChipData key is passed directly — no dictionary iteration needed.
    - **Params**: container - the container whose requirement changed
    - fulfilledRequirement - the ChipData that was just satisfied and must be removed from FillableChipsByData
- `+ RemoveChip(Chip chip): void`
    - **Purpose**: Removes a chip from the collection and cleans up empty data entries. Also removes it from all filtered sub-collections.
    - **Usage**: Called by Chip.Destroy just before the chip GameObject is destroyed
    - **Params**: chip - the chip instance to remove
- `- AddContainerToFillable(ContainerModule container): void`
- `- AddToDict(Dictionary<ChipData, List<Chip>> dict, ChipData key, Chip chip): void`
- `- IsChipMergeable(Chip chip): bool`
- `- IsContainerFillable(ContainerModule container): bool`
- `- RemoveContainerFromFillable(ContainerModule container): void`
- `- RemoveFromDict(Dictionary<ChipData, List<Chip>> dict, ChipData key, Chip chip): void`
---

## ChipContainerData
#### Fields
- `+ containers: ContainerInfo[]`
- `+ NextChipData: ChipData`
---

## ChipContainerElementData

> - **Purpose**: Holds visual element prefab reference required by containers when displaying filled chips.
> - **Usage**: Attached to ChipData via specialDatas array. Retrieved by ContainerInfo to display visual items inside container slots.
> - **Notes**: Implements IChipSpecialData for registration in chip data assets.
#### Fields
- `+- ContainerElementPrefab: GameObject`
---

## ChipContainerRuntimeData

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

## ChipCreatedEventArgs
#### Fields
- `+ Cell: ICell`
- `+ Chip: Chip`
---

## ChipData
**Inherits**: `ScriptableObject`
#### Fields
- `++ DestroyDuration: Nullable<float>`
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

## ChipEffectUnlockedEventArgs
#### Fields
- `+ Chip: Chip`
- `+ EffectId: int`
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

> - **Purpose**: Factory for creating and placing Chip instances on the field grid.
> - **Usage**: Call Init once during scene initialization (Merge2Initializer) then call CreateChip to instantiate chips from ChipData
> - pass runtimeDataInitializer to override runtime data before Init.
> - **Notes**: After SetChipInCell, fires IScenarioEventHandler.RaiseChipCreated so scenario listeners and UVS nodes receive the creation event. Requires ChipData with a valid PrefabLink.
#### Fields
- `~ chipCollections: IChipCollections`
- `~ fieldGrid: IFieldGrid`
- `~ resolver: IObjectResolver`
- `~ scenarioEventHandler: IScenarioEventHandler`
#### Methods
- `+ CreateChip(ICell cell, ChipData chipData, Nullable<Vector3> parentWorldPosition, Action<ChipRuntimeData> runtimeDataInitializer, ChipFlightSettings flightSettings): Chip`
    - **Purpose**: Instantiates and initializes a new chip on the specified cell
    - **Usage**: Call when spawning a new chip on the field (e.g., from container, generator, or initialization)
    - **Params**: cell - target cell to place the chip on
    - chipData - configuration settings for the new chip
    - parentWorldPosition - optional start position for flight animation
    - runtimeDataInitializer - optional delegate to modify runtime state before initialization
    - flightSettings - flight animation configuration
    - **Returns**: The initialized Chip instance, or null if creation failed
- `+ CreateChip(Vector2Int cellPosition, ChipData chipData, Nullable<Vector3> parentWorldPosition, Action<ChipRuntimeData> runtimeDataInitializer, ChipFlightSettings flightSettings): Chip`
    - **Purpose**: Instantiates and initializes a new chip at the specified grid coordinates
    - **Usage**: Convenience wrapper around the cell-based CreateChip method
    - **Params**: cellPosition - target grid coordinates
    - chipData - configuration settings
    - parentWorldPosition - optional start position
    - runtimeDataInitializer - optional delegate
    - flightSettings - flight animation configuration
    - **Returns**: The initialized Chip instance
- `+ Init(IObjectResolver resolver, IFieldGrid fieldGrid, IChipCollections chipCollections, IScenarioEventHandler scenarioEventHandler): void`
    - **Purpose**: Wires all dependencies required by the factory before any chip can be created.
    - **Usage**: Called once by Merge2Initializer / IsoMergeInitializer at scene start. Must be called before CreateChip.
    - **Params**: resolver - VContainer object resolver for prefab instantiation
    - fieldGrid - grid for cell placement
    - chipCollections - registry of active chips
    - scenarioEventHandler - optional
    - when null, scenario events are silently skipped
    - **Notes**: scenarioEventHandler defaults to null to stay backward-compatible with tests that build ChipFactory without a full lifetime scope.
---

## ChipFlightSettings

> - **Purpose**: Encapsulates configuration parameters for a chip's flight animation, including duration, delay, and motion type
> - **Usage**: Passed to chip creation and relocation methods to configure transition animations. Use predefined static presets like Default, ChipSwap, or ChipRelocate
#### Fields
- `+ ChipGrow: ChipFlightSettings`
- `+ ChipRelocate: ChipFlightSettings`
- `+ ChipSwap: ChipFlightSettings`
- `+ Default: ChipFlightSettings`
- `+- Duration: float`
- `+- FlightDelay: float`
- `+- Type: FlightType`
---

## ChipFlyAnimation

> - **Purpose**: Handles the logic for animating a chip flying to a target position
> - **Usage**: Instance per animation context. Call Update per frame.
> - **Notes**: Supports Linear, ArcBounce, HalfArcHalfBounce, HalfArc flight types. Each type is handled in its own private method.
#### Fields
- `+- IsAnimating: bool`
- `- animationDuration: float`
- `- ARC_PATH_RATIO: float`
- `- ARC_PHASE: float`
- `- currentFlightType: FlightType`
- `- elapsedTime: float`
- `- initialLiftHeight: float`
- `- onCompleteCallback: Action`
- `- startPosition: Vector3`
- `- targetPosition: Vector3`
- `- targetTransform: Transform`
#### Methods
- `+ StartAnimation(Transform target, Vector3 startPos, Vector3 endPos, float duration, FlightType flightType, float initialLiftHeight, Action onComplete): void`
    - **Purpose**: Starts the fly animation for a chip
    - **Usage**: Call when a chip needs to fly back to its cell (e.g. after invalid drag or swap)
    - **Params**: target - the transform to animate
    - startPos - local start position
    - endPos - local target position
    - duration - time in seconds
    - flightType - type of flight animation
    - initialLiftHeight - current lift height at animation start used to smoothly blend into arc curve without exceeding arcHeight
    - onComplete - callback when finished
- `+ StopAnimation(): void`
- `+ Update(float deltaTime): float`
    - **Purpose**: Updates the animation state and applies movement to the target transform, returning flight height offset
    - **Usage**: Call in Update loop with deltaTime
    - **Params**: deltaTime - time since last frame
    - **Returns**: The current height offset of the flight animation for this frame
- `- ApplyArcBounce(float t): float`
- `- ApplyHalfArc(float t): float`
- `- ApplyHalfArcHalfBounce(float t): float`
- `- ApplyLinear(float t): float`
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

## ChipGeneratorRuntimeData

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

## ChipLiftController
**Inherits**: `MonoBehaviour`

> - **Purpose**: Manages chip lift height animations using coroutines as a separate MonoBehaviour component
> - **Usage**: Attach to chip prefab
> - handles fast lift height transitions and updates the shadow effect directly
#### Fields
- `++ LiftHeight: float`
- `- liftCoroutine: Coroutine`
- `- shadowEffect: IShadowEffect`
#### Methods
- `+ Init(Chip chip): void`
- `+ StartFastLiftHeight(): void`
- `+ StartFastLowerLiftHeight(): void`
- `+ StartLiftCoroutine(float fromValue, float toValue, float duration): void`
- `+ StopLiftCoroutine(): void`
- `- LiftCoroutine(float fromValue, float toValue, float duration): IEnumerator`
- `- OnDestroy(): void`
---

## ChipLiftControllerRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## ChipMergeAvailableEffect
**Inherits**: `Effect`

> - **Purpose**: Visual effect indicating that a merge is available with the chip below
> - **Usage**: Attached to chip using ChipData.MergeAvailableEffectPrefab
> - Activated by Chip when another compatible chip is dragged over it
> - **Notes**: Handles auto-sizing and positioning based on chip size
#### Fields
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
- `+ ChipMoving(ICell overCell, ICell leftTopCell, ICell sourceCell, ChipFlightSettings flightSettings): void`
    - **Purpose**: Orchestrates the move of a chip to a new position, potentially relocating others
    - **Usage**: Call when a drag operation ends at a specific cell
    - **Params**: overCell - the cell the mouse is over
    - leftTopCell - the target top-left cell for the chip
    - sourceCell - the original cell of the chip
    - flightSettings - flight animation configuration for the movement
    - **Notes**: If relocation is impossible, the chip snaps back to its source position
- `+ ChipsRelocate(ICell leftTopCell, ICell sourceCell, List<ChipMoveAction> plannedRelocations, ChipFlightSettings flightSettings): void`
    - **Purpose**: Executes the planned relocations of chips on the field
    - **Usage**: Final step of the relocation process
    - performs atomic field updates
    - **Params**: leftTopCell - destination for the primary chip
    - sourceCell - original spot of the primary chip
    - plannedRelocations - list of pre-calculated chip moves
    - flightSettings - flight animation configuration for the relocations
    - **Notes**: Temporarily sets dragging state during relocation for visual consistency
- `- GetOccupiedCellsInArea(Vector2Int cellPos, Vector2Int chipSize, IEnumerable<Chip> chipsToExclude): List<ICell>`
---

## ChipPowerBoosterData
#### Fields
- `++ Power: float`
---

## ChipRemovedEventArgs
#### Fields
- `+ Chip: Chip`
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
- `+- SpecialRuntimeDatas: List<IChipSpecialRuntimeData>`
- `- effectDestroyingDataArray: EffectDestroyingRuntimeData[]`
- `- effectEnablesArray: int[]`
#### Methods
- `+ GetSpecialRuntimeData(): T`
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

## ChipTapEvolutionData

> - **Purpose**: Configuration data for chip tap-evolution, defining the next chips with weights.
> - **Usage**: Used in ChipData specialDatas list. Queried by TapEvolutionModule in Init.
> - **Notes**: Evolves into a weighted random chip from NextChips array or gets destroyed if empty when tapped.
#### Fields
- `+ NextChips: EvolutionTarget[]`
#### Methods
- `+ GetRandomNextChip(): ChipData`
    - **Purpose**: Selects a random target chip from the next available evolution options based on their configured probability weights.
    - **Usage**: Called during tap evolution execution to determine which chip type to spawn next.
    - **Returns**: The selected ChipData config, or null if there are no next chips or if cumulative weight is zero.
---

## ChipWaitEvolutionData

> - **Purpose**: Configuration data for chip wait-evolution, defining the time to evolve, next chips with weights, and booster influence toggle.
> - **Usage**: Used in ChipData specialDatas list. Queried by WaitEvolutionModule in Init.
> - **Notes**: Evolves into a weighted random chip from NextChips array or gets destroyed if empty.
#### Fields
- `+ EvolveTime: float`
- `+ IsAffectedByBoosters: bool`
- `+ NextChips: EvolutionTarget[]`
#### Methods
- `+ GetRandomNextChip(): ChipData`
    - **Purpose**: Selects a random target chip from the next available evolution options based on their configured probability weights.
    - **Usage**: Called during evolution execution to determine which chip type to spawn next.
    - **Returns**: The selected ChipData config, or null if there are no next chips or if cumulative weight is zero.
---

## ChipWaitEvolutionRuntimeData

> - **Purpose**: Stores runtime state for a chip wait-evolution, tracking the remaining time until evolution.
> - **Usage**: Created and managed by WaitEvolutionModule. Used to persist state during gameplay.
> - **Notes**: Updated during update ticks as time progress.
#### Fields
- `+ TimeLeft: float`
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

## ContainerHintEffect
**Inherits**: `Effect`

> - **Purpose**: Visual effect controller for ChipContainer, managing spawned elements and displaying visual hints.
> - **Usage**: Attached to ChipContainer prefab
> - implements IEffectContainerHint
> - **Notes**: Triggers Hint animation on spawn (Activate), cell change (OnChangedCell), and tap (OnTap).
#### Fields
- `- delay: float`
- `- hintCoroutine: Coroutine`
- `- layoutForElements: Transform`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ Hint(bool force): void`
    - **Purpose**: Triggers the 'Hint' animator trigger to show a visual container hint, optionally forcing it immediately.
    - **Usage**: Called automatically on activation, cell change, or tap. Pass force=true to trigger immediately.
    - **Notes**: Stops any active hint coroutine. If force is true, triggers immediately
    - otherwise, schedules a new coroutine with the configured delay.
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnTap(): void`
- `+ UpdateElements(Chip chip, Dictionary<ContainerInfo, int> containers, bool isFull): void`
- `- ClearElements(): void`
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
- `- HintCoroutine(): IEnumerator`
---

## ContainerInfo
#### Fields
- `+- ContainerElementPrefab: GameObject`
- `+- Count: int`
- `+- RequiredChip: ChipData`
- `- cachedElementPrefab: GameObject`
---

## ContainerModule
**Inherits**: `MonoBehaviour`

> - **Purpose**: Modular component attached to a Chip that manages container requirements, progress tracking, and fulfillment
> - **Usage**: Attach to chip prefabs that act as containers
> - implements IChipModule
> - **Notes**: Triggers next chip creation or destruction when all container requirements are satisfied
#### Fields
- `+- Chip: Chip`
- `+- ContainerRequirements: IReadOnlyDictionary<ContainerInfo, int>`
- `- chipCollections: IChipCollections`
- `~ chipContainerData: ChipContainerData`
- `- chipFactory: ChipFactory`
- `- containerEffect: EffectContainerHintRef`
- `~ containerRuntimeData: ChipContainerRuntimeData`
- `~ data: ChipData`
- `- OnFillContainer: FillContainerDelegate`
#### Methods
- `+ DestroyModule(): void`
- `+ Init(Chip chip, ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the container module with chip data and runtime state
    - **Usage**: Called during chip initialization
    - sets up event handlers and links the container effect requirements
    - **Params**: chip - parent Chip component
    - data - static ChipData configuration
    - runtimeData - persisted runtime state wrapper
    - **Notes**: Retrieves container-specific special data and sets up visual requirements feedback
- `+ InitRuntimeData(ChipData data, ChipRuntimeData runtimeData): void`
- `+ IsChipCompatible(Chip chip): bool`
    - **Purpose**: Checks if a given chip is compatible with any of the container's remaining requirements
    - **Usage**: Called by interaction logic to determine if a chip can be dropped into this container
    - **Params**: chip - the chip to check for compatibility
    - **Returns**: True if the chip matches a required ChipData configuration
    - false otherwise
    - **Notes**: Does not modify the container's state
    - only performs a read-only check against current requirements
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnDrag(Vector2 position, ICell anchorCell): void`
- `+ OnDragEnd(): void`
- `+ OnDragStart(): void`
- `+ OnEffectRemoved(int effectId): void`
- `+ OnTap(): void`
- `+ TryAddChip(Chip chip): bool`
    - **Purpose**: Attempts to add a chip to the container, updating progress and handling completion logic
    - **Usage**: Called by interaction logic when a chip is dropped onto the container
    - **Params**: chip - the chip being added
    - **Returns**: True if the chip was successfully added
    - otherwise False
    - **Notes**: Side effects: Updates internal container progress
    - Triggers OnFillContainer event
    - If all requirements are met, destroys parent Cell content and spawns NextChipData result
- `+ UpdateVisual(): void`
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
- `- cellsToCheckBuffer: List<ICell>`
- `~ chipFinder: IChipFinder`
- `~ chipInteractionLogics: List<IChipInteractionLogic>`
    - **Purpose**: Collection of all chip interaction logic handlers
    - **Usage**: Internal field
    - populated in Awake and queried during drag operations
    - **Notes**: Populated in Awake from attached components
    - used to check and execute interactions like merge or container fill
- `~ chipMovingLogic: IChipMovingLogic`
- `~ currentMergableCell: ICell`
- `~ currentMergableLogic: IChipInteractionLogic`
- `~ dragFeedbacks: List<IDragFeedback>`
- `~ draggableChip: Chip`
- `~ draggableTransform: Transform`
    - **Purpose**: Cached transform of the chip being dragged
    - **Usage**: Internal field
    - cached for performance during drag operations
    - **Notes**: Set in OnDragStart for performance optimization during drag
- `~ fieldGrid: IFieldGrid`
- `- filterCellsBuffer: List<ICell>`
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
    - **Purpose**: Collects unique non-empty cells overlapping with the dragged chip's target area.
    - **Usage**: Internal helper called by UpdateInteractionState during chip dragging to find potential interaction targets.
    - **Params**: targetCell - the top-left/anchor cell of the candidate placement area
    - **Returns**: A reference to the shared filterCellsBuffer list populated with relevant cells.
    - **Notes**: Uses pre-allocated internal buffers to avoid per-frame GC allocations in the drag interaction loop.
- `- MoveToWorldPosition(Vector3 worldPosition): void`
- `~ NotifyNeighborsOfInteraction(ICell targetCell, Vector2Int targetChipSize): void`
    - **Purpose**: Notifies neighboring chips that an interaction has occurred on the target cell.
    - **Usage**: Called from OnDragEnd after a successful chip interaction.
    - **Params**: targetCell - main target cell of the interaction
    - targetChipSize - size of target chip prior to interaction
- `- ResetCurrentMergable(): void`
- `- ResetDragState(): void`
- `~ ResolveInteractionResult(Chip targetChip): DragInteractionResult`
    - **Purpose**: Determines the DragInteractionResult based on the target chip state after ExecuteInteraction.
    - **Usage**: Called from OnDragEnd after a successful interaction to classify the outcome as Partial or Full.
    - **Params**: targetChip - the chip that was the target of the interaction before it executed
    - **Returns**: Partial if the chip is a still-alive ChipContainer with remaining requirements
    - Full otherwise.
    - **Notes**: ChipContainer is destroyed when fully filled, so a Unity-null check (implicit bool) distinguishes partial from full fill.
- `- UpdateInteractionState(ICell sourceCell, ICell targetCell): void`
    - **Purpose**: Checks if merge or container fill is allowed between two cells.
    - **Usage**: Call before attempting merge or placement.
    - **Params**: sourceCell - cell chip is dragged from
    - targetCell - cell chip is hovered over
    - **Returns**: True if merge or fill is allowed
    - false otherwise.
    - **Notes**: Returns false if no handlers are subscribed.
---

## DragInteractionResult
**Inherits**: `Enum`
#### Fields
- `+ Full: DragInteractionResult`
- `+ None: DragInteractionResult`
- `+ Partial: DragInteractionResult`
- `+ value__: int`
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
- `+- Chip: Chip`
- `+- DestroyingSettings: EffectDestroyingSettings`
- `+~ IsActive: bool`
- `+- IsSkipDestroy: bool`
- `~ animator: Animator`
- `~ autoSize: AutoSizeType`
- `~ deactivateOnMove: bool`
- `~ destroyAfterActivate: bool`
- `~ destroyDelayAfterActivate: float`
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
- `+ OnTap(): void`
    - **Purpose**: Called when the chip is tapped
    - **Usage**: Override in derived classes to implement custom tap reaction visual effects
- `+ SendTrigger(string triggerName, bool allowRepeat): void`
    - **Purpose**: Sends a custom animation trigger to the effect's animator
    - **Usage**: Call to trigger custom animations on the effect
    - used for special interactions like move-locked feedback
    - **Params**: triggerName - name of the animator trigger to activate
    - allowRepeat - if true, bypasses the dontRepeatTrigger check for this call
    - **Notes**: Safely handles null animator
    - allows effects to respond to chip-specific events beyond standard Activate/Deactivate
- `+ SkipDestroy(): void`
    - **Purpose**: Marks the effect to skip automatic destruction when its owner chip is destroyed, and detaches it from the chip transform hierarchy
    - **Usage**: Called when an effect needs to outlive the chip, e.g. a merge visual effect played on a destroyed chip
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
- `+ MergeHint: int`
- `+ MergeLight: int`
- `+ PBoosterConnectorCells: int`
- `+ PBoosterJoin: int`
- `+ ShadowEffect: int`
- `+ TapHint: int`
- `- blockerNameToId: Dictionary<string, int>`
- `- nameToId: Dictionary<string, int>`
#### Methods
- `+ GetAllEffectsEditorOnly(): Dictionary<string, int>`
- `+ GetBlockerEffectsEditorOnly(): Dictionary<string, int>`
- `+ GetNameByIdEditorOnly(int id): string`
---

## EffectContainerHintRef
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

## EffectHintRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## EffectMergeLightRef
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
- `+- LockedAreas: LockedAreaData[]`
- `+- VisualFieldPrefab: GameObject`
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
- `+ GetCells(Vector2Int cellPos, Vector2Int size, List<ICell> result): void`
    - **Purpose**: Non-allocating overload that fills a caller-provided list instead of creating a new one
    - **Usage**: Use in hot paths (e.g., drag loop) to avoid per-frame GC allocations
    - **Params**: cellPos - top-left position
    - size - area dimensions
    - result - pre-allocated list to fill (cleared internally)
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
- `- cameraBoundsMultiplier: float`
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

## FlightType
**Inherits**: `Enum`
#### Fields
- `+ ArcBounce: FlightType`
- `+ HalfArc: FlightType`
- `+ HalfArcHalfBounce: FlightType`
- `+ Linear: FlightType`
- `+ value__: int`
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

## GeneratorModule
**Inherits**: `MonoBehaviour`

> - **Purpose**: Modular component attached to a Chip that handles chip generation, charging, recharges, and evolution
> - **Usage**: Attach to chip prefabs that act as generators
> - implements IChipModule and IPowerBoosterTarget
> - **Notes**: Supports manual and auto modes, coordinates visual feedback via charged and charging effects, and scales generation speed based on booster multipliers
#### Fields
- `+- AppliedBoosters: HashSet<PowerBoosterModule>`
- `+- BlockingState: CombinedBlockingState`
- `+- IsMoving: bool`
- `+- JoinPoints: IReadOnlyList<Transform>`
- `+- RuntimeDataOnlyEditor: ChipGeneratorRuntimeData`
- `- chargedEffect: EffectRef`
- `- chip: Chip`
- `- chipFactory: ChipFactory`
- `- data: ChipData`
- `- field: IFieldEventHandler`
- `- fieldGrid: IFieldGrid`
- `- findFreePlaceForChip: IFreeCellFinder`
- `- generatorData: ChipGeneratorData`
- `- generatorRuntimeData: ChipGeneratorRuntimeData`
- `- isAutoGeneration: bool`
- `- OnCharging: Action<float>`
- `~ powerMultiplier: float`
- `- rechargeEffect: ChargingEffectRef`
#### Methods
- `+ ApplyPowerBooster(PowerBoosterModule booster, bool reapply): bool`
    - **Purpose**: Applies a booster influence to the generator and recalculates the charging speed multiplier
    - **Usage**: Called when booster observation detects this generator in active range
    - **Params**: booster - the PowerBoosterModule instance
    - reapply - if true, allows reapplying even if already present
    - **Returns**: True if the booster was newly added
    - false otherwise
    - **Notes**: Recalculates power multiplier using maximum power among all active boosters
- `+ DestroyModule(): void`
- `+ Init(Chip chip, ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the generator module with static chip data and runtime state
    - **Usage**: Called during chip initialization
    - registers effects on the chip, registers charging callback, and subscribes to field events if auto-mode
    - **Params**: chip - the parent Chip component
    - data - static configuration
    - runtimeData - persistent runtime state wrapper
    - **Notes**: Triggers the initial visual update
- `+ InitRuntimeData(ChipData data, ChipRuntimeData runtimeData): void`
- `+ NotifyEffectRemoved(int effectId): void`
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnDrag(Vector2 position, ICell anchorCell): void`
- `+ OnDragEnd(): void`
- `+ OnDragStart(): void`
- `+ OnEffectRemoved(int effectId): void`
- `+ OnTap(): void`
- `+ RemovePowerBooster(PowerBoosterModule booster): void`
    - **Purpose**: Removes a booster influence and recalculates the charging speed multiplier
    - **Usage**: Called when booster observation detects this generator leaving its active range
    - **Params**: booster - the PowerBoosterModule to remove
    - **Notes**: Resets speed multiplier to 1f if no boosters remain
- `+ UpdateVisual(): void`
- `- OnFieldChanged(): void`
- `- RecalculatePowerMultiplier(): void`
- `- TryGenerateChip(): void`
    - **Purpose**: Attempts to generate a new chip based on generator configuration and availability of space
    - **Usage**: Triggers on user tap (manual mode) or charging cycle completion (auto mode)
    - **Notes**: Finds the nearest free cell. Decrements remaining charge count, and handles recharges/evolution when the cycle is completed.
- `- Update(): void`
---

## HintEffect
**Inherits**: `Effect`

> - **Purpose**: Visual effect handler for displaying hints on chips.
> - **Usage**: Attached to chip prefabs and referenced via IEffectHint contract
> - plays a trigger 'Hint' on the animator.
> - **Notes**: Implements IEffectHint to provide a Hint method that fires a visual hint.
#### Fields
- `- delay: float`
- `- hintCoroutine: Coroutine`
#### Methods
- `+ Deactivate(Chip chip, bool force): void`
- `+ Hint(bool force): void`
    - **Purpose**: Triggers the 'Hint' animator trigger to show a visual hint, optionally forcing it to display immediately.
    - **Usage**: Called automatically when a chip is spawned or moved, or on demand to display hints. Pass force=true to trigger immediately.
    - **Notes**: Stops any active hint coroutine. If force is true, triggers immediately
    - otherwise, schedules a new coroutine with the configured delay.
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
- `- HintCoroutine(): IEnumerator`
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

## IChargingEffect

> - **Purpose**: Interface for chip charging visual effects, used to update charging progress indicator.
> - **Usage**: Implemented by visual effects like ChargingEffect to receive progress updates from generator or evolution modules.
> - **Notes**: Provides a single OnCharging method receiving normalized progress.
#### Methods
- `+ OnCharging(float progress): void`
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
- `+- FillableChipsByData: Dictionary<ChipData, List<Chip>>`
- `+- MergeableChipsByData: Dictionary<ChipData, List<Chip>>`
#### Methods
- `+ AddChip(Chip chip): void`
- `+ OnChipBlockingChanged(Chip chip): void`
- `+ OnContainerRequirementsChanged(ContainerModule container, ChipData fulfilledRequirement): void`
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
- `+ StartAnimation(Transform target, Vector3 startPos, Vector3 endPos, float duration, FlightType flightType, float initialLiftHeight, Action onComplete): void`
    - **Purpose**: Starts the fly animation for a chip
    - **Usage**: Call when a chip needs to fly back to its cell (e.g. after invalid drag)
    - **Params**: target - the transform to animate
    - startPos - local start position
    - endPos - local target position
    - duration - time in seconds
    - flightType - type of flight animation
    - initialLiftHeight - current lift height at animation start used to smoothly blend into arc curve without exceeding arcHeight
    - onComplete - callback when finished
- `+ StopAnimation(): void`
    - **Purpose**: Stops the fly animation
    - **Usage**: Call when a chip needs to stop the animation
- `+ Update(float deltaTime): float`
    - **Purpose**: Updates the animation state and calculates/applies movement
    - **Usage**: Call in Update loop
    - **Params**: deltaTime - time since last frame
    - **Returns**: The current height offset of the flying animation at this frame
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

## IChipLiftController

> - **Purpose**: Interface for managing chip lift height, including coroutine transitions and state
> - **Usage**: Implemented by ChipLiftController
> - accessed by Chip, Cell, and DraggableChipLogic
#### Fields
- `++ LiftHeight: float`
#### Methods
- `+ Init(Chip chip): void`
    - **Purpose**: Initializes the controller with the parent chip reference and retrieves its shadow effect
    - **Usage**: Called automatically during Chip.Awake / initialization
    - **Params**: chip - the parent Chip component
- `+ StartFastLiftHeight(): void`
    - **Purpose**: Starts a quick lift height animation (typically from current height to 0.1f over 0.2s)
    - **Usage**: Called when the user begins dragging the chip to lift it off the board
- `+ StartFastLowerLiftHeight(): void`
    - **Purpose**: Starts a quick lower height animation (typically from current height to 0f over 0.1s)
    - **Usage**: Called when dragging ends or during custom drop animations to return the chip to the board surface
- `+ StartLiftCoroutine(float fromValue, float toValue, float duration): void`
    - **Purpose**: Starts a smooth lerp animation for the chip's lift height using a coroutine
    - **Usage**: Called to animate lift height transitions
    - **Params**: fromValue - starting lift height
    - toValue - target lift height
    - duration - animation duration in seconds
- `+ StopLiftCoroutine(): void`
    - **Purpose**: Stops any active lift height animation coroutine
    - **Usage**: Called to interrupt current height animations (e.g. on drag end or destruction)
---

## IChipModule

> - **Purpose**: Interface for all modular components attached to a Chip GameObject that extend its capabilities
> - **Usage**: Implement this interface for modular components (e.g. GeneratorModule, ContainerModule, PowerBoosterModule)
> - Chip automatically queries and manages components implementing this interface
> - **Notes**: Supports runtime initialization, visual updates, tap/drag input, and cell relocation lifecycles
#### Methods
- `+ DestroyModule(): void`
- `+ Init(Chip chip, ChipData data, ChipRuntimeData runtimeData): void`
- `+ InitRuntimeData(ChipData data, ChipRuntimeData runtimeData): void`
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnDrag(Vector2 position, ICell anchorCell): void`
- `+ OnDragEnd(): void`
- `+ OnDragStart(): void`
- `+ OnEffectRemoved(int effectId): void`
- `+ OnTap(): void`
- `+ UpdateVisual(): void`
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
- `+ ChipMoving(ICell overCell, ICell leftTopCell, ICell sourceCell, ChipFlightSettings flightSettings): void`
    - **Purpose**: Moves a chip to a new cell position, handling potential relocations.
    - **Usage**: Call when a chip is dropped or needs to be moved programmatically.
    - **Params**: overCell - cell under the chip
    - leftTopCell - target anchor cell
    - sourceCell - original anchor cell
    - flightSettings - flight animation configuration for the movement
- `+ ChipsRelocate(ICell leftTopCell, ICell sourceCell, List<ChipMoveAction> plannedRelocations, ChipFlightSettings flightSettings): void`
    - **Purpose**: Executes pre-planned chip relocations.
    - **Usage**: Call after CanChipMoving returns true to apply the relocations.
    - **Params**: leftTopCell - destination for the primary chip
    - sourceCell - original spot of the primary chip
    - plannedRelocations - list of calculated moves
    - flightSettings - flight animation configuration for the relocations
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

## IChipSpecialRuntimeData

> - **Purpose**: Marker interface for special runtime data objects held inside ChipRuntimeData
> - **Usage**: Implement on specific module runtime state classes (e.g., ChipGeneratorRuntimeData, ChipContainerRuntimeData)
---

## IDragFeedback

> - **Purpose**: Contract for visual drag feedback components attached to the same GameObject as DraggableChipLogic.
> - **Usage**: Implement on MonoBehaviours to receive drag lifecycle callbacks for visual hints (e.g., MergeHint, GroupCounter).
> - **Notes**: Collected via GetComponents<IDragFeedback>() in DraggableChipLogic.Awake(). OnDragFeedback is only called when the anchor cell changes.
#### Methods
- `+ OnDragEndFeedback(Chip chip, DragInteractionResult interaction): void`
- `+ OnDragFeedback(Chip chip, ICell prevCell, ICell newCell): void`
- `+ OnDragStartFeedback(Chip chip): void`
---

## IEffect

> - **Purpose**: Interface for all effects that can be activated or deactivated on chips
> - **Usage**: Implement this interface for custom chip effects. Used by Chip class to manage common effect lifecycle.
> - **Notes**: Includes core activation, deactivation, trigger, and cell change handling methods.
#### Fields
- `+- BlockingSettings: EffectBlockingSettings`
- `+- DestroyingSettings: EffectDestroyingSettings`
- `+- gameObject: GameObject`
- `+- IsActive: bool`
- `+- IsSkipDestroy: bool`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ GetId(): int`
- `+ Init(Chip chip, int effectHash): void`
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnInteractionOverCellChanged(ICell prevCell, ICell currentCell, ICell underCell): void`
- `+ OnInteractionUnderCellChanged(ICell underCell, ICell overCell): void`
- `+ OnMovingStateChanged(Chip chip, bool isMoving): void`
- `+ OnTap(): void`
    - **Purpose**: Called on the visual effect when the owner chip is tapped.
    - **Usage**: Called automatically by the Chip component's OnTap method to notify all active effects.
- `+ SendTrigger(string triggerName, bool allowRepeat): void`
- `+ SkipDestroy(): void`
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

## IEffectContainerHint

> - **Purpose**: Interface for ChipContainer specific visual effects, extending hint functionality
> - **Usage**: Implemented by ContainerHintEffect to handle container visuals. Used by ChipContainer.
> - **Notes**: Extends IEffectHint with UpdateElements method.
#### Methods
- `+ UpdateElements(Chip chip, Dictionary<ContainerInfo, int> containers, bool isFull): void`
---

## IEffectHint

> - **Purpose**: Interface for hint visual effects, used to trigger visual hints on demand.
> - **Usage**: Implemented by visual effects that can show hints, typically using animation triggers.
> - **Notes**: Provides a Hint method to fire a visual hint trigger.
#### Methods
- `+ Hint(bool force): void`
---

## IEffectMergeLight

> - **Purpose**: Specialized IEffect contract for MergeLight effects that support a 'little' (small) flash variant.
> - **Usage**: Implemented by MergeLightEffect. Set Little = true before calling Activate — the setter writes to the Animator directly, routing to the small-flash state.
> - **Notes**: The Animator bool 'Little' is updated immediately on property set, so no extra Activate override is required.
#### Fields
- `++ Little: bool`
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
- `+ GetCells(Vector2Int cellPos, Vector2Int size, List<ICell> result): void`
    - **Purpose**: Non-allocating overload that fills a caller-provided list instead of creating a new one
    - **Usage**: Use in hot paths (e.g., drag loop) to avoid per-frame GC allocations
    - **Params**: cellPos - top-left position
    - size - area dimensions
    - result - pre-allocated list to fill (cleared internally)
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
- `+ SetBounds(Transform leftBottom, Transform rightTop): void`
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
- `+- AppliedBoosters: HashSet<PowerBoosterModule>`
- `+- BlockingState: CombinedBlockingState`
- `+- IsMoving: bool`
- `+- JoinPoints: IReadOnlyList<Transform>`
#### Methods
- `+ ApplyPowerBooster(PowerBoosterModule powerBoosterModule, bool reapply): bool`
    - **Purpose**: Adds a booster to this entity.
    - **Usage**: Called when booster observation detects this entity in range.
    - **Params**: powerBoosterModule - booster to apply
    - **Returns**: True if the booster was added
    - false if it was already active.
- `+ NotifyEffectRemoved(int effectId): void`
    - **Purpose**: Notifies that an effect has been removed from this entity.
    - **Usage**: Called by Chip.Effects when an effect is removed
    - allows boosters to re-evaluate influence.
    - **Params**: effectId - identifier of the removed effect
- `+ RemovePowerBooster(PowerBoosterModule powerBoosterModule): void`
    - **Purpose**: Removes a booster from this entity.
    - **Usage**: Called when booster observation no longer includes this entity or booster is removed.
    - **Params**: powerBoosterModule - booster to remove
---

## IScenarioEventHandler

> - **Purpose**: Contract for dispatching key board-state transitions (chip created, removed, effect unlocked, area unlocked) to scenario and quest listeners.
> - **Usage**: Inject via VContainer
> - call Raise* methods from ChipFactory, Chip, and LockedAreaManager at the appropriate lifecycle points. Subscribers may be C# delegates or Unity Visual Scripting EventBus listeners.
> - **Notes**: Registered as Singleton in Merge2LifetimeScope and IsoMergeLifetimeScope. All Raise* methods fire both a C# event and an EventBus.Trigger for UVS graph nodes.
#### Methods
- `+ RaiseAreaUnlocked(int areaId): void`
- `+ RaiseChipCreated(Chip chip, ICell cell): void`
- `+ RaiseChipEffectUnlocked(Chip chip, int effectId): void`
- `+ RaiseChipRemoved(Chip chip): void`
---

## IShadowEffect

> - **Purpose**: Interface representing a shadow effect that responds to changes in the chip's lift height
> - **Usage**: Implemented by ShadowEffect and accessed by ChipLiftController or Cell to update shadow offset/scale dynamically
#### Methods
- `+ OnHeightChanged(float height): void`
    - **Purpose**: Updates the shadow's local position and scale based on the chip's current lift height
    - **Usage**: Called dynamically when the chip is lifted (dragged) or is in a flight animation
    - **Params**: height - the current height value of the chip
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
- `- fadeOutCoroutine: Coroutine`
- `- lockedAreaManager: ILockedAreaManager`
#### Methods
- `+ FadeOutParticles(float duration): void`
    - **Purpose**: Smoothly fades out and extinguishes all active child particles over a specified duration
    - **Usage**: Called via animation events during deactivation, passing the desired fade duration in seconds
    - **Params**: duration - the total time in seconds over which the particles should gradually disappear
    - **Notes**: Stops particle emission immediately and runs a coroutine that scales down the alpha channel of startColor for all active particles frame-by-frame to prevent sudden lifetime progression jumps
- `+ Init(Chip chip, int effectId): void`
    - **Purpose**: Registers this level visual effect with the locked-area runtime manager
    - **Usage**: Called by FieldInitializeCommand after the level visual prefab is instantiated and injected
    - **Params**: chip - ignored because locked-area visuals are not chip-owned
    - effectId - stored for inherited Effect identity
    - **Notes**: Intentionally skips base.Init because the base implementation reads chip.Data and deactivates chip effects
- `- FadeOutParticlesCoroutine(float duration): IEnumerator`
- `- OnDisable(): void`
---

## LockedAreaManager

> - **Purpose**: Coordinates runtime locked-area state, visuals, and deferred chip spawning
> - **Usage**: Registered as a VContainer singleton and initialized after FieldGrid creates cells but before chips are loaded
> - **Notes**: Cells remain blocked until UnlockArea clears them
> - deferred chips are created only during unlock
#### Fields
- `- chipCollections: IChipCollections`
- `- effectsByAreaId: Dictionary<int, List<LockedAreaEffect>>`
- `- fieldData: FieldData`
- `- fieldGrid: IFieldGrid`
- `- scenarioEventHandler: IScenarioEventHandler`
- `- unlockedAreaIds: HashSet<int>`
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
    - **Notes**: Immediately synchronizes visual state so late registrations still match current lock state. Disables the effect's game object if the corresponding LockedAreaId is not found in the field data.
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
> - **Notes**: Wires up dependencies, connects input events, and initializes field and chips. Visual Scripting setup is handled separately by MergeBaseVScriptingInitializer.
#### Fields
- `- chipCollections: IChipCollections`
- `- chipFactory: ChipFactory`
- `- field: IFieldEventHandler`
- `- fieldGrid: IFieldGrid`
- `- fieldInitializeCommand: IFieldInitializeCommand`
- `- inputManager: InputManager`
- `- lockedAreaManager: ILockedAreaManager`
- `- resolver: IObjectResolver`
- `- scenarioEventHandler: IScenarioEventHandler`
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
- `+ SetBounds(Transform leftBottom, Transform rightTop): void`
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

## MergeHintDragFeedback
**Inherits**: `MonoBehaviour`

> - **Purpose**: Activates MergeHint effects on all compatible chips when a chip drag begins.
> - **Usage**: Attach to the same GameObject as DraggableChipLogic. Automatically collected via IDragFeedback.
> - **Notes**: Uses a coroutine to spread hint activation across frames (HintsPerFrame batch size) to avoid FPS spikes.
#### Fields
- `- chipCollections: IChipCollections`
- `- hintCoroutine: Coroutine`
- `- hintedChips: List<Chip>`
- `- HintsPerFrame: int`
#### Methods
- `+ OnDragEndFeedback(Chip chip, DragInteractionResult interaction): void`
- `+ OnDragFeedback(Chip chip, ICell prevCell, ICell newCell): void`
- `+ OnDragStartFeedback(Chip chip): void`
- `- ActivateHintsGradually(Chip chip): IEnumerator`
    - **Purpose**: Gradually activates MergeHint effects on compatible merge targets and fillable containers to avoid FPS spikes.
    - **Usage**: Started as a coroutine from OnDragStartFeedback.
    - **Params**: chip - the chip being dragged
    - **Notes**: Uses MergeableChipsByData (merge targets) and FillableChipsByData (containers) — both already filter out blocked chips. All hinted chips are tracked in hintedChips for cleanup.
---

## MergeLightDragFeedback
**Inherits**: `MonoBehaviour`

> - **Purpose**: Triggers the MergeLight effect on the target chip after a successful interaction.
> - **Usage**: Attach to the same GameObject as DraggableChipLogic. Automatically collected via IDragFeedback.
> - **Notes**: Uses the target chip passed via OnDragEndFeedback when interaction is Full.
#### Methods
- `+ OnDragEndFeedback(Chip chip, DragInteractionResult interaction): void`
- `+ OnDragFeedback(Chip chip, ICell prevCell, ICell newCell): void`
- `+ OnDragStartFeedback(Chip chip): void`
---

## MergeLightEffect
**Inherits**: `Effect`

> - **Purpose**: MergeLight visual effect that supports a 'little' small-flash variant via an Animator bool parameter.
> - **Usage**: Attach to MergeLightEffect prefab instead of the base Effect component. Set Little = true — the setter immediately writes animator.SetBool('Little', true). Then call Activate as usual
> - the Animator routes to the correct state.
> - **Notes**: No Activate/Deactivate overrides are needed — the base Effect handles the 'Activate' trigger
> - Animator routing is fully driven by the 'Little' bool parameter.
#### Fields
- `++ Little: bool`
#### Methods
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
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
- `~ powerBoosterModule: PowerBoosterModule`
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

## PowerBoosterModule
**Inherits**: `MonoBehaviour`

> - **Purpose**: Modular component attached to a Chip that boosts nearby IPowerBoosterTarget entities and manages link effects
> - **Usage**: Attach to booster chip prefabs together with PowerBoosterCellSubscriber
> - implements IChipModule
> - **Notes**: Exposes numeric power boost state and manages line-drawing join links and connector cell highlight visuals
#### Fields
- `+- BoostedTargets: HashSet<IPowerBoosterTarget>`
- `+- Power: float`
- `~ cellSubscriber: PowerBoosterCellSubscriber`
- `- chip: Chip`
- `~ chipPowerBoosterData: ChipPowerBoosterData`
- `~ connectorCellsHighlightEffect: EffectRef`
- `- data: ChipData`
- `- fieldGrid: IFieldGrid`
- `~ joinEffect: EffectPowerBoosterJoinRef`
- `- wasCanApplyModifiers: bool`
#### Methods
- `+ ApplyPowerBooster(IPowerBoosterTarget target, bool reapply): void`
    - **Purpose**: Applies booster influence to a target entity and triggers the join effect
    - **Usage**: Called by PowerBoosterCellSubscriber when a matching target enters observed cells
    - **Params**: target - the target entity receiving this booster
    - reapply - if true, allows reapplying even if already present
- `+ DestroyModule(): void`
- `+ Init(Chip chip, ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the power booster module with chip data and runtime state
    - **Usage**: Called during chip initialization
    - checks dependencies and registers highlight/join effects on the chip
    - **Params**: chip - the parent Chip component
    - data - static ChipData configuration
    - runtimeData - persistent runtime state wrapper
    - **Notes**: Logs errors and returns early if PowerBoosterCellSubscriber or ChipPowerBoosterData are missing
- `+ InitRuntimeData(ChipData data, ChipRuntimeData runtimeData): void`
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnDrag(Vector2 position, ICell anchorCell): void`
- `+ OnDragEnd(): void`
- `+ OnDragStart(): void`
- `+ OnEffectRemoved(int effectId): void`
- `+ OnTap(): void`
- `+ OnTargetChipEffectRemoved(IPowerBoosterTarget chipTarget, int effectId): void`
- `+ OnTargetChipMoved(IPowerBoosterTarget chipTarget, bool value): void`
- `+ RemovePowerBooster(IPowerBoosterTarget target): void`
    - **Purpose**: Removes booster influence from a target entity and triggers the join effect leave notification
    - **Usage**: Called by PowerBoosterCellSubscriber when a matching target leaves observed cells or gets removed
    - **Params**: target - the target entity losing this booster
- `+ UpdateVisual(): void`
---

## ScenarioEventHandler

> - **Purpose**: Dispatches key board-state transitions (chip created, removed, effect unlocked, area unlocked) to C# subscribers.
> - **Usage**: Registered as Singleton via VContainer
> - Raise* methods are called by ChipFactory (OnChipCreated), Chip.Destroy (OnChipRemoved), Chip.RemoveEffect (OnChipEffectUnlocked), and LockedAreaManager.UnlockArea (OnAreaUnlocked).
> - **Notes**: Fires only C# events. Unity Visual Scripting EventBus forwarding is handled by VScriptingScenarioEventBridge in MergeBase.VScripting assembly (compiled only when com.unity.visualscripting is installed).
#### Fields
- `- OnAreaUnlocked: Action<int>`
- `- OnChipCreated: Action<Chip>`
- `- OnChipEffectUnlocked: Action<Chip, int>`
- `- OnChipRemoved: Action<Chip>`
#### Methods
- `+ RaiseAreaUnlocked(int areaId): void`
- `+ RaiseChipCreated(Chip chip, ICell cell): void`
- `+ RaiseChipEffectUnlocked(Chip chip, int effectId): void`
- `+ RaiseChipRemoved(Chip chip): void`
---

## ShadowEffect
**Inherits**: `Effect`

> - **Purpose**: Provides a persistent shadow effect for chips that reacts to movement states
> - **Usage**: Instantiated by Chip.InitEffects
> - sends 'Move' and 'Stop' triggers to its animator when the chip starts or stops moving
> - **Notes**: Does not deactivate on move to ensure the shadow remains visible during dragging
#### Fields
- `- additionallyWhenMoving: int`
- `- cashedLayerOrder: int`
- `- initialLocalPosition: Vector3`
- `- initialLocalScale: Vector3`
- `- isInitialized: bool`
- `- shadowOffsetPerOneHeight: Vector3`
- `- shadowRenderer: SpriteRenderer`
- `- shadowScalePerOneHeight: float`
#### Methods
- `+ Activate(Chip chip): bool`
- `+ Deactivate(Chip chip, bool force): void`
- `+ Init(Chip chip, int effectId): void`
- `+ OnHeightChanged(float height): void`
    - **Purpose**: Updates the shadow sprite renderer's local position and scale to visually simulate height changes
    - **Usage**: Implements IShadowEffect.OnHeightChanged
    - modifies localPosition based on shadowOffsetPerOneHeight and localScale based on shadowScalePerOneHeight
    - **Params**: height - the current height value of the chip
- `+ OnMovingStateChanged(Chip chip, bool isMoving): void`
    - **Purpose**: Reacts to chip movement by sending appropriate animator triggers
    - **Usage**: Called by Chip.NotifyEffectsOnMovingStateChanged
    - sends 'Move' when isMoving is true, and 'Stop' otherwise
    - **Params**: chip - owner chip
    - isMoving - current movement state
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
---

## ShadowEffectRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## SortingLayerData

> - **Purpose**: Stores data for a single renderer's sorting layer configuration
> - **Usage**: Used by IChipSortingLayer to manage multiple renderers within a chip
> - **Notes**: CachedOrder is populated during Init and restored when movement ends
#### Fields
- `+ AdditionallyWhenMoving: int`
- `+ CachedOrder: int`
- `+ Renderer: SpriteRenderer`
---

## TapEvolutionModule
**Inherits**: `MonoBehaviour`

> - **Purpose**: Modular component attached to a Chip that handles evolution on user tap, spawning a weighted random next chip and playing visual effects on spawn and cell change.
> - **Usage**: Attach to chip prefabs that evolve on tap
> - implements IChipModule
> - **Notes**: Evolves into a weighted random chip, respects chip movement/drag deferral, and plays a configured effect on spawn and cell changes.
#### Fields
- `- chip: Chip`
- `- chipFactory: ChipFactory`
- `- data: ChipData`
- `- evolutionData: ChipTapEvolutionData`
- `- fieldGrid: IFieldGrid`
- `- hintEffect: EffectHintRef`
- `- resolver: IObjectResolver`
#### Methods
- `+ DestroyModule(): void`
- `+ Init(Chip chip, ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the module references, sets up tap evolution data, and registers the hint effect.
    - **Usage**: Called automatically by Chip during its initialization phase for each attached chip module.
    - **Params**: chip - the parent Chip component
    - data - configuration settings of the chip
    - runtimeData - mutable state of the chip
- `+ InitRuntimeData(ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the special runtime data structure for tap evolution if needed.
    - **Usage**: Called before module initialization to prepare the initial state for the chip's runtime data collection.
    - **Params**: data - configuration settings of the chip
    - runtimeData - mutable state container of the chip
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnDrag(Vector2 position, ICell anchorCell): void`
- `+ OnDragEnd(): void`
- `+ OnDragStart(): void`
- `+ OnEffectRemoved(int effectId): void`
- `+ OnTap(): void`
    - **Purpose**: Handles tap input by checking permissions, and initiating tap evolution if allowed.
    - **Usage**: Called automatically when the parent chip is tapped by the user.
- `+ UpdateVisual(): void`
    - **Purpose**: Updates the visual state of the tap evolution hint effect based on whether the chip can currently be tapped.
    - **Usage**: Called when chip visual state is refreshed, activating or deactivating the TapHint effect according to CanBeTaped.
- `- ExecuteTapEvolution(): void`
    - **Purpose**: Performs tap evolution by destroying the current chip and spawning the next weighted random chip in the same cell.
    - **Usage**: Called internally when the chip is tapped and not moving or being dragged.
    - **Notes**: If no next chip configuration is determined, the current chip is just destroyed.
---

## UnlockAreaNode
**Inherits**: `Unit`

> - **Purpose**: Triggers the unlocking of a locked area on the field grid via ILockedAreaManager.
> - **Usage**: Connect to a control flow
> - resolves ILockedAreaManager from local object Variables and calls UnlockArea.
> - **Notes**: Supports optional force unlock flag. Emits event OnAreaUnlocked on completion. LockedAreaManager must be set on the ScriptMachine's GameObject local Variables during scene initialization.
#### Fields
- `+- expectedAreaId: ValueInput`
- `+- forceUnlock: ValueInput`
- `+- inputTrigger: ControlInput`
- `+- outputTrigger: ControlOutput`
#### Methods
- `~ Definition(): void`
- `- Trigger(Flow flow): ControlOutput`
---

## VisualField
**Inherits**: `MonoBehaviour`

> - **Purpose**: Provides visual presentation of the 2D merge grid, managing field sprite size and injecting sub-effects like locked areas.
> - **Usage**: Attach to visual field prefabs
> - instantiated dynamically via field initialization.
#### Fields
- `- fieldBorderSpriteRenderer: SpriteRenderer`
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

## WaitEvolutionModule
**Inherits**: `MonoBehaviour`

> - **Purpose**: Modular component attached to a Chip that handles evolution over time, showing progress effect, and reacting to boosters.
> - **Usage**: Attach to chip prefabs that evolve after a delay
> - implements IChipModule and IPowerBoosterTarget
> - **Notes**: Supports evolution into weighted random chips or destruction, respects chip movement/drag deferral, and handles booster speed scale toggling.
#### Fields
- `+- AppliedBoosters: HashSet<PowerBoosterModule>`
- `+- BlockingState: CombinedBlockingState`
- `+- IsMoving: bool`
- `+- JoinPoints: IReadOnlyList<Transform>`
- `- chargingEffect: ChargingEffectRef`
- `- chip: Chip`
- `- chipFactory: ChipFactory`
- `- data: ChipData`
- `- evolutionData: ChipWaitEvolutionData`
- `- evolutionRuntimeData: ChipWaitEvolutionRuntimeData`
- `- fieldGrid: IFieldGrid`
- `- OnCharging: Action<float>`
- `- powerMultiplier: float`
- `- startDeactivateChargingEffect: bool`
#### Methods
- `+ ApplyPowerBooster(PowerBoosterModule powerBoosterModule, bool reapply): bool`
    - **Purpose**: Registers a power booster affecting this module and triggers recalculation of the evolution speed.
    - **Usage**: Called by PowerBoosterModule when a booster is placed nearby or cell observers detect proximity changes.
    - **Params**: powerBoosterModule - the booster component to apply
    - reapply - if true, bypasses modifier receptivity checks
    - **Returns**: True if the booster was successfully added to the active set, false otherwise
- `+ DestroyModule(): void`
- `+ Init(Chip chip, ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the module references, sets up wait evolution data and links the charging progress effect.
    - **Usage**: Called automatically by Chip during its initialization phase for each attached chip module.
    - **Params**: chip - the parent Chip component
    - data - configuration settings of the chip
    - runtimeData - mutable state of the chip
- `+ InitRuntimeData(ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the special runtime data structure for wait evolution if it does not already exist.
    - **Usage**: Called before module initialization to prepare the initial state for the chip's runtime data collection.
    - **Params**: data - configuration settings of the chip
    - runtimeData - mutable state container of the chip
- `+ NotifyEffectRemoved(int effectId): void`
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnDrag(Vector2 position, ICell anchorCell): void`
- `+ OnDragEnd(): void`
- `+ OnDragStart(): void`
- `+ OnEffectRemoved(int effectId): void`
- `+ OnTap(): void`
- `+ RemovePowerBooster(PowerBoosterModule powerBoosterModule): void`
    - **Purpose**: Deregisters a power booster from this module and recalculates the evolution speed.
    - **Usage**: Called by PowerBoosterModule when it is moved away or destroyed.
    - **Params**: powerBoosterModule - the booster component to remove
- `+ UpdateVisual(): void`
- `- ExecuteEvolution(): void`
    - **Purpose**: Performs the evolution process by destroying the current chip and spawning the next weighted random chip in the same cell.
    - **Usage**: Called internally when wait evolution timer expires and the chip is not moving or dragged.
    - **Notes**: If no next chip configuration is determined, the current chip is just destroyed.
- `- RecalculatePowerMultiplier(): void`
- `- Update(): void`
---

## WaitForAreaUnlockedNode
**Inherits**: `Unit`

> - **Purpose**: Suspends graph execution until a locked area is unlocked.
> - **Usage**: Connect to a control flow
> - registers to EventBus dynamically when reached, resumes flow when event triggers.
> - **Notes**: Must be run as a coroutine in UVS (enable Coroutine on the starting event node).
#### Fields
- `+- areaId: ValueOutput`
- `+- expectedAreaId: ValueInput`
- `+- inputTrigger: ControlInput`
- `+- outputTrigger: ControlOutput`
#### Methods
- `~ Definition(): void`
- `- TriggerCoroutine(Flow flow): IEnumerator`
---

## WaitForChipCreatedNode
**Inherits**: `Unit`

> - **Purpose**: Suspends graph execution until a new chip is created, matching optional expectedChipData.
> - **Usage**: Connect to a control flow
> - registers to EventBus dynamically when reached, resumes flow when event triggers.
> - **Notes**: Must be run as a coroutine in UVS (enable Coroutine on the starting event node).
#### Fields
- `+- cell: ValueOutput`
- `+- chip: ValueOutput`
- `+- chipData: ValueOutput`
- `+- expectedChipData: ValueInput`
- `+- inputTrigger: ControlInput`
- `+- outputTrigger: ControlOutput`
#### Methods
- `~ Definition(): void`
- `- TriggerCoroutine(Flow flow): IEnumerator`
---

## WaitForChipEffectUnlockedNode
**Inherits**: `Unit`

> - **Purpose**: Suspends graph execution until a blocker effect is removed from a chip.
> - **Usage**: Connect to a control flow
> - registers to EventBus dynamically when reached, resumes flow when event triggers.
> - **Notes**: Must be run as a coroutine in UVS (enable Coroutine on the starting event node).
#### Fields
- `+- chip: ValueOutput`
- `+- chipData: ValueOutput`
- `+- effectId: ValueOutput`
- `+- expectedChipData: ValueInput`
- `+- expectedEffectId: ValueInput`
- `+- inputTrigger: ControlInput`
- `+- outputTrigger: ControlOutput`
#### Methods
- `~ Definition(): void`
- `- TriggerCoroutine(Flow flow): IEnumerator`
---

## WaitForChipRemovedNode
**Inherits**: `Unit`

> - **Purpose**: Suspends graph execution until a chip is removed.
> - **Usage**: Connect to a control flow
> - registers to EventBus dynamically when reached, resumes flow when event triggers.
> - **Notes**: Must be run as a coroutine in UVS (enable Coroutine on the starting event node).
#### Fields
- `+- chip: ValueOutput`
- `+- chipData: ValueOutput`
- `+- expectedChipData: ValueInput`
- `+- inputTrigger: ControlInput`
- `+- outputTrigger: ControlOutput`
#### Methods
- `~ Definition(): void`
- `- TriggerCoroutine(Flow flow): IEnumerator`
---

