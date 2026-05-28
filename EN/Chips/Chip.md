# Chip (Base Chip)

[← Back to Main](../Main.md)

The base `Chip` class is the visual representation and interaction component for objects on the game field. It is responsible for state rendering, effects, and handling Unity events (Input).

The merge and movement logic itself is moved into dedicated logic classes.

## Architecture and Responsibility

### 1. `Chip.cs` (Base Class)
The `Chip` class is the visual representation and base component.
- **Data (Configuration - `ChipData`)**: Stores a reference to `ChipData`, which contains settings:
  - **Type**: Chip type identifier (string).
  - **PrefabLink**: Reference to chip prefab.
  - **Size**: Chip size in cells (Vector2Int).
  - **MergeData**: access to merge configuration. During `Init(ChipData, ChipRuntimeData)` chip caches `data.GetSpecialData<ChipMergeData>()`.
  - **specialDatas**: Polymorphic collection for additional typed chip settings.
  
- **Special Data**:
  - **GetSpecialData<T>()**: Typed access to an element in `specialDatas`.
  - **IChipSpecialData**: Base contract for specialized data. Implementations: `ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, `ChipExtraEffectsData`.

- **Runtime**:
  - **CellPosition**: Current chip position on field grid (Vector2Int). Updated by system during movement.
  - **RuntimeData**: Current state (see below).
  - **BlockingState**: (`CombinedBlockingState`) aggregated permission state (for example, `CanBeMoved`, `CanBeMergedAsSource`) determined by active effects.
- **Visual Management**:
  - **SortingLayer** (`IChipSortingLayer`): Controls sorting layers of multiple chip renderers, ensuring correct display during movement.
  - **AnimationNode** (`Transform`): Reference to the chip's animation node where visual effects (of type `ParentChipAnimationNode`) that should move with the chip are attached.
- **Others**:
  - **LogEnable**: Flag to enable logging of chip events to console.
- **Effects**: Managed by a centralized system based on `Dictionary<int, IEffect>` with hash keys from `EffectConsts`.  
  For full catalog see [Visual Effects](../Visuals/Effects.md). For blocking and destruction logic details see [Chip Effect Blockers](../Features/ChipEffectBlockers.md).
- **Animations**: Has a reference to `Animator` for playing states (for example, `Merge`, `Generate`, `MoveLocked`).

## Effect Management

Base `Chip` class automatically manages and sends notifications to all visual effects through a centralized hash-dictionary-based system.

### Effect Storage & Access
- **`effects` (Dictionary<int, IEffect>)**: Dictionary of all active chip effects. Keys are hash codes from `EffectConsts`, providing type-safe access without type-based lookup.
- **`GetEffect(int effectHash)`**: Gets effect by its EffectConsts key. Returns `null` if not found:
  ```csharp
  GetEffect(EffectConsts.MoveLocked)?.SendTrigger("MoveLocked", true);
  ```
- **`GetEffect<T>(int effectHash) where T : IEffect`**: Typed access to effect with cast. Often used for specialized interfaces:
  ```csharp
  var containerEffect = GetEffect<IEffectContainer>(EffectConsts.ContainerRequirements);
  containerEffect?.UpdateElements(this, containers, false);
  ```

### Effect Initialization
- **`InitEffects()`**: Virtual method called from `Init(...)` to initialize all effects. Base implementation:
  1. Iterates `ChipExtraEffectsData.Blockers` — for each item whose `EffectId` is in `runtimeData.EffectEnables`, instantiates prefab and adds it into effects dictionary through `AddEffect`
  2. Creates and adds `CellHighlightEffect` from `ChipData.CellHighlightPrefab` (key: `EffectConsts.CellHighlight`)
  3. Creates and adds `ChipMergeAvailableEffect` from `ChipData.MergeAvailableEffectPrefab` (key: `EffectConsts.MergeAvailable`)
  4. If `ShadowEffectPrefab` is set, creates and adds `ShadowEffect` (key: `EffectConsts.ShadowEffect`)
  
  This method is designed to be overridden in derived classes (for example, `ChipGenerator` adds `GeneratorCharging` and `GeneratorCharged`).

- **`AddEffect(IEffect effect, int effectHash, bool activate)`**: Adds effect to dictionary and optionally activates it:
  ```csharp
  var effect = InstantiateEffect<IEffect>(data.CellHighlightPrefab);
  AddEffect(effect, EffectConsts.CellHighlight, true);
  ```

### Effect Constants (EffectConsts)
The whole effect system uses centralized integer constants defined in [EffectConsts.cs](../../../Core/Scripts/Chips/Effects/EffectConsts.cs):
- **Base effects (1-7)**: `MergeAvailable`, `CellHighlight`, `ContainerRequirements`, `GeneratorCharged`, `GeneratorCharging`, `PBoosterConnectorCells`, `PBoosterJoin`
- **Blocker effects (101+)** — `EffectConsts.Blockers`: `BoxEffect` (101), `ChainsEffect` (102), `MoveLockedEffect` (103)
- **Utilities**: `GetIdByName(string)` — resolves string name to ID through `nameToId` dictionary

### Effect Lifecycle
- All effects added to `effects` dictionary automatically receive notifications via `OnChangedCell()`, `OnInteractionOverCellChanged()`, and `OnInteractionUnderCellChanged()` methods.
- **Effect Destroying**: Effects with `DestroyingSettings` support progressive destruction on neighboring merges (details: [Chip Effect Blockers](../Features/ChipEffectBlockers.md#effect-destroying-system)):
  - `InitDestroyingEffectsData()` scans effects and creates `EffectDestroyingRuntimeData` entries.
  - `UpdatePrioritizingDestroyingEffect()` selects effect with highest `Priority` as `effectOfPrioritizingDestroying`.
  - `HandleDestroyingEffects()` increments `NeighboringMergeCount` and calls `TryDestroyEffect`.
  - `RemoveEffect(int effectId)` deactivates effect, removes it from dictionary and `EffectEnables`, removes block from `BlockingState`, selects next priority effect, and updates visual.
- On `Destroy(Cell, bool force)` call, system:
  1. Clears occupancy in `FieldGrid`
  2. Calls `ICellSubscriber.OnChipDestroy(mainCell)`
  3. Destroys all effects from `effects` dictionary with corresponding delay
  4. Starts destruction of `gameObject` itself with delay: 0.1s (if `force=true`) or 0.3s (if `force=false`). 
  
  The longer delay when `force=false` allows final animations (e.g., isometric merge animation) to complete.

### Extensions for Specialized Chips
Specialized chips extend `InitEffects()` to add their own effects:
- **`ChipGenerator.InitEffects()`**: Adds `GeneratorCharging` and `GeneratorCharged` effects
- **`ChipContainer.InitEffects()`**: Adds `ContainerRequirements` effect, implements `IEffectContainer`
- **`ChipPowerBooster.InitEffects()`**: Adds `PBoosterConnectorCells` and `PBoosterJoin` effects

### Movement State Management
System distinguishes between **user dragging state** and **visual movement state**:

#### User Drag State
- **`SetDragging(bool)`**: Sets user dragging state. Called by `DraggableChipLogic` on drag start/end. Automatically calls `SetMoving(true)` when needed.
- **`IsDragging()`**: Checks if chip is being dragged by user. Tracks user interaction itself, not only visual movement.

#### Visual Movement State
- **`SetMoving(bool)`**: Controls visual movement state.
  - Updates `IChipSortingLayer` state to adjust renderer sorting layers.
  - Notifies all effects through `OnMovingStateChanged(chip, isMoving)` method.
  - On movement start (`true`) adds temporary event `NewChip=null` to current cell in `IChipChangeNotifier`, so observer systems instantly react to chip "temporary exit"; on finish (`false`) calls `UpdateVisual()`.
- **`IsMoving()`**: Checks visual movement state (by `sortingOrder`). Returns `true` for both user dragging and system movement.

### Other Methods
- **`OnDraggingChipWithMoveLocked()`**: Virtual method called on attempt to drag a locked chip. First tries sending trigger `"MoveLocked"` to `effectOfPrioritizingDestroying` (effect with highest destroy priority); if absent — to effect with key `EffectConsts.Blockers.MoveLockedEffect`. Uses `allowRepeat=true` to ensure visual response on every attempt.

### Extensions for Specialized Chips
- **`ChipGeneratorRuntimeData`**: Adds charge state, timers, recharge counters.
- **`ChipContainerRuntimeData`**: Adds container fill progress dictionary.


### Merge System
Implements compatibility logic and merge process of two chips through `IChipInteractionLogic` mechanism.  
For full description (CanInteract, ExecuteInteraction, Weighted Random, Extra Chips, Relocation), see [MergeableChipLogic](../Interactions/MergeableChipLogic.md).
