# Technical Design

[← Back to Main](../Main.md)


Architecture is built on principles of modularity, Dependency Injection, and clear separation of responsibilities.

## Dependency Injection (VContainer)
We use **VContainer** for dependency management.
- **LifetimeScope**: `Merge2LifetimeScope` is the configuration point for the Merge2 scene. Static level data (`FieldData`, `ChipDataCollection`) is also registered here as singletons. `FieldData` can be assigned dynamically, allowing use of the same LifetimeScope for different field configurations (for example, in tests).
- **Initialization**: `Merge2Initializer` acts as Entry Point. It receives key interfaces through constructor (`IFieldGrid`, `IFieldEventHandler`, `ChipFactory`, `IInputManager`) and initializes field through `IFieldInitializeCommand`.
- **Component Injection**: All gameplay services and logic classes receive dependencies through `[Inject]` or constructor.

## Core Interfaces & Implementations
Main system abstractions and their implementations:

### Grid & Data
- **`IFieldGrid`** -> `FieldGrid`
  - **Purpose**: Manages grid state (2D `Cell` array).
  - **Responsibility**: Cell creation, coordinate validation, low-level chip placement operations (`SetChipInCell`).
  - **`SetChipInCell` detail**: When setting chip, `FieldGrid` assigns `chip.CellPosition` before `IChipChangeNotifier.Enqueue(...)` so subscribers receive event with already up-to-date coordinates. During cleanup, it first clears occupancy (`ClearCells`), then enqueues `oldChip -> null` event.

- **`IFieldInitializeCommand`** -> `FieldInitializeCommand`
  - **Purpose**: Level initialization command.
  - **Responsibility**: Creating visual grid and loading initial chips. Receives `FieldData` and `ChipDataCollection` through Injection.

- **`IChipChangeNotifier`** -> `DeferredChipChangeNotifier`
  - **Purpose**: Aggregating cell changes during frame and a single `Flush` in `LateUpdate`.
  - **Responsibility**: `FieldGrid` adds events through `Enqueue`, `FieldEventHandler` calls `Flush`, and subscribers receive a consistent set of `ChipChangedEvent`.
  - **Details**: [Cell Observer System](../Features/CellObserverSystem.md).

- **`ICellSubscriber`** -> `CellSubscriber`, `PowerBoosterCellSubscriber`
  - **Purpose**: Contract for components reacting to changes in neighboring cells.
  - **Responsibility**: `OnChipChangedCell` rebinds subscriptions after movement, `OnChipDestroy` performs cleanup before chip destruction, `OnObservedCellChipChanged` handles batch events through [Cell Observer System](../Features/CellObserverSystem.md).

### Logic & Interaction
- **`IInputManager`** -> `InputManager`
  - **Purpose**: Input system abstraction for Dependency Injection.
  - **Responsibility**: Defining input events (OnTap, OnDragStart, OnDrag, OnDragEnd) and simulation methods for testing. `InputManager` implementation handles Unity Input System and transforms input events into C# events.

- **`IFieldEventHandler`** -> `FieldEventHandler`
  - **Purpose**: Entry point for input system events.
  - **Responsibility**: Handling Tap/Drag events, coordinating interaction between IInputManager and field logic.

- **`IFreeCellFinder`** -> `FreeCellFinder`
  - **Purpose**: Algorithmic free-space search.
  - **Responsibility**: Finding nearest free cell (spiral search) for chip spawn or movement. Supports `onlyAround` mode to restrict search to neighboring cells only.

- **`IChipMovingLogic`** -> `ChipMovingLogic`
  - **Purpose**: Complex movement logic.
  - **Responsibility**: Movement validation, collision handling, and calculation of chain movement (relocation) of other chips to free space.

- **`IPowerBoosterTarget`** -> `ChipGenerator` (`partial` in `ChipGenerator.PowerBoosterTarget.cs`)
  - **Purpose**: Contract for entities that can be boosted by `ChipPowerBooster`.
  - **Responsibility**: Storing active booster set, implementing apply/remove impact, providing `JoinPoints` for join visualization, exporting `BlockingState` to check `CanReceiveModifiers`, and `NotifyEffectRemoved(int)` to notify boosters about blocking effect changes.

- **`IChipFinder`** -> `NeighborChipFinder`
  - **Purpose**: Finding neighboring chips around a cell considering chip size.
  - **Responsibility**: Allocation-free iteration over 4 bounds of chip bounding box, collecting unique neighbors via `HashSet<Chip>`. Used by `MergeableChipLogic` for `NotifyNeighborsOfMerge`.

## Visual Effects System
Visual chip effects are implemented through interface system for flexibility and logic separation.
- **`IEffect`**: Base interface for all chip effects (activation, triggers, cell change).
- **`IEffectContainer`**: Specialized interface for `ChipContainer` visualization, extends `IEffect` with `UpdateElements` method.
- **`IEffectGeneratorCharging`**: Specialized interface for `ChipGenerator` charge visualization, extends `IEffect` with `OnCharging` method.
- **`IEffectPowerBoosterJoin`**: Specialized interface for booster join visualization (`OnJoin`, `OnLeave`, `Show`) between `ChipPowerBooster` and `IPowerBoosterTarget`.
- **`IChipSortingLayer`**: Contract for sorting layer management of multiple chip renderers. Allows automatic `sortingOrder` adjustment during movement (drag) so chip is visually above field.
- **`InterfaceRef<T>`**: We use a special serialized wrapper (`EffectRef`, `EffectContainerRef`, `EffectPowerBoosterJoinRef`, etc.) for assigning MonoBehaviour implementations of interfaces directly in Unity inspector, ensuring typing and modularity.

## Interaction Strategies
We use the Strategy pattern. Logic of how chips interact during drag is extracted into separate components:
- **Interface**: `IChipInteractionLogic`.
- **Implementations** (components of `DraggableChipLogic`):
  - `MergeableChipLogic` — logic of merging compatible chips (according to `ChipMergeData` settings).
  - `FillContainerLogic` — logic of adding chips into containers.
- **Specifics**:
  - Strategy components must be placed on the same **GameObject** as `DraggableChipLogic`.
  - **Priority**: Component order in Unity inspector defines interaction check order. First strategy returning `true` in `CanInteract` is selected for execution.
- This allows dynamic setup of possible field interactions by simply adding or removing corresponding components.

## Input System
Input system is based on **Unity Input System (New Package)**.
- **Asset**: `Merge2Input.inputactions` — contains Action Maps definitions (Game, UI).
- **Architecture**:
  - **Interface**: `IInputManager` — defines input system contract, enables Dependency Injection through VContainer.
  - **Implementation**: `InputManager` — adapter class that initializes Generated Class `Merge2Input` and transforms input events (Press, Drag) into C# events (`OnTap`, `OnDragStart`, `OnDrag`, `OnDragEnd`). Provides simulation methods (`SimulateTap`, `SimulateDrag*`) for testing in Unity Editor.
  - **Integration**: `FieldEventHandler` subscribes to `IInputManager` events and delegates handling to corresponding components (chips or field).

## Animation System
Animations are separated from data logic.
- **Interface**: `IChipFlyAnimation`.
- **Purpose**: Controls visual object movement (Tweening).
- **Benefit**: `Cell` or `Chip` logic says "move there", and animation system decides "how" to do it (speed, curve, effects), without blocking game logical state.

## State Management
Project uses ScriptableObjects for storing level state and metadata.

### ChipData & SpecialData
`ChipData` stores base chip parameters (type, prefab, size), and extendable data is moved to `specialDatas` (`SerializeReference`).
- **Contract**: `IChipSpecialData` — base interface for specialized configurations.
- **Merge as SpecialData**: `ChipMergeData` is now one of `IChipSpecialData` blocks and is not stored as a separate field in `ChipData`.
- **Access**: 
  - `GetSpecialData<T>()` returns typed data block (`ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, `ChipExtraEffectsData`, etc.).
  - `CreateSpecialData<T>()` dynamically creates a new special data instance, adds it to collection, and returns reference. Useful for tests when `ChipData` must be cloned and config changed on the fly.
  - `AddSpecialData(IChipSpecialData)` adds ready special data instance to collection (used, in particular, when cloning default templates in Chip Creator).
    ```csharp
    ChipData clonedData = originalData.Clone();
    ChipMergeData mergeData = clonedData.CreateSpecialData<ChipMergeData>();
    mergeData.Combinations = new MergeCombination[] { /* ... */ };
    ```
- **Benefit**: Allows adding new data types without extending base `ChipData` with separate fields.
- **Blocker Effects as SpecialData**: Blocker-effect settings are moved to `ChipExtraEffectsData` (`ExtraEffectData[]` array with `effectName` + `Prefab`), so `Chip.InitEffects()` gets configuration through `GetSpecialData<ChipExtraEffectsData>()`.
- **Runtime access to merge**: During `Chip.Init`, merge data is cached in `Chip.MergeData`; `MergeableChipLogic` uses this access.

### FieldData & CellData
`FieldData` describes initial field state. Each cell is represented by `CellData` structure:
- **FieldChipData**: Contains chip data (**ChipId**) and array of active blocker effects (**BlockerEffectIds**, for example `EffectConsts.Blockers.MoveLockedEffect`).
- **Position**: Anchor coordinates (top-left).
- **Code location**: `FieldData` and `FieldChipData` are located in `Core/Scripts/Field/Data`.

### Runtime State
In game, information about active blocker effects is stored in `ChipRuntimeData.EffectEnables` (`HashSet<int>`). This set is used as state indicator — `Chip.InitEffects()` and `UpdateVisual()` activate effects whose IDs are in set. Then the effect itself, having `EffectBlockingSettings`, passes specific restrictions into chip global `CombinedBlockingState` (which gameplay logic already uses). Additionally, `EffectDestroyingData` (`Dictionary<int, EffectDestroyingRuntimeData>`) tracks effect destruction progress on neighboring merges. This allows dynamically changing chip states (for example, unlocking after specific conditions), separating visual effects and blocking logic.

## Editor Tools
We provide specialized tools to simplify content creation and setup.
- **Level Editor**: Visual level editor allowing designers to configure grid and place chips.
- **Property Drawers**: Specialized attributes `[ChipSelector]` and `[EffectBlockerSelector]` are integrated into `FieldChipData` for convenient selection of chip and effect IDs in the inspector drop-downs.
- **Chip Creator**: `ChipData` editor that supports editing `specialDatas` (including polymorphic `IChipSpecialData` types).
- **Undo/Redo (Command Pattern)**: All editor actions are encapsulated into command objects (`IEditorCommand`). This allows implementing a reliable undo/redo system, preventing progress loss on editing mistakes.
- **Validation**: Automatic system for checking level data integrity before saving.

## Context Awareness
For easier code understanding (especially for AI), `[ContextCodeAnalyzer]` attribute is used.
- **Fields**: `@purpose`, `@usage`, `@params`, `@notes`.
- **Usage**: Documenting non-trivial logic directly in code. This helps generate up-to-date documentation and gives context during LLM analysis.
