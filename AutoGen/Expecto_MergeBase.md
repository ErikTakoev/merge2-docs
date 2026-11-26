# Namespace: Expecto.MergeBase

## Table of Contents
- [AnimationSortingData](#animationsortingdata)
- [BlockedCell](#blockedcell)
- [BlockerEffectData](#blockereffectdata)
- [Cell](#cell)
- [CellHighlightEffect](#cellhighlighteffect)
- [CellObserverManager](#cellobservermanager)
- [CellSubscriber](#cellsubscriber)
- [ChargingEffect](#chargingeffect)
- [ChargingEffectRef](#chargingeffectref)
- [Chip](#chip)
- [ChipChangedEvent](#chipchangedevent)
- [ChipCollections](#chipcollections)
- [ChipCollectionType](#chipcollectiontype)
- [ChipData](#chipdata)
- [ChipDataCollection](#chipdatacollection)
- [ChipEffectsData](#chipeffectsdata)
- [ChipEvolutionExecutor](#chipevolutionexecutor)
- [ChipEvolutionPlanner](#chipevolutionplanner)
- [ChipEvolutionService](#chipevolutionservice)
- [ChipFactory](#chipfactory)
- [ChipFlightSettings](#chipflightsettings)
- [ChipFlyAnimation](#chipflyanimation)
- [ChipLiftController](#chipliftcontroller)
- [ChipLiftControllerRef](#chipliftcontrollerref)
- [ChipMaterialController](#chipmaterialcontroller)
- [ChipMaterialControllerRef](#chipmaterialcontrollerref)
- [ChipMaterialEffect](#chipmaterialeffect)
- [ChipMovingLogic](#chipmovinglogic)
- [ChipPowerBoosterData](#chippowerboosterdata)
- [ChipRuntimeData](#chipruntimedata)
- [ChipSelectorAttribute](#chipselectorattribute)
- [ChipSelectorDrawer](#chipselectordrawer)
- [ChipSortingLayer](#chipsortinglayer)
- [ChipSpawnData](#chipspawndata)
- [CombinedBlockingState](#combinedblockingstate)
- [ConstantEvolutionProvider](#constantevolutionprovider)
- [DeferredChipChangeNotifier](#deferredchipchangenotifier)
- [DraggableChipLogic](#draggablechiplogic)
- [DragInteractionResult](#draginteractionresult)
- [Effect](#effect)
- [EffectBlockerDefinitionAttribute](#effectblockerdefinitionattribute)
- [EffectBlockerSelectorAttribute](#effectblockerselectorattribute)
- [EffectBlockerSelectorDrawer](#effectblockerselectordrawer)
- [EffectBlockingSettings](#effectblockingsettings)
- [EffectConsts](#effectconsts)
- [EffectData](#effectdata)
- [EffectDefinitionAttribute](#effectdefinitionattribute)
- [EffectDestroyingRuntimeData](#effectdestroyingruntimedata)
- [EffectDestroyingSettings](#effectdestroyingsettings)
- [EffectHintRef](#effecthintref)
- [EffectMergeLightRef](#effectmergelightref)
- [EffectPowerBoosterJoinRef](#effectpowerboosterjoinref)
- [EffectRef](#effectref)
- [EffectSelectorAttribute](#effectselectorattribute)
- [EffectSelectorDrawer](#effectselectordrawer)
- [EffectSortingData](#effectsortingdata)
- [EvolutionBehaviorSettings](#evolutionbehaviorsettings)
- [EvolutionExecutionSettings](#evolutionexecutionsettings)
- [EvolutionPlacementItem](#evolutionplacementitem)
- [EvolutionPlan](#evolutionplan)
- [EvolutionPlanResult](#evolutionplanresult)
- [EvolutionSpawnFlightSettings](#evolutionspawnflightsettings)
- [EvolutionTriggerSettings](#evolutiontriggersettings)
- [FieldData](#fielddata)
- [FieldEventHandler](#fieldeventhandler)
- [FieldGrid](#fieldgrid)
- [FieldInitializeCommand](#fieldinitializecommand)
- [FlightType](#flighttype)
- [FreeCellFinder](#freecellfinder)
- [HintEffect](#hinteffect)
- [HintService](#hintservice)
- [ICell](#icell)
- [ICellSubscriber](#icellsubscriber)
- [IChargingEffect](#ichargingeffect)
- [IChipChangeNotifier](#ichipchangenotifier)
- [IChipCollectionHandler](#ichipcollectionhandler)
- [IChipCollections](#ichipcollections)
- [IChipEvolutionProvider](#ichipevolutionprovider)
- [IChipEvolutionService](#ichipevolutionservice)
- [IChipFinder](#ichipfinder)
- [IChipFlyAnimation](#ichipflyanimation)
- [IChipLiftController](#ichipliftcontroller)
- [IChipMaterialController](#ichipmaterialcontroller)
- [IChipModule](#ichipmodule)
- [IChipModuleData](#ichipmoduledata)
- [IChipModuleRuntimeData](#ichipmoduleruntimedata)
- [IChipMovingLogic](#ichipmovinglogic)
- [IChipSortingLayer](#ichipsortinglayer)
- [IDragFeedback](#idragfeedback)
- [IEffect](#ieffect)
- [IEffectBlockingSettings](#ieffectblockingsettings)
- [IEffectHint](#ieffecthint)
- [IEffectMergeLight](#ieffectmergelight)
- [IEffectPowerBoosterJoin](#ieffectpowerboosterjoin)
- [IFieldEventHandler](#ifieldeventhandler)
- [IFieldGrid](#ifieldgrid)
- [IFieldInitializeCommand](#ifieldinitializecommand)
- [IFieldModuleData](#ifieldmoduledata)
- [IFreeCellFinder](#ifreecellfinder)
- [IHintService](#ihintservice)
- [IInputBlocker](#iinputblocker)
- [IMergeCamera](#imergecamera)
- [IMergeEventHandler](#imergeeventhandler)
- [IMergeLifetimeScope](#imergelifetimescope)
- [IPowerBoosterTarget](#ipowerboostertarget)
- [IShadowEffect](#ishadoweffect)
- [ISubEffect](#isubeffect)
- [ISubEffectOwner](#isubeffectowner)
- [IVisualField](#ivisualfield)
- [IVisualFieldElement](#ivisualfieldelement)
- [MergeCamera](#mergecamera)
- [MergeEventHandler](#mergeeventhandler)
- [MergeHintDragFeedback](#mergehintdragfeedback)
- [MergeHintEffect](#mergehinteffect)
- [MergeLightDragFeedback](#mergelightdragfeedback)
- [MergeLightEffect](#mergelighteffect)
- [NeighborChipFinder](#neighborchipfinder)
- [PowerBoosterCellSubscriber](#powerboostercellsubscriber)
- [PowerBoosterConnectorCellsHighlightEffect](#powerboosterconnectorcellshighlighteffect)
- [PowerBoosterJoinEffect](#powerboosterjoineffect)
- [PowerBoosterModule](#powerboostermodule)
- [RandomEvolutionProvider](#randomevolutionprovider)
- [RequireModuleDataAttribute](#requiremoduledataattribute)
- [SepiaEffect](#sepiaeffect)
- [ShadowEffect](#shadoweffect)
- [ShadowEffectRef](#shadoweffectref)
- [SortingLayerData](#sortinglayerdata)
- [SpriteDirectionalFadeController](#spritedirectionalfadecontroller)
- [SubEffect](#subeffect)
- [VisualField](#visualfield)
- [VisualFieldRef](#visualfieldref)

---

## AnimationSortingData

> - **Purpose**: Stores sorting order adjustment data when a specific animation trigger is executed
> - **Usage**: Configured in ChipSortingLayer to modify sorting orders during animation playback
> - **Notes**: TriggerName matches the animator trigger/state name
> - AdditionalOrder is applied during animation
#### Fields
- `+ AdditionalOrder: int`
- `+ TriggerName: string`
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

## BlockerEffectData

> - **Purpose**: Serializable entry mapping an effect ID to its prefab for blocker/overlay effects
> - **Usage**: Configured in ChipEffectsData.blockers array
> - integrates directly with EffectConsts IDs
> - **Notes**: effectId is updated via inspector combobox using [EffectSelector]
#### Fields
- `+- EffectId: int`
- `+- Prefab: GameObject`
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
- `- NeighborOffsetsCache: Dictionary<Vector2Int, Vector2Int[]>`
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
- `- GetNeighborOffsets(Vector2Int size): Vector2Int[]`
    - **Purpose**: Computes and caches relative grid offsets for cells surrounding a chip of given dimensions
    - **Usage**: Called when subscribing to neighbor cells to avoid redundant offset calculations across chips of identical sizes
    - **Params**: size - dimensions of the chip in grid cells
    - **Returns**: Array of relative Vector2Int coordinate offsets for all perimeter neighbors
    - **Notes**: Caches results in static dictionary to eliminate allocations during gameplay
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
- `+- EffectOfPrioritizingDestroying: IEffect`
- `+- FlightSettings: ChipFlightSettings`
- `+- IsAnimatorPaused: bool`
- `+- IsDestroying: bool`
- `+- IsFinishDestroyPending: bool`
- `+- IsMoving: bool`
- `+- IsSpawning: bool`
- `+- LiftController: IChipLiftController`
- `+- MaterialController: IChipMaterialController`
- `+- RuntimeData: ChipRuntimeData`
- `+- SortingLayer: IChipSortingLayer`
- `~ chipChangeNotifier: IChipChangeNotifier`
- `~ chipCollections: IChipCollections`
- `~ dontRepeatTrigger: bool`
- `~ effects: Dictionary<int, IEffect>`
- `~ effectsData: ChipEffectsData`
- `~ extraEffects: EffectRef[]`
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
- `- materialControllerRef: ChipMaterialControllerRef`
- `~ mergeEventHandler: IMergeEventHandler`
- `~ modules: IChipModule[]`
- `~ resolver: IObjectResolver`
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
- `+ CanInteract(ICell sourceCell, ICell targetCell): bool`
    - **Purpose**: Checks whether any attached module can handle an interaction between source and target cells.
    - **Usage**: Called by DraggableChipLogic during drag hover checks.
    - **Params**: sourceCell - cell of the dragged chip
    - targetCell - hovered destination cell
    - **Returns**: True if any attached module accepts the interaction
    - false otherwise.
- `+ CanMoving(): bool`
    - **Purpose**: Checks whether the chip can currently be moved by the player
    - **Usage**: Called by DraggableChipLogic before allowing drag operations
    - returns false if chip is locked
    - **Returns**: True if chip can be moved
    - false if movement is locked
    - **Notes**: Based on runtimeData.IsMoveLocked
    - prevents drag-and-drop when locked
- `+ CollectHintTargets(IChipCollections collections, ICollection<Chip> targets): void`
    - **Purpose**: Collects compatible hint targets from all attached modules.
    - **Usage**: Called by MergeHintDragFeedback when a drag starts.
    - **Params**: collections - active chip collections on the field
    - targets - output collection of compatible target chips
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
    - **Notes**: Stops active lift coroutine via LiftController
    - defers destruction and sets IsFinishDestroyPending to true if any attached IChipModule returns false from CanFinishDestroy().
- `+ GetEffect(int effectHash): IEffect`
    - **Purpose**: Retrieves an effect from the effects dictionary by its EffectConsts hash key
    - **Usage**: Call in methods that need to access a specific effect without type casting
    - **Params**: effectHash - One of EffectConsts values (e.g., EffectConsts.MoveLocked)
    - **Returns**: The IEffect instance or null if not found
    - **Notes**: Use GetEffect<T>() for type-safe access when specific interfaces are needed
- `+ GetEffect(int effectHash): T`
    - **Purpose**: Retrieves and casts an effect from the effects dictionary by its EffectConsts hash key
    - **Usage**: Call when you need a specific effect interface (e.g., IEffectGeneratorCharging)
    - **Params**: effectHash - One of EffectConsts values (e.g., EffectConsts.GeneratorCharging)
    - T - Target effect interface type
    - **Returns**: The effect cast to type T, or null if not found or cannot be cast
    - **Notes**: Handles null safely with null-conditional operator (?.) at callsite
- `+ Init(ChipData data, ChipRuntimeData runtimeData): void`
    - **Purpose**: Initializes the chip with data and sets up all required components, effects, and attached modules
    - **Usage**: Call after creating a chip instance to assign data and prepare effects
    - override in derived classes for custom initialization
    - **Params**: data - static ChipData configuration
    - runtimeData - optional persisted runtime state
    - **Notes**: Initializes MaterialController, effects via InitEffects and PostInitEffects, LiftController, and caches and initializes all attached IChipModule components
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
- `+ OnInteract(ICell sourceCell, ICell targetCell, DragInteractionResult& result): bool`
    - **Purpose**: Executes an interaction via the first matching attached module in component order.
    - **Usage**: Called by DraggableChipLogic on drag end when an interaction is pending.
    - **Params**: sourceCell - cell of the dragged chip
    - targetCell - destination cell
    - result - outcome reported by the handling module
    - **Returns**: True if interaction executed successfully
    - false otherwise.
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
- `+ PauseAnimator(bool pause): void`
    - **Purpose**: Pauses or resumes the animator by modifying animator.speed.
    - **Usage**: Called when flight delay requires freezing animation execution until flight starts.
    - **Params**: pause - if true, sets animator.speed to 0f
    - if false, resets animator.speed to 1f
    - **Notes**: Does nothing if animator reference is null.
- `+ RemoveEffect(int effectId): void`
    - **Purpose**: Removes a destroying effect from the chip after it reaches destruction threshold
    - **Usage**: Called from HandleDestroyingEffects when TryDestroyEffect returns true or from chip modules
    - **Params**: effectId - The unique hash/ID of the effect to remove
    - **Notes**: Deactivates the effect, removes it from dictionary, and updates the priority selection
- `+ ResetTrigger(string triggerName): void`
- `+ SendTrigger(AnimatorTrigger trigger, bool allowRepeat): void`
- `+ SendTrigger(string trigger, bool allowRepeat): void`
    - **Purpose**: Sends a trigger to the animator and synchronizes the 'Little' state.
    - **Usage**: Called when a chip state change requires an animation trigger (e.g., Generate, Recharge, Spawn).
    - **Params**: trigger - the animator trigger name to fire
    - allowRepeat - if true, bypasses the dontRepeatTrigger check
    - **Notes**: Automatically updates the 'Little' boolean in the animator based on the chip's blocking state. Fires animator.SetTrigger and immediately forces animator.Update(0f) to apply the first animation frame without frame delay. Prevents redundant triggers if dontRepeatTrigger is enabled. Notifies sortingLayer and broadcasts the trigger to all attached effects via NotifyEffectsOnAnimationTrigger.
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
    - **Purpose**: Deactivates all attached visual effects except those marked with IsSkipDestroy
    - **Usage**: Called from Destroy() to trigger visual deactivation effects before playing the chip destruction animation
- `- DeferredDestroyRoutine(): IEnumerator`
    - **Purpose**: Defers GameObject destruction by one frame to allow coroutines and event listeners to process before the object is destroyed.
    - **Usage**: Started by FinishDestroy when force destruction is requested.
- `~ GetModules(): IChipModule[]`
- `- HandleDestroyingEffects(): void`
    - **Purpose**: Processes damage/destruction progress on the highest-priority blocker effect when a neighboring interaction occurs
    - **Usage**: Called internally when neighboring chip interactions are handled, provided CanDestroyEffects is true
    - **Notes**: Skips processing if blockingState.CanDestroyEffects is false or if no prioritizing effect is present
    - increments NeighboringMergeCount and removes the effect if TryDestroyEffect returns true
- `~ HasState(string name): bool`
    - **Purpose**: Checks whether the animator contains a state with the specified name across any of its layers
    - **Usage**: Call before animator.Play to avoid 'State could not be found' warnings or errors
    - **Params**: name - name of the animator state to verify
    - **Returns**: True if state exists in any animator layer, false otherwise
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
    - **Notes**: Iterates runtimeData.EffectEnables and instantiates matching blocker effects from effectsData.BlockersDict
    - then instantiates OtherEffects and activates all enabled blocker effects
- `~ InstantiateEffect(GameObject prefab): T`
    - **Purpose**: Instantiates an effect prefab and initializes it with the current chip
    - **Usage**: Call during Init or whenever a new visual effect needs to be added to the chip
    - **Params**: prefab - The GameObject prefab containing the effect component
    - T - The type of the effect component (must implement IEffect)
    - **Returns**: The instantiated effect component of type T, or null if prefab is null
    - **Notes**: Automatically calls Init(this) on the instantiated effect
    - return value can be added to the effects list
    - T must be a class and implement IEffect
- `~ NotifyEffectsOnAnimationTrigger(string trigger): void`
    - **Purpose**: Broadcasts chip animation trigger events to all attached effects
    - **Usage**: Called from Chip.SendTrigger when an animation trigger is fired on the chip
    - **Params**: trigger - name of the trigger being fired on the chip
- `~ NotifyEffectsOnChangedCell(ICell sourceCell, ICell targetCell): void`
- `~ NotifyEffectsOnMovingStateChanged(bool isMoving): void`
    - **Purpose**: Broadcasts movement state changes to all attached effects
    - **Usage**: Called from SetMoving when a chip starts or ends visual movement
    - **Params**: isMoving - true if movement started, false if it ended
- `~ PostInitEffects(): void`
- `~ RemoveAllEffects(): void`
    - **Purpose**: Destroys GameObject instances of all attached effects except those marked with IsSkipDestroy and clears the effects collection
    - **Usage**: Called from OnDestroy() to ensure effect GameObjects are cleaned up when the chip GameObject is destroyed
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

> - **Purpose**: Provides a centralized collection of all active chips on the field, categorized by their data type. Also maintains filtered sub-collections for fast merge-target and movable chip lookups. Module-specific collections (e.g. FillableChipsByData for ContainerModule) are maintained by registered IChipCollectionHandlers.
> - **Usage**: Injected as IChipCollections
> - used to find all chips of a specific type or to iterate over all active chips. Call OnChipBlockingChanged when a chip's BlockingState or its cell's IsBlocked flag changes. Module handlers register themselves via RegisterHandler().
> - **Notes**: Updated by ChipFactory during creation and by Chip during destruction. MergeableChipsByData and MovableChipsByData are kept in sync via invalidation callbacks
> - never stale-read by polling. Registered IChipCollectionHandlers receive the same lifecycle events and maintain their own collections.
#### Fields
- `+- AllChipsByData: Dictionary<ChipData, List<Chip>>`
- `+- MergeableChipsByData: Dictionary<ChipData, List<Chip>>`
- `+- MovableChipsByData: Dictionary<ChipData, List<Chip>>`
- `- customCollections: Dictionary<ChipCollectionType, Dictionary<ChipData, List<Chip>>>`
- `- fieldGrid: IFieldGrid`
- `- handlers: List<IChipCollectionHandler>`
#### Methods
- `+ AddChip(Chip chip): void`
    - **Purpose**: Adds a chip to the collection, grouping it by its ChipData. Also inserts it into filtered sub-collections if it passes the merge/movable criteria. Notifies all registered handlers.
    - **Usage**: Called by ChipFactory immediately after a new chip is initialized
    - **Params**: chip - the chip instance to add
- `+ GetCollection(ChipCollectionType collectionType): Dictionary<ChipData, List<Chip>>`
    - **Purpose**: Returns the dictionary of active chips corresponding to the specified ChipCollectionType.
    - **Usage**: Called by UVS nodes like FindAnyChipNode or query logic to retrieve a chip sub-collection by enum.
    - **Params**: collectionType - enum value selecting All, Mergeable, Movable, or a module-registered collection
- `+ GetCollection(ChipData chipData, ChipCollectionType collectionType): List<Chip>`
    - **Purpose**: Returns the list of active chips matching the given ChipData from the specified collection, or null if none are present.
    - **Usage**: Called when querying chips by ChipData and collection type directly without manual dictionary lookups.
    - **Params**: chipData - key to search for
    - collectionType - enum value selecting the target collection
- `+ OnChipBlockingChanged(Chip chip): void`
    - **Purpose**: Re-evaluates a chip's presence in MergeableChipsByData and MovableChipsByData after its BlockingState or cell's IsBlocked flag changes. Notifies all registered handlers.
    - **Usage**: Called by Chip.RemoveEffect after a blocker effect is destroyed, and by LockedAreaManager when a cell's IsBlocked flag changes.
    - **Params**: chip - the chip whose eligibility may have changed
- `+ RegisterCollection(ChipCollectionType collectionType, Dictionary<ChipData, List<Chip>> collection): void`
    - **Purpose**: Registers a custom chip collection dictionary under the specified ChipCollectionType.
    - **Usage**: Called from IInitializable.Initialize() of a ChipModule's collection handler.
    - **Params**: collectionType - enum key for the collection
    - collection - dictionary maintained by the module
- `+ RegisterHandler(IChipCollectionHandler handler): void`
    - **Purpose**: Registers a module-level handler that receives chip lifecycle events.
    - **Usage**: Called from IInitializable.Initialize() of a ChipModule's collection handler.
    - **Params**: handler - the handler to add
    - must not be null
- `+ RemoveChip(Chip chip): void`
    - **Purpose**: Removes a chip from the collection and cleans up empty data entries. Also removes it from all filtered sub-collections. Notifies all registered handlers.
    - **Usage**: Called by Chip.Destroy just before the chip GameObject is destroyed
    - **Params**: chip - the chip instance to remove
- `+ UnregisterCollection(ChipCollectionType collectionType): void`
    - **Purpose**: Unregisters a previously registered custom collection.
    - **Usage**: Called from IDisposable.Dispose() of a ChipModule's collection handler.
- `+ UnregisterHandler(IChipCollectionHandler handler): void`
    - **Purpose**: Unregisters a previously registered handler.
    - **Usage**: Called from IDisposable.Dispose() of a ChipModule's collection handler.
- `- AddToDict(Dictionary<ChipData, List<Chip>> dict, ChipData key, Chip chip): void`
- `- IsChipMergeable(Chip chip): bool`
- `- IsChipMovable(Chip chip): bool`
- `- RemoveFromDict(Dictionary<ChipData, List<Chip>> dict, ChipData key, Chip chip): void`
---

## ChipCollectionType
**Inherits**: `Enum`
#### Fields
- `+ All: ChipCollectionType`
- `+ Fillable: ChipCollectionType`
- `+ Mergeable: ChipCollectionType`
- `+ Movable: ChipCollectionType`
- `+ value__: int`
---

## ChipData
**Inherits**: `ScriptableObject`
#### Fields
- `++ DestroyDuration: Nullable<float>`
- `++ Size: Vector2Int`
- `+ PrefabLink: GameObject`
- `+- ModuleDatas: IReadOnlyList<IChipModuleData>`
#### Methods
- `+ AddModuleData(IChipModuleData moduleData): void`
- `+ Clone(): ChipData`
    - **Purpose**: Creates a deep copy of this ChipData asset
    - **Usage**: Call at runtime to create a unique instance of ChipData that can be modified without affecting the original asset
    - **Returns**: A new ChipData instance with identical field values
- `+ CreateModuleData(): T`
- `+ GetModuleData(): T`
- `+ HasModuleData(): bool`
- `+ HasModuleData(Type type): bool`
- `- OnEnable(): void`
---

## ChipDataCollection
**Inherits**: `ScriptableObject`

> - **Purpose**: ScriptableObject catalog holding all ChipData references, with name-based dictionary caching for fast O(1) lookups at runtime
> - **Usage**: Injected or referenced by ChipFactory, ContainerModule, FieldInitializeCommand to retrieve ChipData by name
> - **Notes**: Cache is invalidated in Editor playmode changes and OnValidate to prevent stale data
#### Fields
- `++ Data: ChipData[]`
- `- cache: Dictionary<string, ChipData>`
#### Methods
- `+ ClearCache(): void`
    - **Purpose**: Clears and resets the internal dictionary lookup cache
    - **Usage**: Called on OnDisable and when exiting play mode
- `+ GetChipData(string chipName): ChipData`
    - **Purpose**: Retrieves ChipData by chipName from internal cache in O(1) time
    - **Usage**: Called by ChipSpawnData.GetChipData and various field/container logics to locate ChipData
    - **Params**: chipName - string key matching ChipData asset name
    - **Returns**: ChipData instance if found
    - null otherwise
- `+ RebuildCache(): void`
    - **Purpose**: Rebuilds internal dictionary cache mapping ChipData.name to ChipData instances
    - **Usage**: Called automatically on OnEnable, OnValidate, or when cache is null
- `- OnDisable(): void`
- `- OnEnable(): void`
- `- OnPlayModeStateChanged(PlayModeStateChange state): void`
- `- OnValidate(): void`
---

## ChipEffectsData

> - **Purpose**: Configures available blocker/overlay effects for a chip type via name-to-prefab mappings
> - **Usage**: Added to ChipData.moduleDatas
> - Chip.InitEffects iterates Blockers and instantiates only those whose EffectId is in runtimeData.EffectEnables
> - **Notes**: BlockersDict provides O(1) lookup by EffectId
> - built during serialization callbacks
#### Fields
- `+- Blockers: BlockerEffectData[]`
- `+- BlockersDict: Dictionary<int, BlockerEffectData>`
- `+- OtherEffects: EffectData[]`
- `+- OtherEffectsDict: Dictionary<int, EffectData>`
#### Methods
- `+ OnAfterDeserialize(): void`
- `+ OnBeforeSerialize(): void`
- `- UpdateDict(): void`
---

## ChipEvolutionExecutor

> - **Purpose**: Executes a pre-calculated EvolutionPlan: parent destruction, neighbor relocations, unified chip spawning, VFX, and provider confirmation.
> - **Usage**: Owned and invoked by ChipEvolutionService.Evolve after ChipEvolutionPlanner succeeds.
> - **Notes**: Stateless collaborator
> - passes EvolutionPlan.PlannedRelocations directly as IReadOnlyList without buffer copying.
#### Fields
- `- chipFactory: ChipFactory`
- `- chipMovingLogic: IChipMovingLogic`
- `- fieldGrid: IFieldGrid`
#### Methods
- `+ Execute(Chip chip, EvolutionPlan plan, EvolutionExecutionSettings settings): void`
    - **Purpose**: Executes parent destruction, neighbor relocations, result chip instantiation, MergeLight activation, and provider confirmation.
    - **Usage**: Called by ChipEvolutionService.Evolve on a valid non-destroying chip.
    - **Params**: chip - evolving parent chip
    - plan - pre-calculated EvolutionPlan
    - settings - grouped execution configuration
- `- CreateRuntimeDataInitializer(int[] blockerEffectIds): Action<ChipRuntimeData>`
- `- ResolveMultiChipFlightSettings(EvolutionSpawnFlightSettings spawnFlight, int delayIndex): ChipFlightSettings`
    - **Purpose**: Computes flight animation settings for an extra or delayed evolved chip.
    - **Usage**: Called by ResolveSpawnParameters for flying spawned chips using module flight settings.
    - **Params**: spawnFlight - module spawn flight settings
    - delayIndex - zero-based index used to stagger consecutive spawns
    - **Returns**: Configured ChipFlightSettings combining base flight parameters with initial and per-chip spawn delays.
    - **Notes**: Falls back to default flight duration and type with pauseAnimationOnDelay enabled when MultiChipFlightSettings duration is non-positive.
- `- ResolveSpawnParameters(int index, ChipData nextChipData, Vector2Int parentSize, bool isMultiChip, Vector3 parentCenter, Vector3 extraSpawnOrigin, EvolutionExecutionSettings settings): SpawnParameters`
    - **Purpose**: Unifies origin, flight settings, and animator trigger selection across growing, in-place, and flying spawned chips.
    - **Usage**: Called once per item in EvolutionPlan.Placements during SpawnPlacements.
    - **Params**: index - placement index
    - nextChipData - spawned chip configuration
    - parentSize - parent grid dimensions
    - isMultiChip - whether plan contains multiple chips
    - parentCenter - origin for primary or unified flight
    - extraSpawnOrigin - origin for secondary chips
    - settings - grouped execution configuration
    - **Returns**: Resolved SpawnParameters struct for ChipFactory.CreateChip and SendTrigger.
- `- SpawnPlacements(EvolutionPlan plan, Vector2Int parentSize, Vector3 chipWorldPos, Vector3 parentCenter, EvolutionExecutionSettings settings): void`
    - **Purpose**: Instantiates all planned result chips with resolved flight origins, staggered delays, initial blockers, and animator triggers.
    - **Usage**: Called by Execute after parent destruction and neighbor relocations.
    - **Params**: plan - pre-calculated non-empty EvolutionPlan
    - parentSize - parent chip grid size
    - chipWorldPos - parent chip world position
    - parentCenter - primary flight origin
    - settings - grouped execution configuration
    - **Notes**: Uses TargetCell of primary placement as extraSpawnOrigin when EvolutionPoint is null and KeepPrimaryChipInPlace is enabled.
- `- TryPlayMergeLight(Chip chip, bool playMergeLight): void`
---

## ChipEvolutionPlanner

> - **Purpose**: Plans chip evolution placements and neighbor relocations atomically without mutating field state.
> - **Usage**: Owned and invoked by ChipEvolutionService.CanEvolve to validate space and construct an EvolutionPlan.
> - **Notes**: Encapsulates all planning scratch buffers and clears them on exit
> - preserves the fast-path via IFreeCellFinder when no size growth, clamping, or exclusion overlap occurs.
#### Fields
- `- batchResultsBuffer: List<ValueTuple<ChipData, ICell>>`
- `- chipDataCollection: ChipDataCollection`
- `- chipDataListBuffer: List<ChipData>`
- `- chipMovingLogic: IChipMovingLogic`
- `- chipsToExcludeBuffer: List<Chip>`
- `- extraCellsToExcludeBuffer: HashSet<ICell>`
- `- extraCellsToFreeBuffer: HashSet<ICell>`
- `- fieldGrid: IFieldGrid`
- `- freeCellFinder: IFreeCellFinder`
#### Methods
- `+ TryCreatePlan(Chip chip, IChipEvolutionProvider provider, EvolutionPlan& plan, HashSet<ICell> cellsToExclude, HashSet<ICell> cellsToFree): EvolutionPlanResult`
    - **Purpose**: Evaluates whether a chip can evolve and constructs an EvolutionPlan with placements and optional relocations.
    - **Usage**: Called by ChipEvolutionService.CanEvolve before any parent or partner chips are destroyed.
    - **Params**: chip - evolving parent chip
    - provider - strategy yielding next chip configurations
    - plan - output EvolutionPlan
    - cellsToExclude - optional cells forbidden for placement and relocations
    - cellsToFree - optional external cells treated as freed
    - **Returns**: EvolutionPlanResult indicating Success, InvalidInput (no space notification), or NotEnoughSpace.
- `- ClearBuffers(): void`
- `- CollectChipsToExclude(Chip parentChip, HashSet<ICell> cellsToFree): void`
    - **Purpose**: Populates the exclusion list with the evolving parent chip and any chips occupying cells about to be freed.
    - **Usage**: Called before TryResolveCellPosition so absorbed or consumed chips do not block placement.
    - **Params**: parentChip - chip undergoing evolution
    - cellsToFree - optional set of external cells whose occupying chips will be destroyed
- `- OverlapsSet(Vector2Int cellPos, Vector2Int size, HashSet<ICell> cellSet): bool`
    - **Purpose**: Checks whether a rectangular footprint starting at cellPos overlaps any cell in the given set.
    - **Usage**: Called to detect whether preferred primary placement intersects cellsToExclude and requires relocation resolution.
    - **Params**: cellPos - top-left grid coordinates
    - size - footprint dimensions in cells
    - cellSet - set of cells to test against
    - **Returns**: True if at least one cell in the footprint belongs to cellSet
    - false otherwise.
- `- TryFindExtraChipPlacements(Chip parentChip, ICell parentMainCell, ICell primaryCell, Vector2Int primaryChipSize, List<ChipMoveAction> plannedRelocations, HashSet<ICell> cellsToExclude, HashSet<ICell> cellsToFree): bool`
    - **Purpose**: Plans atomic free-cell placements for secondary evolved chips while accounting for parent cleanup and neighbor relocations.
    - **Usage**: Called after resolving the primary chip cell whenever the provider yields multiple chips under relocation.
    - **Params**: parentChip - evolving parent chip
    - parentMainCell - parent anchor cell
    - primaryCell - resolved cell for primary result
    - primaryChipSize - footprint of primary result
    - plannedRelocations - neighbor moves already scheduled
    - cellsToExclude - external forbidden cells
    - cellsToFree - external freed cells
    - **Returns**: True if all extra chips fit into free cells
    - false otherwise.
    - **Notes**: Marks parent cells, cellsToFree, and relocation origin cells as freed while excluding primary result cells, cellsToExclude, and relocation destination cells.
- `- TryPlanStandard(Chip chip, IChipEvolutionProvider provider, List<ChipSpawnData> nextChips, HashSet<ICell> cellsToExclude, HashSet<ICell> cellsToFree, EvolutionPlan& plan): EvolutionPlanResult`
    - **Purpose**: Executes the fast-path planning branch via IFreeCellFinder without invoking neighbor relocation logic.
    - **Usage**: Called by TryCreatePlan when primary chip size fits without growth, clamping, or exclusion overlap.
    - **Params**: chip - parent chip
    - provider - evolution provider
    - nextChips - resolved spawn configs
    - cellsToExclude - forbidden cells
    - cellsToFree - external freed cells
    - plan - output EvolutionPlan without relocations
    - **Returns**: Success if all chips fit atomically via FindFreeCellsForChips, or NotEnoughSpace otherwise.
- `- TryPlanWithRelocation(Chip chip, ICell mainCell, ICell clampedCell, IChipEvolutionProvider provider, List<ChipSpawnData> nextChips, HashSet<ICell> cellsToExclude, HashSet<ICell> cellsToFree, EvolutionPlan& plan): EvolutionPlanResult`
    - **Purpose**: Plans primary placement via IChipMovingLogic relocation resolution and atomically places any extra chips around the resolved cell.
    - **Usage**: Called by TryCreatePlan when the primary evolved chip grows, is clamped to grid bounds, or overlaps cellsToExclude.
    - **Params**: chip - parent chip
    - mainCell - original parent anchor cell
    - clampedCell - bounds-clamped preferred cell
    - provider - evolution provider
    - nextChips - resolved spawn configs
    - cellsToExclude - forbidden cells
    - cellsToFree - external cells freed by caller
    - plan - output EvolutionPlan
    - **Returns**: Success with placements and planned relocations, or NotEnoughSpace if placement or extra-chip search fails.
    - **Notes**: Excludes parent chip and chips in cellsToFree from blocking relocation via chipsToExcludeBuffer
    - reads extra placements from batchResultsBuffer.
- `- TryResolveChipDatas(List<ChipSpawnData> nextChips): bool`
---

## ChipEvolutionService

> - **Purpose**: Centralized facade implementing IChipEvolutionService to coordinate evolution planning, execution, and space failure notifications across all chip modules.
> - **Usage**: Injected as singleton via VContainer into Merge2Module, TapEvolutionModule, UnlockAreaModule, WaitEvolutionModule, ContainerModule, and GeneratorModule.
> - **Notes**: Delegates spatial planning to ChipEvolutionPlanner and spawning/relocations to ChipEvolutionExecutor while keeping the DI surface unchanged.
#### Fields
- `- executor: ChipEvolutionExecutor`
- `- mergeEventHandler: IMergeEventHandler`
- `- planner: ChipEvolutionPlanner`
#### Methods
- `+ CanEvolve(Chip chip, IChipEvolutionProvider provider, EvolutionPlan& plan, bool notifyNotEnoughSpace, HashSet<ICell> cellsToExclude, HashSet<ICell> cellsToFree): bool`
    - **Purpose**: Checks whether the chip can evolve using the given evolution provider and available field space, pre-calculating placements and relocations.
    - **Usage**: Call to test if evolution can be performed
    - populates plan with pre-allocated cells and planned relocations, and raises space failure notification when enabled.
    - **Params**: chip - target parent chip
    - provider - evolution strategy provider
    - plan - out parameter populated with pre-calculated evolution plan
    - notifyNotEnoughSpace - whether to notify IMergeEventHandler when space is insufficient
    - cellsToExclude - optional cells to exclude from free cell and relocation search
    - cellsToFree - optional external cells to consider freed despite existing chips
    - **Returns**: True if evolution can proceed
    - false otherwise.
- `+ Evolve(Chip chip, EvolutionPlan plan, EvolutionExecutionSettings settings): void`
    - **Purpose**: Executes the full evolution pipeline using a pre-calculated EvolutionPlan via ChipEvolutionExecutor.
    - **Usage**: Called by chip modules only after a successful CanEvolve check.
    - **Params**: chip - evolving parent chip
    - plan - pre-calculated evolution plan from CanEvolve
    - settings - grouped execution configuration parameters
---

## ChipFactory

> - **Purpose**: Factory for creating and placing Chip instances on the field grid.
> - **Usage**: Call Init once during scene initialization (Merge2Initializer) then call CreateChip to instantiate chips from ChipData
> - pass runtimeDataInitializer to override runtime data before Init.
> - **Notes**: After SetChipInCell, fires IMergeEventHandler.RaiseChipCreated so scenario listeners and UVS nodes receive the creation event. Requires ChipData with a valid PrefabLink.
#### Fields
- `~ chipCollections: IChipCollections`
- `~ fieldGrid: IFieldGrid`
- `~ mergeEventHandler: IMergeEventHandler`
- `- OnChipCreatedBeforeNotify: Action<Chip, ICell>`
- `~ resolver: IObjectResolver`
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
    - **Notes**: Fires OnChipCreatedBeforeNotify before grid placement and IMergeEventHandler.RaiseChipCreated after placement.
- `+ CreateChip(Vector2Int cellPosition, ChipData chipData, Nullable<Vector3> parentWorldPosition, Action<ChipRuntimeData> runtimeDataInitializer, ChipFlightSettings flightSettings): Chip`
    - **Purpose**: Instantiates and initializes a new chip at the specified grid coordinates
    - **Usage**: Convenience wrapper around the cell-based CreateChip method
    - **Params**: cellPosition - target grid coordinates
    - chipData - configuration settings
    - parentWorldPosition - optional start position
    - runtimeDataInitializer - optional delegate
    - flightSettings - flight animation configuration
    - **Returns**: The initialized Chip instance
- `+ Init(IObjectResolver resolver, IFieldGrid fieldGrid, IChipCollections chipCollections, IMergeEventHandler mergeEventHandler): void`
    - **Purpose**: Wires all dependencies required by the factory before any chip can be created.
    - **Usage**: Called once by Merge2Initializer / IsoMergeInitializer at scene start. Must be called before CreateChip.
    - **Params**: resolver - VContainer object resolver for prefab instantiation
    - fieldGrid - grid for cell placement
    - chipCollections - registry of active chips
    - mergeEventHandler - optional
    - when null, merge events are silently skipped
    - **Notes**: mergeEventHandler defaults to null to stay backward-compatible with tests that build ChipFactory without a full lifetime scope.
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
- `+- PauseAnimationOnDelay: bool`
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

## ChipMaterialController
**Inherits**: `MonoBehaviour`

> - **Purpose**: Manages chip materials and MaterialPropertyBlock parameters for visual effects with batching preservation
> - **Usage**: Attach to chip prefab
> - initialized by Chip.Init
> - manipulated by ChipMaterialEffect instances
> - **Notes**: Uses sharedMaterial and MaterialPropertyBlock to prevent runtime material instancing leaks and restore dynamic batching
#### Fields
- `+- CurrentMaterial: Material`
- `+- DefaultMaterial: Material`
- `- activeEffectId: int`
- `- effectMaterials: Dictionary<int, Material>`
- `- propertyBlock: MaterialPropertyBlock`
- `- renderers: SpriteRenderer[]`
#### Methods
- `+ ApplyEffectMaterial(int effectId): void`
    - **Purpose**: Applies the registered shared material for the specified effect to all target renderers
    - **Usage**: Called when a ChipMaterialEffect activates
    - **Params**: effectId - unique effect hash/ID
- `+ ClearPropertyBlock(): void`
    - **Purpose**: Clears MaterialPropertyBlock on renderers to re-enable static batching
    - **Usage**: Called after animations complete or upon effect deactivation
- `+ Init(Chip chip): void`
    - **Purpose**: Initializes the controller with the chip reference and caches default materials
    - **Usage**: Called from Chip.Init
    - **Params**: chip - the parent Chip component
- `+ RegisterEffectMaterial(int effectId, Material material): void`
    - **Purpose**: Registers a custom shared material for a specific effect ID
    - **Usage**: Called from ChipMaterialEffect.Init during chip setup
    - **Params**: effectId - unique effect hash/ID
    - material - shared material instance for the effect
    - **Notes**: Null materials are ignored
- `+ RestoreDefaultMaterial(int effectId): void`
    - **Purpose**: Restores the default shared material when an active effect deactivates
    - **Usage**: Called when a ChipMaterialEffect deactivates
    - **Params**: effectId - unique effect hash/ID being deactivated
    - **Notes**: Only resets if the deactivated effect was the currently active material effect
    - clears property block
- `+ SetPropertyColor(int propertyId, Color value): void`
    - **Purpose**: Updates a Color shader property via MaterialPropertyBlock on all renderers
    - **Usage**: Called during color animations
    - **Params**: propertyId - shader property ID
    - value - Color value to set
- `+ SetPropertyFloat(int propertyId, float value): void`
    - **Purpose**: Updates a float shader property via MaterialPropertyBlock on all renderers
    - **Usage**: Called during parameter animations
    - **Params**: propertyId - shader property ID
    - value - float value to set
- `+ SetPropertyVector(int propertyId, Vector4 value): void`
    - **Purpose**: Updates a Vector4 shader property via MaterialPropertyBlock on all renderers
    - **Usage**: Called during vector/offset animations
    - **Params**: propertyId - shader property ID
    - value - Vector4 value to set
- `- ApplyPropertyBlock(): void`
- `- SetSharedMaterial(Material mat): void`
---

## ChipMaterialControllerRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## ChipMaterialEffect
**Inherits**: `Effect`

> - **Purpose**: Base class for chip visual effects that modify chip materials and shader parameters via MaterialPropertyBlock
> - **Usage**: Inherit from this class for effects that apply custom materials or animate shader properties on chips
> - **Notes**: Registers material with ChipMaterialController in Init, applies on Activate, restores default on Deactivate, and provides MPB helpers
#### Fields
- `+- EffectMaterial: Material`
- `~ isMaterialAnimating: bool`
#### Methods
- `+ Activate(Chip chip): bool`
    - **Purpose**: Activates the effect and switches the chip renderer material to the custom effect material
    - **Usage**: Called to activate this visual effect on the chip
    - **Params**: chip - target Chip component
    - **Returns**: True if activation succeeded and was not hidden by blocking state
- `+ Deactivate(Chip chip, bool force): void`
    - **Purpose**: Deactivates the effect and restores the chip's default material
    - **Usage**: Called to disable this visual effect
    - **Params**: chip - target Chip component
    - force - whether to force immediate state transition
- `+ Init(Chip chip, int effectId): void`
    - **Purpose**: Initializes the effect and registers its custom material with the chip's material controller
    - **Usage**: Called when the effect is initialized on a chip
    - **Params**: chip - parent Chip component
    - effectId - unique effect hash/ID
- `+ OnAnimationEnd(): void`
    - **Purpose**: Called via Unity Animation Event when activate or deactivate animation finishes
    - **Usage**: Hook as an Animation Event at the end of activate/deactivate animation clips
    - **Notes**: When active: stops animating and clears MaterialPropertyBlock for batching
    - **When inactive**: stops animating and restores default material
- `~ Update(): void`
- `~ UpdateMaterialProperties(float dt): void`
    - **Purpose**: Updates material and shader properties on each frame or on activation
    - **Usage**: Override in derived classes to animate shader parameters via MaterialPropertyBlock
    - **Params**: dt - delta time since last update (0f on initial activation)
---

## ChipMovingLogic

> - **Purpose**: Handles logic for moving chips, clamping multi-cell footprints to field bounds, and relocating conflicting chips.
> - **Usage**: Called when a chip is dragged or when an evolved/merged chip grows and needs more space.
> - **Notes**: Reuses internal scratch buffers to avoid allocations during CanChipMoving and TryResolveCellPosition checks.
#### Fields
- `- excludeChipSetBuffer: HashSet<Chip>`
- `- fieldGrid: IFieldGrid`
- `- freeCellFinder: IFreeCellFinder`
- `- movingChipsSetBuffer: HashSet<Chip>`
- `- NeighborOffsets: Vector2Int[]`
- `- occupiedCellsBuffer: List<ICell>`
- `- singleChipExcludeBuffer: Chip[]`
- `- unavailableCellsBuffer: HashSet<ICell>`
#### Methods
- `+ CanChipMoving(ICell leftTopCell, IReadOnlyList<Chip> chipsToExclude, Vector2Int chipSize, List`1& plannedRelocations, int maxRelocations, HashSet<ICell> cellsToExclude): bool`
    - **Purpose**: Checks if a chip can move to a target and if conflicting chips can be relocated.
    - **Usage**: Internal check before performing actual relocation
    - also used by evolution, merge, and scenario logic.
    - **Params**: leftTopCell - target top-left cell
    - chipsToExclude - chips to ignore during collision checks
    - chipSize - dimensions of the chip
    - plannedRelocations - output list of moves to perform or null if none needed
    - maxRelocations - maximum allowed relocations
    - cellsToExclude - optional cells forbidden for both target placement and relocated chips
    - **Returns**: True if a valid state is possible for all chips involved.
    - **Notes**: Uses reusable scratch buffers to avoid allocations
    - skips allocating plannedRelocations when no conflicting chips occupy the area
    - clears scratch buffers before returning.
- `+ ChipMoving(ICell overCell, ICell leftTopCell, ICell sourceCell, ChipFlightSettings flightSettings): void`
    - **Purpose**: Orchestrates the move of a chip to a new position, potentially relocating others
    - **Usage**: Call when a drag operation ends at a specific cell
    - **Params**: overCell - the cell the mouse is over
    - leftTopCell - the target top-left cell for the chip
    - sourceCell - the original cell of the chip
    - flightSettings - flight animation configuration for the movement
    - **Notes**: If relocation is impossible, the chip snaps back to its source position
- `+ ChipsRelocate(ICell leftTopCell, ICell sourceCell, IReadOnlyList<ChipMoveAction> plannedRelocations, ChipFlightSettings flightSettings): void`
    - **Purpose**: Executes the planned relocations of chips on the field
    - **Usage**: Final step of the relocation process
    - performs atomic field updates
    - **Params**: leftTopCell - destination for the primary chip
    - sourceCell - original spot of the primary chip
    - plannedRelocations - read-only list of pre-calculated chip moves (may be null or empty)
    - flightSettings - flight animation configuration for the relocations
    - **Notes**: Uses indexed loops over IReadOnlyList to avoid enumerator boxing.
- `+ ClampCellToField(ICell preferredCell, Vector2Int chipSize): ICell`
    - **Purpose**: Clamps an anchor cell so a chip footprint of chipSize stays within field grid bounds.
    - **Usage**: Called before TryResolveCellPosition when a chip grows or spawns near field edges.
    - **Params**: preferredCell - desired top-left anchor cell
    - chipSize - target dimensions in cells
    - **Returns**: Valid clamped anchor cell within field bounds, or null if preferredCell is null or the footprint does not fit in the field.
- `+ TryResolveCellPosition(ICell preferredCell, IReadOnlyList<Chip> chipsToExclude, Vector2Int chipSize, ICell& resolvedCell, List`1& plannedRelocations, HashSet<ICell> cellsToExclude): bool`
    - **Purpose**: Finds the best valid cell for a chip around preferredCell, prioritizing zero neighbor relocations and otherwise minimizing relocation count.
    - **Usage**: Called when a chip grows, was clamped to grid bounds, or overlaps excluded cells.
    - **Params**: preferredCell - preferred anchor cell
    - chipsToExclude - chips ignored during collision checks
    - chipSize - target dimensions
    - resolvedCell - chosen anchor cell
    - plannedRelocations - calculated neighbor moves or null when zero relocations are needed
    - cellsToExclude - optional forbidden cells for target and relocated chips
    - **Returns**: True if a valid placement and relocation plan was found
    - false otherwise.
    - **Notes**: Tests preferredCell first, then 8 NeighborOffsets (orthogonal before diagonal), skipping blocked cells and returning early on zero relocations.
- `- ClearCheckBuffers(): void`
- `- PopulateOccupiedCellsInArea(Vector2Int cellPos, Vector2Int chipSize, IReadOnlyList<Chip> chipsToExclude): void`
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
- `+ EffectEnables: List<int>`
    - **Purpose**: List of blocker effect IDs (from EffectConsts/EffectConsts.Blockers) that should be active on this chip
    - **Usage**: Populated from ChipSpawnData.BlockerEffectIds during level load or at runtime
    - queried by Chip.InitEffects and UpdateVisual to determine which blocker effects to instantiate and activate
    - **Notes**: Serialized directly for save/load support
- `+- ModuleRuntimeDatas: List<IChipModuleRuntimeData>`
- `- effectDestroyingDataArray: EffectDestroyingRuntimeData[]`
#### Methods
- `+ GetModuleRuntimeData(): T`
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

> - **Purpose**: Custom PropertyDrawer for fields decorated with [ChipSelector], rendering a searchable dropdown or text field to pick chips from the active ChipDataCollection.
> - **Usage**: Applied automatically in Inspector for string or ChipSpawnData fields marked with [ChipSelector].
> - **Notes**: Queries EditorContext.ActiveChipDataCollection to populate available chip options.
#### Fields
- `- s_dropdownState: AdvancedDropdownState`
#### Methods
- `+ OnGUI(Rect position, SerializedProperty property, GUIContent label): void`
- `- HandleDragAndDrop(Rect dropRect, SerializedProperty property): void`
- `- OpenSearchDropdown(Rect buttonRect, ChipDataCollection collection, SerializedProperty property): void`
---

## ChipSortingLayer
**Inherits**: `MonoBehaviour`

> - **Purpose**: Implements movement-based, effect-based, animation-based, and manual override sorting order adjustments for chip renderers
> - **Usage**: Attach to chip prefab
> - initialized by Chip.Init
> - handles visual depth changes during dragging, active effects, animation playback, or manual overrides
> - **Notes**: Supports multiple renderers, ensuring each one maintains its relative depth while moving, reacting to active effects, playing animations, or when overridden
#### Fields
- `+- AnimationsSortingData: AnimationSortingData[]`
- `+- EffectsSortingData: EffectSortingData[]`
- `+- IsSortingOrderOverridden: bool`
- `+- MovingOrderOffset: int`
- `+- SortingLayers: List<SortingLayerData>`
- `+- SortingOrderOverride: Nullable<int>`
- `- activeEffects: List<int>`
- `- animationCoroutine: Coroutine`
- `- animationOffsets: Dictionary<string, int>`
- `- animator: Animator`
- `- currentAnimationOffset: int`
- `- effectOffsets: Dictionary<int, int>`
- `- isAnimationActive: bool`
- `- isMoving: bool`
#### Methods
- `+ AddSortingLayer(SpriteRenderer renderer): void`
    - **Purpose**: Adds a SpriteRenderer to dynamically managed sorting layers, caching its current sorting order and applying active offsets
    - **Usage**: Called by effects during Init to register their visual renderers with the chip sorting hierarchy
    - **Params**: renderer - The SpriteRenderer to register and manage
    - **Notes**: Null-safe
    - ignores duplicates
    - immediately synchronizes renderer sorting order to current chip sorting offset
- `+ ClearSortingOrderOverride(): void`
    - **Purpose**: Clears any active sorting order offset override and recalculates sorting order based on current states
    - **Usage**: Call when returning chip to standard sorting behavior after override was applied
    - **Notes**: Restores order based on isMoving, activeEffects, or animation states
- `+ Init(): void`
    - **Purpose**: Caches the initial sorting orders of all renderers and prepares effect and animation offset lookups
    - **Usage**: Called from Chip.Init before any movement or effect activation occurs
    - **Notes**: Required for restoring correct sorting order after movement ends, effects deactivate, or animations complete
- `+ OnAnimationTrigger(string triggerName): void`
    - **Purpose**: Handles sorting order adjustment during animation playback triggered by SendTrigger
    - **Usage**: Called from Chip.SendTrigger when an animation trigger is fired
    - **Params**: triggerName - name of the animation trigger/state
    - **Notes**: Stops any active animation sorting coroutine and resets offset
    - inspects current and next AnimatorStateInfo for matching state to retrieve animation duration
    - applies offset and restores order when animation completes
- `+ RemoveSortingLayer(SpriteRenderer renderer): void`
    - **Purpose**: Removes a SpriteRenderer from managed sorting layers and restores its cached sorting order
    - **Usage**: Called by effects during OnDestroy to unregister their visual renderers from the chip sorting hierarchy
    - **Params**: renderer - The SpriteRenderer to unregister
    - **Notes**: Restores CachedOrder before removing
    - null-safe
- `+ SetEffectActive(int effectId, bool value): void`
    - **Purpose**: Updates the active state of an effect for sorting order adjustments
    - **Usage**: Called when an effect is activated or deactivated on the chip
    - **Params**: effectId - ID of the effect
    - value - true if active, false if deactivated
    - **Notes**: Logs a warning if multiple configured effects are active simultaneously
    - applies last activated effect
- `+ SetMoving(bool value): void`
    - **Purpose**: Adjusts sorting orders of all renderers based on movement state
    - **Usage**: Called from Chip.SetMoving(true/false) when movement starts or ends
    - **Params**: value - true if starting movement (increases order), false if stopping (restores order)
    - **Notes**: Uses MovingOrderOffset
    - when moving, movement offset takes priority over active effects and animations
- `+ SetSortingOrderOverride(int offset): void`
    - **Purpose**: Sets a custom sorting order offset override that takes highest priority over all other sorting states
    - **Usage**: Call when a chip needs forced visual depth elevation, e.g. custom sequences or modal display
    - **Params**: offset - additional order offset added to CachedOrder for each renderer
    - **Notes**: Overrides moving, effect, and animation offsets until ClearSortingOrderOverride is called
- `- AnimationSortingCoroutine(float duration): IEnumerator`
- `- ApplySortingOrder(): void`
- `- OnDisable(): void`
---

## ChipSpawnData

> - **Purpose**: Contains data required to spawn a chip (ID and initial blocker/effect states)
> - **Usage**: Used within FieldData.CellData, ChipContainerData, and ChipMerge2Data to define chip spawn configurations
#### Fields
- `+ BlockerEffectIds: int[]`
    - **Purpose**: Array of blocker effect IDs to activate on this chip when loaded from level data
    - **Usage**: Populated by Level Editor
    - consumed by FieldInitializeCommand to populate ChipRuntimeData.EffectEnables
    - **Notes**: Values correspond to EffectConsts.Blockers constants (e.g. MoveLockedEffect=103)
- `+ ChipId: string`
    - **Purpose**: Unique identifier for the chip type
    - **Usage**: Should correspond to a chip name in the ChipDataCollection
#### Methods
- `+ GetChipData(ChipDataCollection collection): ChipData`
    - **Purpose**: Retrieves ChipData reference from ChipDataCollection
    - **Usage**: Call instead of manual ChipDataCollection lookup to optimize performance
    - **Params**: collection - ChipDataCollection to search if cache is missing or stale
    - **Returns**: ChipData instance or null if not found
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
- `+~ CanCharge: bool`
- `+~ CanDestroyEffects: bool`
- `+~ CanGenerate: bool`
- `+~ CanReceiveModifiers: bool`
- `+- HideEffectIds: HashSet<int>`
- `+~ IsLittleChip: bool`
- `- appliedBlocks: HashSet<IEffectBlockingSettings>`
#### Methods
- `+ ApplyBlock(IEffectBlockingSettings blockSettings): void`
- `+ RemoveBlock(IEffectBlockingSettings blockSettings): bool`
    - **Purpose**: Removes an active block setting from this aggregate and recalculates the combined state
    - **Usage**: Called when an effect or locked area is deactivated or removed from a chip
    - **Params**: blockSettings - the blocking settings instance to remove
    - **Returns**: True if the blocking settings were successfully removed from applied blocks
    - false otherwise
- `- Recalculate(): void`
- `~ Reset(): void`
---

## ConstantEvolutionProvider

> - **Purpose**: Chip evolution provider strategy that always returns a fixed constant list of chips.
> - **Usage**: Configured inside ChipTapEvolutionData or ChipUnlockAreaData via SerializeReference selector.
> - **Notes**: Zero allocation strategy returning the internal list directly without caching overhead.
#### Fields
- `+ Chips: List<ChipSpawnData>`
- `+- LastResult: List<ChipSpawnData>`
#### Methods
- `+ Confirm(): void`
- `+ GetEvolutionChips(): List<ChipSpawnData>`
- `+ Init(): void`
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
- `~ chipMovingLogic: IChipMovingLogic`
- `~ currentInteractionChip: Chip`
- `~ currentMergableCell: ICell`
- `~ dragFeedbacks: List<IDragFeedback>`
- `~ draggableChip: Chip`
- `~ draggableTransform: Transform`
    - **Purpose**: Cached transform of the chip being dragged
    - **Usage**: Internal field
    - cached for performance during drag operations
    - **Notes**: Set in OnDragStart for performance optimization during drag
- `~ fieldGrid: IFieldGrid`
- `- filterCellsBuffer: List<ICell>`
- `~ inputBlocker: IInputBlocker`
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
- `+ OnDragEnd(ICell overCell, ICell targetAnchorCell): void`
    - **Purpose**: Handles logic when drag ends, including merge, swap, and cleanup.
    - **Usage**: Call when drag operation finishes, passing the cell under the chip and the anchor cell.
    - **Params**: overCell - cell under chip on drop
    - targetAnchorCell - anchor cell for placement
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
- `- InjectInputBlocker(IObjectResolver resolver): void`
- `- MoveToWorldPosition(Vector3 worldPosition): void`
- `~ NotifyNeighborsOfInteraction(ICell targetCell, Vector2Int targetChipSize): void`
    - **Purpose**: Notifies neighboring chips that an interaction has occurred on the target cell.
    - **Usage**: Called from OnDragEnd after a successful chip interaction.
    - **Params**: targetCell - main target cell of the interaction
    - targetChipSize - size of target chip prior to interaction
- `- ResetCurrentMergable(): void`
- `- ResetDragState(): void`
- `- UpdateInteractionState(ICell sourceCell, ICell targetCell): void`
    - **Purpose**: Checks if any chip module can handle interaction between sourceCell and target area.
    - **Usage**: Call before attempting interaction on drag end.
    - **Params**: sourceCell - cell chip is dragged from
    - targetCell - cell chip is hovered over
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
- `+- Owners: HashSet<ISubEffectOwner>`
- `+- SortingSpriteRenderers: IReadOnlyList<SpriteRenderer>`
- `+- SubEffects: ISubEffect[]`
- `~ animator: Animator`
- `~ autoSize: AutoSizeType`
- `~ cellSpacing: Vector2`
- `~ deactivateOnMove: bool`
- `~ delayCoroutines: Dictionary<int, Coroutine>`
- `~ destroyAfterActivate: bool`
- `~ destroyDelayAfterActivate: float`
- `~ dontRepeatTrigger: bool`
- `~ durationMovePositionDependingOnSize: Vector2`
- `~ effectId: int`
- `~ lastTriggerName: string`
- `~ movePositionDependingOnSize: Transform`
- `~ parentType: EffectParentType`
- `~ restoreStateAfterMove: bool`
    - **Purpose**: Tracks whether this effect was active before movement or had activation deferred during movement, so it can be restored when movement finishes
    - **Usage**: Set in Activate (if moving) or OnMovingStateChanged (if active when movement starts)
    - cleared and queried in OnMovingStateChanged when movement stops
- `~ sendAnimatorTrigger: bool`
#### Methods
- `+ Activate(Chip chip): bool`
    - **Purpose**: Activates the effect on the specified chip
    - **Usage**: Call when the chip or non-chip visual owner is activated
    - trigger 'Activate' animation if configured
    - **Params**: chip - the chip this effect belongs to
    - **Returns**: True if the effect was activated
    - false if skipped because effectId is in chip.BlockingState.HideEffectIds or if activation is deferred due to chip movement with deactivateOnMove enabled
    - **Notes**: Pass null only for non-chip visuals
    - when chip is provided, applies BlockingSettings and respects HideEffectIds. If deactivateOnMove is true and chip is moving, activation is deferred until movement finishes (restoreStateAfterMove set to true).
- `+ ActivateByOwner(Chip chip, ISubEffectOwner owner): bool`
    - **Purpose**: Activates the effect on the specified chip with an owner reference
    - **Usage**: Called by sub-effect owners during ActivateSubEffects to track active owners
    - activates effect if not already active
    - **Params**: chip - the chip this effect belongs to
    - owner - the owner initiating activation
    - **Returns**: True if the effect is or became active
- `+ Deactivate(Chip chip, bool force): void`
    - **Purpose**: Deactivates the effect on the specified chip
    - **Usage**: Call when the chip is deactivated or disabled
    - trigger 'Deactivate' animation if configured
    - **Params**: chip - the chip this effect belongs to
    - force - if true, forces immediate animation state change (plays 'Deactivate' state at end time)
- `+ DeactivateByOwner(Chip chip, ISubEffectOwner owner, bool force): void`
    - **Purpose**: Deactivates the effect on the specified chip with an owner reference
    - **Usage**: Called by sub-effect owners during DeactivateSubEffects
    - deactivates effect only when all owners have deactivated
    - **Params**: chip - the chip this effect belongs to
    - owner - the owner initiating deactivation
    - force - whether to force immediate deactivation
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
- `+ OnChipAnimationTrigger(string triggerName): void`
    - **Purpose**: Called on the effect when an animation trigger is sent to the owner chip
    - **Usage**: Override in derived effect classes to react to owner chip animation triggers
    - **Params**: triggerName - name of the trigger sent to the chip
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
    - deactivates only active effects on move start and restores them when movement stops using restoreStateAfterMove
- `+ OnTap(): void`
    - **Purpose**: Called when the chip is tapped
    - **Usage**: Override in derived classes to implement custom tap reaction visual effects
- `+ SendTrigger(string triggerName, bool allowRepeat): void`
    - **Purpose**: Sends a custom animation trigger to the effect's animator
    - **Usage**: Call to trigger custom animations on the effect
    - used for custom interactions like move-locked feedback
    - **Params**: triggerName - name of the animator trigger to activate
    - allowRepeat - if true, bypasses the dontRepeatTrigger check for this call
    - **Notes**: Safely handles null animator
    - allows effects to respond to chip-specific events beyond standard Activate/Deactivate
- `+ SkipDestroy(): void`
    - **Purpose**: Marks the effect to skip automatic destruction when its owner chip is destroyed, and detaches it from the chip transform hierarchy
    - **Usage**: Called when an effect needs to outlive the chip, e.g. a merge visual effect played on a destroyed chip
- `+ TryDestroyEffect(Chip chip, EffectDestroyingSettings settings, EffectDestroyingRuntimeData destroyingData): bool`
    - **Purpose**: Evaluates whether the effect has reached its destruction threshold based on neighboring merge count and transitions through additional states
    - **Usage**: Called from Chip.HandleDestroyingEffects when effectOfPrioritizingDestroying processes a neighboring merge event
    - **Params**: chip - owner chip
    - settings - destruction configuration (additional states, priority)
    - destroyingData - runtime counter tracking merge events
    - **Returns**: True if the effect should be removed (merge count exceeded additional states)
    - false if still alive
    - **Notes**: When not yet destroyed, sends a trigger from AdditionalStates to the effect animator
    - when all additional states are exhausted, deactivates the effect
- `- ActivateSubEffectCoroutine(Chip chip, ISubEffect se): IEnumerator`
- `~ ActivateSubEffects(Chip chip): void`
    - **Purpose**: Activates all configured sub-effects, either immediately or after their respective activation delays
    - **Usage**: Called when the parent effect successfully activates in Effect.Activate
    - **Params**: chip - The owner chip activating this effect
    - **Notes**: Passes this effect as owner
    - stops any running delay coroutine before activating
- `~ ApplyAutoSize(Vector2Int chipSize): void`
    - **Purpose**: Applies auto-sizing logic to the effect based on the autoSize setting and chip dimensions
    - **Usage**: Called internally during Init
    - scales the effect proportionally or non-proportionally
    - **Params**: chipSize - the grid dimensions of the chip (e.g., Data.Size)
- `- DeactivateSubEffectCoroutine(Chip chip, ISubEffect se): IEnumerator`
- `~ DeactivateSubEffects(Chip chip, bool force): void`
    - **Purpose**: Deactivates all configured sub-effects, either immediately or after their respective deactivation delays
    - **Usage**: Called when the parent effect deactivates in Effect.Deactivate
    - **Params**: chip - The owner chip deactivating this effect
    - force - Whether to force immediate deactivation ignoring delays
    - **Notes**: Passes this effect as owner
    - when force is true, delays are ignored and sub-effects are deactivated immediately with force
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
- `~ HasState(string name): bool`
    - **Purpose**: Checks whether the animator contains a state with the specified name across any of its layers
    - **Usage**: Call before animator.Play to avoid 'State could not be found' warnings or errors
    - **Params**: name - name of the animator state to verify
    - **Returns**: True if state exists in any animator layer, false otherwise
- `~ HasTrigger(string name): bool`
- `~ InitSubEffects(Chip chip): void`
    - **Purpose**: Initializes sub-effects configuration for the owner chip
    - **Usage**: Called during Effect.Init to set up sub-effects state
    - **Params**: chip - The owner chip that hosts the effects
- `~ OnDestroy(): void`
    - **Purpose**: Cleans up effect sub-effect coroutines and unregisters managed sorting sprite renderers from the owner chip's sorting layer
    - **Usage**: Called by Unity lifecycle when the effect GameObject or component is destroyed
- `~ ResetTrigger(string triggerName): void`
    - **Purpose**: Resets an animation trigger on the effect's animator
    - **Usage**: Internal helper to clear opposing triggers when switching states
    - **Params**: triggerName - name of the trigger to reset
    - **Notes**: Safely handles null animator
- `~ StopSubEffectsCoroutines(): void`
    - **Purpose**: Stops all currently running sub-effect delay coroutines
    - **Usage**: Called during cleanup and destruction to prevent orphan coroutines
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
- `+- CanCharge: bool`
- `+- CanDestroyEffects: bool`
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
> - Extra subclass (101+) is for optional blocker/overlay effects configured through ChipEffectsData
> - nameToId dictionary enables serialized string-to-int resolution. Supports extension via EffectDefinitionAttribute.
#### Fields
- `+ CellHighlight: int`
- `+ ContainerRequirements: int`
- `+ GeneratorCharged: int`
- `+ GeneratorCharging: int`
- `+ GlowEffect: int`
- `+ MergeAvailable: int`
- `+ MergeHint: int`
- `+ MergeLight: int`
- `+ None: int`
- `+ PBoosterConnectorCells: int`
- `+ PBoosterJoin: int`
- `+ SepiaEffect: int`
- `+ ShadowEffect: int`
- `+ TapHint: int`
- `- blockerNameToId: Dictionary<string, int>`
- `- nameToId: Dictionary<string, int>`
#### Methods
- `+ GetAllEffectsEditorOnly(): Dictionary<string, int>`
- `+ GetBlockerEffectsEditorOnly(): Dictionary<string, int>`
- `+ GetNameByIdEditorOnly(int id): string`
---

## EffectData

> - **Purpose**: Extended effect data include activation/deactivation triggers
> - **Usage**: Used in OtherEffects list for chips that need effects with specific lifecycle control
> - **Notes**: Supports custom activation/deactivation logic during chip initialization
#### Fields
- `+- ActivateOnStart: bool`
- `+- DeactivateOnStart: bool`
- `+- EffectId: int`
- `+- Prefab: GameObject`
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

> - **Purpose**: Configures destruction priority and progressive animation triggers for neighboring merge events on an effect
> - **Usage**: Assigned to Effect prefabs
> - processed by Effect.TryDestroyEffect during neighboring merge events
> - **Notes**: AdditionalStates contains sequential trigger names sent to the animator on each neighboring merge before final destruction
#### Fields
- `+- AdditionalStates: string[]`
- `+- Priority: int`
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

## EffectSortingData

> - **Purpose**: Stores sorting order adjustment data when a specific effect is active
> - **Usage**: Configured in ChipSortingLayer to modify sorting orders based on active visual effects
> - **Notes**: EffectId uses [EffectSelector] for inspector dropdown selection
#### Fields
- `+ AdditionalOrder: int`
- `+ EffectId: int`
---

## EvolutionBehaviorSettings

> - **Purpose**: Groups behavioral flags controlling evolution VFX, empty destruction mode, and space-unavailable notifications.
> - **Usage**: Serialized inside EvolutionExecutionSettings.Behavior on chip modules.
#### Fields
- `+ ForceDestroyOnEmpty: bool`
    - **Purpose**: Whether to force destroy the parent chip immediately without Mecanim animation when no next chips are produced.
    - **Usage**: True for generator evolution into null
    - false for other modules.
- `+ NotifyNotEnoughSpace: bool`
    - **Purpose**: Whether to notify IMergeEventHandler when evolution cannot proceed due to insufficient space.
    - **Usage**: Passed to IChipEvolutionService.CanEvolve
    - defaults to true and set to false for modules like WaitEvolutionModule that silently wait for space.
- `+ PlayMergeLight: bool`
    - **Purpose**: Whether to activate the IEffectMergeLight effect on the evolving chip if present.
    - **Usage**: True for tap, unlock area, and wait evolution
    - false for container, generator, and Merge2.
- `+- Default: EvolutionBehaviorSettings`
---

## EvolutionExecutionSettings

> - **Purpose**: Parameters for controlling chip evolution execution in IChipEvolutionService.
> - **Usage**: Construct and pass to IChipEvolutionService.Evolve() or serialize on chip modules using grouped Triggers, SpawnFlight, and Behavior sections.
#### Fields
- `+ Behavior: EvolutionBehaviorSettings`
- `+ SpawnFlight: EvolutionSpawnFlightSettings`
- `+ Triggers: EvolutionTriggerSettings`
- `+- Default: EvolutionExecutionSettings`
---

## EvolutionPlacementItem

> - **Purpose**: Encapsulates placement information for a single chip spawned during evolution.
> - **Usage**: Item within EvolutionPlan.Placements list used by IChipEvolutionService.Evolve.
#### Fields
- `+- ChipData: ChipData`
- `+- SpawnData: ChipSpawnData`
- `+- TargetCell: ICell`
---

## EvolutionPlan

> - **Purpose**: Pre-calculated evolution plan containing target placement cells and spawn data.
> - **Usage**: Created by IChipEvolutionService.CanEvolve and passed to IChipEvolutionService.Evolve.
#### Fields
- `+- IsEmpty: bool`
- `+- Placements: IReadOnlyList<EvolutionPlacementItem>`
- `+- PlannedRelocations: IReadOnlyList<ChipMoveAction>`
- `+- Provider: IChipEvolutionProvider`
---

## EvolutionPlanResult
**Inherits**: `Enum`
#### Fields
- `+ InvalidInput: EvolutionPlanResult`
- `+ NotEnoughSpace: EvolutionPlanResult`
- `+ Success: EvolutionPlanResult`
- `+ value__: int`
---

## EvolutionSpawnFlightSettings

> - **Purpose**: Groups flight origin, spawn stagger delays, and multi-chip placement animation parameters for evolution.
> - **Usage**: Serialized inside EvolutionExecutionSettings.SpawnFlight on chip modules.
#### Fields
- `+ DelayBeforeFirstSpawn: float`
    - **Purpose**: Initial flight delay in seconds before the first chip spawns.
    - **Usage**: Configured on module prefabs when evolution waits for destruction animation before spawning.
- `+ DelayBetweenSpawns: float`
    - **Purpose**: Delay interval in seconds between consecutive chip spawns.
    - **Usage**: Used to stagger multi-chip spawn animations.
- `+ EvolutionPoint: Transform`
    - **Purpose**: World transform used as the origin point for new chip flight animations. If null, the parent chip transform position is used.
    - **Usage**: Assigned from module prefab evolution point reference.
- `+ KeepPrimaryChipInPlace: bool`
    - **Purpose**: When true and multiple chips spawn, keeps the primary chip at its resolved cell with SpawnTrigger or GrowTrigger while flying extra chips with MultiSpawnTrigger.
    - **Usage**: True for Merge2Module
    - false for standard multi-chip evolution modules.
- `+ MultiChipFlightSettings: ChipFlightSettings`
    - **Purpose**: Optional flight animation settings applied to extra or flying spawned chips when Duration is positive.
    - **Usage**: Set to ChipFlightSettings.ChipRelocate for Merge2Module
    - leave Duration non-positive to fall back to ChipFlightSettings.Default with pauseAnimationOnDelay enabled.
- `+- Default: EvolutionSpawnFlightSettings`
---

## EvolutionTriggerSettings

> - **Purpose**: Groups Mecanim animator triggers dispatched during chip evolution lifecycle events.
> - **Usage**: Serialized inside EvolutionExecutionSettings.Triggers on chip modules.
#### Fields
- `+ DestroyTrigger: AnimatorTrigger`
    - **Purpose**: Mecanim animation trigger sent to the parent chip when destruction begins upon successful evolution.
    - **Usage**: Set to trigger appropriate destruction animation such as TapEvolutionDestroy, WaitEvolutionDestroy, or EvolutionDestroy.
- `+ EmptyDestroyTrigger: AnimatorTrigger`
    - **Purpose**: Mecanim animation trigger sent to the parent chip when the evolution provider produces no next chips.
    - **Usage**: Defaults to AnimatorTrigger.Destroy.
- `+ GrowTrigger: AnimatorTrigger`
    - **Purpose**: Mecanim animation trigger sent to the primary spawned chip when its grid size increases compared to the parent chip.
    - **Usage**: Defaults to AnimatorTrigger.MergeGrow
    - falls back to MergeGrow when left at Idle.
- `+ MultiSpawnTrigger: AnimatorTrigger`
    - **Purpose**: Mecanim animation trigger sent to newly spawned chips when spawning with flight animation.
    - **Usage**: Defaults to AnimatorTrigger.Generate.
- `+ SpawnTrigger: AnimatorTrigger`
    - **Purpose**: Mecanim animation trigger sent to newly spawned chips when spawning a primary chip in place.
    - **Usage**: Set to module-specific spawn trigger such as TapEvolutionSpawn, WaitEvolutionSpawn, EvolutionSpawn, or Merge.
- `+- Default: EvolutionTriggerSettings`
---

## FieldData
**Inherits**: `ScriptableObject`
#### Fields
- `+- BlockedCells: Vector2Int[]`
- `+- Cells: CellData[]`
- `+- ChipDataCollection: ChipDataCollection`
- `+- FieldSize: Vector2Int`
- `+- ModuleDatas: IReadOnlyList<IFieldModuleData>`
- `+- VisualField: IVisualField`
#### Methods
- `+ AddModuleData(IFieldModuleData moduleData): void`
    - **Purpose**: Adds a module data instance to moduleDatas collection
    - **Usage**: Called to attach configuration data for a new field feature
    - **Params**: moduleData - instance of IFieldModuleData to add
- `+ GetModuleData(): T`
    - **Purpose**: Finds and returns the first registered module data instance of type T
    - **Usage**: Call to retrieve typed configuration for a specific field feature
    - **Returns**: The module data instance if found
    - otherwise default (null)
- `+ HasModuleData(): bool`
    - **Purpose**: Checks whether a module data instance of type T is registered in moduleDatas
    - **Usage**: Call to check feature availability on this field before querying its configuration
- `+ HasModuleData(Type type): bool`
    - **Purpose**: Checks whether a module data instance matching the specified Type is registered in moduleDatas
    - **Usage**: Used by editors or reflection tools to check for specific module data types
    - **Params**: type - Type implementing IFieldModuleData to check for
- `+ InsertModuleData(int index, IFieldModuleData moduleData): void`
    - **Purpose**: Inserts a module data instance at the specified index in moduleDatas
    - **Usage**: Called when restoring module data order during Undo
    - **Params**: index - insertion index
    - moduleData - instance to insert
- `+ RemoveModuleData(IFieldModuleData moduleData): bool`
    - **Purpose**: Removes a specific module data instance from moduleDatas
    - **Usage**: Called when detaching a module or deleting its configuration
    - **Params**: moduleData - instance of IFieldModuleData to remove
    - **Returns**: True if removed
    - false if not found or null
- `+ RemoveModuleData(): bool`
    - **Purpose**: Removes the first module data instance of type T from moduleDatas
    - **Usage**: Called when removing a module configuration by type
    - **Returns**: True if an instance of type T was found and removed
    - otherwise false
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
- `+ SetVisualField(MonoBehaviour target): void`
    - **Purpose**: Editor-only setter for the level visual field reference
    - **Usage**: Called by level editing tools when assigning visuals that implement IVisualField
    - **Params**: target - MonoBehaviour component implementing IVisualField on the visual field prefab
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
- `~ inputBlocker: IInputBlocker`
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
- `- InjectInputBlocker(IObjectResolver resolver): void`
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
- `- resolver: IObjectResolver`
#### Methods
- `+ CreateCells(FieldData fieldData): void`
    - **Purpose**: Creates and initializes the grid of cells dynamically for the requested ICell component type.
    - **Usage**: Call during field initialization to create all cells.
    - **Params**: fieldData - configuration for the field including size and blocked cell coordinates.
    - **Notes**: Centers cells around origin
    - creates Cell GameObjects with AddComponent<TCell>() and VContainer injection, assigning BlockedCell for blocked coordinates.
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
- `+ GetCells(Vector2Int cellPos, Vector2Int size, HashSet<ICell> result, bool clear): void`
    - **Purpose**: Fills a caller-provided HashSet with cells in a rectangular area without clearing unless requested
    - **Usage**: Use to aggregate occupied or excluded cells for multi-cell chips into hash sets without allocations
    - **Params**: cellPos - top-left position
    - size - area dimensions
    - result - target hash set
    - clear - if true, clears the set first
- `+ GetCellWorldPosition(Vector2Int cellPos): Vector3`
    - **Purpose**: Calculates the world position of the cell or chip center at the given grid coordinates, handling multi-cell anchors and chip sizes
    - **Usage**: Called by tutorial components and UI helpers to snap finger/hand indicators to grid cells or chips
    - **Params**: cellPos - the grid position of the target cell
    - **Returns**: Vector3 world position of the cell or chip center, or Vector3.zero if invalid grid coordinates
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
- `- ClearCells(ICell cell): void`
    - **Purpose**: Clears all chips from cells occupied by the specified cell's chip.
    - **Usage**: Call to remove a chip and its multi-cell occupancy from the field.
    - **Params**: cell - the main or secondary cell to clear.
- `- Expecto.MergeBase.IFieldGrid.get_transform(): Transform`
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
- `- mergeCamera: IMergeCamera`
- `- resolver: IObjectResolver`
- `- visualFieldInstance: GameObject`
- `- vScriptingFieldInstance: GameObject`
#### Methods
- `+ CreateField(): void`
    - **Purpose**: Creates the field grid and sets up camera based on field data.
    - **Usage**: Call first during initialization to create the field structure.
    - **Notes**: Validates FieldData before proceeding
    - sets camera size and creates cells of specified type.
- `+ CreateLevelVisual(): void`
    - **Purpose**: Creates the level visual prefab and initializes field-wide visual components
    - **Usage**: Call after field and locked-area runtime state are initialized
    - **Notes**: Only initializes the root IVisualField
    - concrete visual fields are responsible for their child visual components
- `+ CreateVScripting(): void`
    - **Purpose**: Creates the VScripting field prefab if present in FieldData.
    - **Usage**: Called during initialization after visual field setup.
    - **Notes**: Instantiates ScenarioFieldData.VScriptingField under field parent and sets position/rotation to zero.
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
- `- batchExcludedCellsBuffer: HashSet<ICell>`
- `- fieldGrid: IFieldGrid`
- `- priorityPositionsBuffer: List<Vector2Int>`
#### Methods
- `+ FindFreeCellsForChips(Vector2Int parentPos, Vector2Int parentSize, IReadOnlyList<ChipData> chipsToPlace, Chip chipToIgnore, List`1& resultsBuffer, HashSet<ICell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove, bool onlyAround, HashSet<ICell> cellsToFree, int startIndex): bool`
    - **Purpose**: Finds free cells for placing multiple chips atomically without GC allocations.
    - **Usage**: Call to locate valid free cells for a batch of chips (e.g. during tap evolution). Returns true if cells for all chips were successfully allocated into resultsBuffer.
    - **Params**: parentPos - origin area position
    - parentSize - origin area size
    - chipsToPlace - list of chip data
    - chipToIgnore - chip instance to ignore
    - resultsBuffer - ref list to receive (ChipData, ICell) pairs
    - cellsToExclude - optional external cells to exclude
    - chipsToPotentiallyMove - optional movable chips
    - onlyAround - whether to limit search around origin
    - cellsToFree - optional external cells treated as freed
    - startIndex - starting index in chipsToPlace.
    - **Returns**: True if placement is found for all chips from startIndex onward, false otherwise.
- `+ FindNearestFreeCell(Vector2Int parentPos, Vector2Int parentSize, Vector2Int childChipSize, Chip chipToPlace, HashSet<ICell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove, bool onlyAround, IReadOnlyList<Vector2Int> priorityPositions, HashSet<ICell> cellsToFree): ICell`
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
- `- IsAreaCompletelyFree(Vector2Int cellPos, Vector2Int chipSize, Chip chipToPlace, HashSet<ICell> excludedCellsSet, HashSet<Chip> movingChipsSet, HashSet<ICell> cellsToFree): bool`
---

## HintEffect
**Inherits**: `Effect`

> - **Purpose**: Visual effect handler for displaying hints on chips.
> - **Usage**: Attached to chip prefabs and referenced via IEffectHint contract
> - plays a trigger 'Hint' on the animator.
> - **Notes**: Implements IEffectHint to provide a Hint method that fires a visual hint
> - suppresses visual hints when IHintService.CanShowHint() is false unless force=true.
#### Fields
- `- delay: float`
- `- hintCoroutine: Coroutine`
- `- hintService: IHintService`
#### Methods
- `+ Deactivate(Chip chip, bool force): void`
- `+ Hint(bool force): void`
    - **Purpose**: Triggers the 'Hint' animator trigger to show a visual hint, optionally forcing it to display immediately.
    - **Usage**: Called automatically when a chip is spawned or moved, or on demand to display hints. Pass force=true to trigger immediately.
    - **Notes**: Stops any active hint coroutine. If hints are suppressed (IHintService.CanShowHint() == false) and force is false, suppresses the hint. If force is true, triggers immediately
    - otherwise, schedules a new coroutine with the configured delay.
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
- `- HintCoroutine(): IEnumerator`
---

## HintService

> - **Purpose**: Default implementation of IHintService that tracks active hint suppressors using a HashSet.
> - **Usage**: Registered as a Singleton in Merge2LifetimeScope.
> - **Notes**: CanShowHint() returns true only when the suppressor set is empty.
#### Fields
- `- suppressors: HashSet<object>`
#### Methods
- `+ AddSuppressor(object owner): void`
- `+ CanShowHint(): bool`
- `+ RemoveSuppressor(object owner): void`
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

## IChipCollectionHandler

> - **Purpose**: Extension point for ChipCollections. Each ChipModule that needs to maintain its own chip grouping implements this interface and registers itself via IChipCollections.RegisterHandler().
> - **Usage**: Implement in a ChipModule assembly
> - register via IInitializable calling chipCollections.RegisterHandler(this). Core never references concrete module types directly.
> - **Notes**: Called synchronously from ChipCollections lifecycle methods. Implementations must be allocation-free in hot paths.
#### Methods
- `+ OnChipAdded(Chip chip): void`
- `+ OnChipBlockingChanged(Chip chip): void`
- `+ OnChipRemoved(Chip chip): void`
---

## IChipCollections
#### Fields
- `+- AllChipsByData: Dictionary<ChipData, List<Chip>>`
- `+- MergeableChipsByData: Dictionary<ChipData, List<Chip>>`
- `+- MovableChipsByData: Dictionary<ChipData, List<Chip>>`
#### Methods
- `+ AddChip(Chip chip): void`
- `+ GetCollection(ChipCollectionType collectionType): Dictionary<ChipData, List<Chip>>`
- `+ GetCollection(ChipData chipData, ChipCollectionType collectionType): List<Chip>`
- `+ OnChipBlockingChanged(Chip chip): void`
- `+ RegisterCollection(ChipCollectionType collectionType, Dictionary<ChipData, List<Chip>> collection): void`
- `+ RegisterHandler(IChipCollectionHandler handler): void`
- `+ RemoveChip(Chip chip): void`
- `+ UnregisterCollection(ChipCollectionType collectionType): void`
- `+ UnregisterHandler(IChipCollectionHandler handler): void`
---

## IChipEvolutionProvider

> - **Purpose**: Interface for chip evolution selection strategies used in evolution mechanics.
> - **Usage**: Polymorphic field inside ChipTapEvolutionData and ChipUnlockAreaData, initialized and queried by TapEvolutionModule and UnlockAreaModule.
> - **Notes**: Implementations should be sealed classes with [Serializable] for polymorphic serialization via SerializeReference.
#### Fields
- `+- LastResult: List<ChipSpawnData>`
#### Methods
- `+ Confirm(): void`
- `+ GetEvolutionChips(): List<ChipSpawnData>`
- `+ Init(): void`
---

## IChipEvolutionService

> - **Purpose**: Centralized service managing chip evolution planning, cell space finding, neighbor relocations, parent destruction, and result chip instantiation.
> - **Usage**: Injected via VContainer as IChipEvolutionService into chip modules including Merge2Module, TapEvolutionModule, UnlockAreaModule, WaitEvolutionModule, ContainerModule, and GeneratorModule.
> - **Notes**: Coordinates ChipEvolutionPlanner, ChipEvolutionExecutor, and IMergeEventHandler.
#### Methods
- `+ CanEvolve(Chip chip, IChipEvolutionProvider provider, EvolutionPlan& plan, bool notifyNotEnoughSpace, HashSet<ICell> cellsToExclude, HashSet<ICell> cellsToFree): bool`
    - **Purpose**: Checks whether the chip can evolve using the given evolution provider and available field space, pre-calculating placements and relocations.
    - **Usage**: Call before initiating evolution
    - computes EvolutionPlan with assigned cells and optionally notifies if space is insufficient.
    - **Params**: chip - the parent chip undergoing evolution
    - provider - strategy yielding target ChipSpawnData configurations
    - plan - out parameter populated with pre-calculated evolution plan
    - notifyNotEnoughSpace - whether to raise RaiseNotEnoughSpaceForEvolution on space failure
    - cellsToExclude - optional cells to exclude from free cell and relocation search
    - cellsToFree - optional external cells to consider freed despite existing chips
    - **Returns**: True if space is available and evolution can proceed
    - false otherwise
- `+ Evolve(Chip chip, EvolutionPlan plan, EvolutionExecutionSettings settings): void`
    - **Purpose**: Executes the chip evolution using a pre-calculated EvolutionPlan: destroys the parent chip, relocates neighbors, spawns next chips, plays effects, and confirms the provider.
    - **Usage**: Called only after a successful CanEvolve check.
    - **Params**: chip - the parent chip undergoing evolution
    - plan - pre-calculated evolution plan from CanEvolve
    - settings - grouped execution configuration for triggers, flight, and behavior
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

## IChipMaterialController

> - **Purpose**: Interface for managing chip materials and shader property blocks with dynamic batching optimization
> - **Usage**: Implemented by ChipMaterialController
> - accessed by Chip and ChipMaterialEffect instances
#### Fields
- `+- CurrentMaterial: Material`
- `+- DefaultMaterial: Material`
#### Methods
- `+ ApplyEffectMaterial(int effectId): void`
    - **Purpose**: Applies a registered custom material to the chip's renderers
    - **Usage**: Called when a material-based effect activates
    - **Params**: effectId - unique effect hash/ID whose material should be applied
- `+ ClearPropertyBlock(): void`
    - **Purpose**: Clears the MaterialPropertyBlock on renderers to fully restore static batching
    - **Usage**: Called when shader animation finishes and the chip enters static state
- `+ Init(Chip chip): void`
    - **Purpose**: Initializes the controller with the parent chip reference and caches default material
    - **Usage**: Called automatically during Chip.Init
    - **Params**: chip - the parent Chip component
- `+ RegisterEffectMaterial(int effectId, Material material): void`
    - **Purpose**: Registers an effect's custom material by effectId
    - **Usage**: Called during effect initialization (Init)
    - **Params**: effectId - unique effect hash/ID
    - material - custom shared material for the effect
- `+ RestoreDefaultMaterial(int effectId): void`
    - **Purpose**: Restores the chip's default material when an effect deactivates
    - **Usage**: Called when a material-based effect deactivates
    - **Params**: effectId - unique effect hash/ID being deactivated
- `+ SetPropertyColor(int propertyId, Color value): void`
    - **Purpose**: Sets a Color parameter in the MaterialPropertyBlock without instantiating materials
    - **Usage**: Called during shader color transitions
    - **Params**: propertyId - shader property ID
    - value - Color value to assign
- `+ SetPropertyFloat(int propertyId, float value): void`
    - **Purpose**: Sets a float parameter in the MaterialPropertyBlock without instantiating materials
    - **Usage**: Called during shader parameter animations (e.g. progress, dissolve)
    - **Params**: propertyId - shader property ID (from Shader.PropertyToID)
    - value - float value to assign
- `+ SetPropertyVector(int propertyId, Vector4 value): void`
    - **Purpose**: Sets a Vector4 parameter in the MaterialPropertyBlock without instantiating materials
    - **Usage**: Called during shader vector/position animations
    - **Params**: propertyId - shader property ID
    - value - Vector4 value to assign
---

## IChipModule

> - **Purpose**: Interface for all modular components attached to a Chip GameObject that extend its capabilities
> - **Usage**: Implement this interface for modular components (e.g. GeneratorModule, ContainerModule, PowerBoosterModule, Merge2Module)
> - Chip automatically queries and manages components implementing this interface
> - **Notes**: Supports runtime initialization, visual updates, tap/drag input, cell relocation, and drag-and-drop interactions
#### Methods
- `+ CanFinishDestroy(): bool`
    - **Purpose**: Checks whether this module allows the chip to finish destruction and delete its GameObject.
    - **Usage**: Called by Chip.FinishDestroy
    - if any module returns false, destruction is deferred.
    - **Returns**: True if module is ready for chip destruction, false to defer FinishDestroy.
- `+ CanInteract(ICell sourceCell, ICell targetCell): bool`
    - **Purpose**: Checks whether this module can perform a drag-and-drop interaction between source and target cells.
    - **Usage**: Called by Chip.CanInteract in component order during drag hovering.
    - **Params**: sourceCell - cell containing the chip being dragged
    - targetCell - hovered destination cell
    - **Returns**: True if this module can handle the interaction
    - false otherwise.
- `+ CollectHintTargets(IChipCollections collections, ICollection<Chip> targets): void`
    - **Purpose**: Collects compatible target chips on the field for drag hint highlighting.
    - **Usage**: Called by Chip.CollectHintTargets during drag start feedback.
    - **Params**: collections - active field chip collections
    - targets - output collection to populate with compatible target chips
- `+ DestroyModule(): void`
- `+ Init(Chip chip, ChipData data, ChipRuntimeData runtimeData): void`
- `+ InitRuntimeData(ChipData data, ChipRuntimeData runtimeData): void`
- `+ OnChangedCell(ICell sourceCell, ICell targetCell): void`
- `+ OnDrag(Vector2 position, ICell anchorCell): void`
- `+ OnDragEnd(): void`
- `+ OnDragStart(): void`
- `+ OnEffectRemoved(int effectId): void`
- `+ OnInteract(ICell sourceCell, ICell targetCell, DragInteractionResult& result): bool`
    - **Purpose**: Executes a drag-and-drop interaction between source and target cells.
    - **Usage**: Called by Chip.OnInteract when dragging ends over a valid target.
    - **Params**: sourceCell - cell containing the chip being dragged
    - targetCell - destination cell
    - result - classification of the interaction outcome
    - **Returns**: True if the interaction executed successfully
    - false otherwise.
- `+ OnTap(): void`
- `+ UpdateVisual(): void`
---

## IChipModuleData

> - **Purpose**: Base interface for polymorphic module configuration data stored in ChipData.moduleDatas
> - **Usage**: Implement on module configuration classes (e.g., ChipGeneratorData, ChipContainerData)
---

## IChipModuleRuntimeData

> - **Purpose**: Marker interface for module runtime data objects held inside ChipRuntimeData
> - **Usage**: Implement on specific module runtime state classes (e.g., ChipGeneratorRuntimeData, ChipContainerRuntimeData)
---

## IChipMovingLogic

> - **Purpose**: Interface for chip movement, boundary clamping, and relocation resolution logic.
> - **Usage**: Use to manage chip movement on the field, clamp multi-cell footprints inside grid bounds, and resolve placements with minimal neighbor relocations.
> - **Notes**: Implemented by ChipMovingLogic.
#### Methods
- `+ CanChipMoving(ICell leftTopCell, IReadOnlyList<Chip> chipsToExclude, Vector2Int chipSize, List`1& plannedRelocations, int maxRelocations, HashSet<ICell> cellsToExclude): bool`
    - **Purpose**: Checks if a chip can move to a target and plans relocations if necessary.
    - **Usage**: Call before moving a chip to validate if the move is possible.
    - **Params**: leftTopCell - target anchor cell
    - chipsToExclude - chips to ignore during collision checks
    - chipSize - size of the chip being moved
    - plannedRelocations - output list of moves to perform or null when zero relocations are needed
    - maxRelocations - maximum allowed number of relocations
    - cellsToExclude - optional cells forbidden for both target placement and relocated chips
    - **Returns**: True if the move and all necessary relocations are possible.
- `+ ChipMoving(ICell overCell, ICell leftTopCell, ICell sourceCell, ChipFlightSettings flightSettings): void`
    - **Purpose**: Moves a chip to a new cell position, handling potential relocations.
    - **Usage**: Call when a chip is dropped or needs to be moved programmatically.
    - **Params**: overCell - cell under the chip
    - leftTopCell - target anchor cell
    - sourceCell - original anchor cell
    - flightSettings - flight animation configuration for the movement
- `+ ChipsRelocate(ICell leftTopCell, ICell sourceCell, IReadOnlyList<ChipMoveAction> plannedRelocations, ChipFlightSettings flightSettings): void`
    - **Purpose**: Executes pre-planned chip relocations.
    - **Usage**: Call after CanChipMoving or TryResolveCellPosition returns true to apply the relocations.
    - **Params**: leftTopCell - destination for the primary chip
    - sourceCell - original spot of the primary chip
    - plannedRelocations - read-only list of calculated moves (may be null or empty)
    - flightSettings - flight animation configuration for the relocations
- `+ ClampCellToField(ICell preferredCell, Vector2Int chipSize): ICell`
    - **Purpose**: Clamps an anchor cell so a chip footprint of chipSize stays within field grid bounds.
    - **Usage**: Call before relocation resolution when a multi-cell chip could overflow the right or bottom grid borders.
    - **Params**: preferredCell - desired top-left anchor cell
    - chipSize - target chip dimensions in cells
    - **Returns**: Valid clamped anchor cell within field bounds, or null if preferredCell is null or the chip exceeds field dimensions.
- `+ TryResolveCellPosition(ICell preferredCell, IReadOnlyList<Chip> chipsToExclude, Vector2Int chipSize, ICell& resolvedCell, List`1& plannedRelocations, HashSet<ICell> cellsToExclude): bool`
    - **Purpose**: Finds the best valid cell for a chip around preferredCell, prioritizing zero relocations and otherwise minimizing relocation count.
    - **Usage**: Call when a growing, boundary-clamped, or overlapping chip requires relocation-aware placement.
    - **Params**: preferredCell - preferred anchor cell
    - chipsToExclude - chips ignored during collision checks
    - chipSize - target footprint dimensions
    - resolvedCell - output chosen anchor cell
    - plannedRelocations - output calculated neighbor moves or null when none needed
    - cellsToExclude - optional cells forbidden for both target placement and relocated chips
    - **Returns**: True if a valid placement and relocation plan was found
    - false otherwise.
---

## IChipSortingLayer

> - **Purpose**: Defines the contract for managing chip sorting layers, dynamic renderer registration, movement-based, effect-based, animation-based, and manual override sorting adjustments
> - **Usage**: Implemented by ChipSortingLayer
> - used by Chip and effects to register renderers and adjust visual depth during movement, active effects, animation playback, or manual overrides
> - **Notes**: Supports dynamic additions and removals of renderers per chip
> - manual override takes precedence over other sorting offsets
#### Fields
- `+- AnimationsSortingData: AnimationSortingData[]`
- `+- EffectsSortingData: EffectSortingData[]`
- `+- IsSortingOrderOverridden: bool`
- `+- MovingOrderOffset: int`
- `+- SortingLayers: List<SortingLayerData>`
- `+- SortingOrderOverride: Nullable<int>`
#### Methods
- `+ AddSortingLayer(SpriteRenderer renderer): void`
- `+ ClearSortingOrderOverride(): void`
- `+ Init(): void`
- `+ OnAnimationTrigger(string triggerName): void`
- `+ RemoveSortingLayer(SpriteRenderer renderer): void`
- `+ SetEffectActive(int effectId, bool value): void`
- `+ SetMoving(bool value): void`
- `+ SetSortingOrderOverride(int offset): void`
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
> - **Notes**: Includes core activation, deactivation, trigger, and cell change handling methods. Inherits sub-effect and parent-effect handling from ISubEffectOwner.
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
- `+ OnChipAnimationTrigger(string triggerName): void`
    - **Purpose**: Called on the effect when an animation trigger is sent to the owner chip
    - **Usage**: Invoked automatically by Chip.SendTrigger to notify active effects of chip animation state transitions
    - **Params**: triggerName - name of the trigger sent to the chip
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
> - consumed by game logic (DraggableChipLogic, Merge2Module, GeneratorModule, PowerBoosterModule) and Chip.HandleDestroyingEffects
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
- `+- CanCharge: bool`
- `+- CanDestroyEffects: bool`
- `+- CanGenerate: bool`
- `+- CanReceiveModifiers: bool`
- `+- HideEffectIds: HashSet<int>`
- `+- IsLittleChip: bool`
---

## IEffectHint

> - **Purpose**: Interface for hint visual effects, used to trigger visual hints on demand.
> - **Usage**: Implemented by visual effects that can show hints, typically using animation triggers.
> - **Notes**: Provides a Hint method to fire a visual hint trigger.
#### Methods
- `+ Hint(bool force): void`
---

## IEffectMergeLight

> - **Purpose**: Dedicated IEffect contract for MergeLight effects that support a 'little' (small) flash variant.
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
- `+- transform: Transform`
#### Methods
- `+ CreateCells(FieldData fieldData): void`
    - **Purpose**: Initializes the grid with cells of specified type
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
- `+ GetCells(Vector2Int cellPos, Vector2Int size, HashSet<ICell> result, bool clear): void`
    - **Purpose**: Fills a caller-provided HashSet with cells in a rectangular area without clearing unless requested
    - **Usage**: Use to aggregate occupied or excluded cells for multi-cell chips into hash sets without allocations
    - **Params**: cellPos - top-left position
    - size - area dimensions
    - result - target hash set
    - clear - if true, clears the set first
- `+ GetCellWorldPosition(Vector2Int cellPos): Vector3`
    - **Purpose**: Calculates the world position of the cell or chip center at the given grid coordinates, handling multi-cell anchors and chip sizes
    - **Usage**: Called by tutorial components and UI helpers to snap finger/hand indicators to grid cells or chips
    - **Params**: cellPos - the grid position of the target cell
    - **Returns**: Vector3 world position of the cell or chip center, or Vector3.zero if invalid grid coordinates
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
    - **Purpose**: Creates the cells and logical field grid structure with cells of specified type.
    - **Usage**: Called first during initialization.
- `+ CreateLevelVisual(): void`
    - **Purpose**: Creates the level visual prefab and initializes field-wide visual components.
    - **Usage**: Called after logical cell states are ready.
- `+ CreateVScripting(): void`
    - **Purpose**: Creates the VScripting field prefab and attaches it to the field hierarchy.
    - **Usage**: Called after visual field initialization.
- `+ InitCamera(): void`
    - **Purpose**: Initializes the merge camera parameters (position, bounds, viewport scale) relative to the field bounds.
    - **Usage**: Called as the final step of field setup to ensure correct camera framing.
- `+ LoadChips(): void`
    - **Purpose**: Populates the cells on the field grid with initial chips from cell data.
    - **Usage**: Called after logical and visual field structures are set up.
---

## IFieldModuleData

> - **Purpose**: Base interface for polymorphic module configuration data stored in FieldData.moduleDatas
> - **Usage**: Implement on field module configuration classes (e.g., LockedAreasFieldModuleData, VScriptingFieldModuleData)
#### Methods
- `+ Validate(FieldData fieldData, List<string> errors): void`
---

## IFreeCellFinder

> - **Purpose**: Interface for finding free cells on the field grid.
> - **Usage**: Injected into components that need to spawn or relocate chips.
> - **Notes**: Provides logic for nearest free cell search with spiral expansion.
#### Methods
- `+ FindFreeCellsForChips(Vector2Int parentPos, Vector2Int parentSize, IReadOnlyList<ChipData> chipsToPlace, Chip chipToIgnore, List`1& resultsBuffer, HashSet<ICell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove, bool onlyAround, HashSet<ICell> cellsToFree, int startIndex): bool`
    - **Purpose**: Finds free cells for placing multiple chips atomically.
    - **Usage**: Call to find suitable cells for a list of chips, returning true only if all chips can be placed.
    - **Params**: parentPos - search origin
    - parentSize - size of origin area
    - chipsToPlace - list of chip data to place
    - chipToIgnore - chip instance to ignore (e.g. parent)
    - resultsBuffer - ref list to store (ChipData, TargetCell) pairs
    - cellsToExclude - set of cells to skip
    - chipsToPotentiallyMove - set of chips allowed to move
    - onlyAround - if true, limits search to immediate neighborhood
    - cellsToFree - optional external cells to consider freed despite existing chips
    - startIndex - starting index in chipsToPlace to allow slicing without copying.
    - **Returns**: True if all chips in the list from startIndex onward can be assigned free cells, false otherwise.
- `+ FindNearestFreeCell(Vector2Int parentPos, Vector2Int parentSize, Vector2Int childChipSize, Chip chipToPlace, HashSet<ICell> cellsToExclude, HashSet<Chip> chipsToPotentiallyMove, bool onlyAround, IReadOnlyList<Vector2Int> priorityPositions, HashSet<ICell> cellsToFree): ICell`
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

## IHintService

> - **Purpose**: Contract for querying whether visual hints can be shown on chips and managing hint suppressors.
> - **Usage**: Injected into HintEffect, ContainerHintEffect, and TutorialManager.
> - **Notes**: Hints are suppressed whenever one or more suppressors are registered via AddSuppressor.
#### Methods
- `+ AddSuppressor(object owner): void`
    - **Purpose**: Adds a suppressor owner that prevents visual hints from displaying.
    - **Usage**: Called when a tutorial or modal begins (e.g. TutorialManager.Activate).
    - **Params**: owner - The object representing the suppression source.
- `+ CanShowHint(): bool`
    - **Purpose**: Checks whether hints are allowed to be shown.
    - **Usage**: Read by HintEffect and ContainerHintEffect before triggering visual hints.
    - **Returns**: True if there are no active suppressors
    - false otherwise.
- `+ RemoveSuppressor(object owner): void`
    - **Purpose**: Removes a suppressor owner, restoring hint display when no suppressors remain.
    - **Usage**: Called when a tutorial or modal ends (e.g. TutorialManager.Deactivate).
    - **Params**: owner - The object representing the suppression source.
---

## IInputBlocker

> - **Purpose**: Read-only contract for querying whether field input (tap and drag) is currently blocked.
> - **Usage**: Injected into FieldEventHandler and DraggableChipLogic to gate user input.
> - **Notes**: When AllowedStart == AllowedEnd — tap-mode: only tap on that cell is permitted. When AllowedStart != AllowedEnd — drag-mode: only drag from AllowedStart to AllowedEnd is permitted.
#### Fields
- `+- AllowedEnd: Nullable<Vector2Int>`
- `+- AllowedStart: Nullable<Vector2Int>`
- `+- IsBlocked: bool`
- `+- IsTapMode: bool`
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

## IMergeEventHandler

> - **Purpose**: Contract for dispatching core board-state transitions (chip created, removed, effect unlocked, space unavailable) to game systems and event listeners.
> - **Usage**: Inject via VContainer
> - call Raise* methods from ChipFactory, Chip, and gameplay services at appropriate lifecycle points. Subscribers may be C# delegates or event bridges.
> - **Notes**: Registered as Singleton in Merge2LifetimeScope and IsoMergeLifetimeScope. Fires C# events for chip lifecycle and interaction boundaries.
#### Methods
- `+ RaiseChipCreated(Chip chip, ICell cell): void`
- `+ RaiseChipEffectUnlocked(Chip chip, int effectId): void`
- `+ RaiseChipRemoved(Chip chip): void`
- `+ RaiseChipTapped(Chip chip): void`
- `+ RaiseNotEnoughSpace(Chip chip): void`
- `+ RaiseNotEnoughSpaceForEvolution(Chip chip): void`
---

## IMergeLifetimeScope

> - **Purpose**: Interface for LifetimeScope components in MergeClassic and IsoMerge to expose configuration and Unity lifecycle reference
> - **Usage**: Implemented by MergeClassicLifetimeScope and IsoMergeLifetimeScope to allow Editor and Runtime tools to query and set properties without tight coupling
#### Fields
- `++ FieldData: FieldData`
- `+- CellType: Type`
- `+- Container: IObjectResolver`
- `+- UnityObject: object`
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

## IShadowEffect

> - **Purpose**: Interface representing a shadow effect that responds to changes in the chip's lift height
> - **Usage**: Implemented by ShadowEffect and accessed by ChipLiftController or Cell to update shadow offset/scale dynamically
#### Methods
- `+ OnHeightChanged(float height): void`
    - **Purpose**: Updates the shadow's local position and scale based on the chip's current lift height
    - **Usage**: Called dynamically when the chip is lifted (dragged) or is in a flight animation
    - **Params**: height - the current height value of the chip
---

## ISubEffect

> - **Purpose**: Contract for sub-effect configuration linked to a parent effect or area effect
> - **Usage**: Implemented by SubEffect class
> - inspected by ISubEffectOwner implementations
> - **Notes**: Provides read-only access to sub-effect ID and optional activation/deactivation delays
#### Fields
- `+- ActivateDelay: float`
- `+- DeactivateDelay: float`
- `+- EffectId: int`
---

## ISubEffectOwner

> - **Purpose**: Contract for any component or effect that owns, manages, or is controlled by sub-effect owners
> - **Usage**: Implemented by IEffect and ILockedAreaEffect
> - passed as owner into ActivateByOwner/DeactivateByOwner
> - **Notes**: Provides access to configured sub-effects and tracks active owners via Owners
#### Fields
- `+- Owners: HashSet<ISubEffectOwner>`
- `+- SubEffects: ISubEffect[]`
#### Methods
- `+ ActivateByOwner(Chip chip, ISubEffectOwner owner): bool`
- `+ DeactivateByOwner(Chip chip, ISubEffectOwner owner, bool force): void`
---

## IVisualField

> - **Purpose**: Contract for the root visual field component created from FieldData.VisualField
> - **Usage**: Implement on the level visual prefab root so FieldInitializeCommand can initialize field-wide visual services
> - **Notes**: Use for visual field concerns such as camera bounds without coupling MergeBase to a concrete view implementation
#### Fields
- `+- FieldOffset: Vector3`
- `+- gameObject: GameObject`
#### Methods
- `+ InitVisualField(FieldData fieldData): void`
- `+ OnFieldSizeChanged(Vector2Int newSize): void`
---

## IVisualFieldElement

> - **Purpose**: Contract for visual field child elements requiring initialization during level setup.
> - **Usage**: Implemented by components inside visual field prefabs (e.g. LockedAreaEffect)
> - invoked by VisualField.ApplyEffects.
> - **Notes**: Decouples VisualField from concrete effect implementations.
#### Methods
- `+ Init(): void`
    - **Purpose**: Initializes the visual field child element after injection.
    - **Usage**: Called by VisualField.ApplyEffects during level visual setup.
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

## MergeEventHandler

> - **Purpose**: Dispatches core board-state transitions (chip created, removed, effect unlocked, space unavailable) to C# subscribers.
> - **Usage**: Registered as Singleton via VContainer
> - Raise* methods are called by ChipFactory (OnChipCreated), Chip.Destroy (OnChipRemoved), Chip.RemoveEffect (OnChipEffectUnlocked), GeneratorModule (OnNotEnoughSpace), and ChipEvolutionService (OnNotEnoughSpaceForEvolution).
> - **Notes**: Fires only C# events. Unity Visual Scripting EventBus forwarding is handled by event bridge in MergeBase.VScripting assembly (compiled only when com.unity.visualscripting is installed).
#### Fields
- `- OnChipCreated: Action<Chip>`
- `- OnChipEffectUnlocked: Action<Chip, int>`
- `- OnChipRemoved: Action<Chip>`
- `- OnChipTapped: Action<Chip>`
- `- OnNotEnoughSpace: Action<Chip>`
- `- OnNotEnoughSpaceForEvolution: Action<Chip>`
#### Methods
- `+ RaiseChipCreated(Chip chip, ICell cell): void`
- `+ RaiseChipEffectUnlocked(Chip chip, int effectId): void`
- `+ RaiseChipRemoved(Chip chip): void`
- `+ RaiseChipTapped(Chip chip): void`
- `+ RaiseNotEnoughSpace(Chip chip): void`
- `+ RaiseNotEnoughSpaceForEvolution(Chip chip): void`
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
- `- hintTargetChips: HashSet<Chip>`
#### Methods
- `+ OnDragEndFeedback(Chip chip, DragInteractionResult interaction): void`
- `+ OnDragFeedback(Chip chip, ICell prevCell, ICell newCell): void`
- `+ OnDragStartFeedback(Chip chip): void`
- `- ActivateHintsGradually(Chip chip): IEnumerator`
    - **Purpose**: Gradually activates MergeHint effects on compatible targets gathered from chip modules and fillable containers to avoid FPS spikes.
    - **Usage**: Started as a coroutine from OnDragStartFeedback.
    - **Params**: chip - the chip being dragged
    - **Notes**: Collects module-provided targets via Chip.CollectHintTargets and checks ChipCollectionType.Fillable via chipCollections.GetCollection. All hinted chips are tracked in hintedChips for cleanup.
---

## MergeHintEffect
**Inherits**: `Effect`

> - **Purpose**: Optimized merge hint effect implementation that passes physical scale, aspect ratio, and normalized corner radius into shader via MaterialPropertyBlock
> - **Usage**: Attach to hint effect prefabs (e.g. HintEffect.prefab) and assign targetRenderer in Editor inspector.
> - **Notes**: Inherits directly from Effect
> - Zero-allocation O(1) renderer update without GetComponentsInChildren overhead.
#### Fields
- `- cornerRadius: float`
- `- ScalePropertyId: int`
- `- sharedPropertyBlock: MaterialPropertyBlock`
- `- targetRenderer: Renderer`
#### Methods
- `+ Init(Chip chip, int effectId): void`
- `~ UpdateMaterialScale(Vector2Int chipSize): void`
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
- `+ Activate(Chip chip): bool`
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
---

## NeighborChipFinder

> - **Purpose**: Finds all unique neighboring chips around a given cell, handling multi-cell chips correctly
> - **Usage**: Injected through VContainer with key 'Neighbors'
> - called from Merge2Module to identify chips affected by merges
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

## RandomEvolutionProvider

> - **Purpose**: Chip evolution provider strategy that selects a weighted random provider.
> - **Usage**: Configured inside ChipTapEvolutionData or ChipUnlockAreaData via SerializeReference selector.
> - **Notes**: Pre-computes total weight in Init to achieve zero per-tap allocations.
#### Fields
- `+ Entries: WeightedProviderEntry[]`
- `+- LastResult: List<ChipSpawnData>`
- `- cachedTotalWeight: int`
#### Methods
- `+ Confirm(): void`
- `+ GetEvolutionChips(): List<ChipSpawnData>`
- `+ Init(): void`
- `- SelectWeightedRandom(): IChipEvolutionProvider`
---

## RequireModuleDataAttribute
**Inherits**: `Attribute`

> - **Purpose**: Specifies that an IChipModule component requires a specific IChipModuleData in the ChipData.moduleDatas list
> - **Usage**: Apply to IChipModule classes (e.g., [RequireModuleData(typeof(ChipTapEvolutionData))])
> - **Notes**: Used by MergeToolkitWindow to display missing data warnings when editing ChipData assets
#### Fields
- `+- ModuleDataType: Type`
---

## SepiaEffect
**Inherits**: `ChipMaterialEffect`

> - **Purpose**: Visual effect that applies a sepia material and animates the _Intensity shader parameter on chips
> - **Usage**: Attach to sepia effect prefabs, assign effectMaterial with a sepia shader, and animate the intensity property via Animation Clips
> - **Notes**: Inherits from ChipMaterialEffect
> - pushes intensity into ChipMaterialController via MaterialPropertyBlock during animations
#### Fields
- `++ Intensity: float`
- `- IntensityPropertyId: int`
#### Methods
- `~ UpdateMaterialProperties(float dt): void`
    - **Purpose**: Updates the _Intensity shader property on the chip material controller
    - **Usage**: Called automatically on each frame while isMaterialAnimating is true or during activation
    - **Params**: dt - delta time since last update (0f on initial activation)
---

## ShadowEffect
**Inherits**: `Effect`

> - **Purpose**: Provides a persistent shadow effect for chips that reacts to movement states
> - **Usage**: Instantiated by Chip.InitEffects
> - sends 'Move' and 'Stop' triggers to its animator when the chip starts or stops moving
> - **Notes**: Does not deactivate on move to ensure the shadow remains visible during dragging
#### Fields
- `- alphaFollowCoroutine: Coroutine`
- `- alphaFollowDuration: float`
    - **Purpose**: Duration in seconds to track and mirror the owner chip's alpha after activation and deactivation
    - **Usage**: Configured in inspector
    - controls the duration of FollowAlphaRoutine coroutine
- `- chipRenderer: SpriteRenderer`
- `- initialLocalPosition: Vector3`
- `- initialLocalScale: Vector3`
- `- isInitialized: bool`
- `- shadowOffsetPerOneHeight: Vector3`
- `- shadowRenderer: SpriteRenderer`
- `- shadowScalePerOneHeight: float`
#### Methods
- `+ Activate(Chip chip): bool`
    - **Purpose**: Activates the shadow effect and starts tracking the owner chip's alpha
    - **Usage**: Called when activating the effect on the chip
    - initiates smooth alpha tracking
    - **Params**: chip - owner chip
    - **Returns**: Always returns true to indicate successful activation
- `+ Deactivate(Chip chip, bool force): void`
    - **Purpose**: Deactivates the shadow effect, tracking alpha fade-out or instantly setting alpha to zero if forced
    - **Usage**: Called when deactivating the effect on the chip
    - sets alpha to 0 immediately when force is true, otherwise follows chip alpha fade-out
    - **Params**: chip - owner chip
    - force - whether to immediately set alpha to 0 without tracking
- `+ Init(Chip chip, int effectId): void`
    - **Purpose**: Initializes the shadow effect and records baseline transform values
    - **Usage**: Called by Chip during effect initialization
    - registers with base effect lifecycle
    - **Params**: chip - owner chip
    - effectId - unique effect identifier
- `+ OnChipAnimationTrigger(string triggerName): void`
    - **Purpose**: Reacts to owner chip animation triggers by immediately synchronizing shadow alpha for the current frame
    - **Usage**: Called by Chip.NotifyEffectsOnAnimationTrigger via IEffect.OnChipAnimationTrigger
    - **Params**: triggerName - name of the trigger sent to the chip
- `+ OnHeightChanged(float height): void`
    - **Purpose**: Updates the shadow sprite renderer's local position and scale to visually simulate height changes
    - **Usage**: Implements IShadowEffect.OnHeightChanged
    - modifies localPosition based on shadowOffsetPerOneHeight and localScale based on shadowScalePerOneHeight
    - **Params**: height - the current height value of the chip
- `+ OnMovingStateChanged(Chip chip, bool isMoving): void`
    - **Purpose**: Reacts to chip movement by delegating to base effect movement handling
    - **Usage**: Called by Chip.NotifyEffectsOnMovingStateChanged when movement starts or stops
    - **Params**: chip - owner chip
    - isMoving - current movement state
- `- Expecto.MergeBase.IEffect.get_gameObject(): GameObject`
- `- FollowAlphaRoutine(): IEnumerator`
    - **Purpose**: Coroutine that continuously copies the owner chip renderer's alpha to the shadow renderer over alphaFollowDuration
    - **Usage**: Started by StartAlphaFollow
    - yields each frame until alphaFollowDuration elapses
- `- OnDisable(): void`
- `- SetShadowAlpha(float alpha): void`
    - **Purpose**: Updates the shadow SpriteRenderer's color alpha component while preserving its RGB values
    - **Usage**: Called by FollowAlphaRoutine, StartAlphaFollow fallback, and Deactivate(force: true)
    - **Params**: alpha - the new alpha value to set
- `- StartAlphaFollow(): void`
    - **Purpose**: Initiates the coroutine to track the owner chip renderer's alpha over time
    - **Usage**: Called internally by Activate and Deactivate(force: false)
    - stops any existing tracking coroutine before starting
    - **Notes**: If the GameObject is inactive in hierarchy, falls back to snapping alpha directly
- `- StopAlphaFollow(): void`
    - **Purpose**: Stops the active alpha tracking coroutine if one is running
    - **Usage**: Called before starting a new tracking coroutine, during force deactivation, or on disable
---

## ShadowEffectRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

## SortingLayerData

> - **Purpose**: Stores data for a single renderer's sorting layer configuration
> - **Usage**: Used by IChipSortingLayer to manage multiple renderers within a chip
> - **Notes**: CachedOrder is populated during Init and restored when movement or effect changes end
#### Fields
- `+ CachedOrder: int`
- `+ Renderer: SpriteRenderer`
---

## SpriteDirectionalFadeController
**Inherits**: `MonoBehaviour`

> - **Purpose**: Controls diagonal fade shader parameters, assigns fadeMaterial, and handles smooth fade in/out transitions on sliced or simple sprites using MaterialPropertyBlock
> - **Usage**: Attach to a GameObject with SpriteRenderer using the MergeDirectionalFadeGraph ShaderGraph material
> - call FadeOut or FadeIn from script
> - **Notes**: Assigns fadeMaterial to SpriteRenderer.sharedMaterial on StartFade
> - uses MaterialPropertyBlock to avoid instantiating material clones and preserve batching
> - automatically updates _Size based on SpriteRenderer.size
#### Fields
- `+- CurrentProgress: float`
- `+- FadeMaterial: Material`
- `+- SpriteRenderer: SpriteRenderer`
- `- autoSyncSize: bool`
- `- defaultDuration: float`
- `- fadeCoroutine: Coroutine`
- `- fadeCurve: AnimationCurve`
- `- ProgressPropId: int`
- `- propBlock: MaterialPropertyBlock`
- `- SizePropId: int`
#### Methods
- `+ FadeIn(float duration, Action onComplete): void`
    - **Purpose**: Starts a smooth fade-in animation toward 0.0 (visible)
    - **Usage**: Call when revealing an area or showing the sprite
    - **Params**: duration - duration in seconds (if negative or default, uses defaultDuration)
    - onComplete - optional callback on finish
- `+ FadeOut(float duration, Action onComplete): void`
    - **Purpose**: Starts a smooth fade-out animation toward 1.0 (hidden)
    - **Usage**: Call when unlocking an area, clearing an obstacle, or hiding the sprite
    - **Params**: duration - duration in seconds (if negative or default, uses defaultDuration)
    - onComplete - optional callback on finish
- `+ ResetFade(): void`
    - **Purpose**: Stops active fade animation and snaps progress back to 0.0 (fully visible)
    - **Usage**: Call to reset the fade state before replaying or restoring initial appearance
- `+ StopFade(): void`
    - **Purpose**: Stops any active fade animation coroutine
    - **Usage**: Call when interrupting transitions or resetting the object
- `+ UpdateSize(): void`
    - **Purpose**: Synchronizes the sprite size property into the shader
    - **Usage**: Call when the SpriteRenderer.size changes dynamically at runtime
- `- Awake(): void`
- `- EnsurePropBlock(): void`
- `- FadeRoutine(float targetProgress, float duration, Action onComplete): IEnumerator`
- `- OnDisable(): void`
- `- SetProgress(float progress): void`
    - **Purpose**: Sets the fade progress immediately without animating
    - **Usage**: Internal method to snap to a specific fade state
    - **Params**: progress - 0.0 is fully visible, 1.0 is fully hidden
- `- StartFade(float targetProgress, float duration, Action onComplete): void`
- `- UpdateMaterialProperties(): void`
---

## SubEffect

> - **Purpose**: Sub-effect configuration linked to an owner with configurable activation and deactivation delays
> - **Usage**: Configured in inspector of Effect and LockedAreaEffect prefabs
> - read via ISubEffect by owners
> - **Notes**: EffectId uses [EffectSelector] for inspector dropdown
> - delays <= 0 trigger immediately
#### Fields
- `+- ActivateDelay: float`
- `+- DeactivateDelay: float`
- `+- EffectId: int`
---

## VisualField
**Inherits**: `MonoBehaviour`

> - **Purpose**: Provides visual presentation of the 2D merge grid, managing field sprite size and injecting sub-effects like locked areas.
> - **Usage**: Attach to visual field prefabs
> - instantiated dynamically via field initialization.
#### Fields
- `+- FieldOffset: Vector3`
- `+- FieldOffsetTransform: Transform`
- `- fieldBorderSpriteRenderer: SpriteRenderer`
- `- fieldGrid: IFieldGrid`
- `- fieldSpriteRenderer: SpriteRenderer`
- `- resolver: IObjectResolver`
#### Methods
- `+ InitVisualField(FieldData fieldData): void`
    - **Purpose**: Initializes the 2D visual field using field configuration data.
    - **Usage**: Called dynamically after instantiating the prefab during field initialization.
    - **Params**: fieldData - configuration specifying grid size and other settings.
- `+ OnFieldSizeChanged(Vector2Int newSize): void`
- `- ApplyEffects(): void`
- `- ApplyFieldOffset(): void`
- `- ApplySize(Vector2Int newSize): void`
- `- Expecto.MergeBase.IVisualField.get_gameObject(): GameObject`
---

## VisualFieldRef
**Inherits**: `0, Culture=neutral, PublicKeyToken=null]]`
---

