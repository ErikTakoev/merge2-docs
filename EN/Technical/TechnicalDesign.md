# Technical Design

[← Back to Main](../Main.md)


Architecture is built on the principles of modularity, Dependency Injection, and clear separation of responsibility.

## Dependency Injection (VContainer)
We use **VContainer** for dependency management.
- **LifetimeScope**: `Merge2LifetimeScope` is the configuration point for the Merge2 scene. Static level data (`FieldData`, `ChipDataCollection`) are also registered as singletons here.
- **Initialization**: `Merge2Initializer` acts as an Entry Point. It receives key interfaces (`IFieldGrid`, `IFieldEventHandler`, `ChipFactory`, `IInputManager`) through the constructor and initializes the field via `IFieldInitializeCommand`.
- **Component Injection**: All game services and logic classes receive dependencies through `[Inject]` or constructor.

## Core Interfaces & Implementations
Main system abstractions and their implementations:

### Grid & Data
- **`IFieldGrid`** -> `FieldGrid`
  - **Purpose**: Manages grid state (2D array of `Cell`).
  - **Responsibility**: Creating cells, coordinate validation, low-level chip placement operations (`SetChipInCell`).

- **`IFieldInitializeCommand`** -> `FieldInitializeCommand`
  - **Purpose**: Command for level initialization.
  - **Responsibility**: Creating the visual grid and loading initial chips. Receives `FieldData` and `ChipDataCollection` through Injection.

### Logic & Interaction
- **`IInputManager`** -> `InputManager`
  - **Purpose**: Abstraction of the input system for Dependency Injection.
  - **Responsibility**: Defining input events (OnTap, OnDragStart, OnDrag, OnDragEnd) and simulation methods for testing. The `InputManager` implementation handles Unity Input System and transforms input events into C# events.

- **`IFieldEventHandler`** -> `FieldEventHandler`
  - **Purpose**: Entry point for system input events.
  - **Responsibility**: Handling Tap/Drag events, coordinating interaction between `IInputManager` and field logic.

- **`IFreeCellFinder`** -> `FreeCellFinder`
  - **Purpose**: Algorithmic search for free space.
  - **Responsibility**: Finding the nearest free cell (spiral search) for spawning or moving chips. Supports `onlyAround` mode to limit search to only neighboring cells.

- **`IChipMovingLogic`** -> `ChipMovingLogic`
  - **Purpose**: Complex movement logic.
  - **Responsibility**: Movement validation, collision handling, and calculating chain movements (relocation) of other chips to free up space.

## Visual Effects System
Visual effects for chips are implemented via a decoupled interface system to allow for various visual representations.
- **`IEffect`**: Base interface for all chip effects (activation, triggers, cell changes).
- **`IEffectContainer`**: Specialized interface for `ChipContainer` visuals, extending `IEffect` with `UpdateElements`.
- **`IEffectGeneratorCharging`**: Specialized interface for `ChipGenerator` charging visuals, extending `IEffect` with `OnCharging`.
- **`InterfaceRef<T>`**: We use a custom serialized wrapper (`EffectRef`, `EffectContainerRef`, etc.) to assign interface-implementing MonoBehaviours directly in the Inspector, ensuring type safety and modularity.

## Interaction Strategies
We use the "Strategy" pattern. The logic of how chips interact with each other during dragging is extracted into separate components:
- **Interface**: `IChipInteractionLogic`.
- **Implementations (components of `DraggableChipLogic`)**:
  - `MergeableChipLogic` — merge logic for compatible chips (according to `ChipMergeData` settings).
  - `FillContainerLogic` — logic for adding chips to containers.
- **Features**:
  - Strategy components must be placed on the same **GameObject** as `DraggableChipLogic`.
  - **Priority**: The order of components in the Unity inspector determines the order of interaction checks. The first strategy that returns `true` in `CanInteract` will be selected for execution.
- This allows dynamically configuring possible interactions on the field by simply adding or removing appropriate components.

## Input System
The input system is based on **Unity Input System (New Package)**.
- **Asset**: `Merge2Input.inputactions` — contains Action Maps definitions (Game, UI).
- **Architecture**:
  - **Interface**: `IInputManager` — defines the contract for the input system, allowing Dependency Injection through VContainer.
  - **Implementation**: `InputManager` — adapter class that initializes the Generated Class `Merge2Input` and transforms input events (Press, Drag) into C# events (`OnTap`, `OnDragStart`, `OnDrag`, `OnDragEnd`). Provides simulation methods (`SimulateTap`, `SimulateDrag*`) for testing in Unity Editor.
  - **Integration**: `FieldEventHandler` subscribes to `IInputManager` events and delegates their handling to appropriate components (chips or field).

## Animation System
Animations are separated from data logic.
- **Interface**: `IChipFlyAnimation`.
- **Purpose**: Manages visual movement (Tweening) of objects.
- **Advantage**: Logic in `Cell` or `Chip` says "move there", while the animation system decides "how" to do it (speed, curve, effects) without blocking game logic state.

## State Management
The project uses ScriptableObjects to store level state and metadata.

### FieldData & CellData
`FieldData` describes the initial field state. Each cell is represented by the `CellData` structure:
- **FieldChipData**: Contains chip data (**ChipId**) and its current state or effects (**IsMoveLocked**).
- **Position**: Anchor coordinates (top-left).

### Runtime State
During the game, lock state is transferred to `ChipRuntimeData`. This allows dynamically changing chip states (e.g., unlocking after meeting certain conditions) while syncing visual effects via `UpdateRuntimeData()`.

## Editor Tools
We provide specialized tools to facilitate the process of content creation and configuration.
- **Level Editor**: A visual level editor that allows designers to set up the grid and place chips.
- **Undo/Redo (Command Pattern)**: All editor actions are encapsulated in command objects (`IEditorCommand`). This enables a reliable undo/redo system, preventing progress loss due to editing errors.
- **Validation**: An automatic system for checking level data integrity before saving.

## Context Awareness
To facilitate code comprehension (especially for AI), the `[ContextCodeAnalyzer]` attribute is used.
- **Fields**: `@purpose`, `@usage`, `@params`, `@notes`.
- **Usage**: Documenting non-obvious logic directly in code. This helps generate up-to-date documentation and provides context for LLM analysis.
