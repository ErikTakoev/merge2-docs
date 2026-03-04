# Extension Guide

[← Back to Main](Main.md)

This document describes how to extend the Merge Toolkit project: from creating a new game to adding new types of chips, effects, and interactions.

---

## 1. Creating a New Game

Before extending, it is recommended to create a clean project structure. Detailed instructions are in [Quick Start → Create New Merge Game (Clean Project)](Quick_Start.md#create-new-merge-game-clean-project).

**Quickly:**
1. Open **Window → Merge2 → Chip Viewer**.
2. In the **Settings** tab, click **New Game Folder**.
3. The tool will automatically create a full folder structure, scene, `Merge2LifetimeScope`, `ChipCreatorSettings`, tests, and all necessary assets.

After this, your project is ready for extension.

---

## 2. Extension via VContainer (Dependency Injection)

The project uses **VContainer** for dependency management. The configuration point is the `Merge2LifetimeScope` class, which inherits from `LifetimeScope`.

### How it Works

In the `Configure(IContainerBuilder builder)` method, all services and data are registered:

```csharp
protected override void Configure(IContainerBuilder builder)
{
    // Static data (instances)
    builder.RegisterInstance(fieldData);
    builder.RegisterInstance(fieldData.ChipDataCollection);

    // MonoBehaviour components (scene hierarchy)
    builder.RegisterComponentInHierarchy<FieldEventHandler>().As<IFieldEventHandler>();
    builder.RegisterComponentInHierarchy<FieldGrid>().As<IFieldGrid>();
    builder.RegisterComponentInHierarchy<FieldInitializeCommand>().As<IFieldInitializeCommand>();
    builder.RegisterComponentInHierarchy<InputManager>().AsSelf();

    // Pure C# services (singletons)
    builder.Register<ChipFactory>(Lifetime.Singleton);
    builder.Register<ChipMovingLogic>(Lifetime.Singleton).As<IChipMovingLogic>();
    builder.Register<FreeCellFinder>(Lifetime.Singleton).As<IFreeCellFinder>();
    builder.Register<ChipFlyAnimation>(Lifetime.Transient).As<IChipFlyAnimation>();

    // Entry Point
    builder.RegisterEntryPoint<Merge2Initializer>();
}
```

### Key Interfaces

| Interface | Implementation | Purpose |
|---|---|---|
| `IFieldGrid` | `FieldGrid` | Grid management (2D `Cell` array): cell creation, coordinate validation, chip placement |
| `IFieldEventHandler` | `FieldEventHandler` | Entry point for input events (Tap, Drag), coordination of interaction between Input and the field |
| `IFieldInitializeCommand` | `FieldInitializeCommand` | Level initialization: visual grid creation and initial chip loading |
| `IChipMovingLogic` | `ChipMovingLogic` | Validation and execution of moves, calculation of chain relocations for adjacent chips |
| `IFreeCellFinder` | `FreeCellFinder` | Search for the nearest free cell (spiral algorithm) |
| `IChipFlyAnimation` | `ChipFlyAnimation` | Animation of the chip "flight" to the target position |
| `IChipInteractionLogic` | `MergeableChipLogic`, `FillContainerLogic` | Strategy for interaction between chips (merge, fill container) |

### Recommended Approach: Creating Your Own LifetimeScope

> [!IMPORTANT]
> It is recommended **not to modify** `Merge2LifetimeScope` directly. Instead, create **your own class** that inherits from `LifetimeScope` and copy the configuration from `Merge2LifetimeScope` as a base. After that, replace or add implementations as needed.

This allows you to:
- Keep the original `Merge2LifetimeScope` unchanged.
- Have full control over dependency registration.
- Replace individual implementations (e.g., `IChipMovingLogic`, `IFreeCellFinder`) with your own.
- Since `Merge2LifetimeScope` is not used — it will not be included in the build.

```csharp
public class MyGameLifetimeScope : LifetimeScope
{
    [SerializeField] private FieldData fieldData;

    protected override void Configure(IContainerBuilder builder)
    {
        // --- Copied from Merge2LifetimeScope ---
        builder.RegisterInstance(fieldData);
        builder.RegisterInstance(fieldData.ChipDataCollection);

        builder.RegisterComponentInHierarchy<FieldEventHandler>().As<IFieldEventHandler>();
        builder.RegisterComponentInHierarchy<FieldGrid>().As<IFieldGrid>();
        builder.RegisterComponentInHierarchy<FieldInitializeCommand>().As<IFieldInitializeCommand>();
        builder.RegisterComponentInHierarchy<InputManager>().AsSelf();

        builder.Register<ChipFactory>(Lifetime.Singleton);
        builder.Register<FreeCellFinder>(Lifetime.Singleton).As<IFreeCellFinder>();
        builder.Register<ChipFlyAnimation>(Lifetime.Transient).As<IChipFlyAnimation>();

        builder.RegisterEntryPoint<Merge2Initializer>();

        // --- Replaced implementation ---
        builder.Register<MyChipMovingLogic>(Lifetime.Singleton).As<IChipMovingLogic>();

        // --- Added new services ---
        builder.Register<MyNewService>(Lifetime.Singleton).As<IMyNewService>();
    }
}
```

After that, replace the `Merge2LifetimeScope` component with `MyGameLifetimeScope` on the scene.

### Recommended Approach: Creating Your Own Initializer

> [!IMPORTANT]
> Similar to `Merge2LifetimeScope`, it is recommended to create **your own initializer class** instead of modifying `Merge2Initializer`. Copy it as a base and extend as needed.

`Merge2Initializer` is the **Entry Point**, registered via `builder.RegisterEntryPoint<Merge2Initializer>()`. It implements VContainer's `IInitializable` and is automatically called when the game starts. Its tasks:
- `ChipFactory` initialization.
- Connecting input events (`OnTap`, `OnDrag`, etc.) to `IFieldEventHandler`.
- Creating the game field and loading initial chips.

```csharp
public class MyGameInitializer : IInitializable
{
    [Inject] private readonly InputManager inputManager;
    [Inject] private readonly ChipFactory chipFactory;
    [Inject] private readonly IObjectResolver resolver;
    [Inject] private readonly IFieldEventHandler field;
    [Inject] private readonly IFieldGrid fieldGrid;
    [Inject] private readonly IFieldInitializeCommand fieldInitializeCommand;

    // --- Added dependencies ---
    [Inject] private readonly IMyNewService myNewService;

    public void Initialize()
    {
        // --- Copied from Merge2Initializer ---
        chipFactory.Init(resolver, fieldGrid);

        inputManager.OnTap += field.OnTap;
        inputManager.OnDragStart += field.OnDragStart;
        inputManager.OnDrag += field.OnDrag;
        inputManager.OnDragEnd += field.OnDragEnd;

        fieldInitializeCommand.CreateField();
        fieldInitializeCommand.LoadChips();

        // --- Added custom initialization ---
        myNewService.Init();
    }
}
```

Don't forget to replace the registration in your `LifetimeScope`:

```csharp
// Instead of: builder.RegisterEntryPoint<Merge2Initializer>();
builder.RegisterEntryPoint<MyGameInitializer>();
```

### How to Add Your Own Service

1. **Create an interface** (if necessary) and **implementation**.
2. **Register in `Merge2LifetimeScope`**:
   ```csharp
   // Pure C# service
   builder.Register<MyService>(Lifetime.Singleton).As<IMyService>();

   // Or MonoBehaviour in hierarchy
   builder.RegisterComponentInHierarchy<MyComponent>().As<IMyComponent>();
   ```
3. **Inject** via `[Inject]` or constructor:
   ```csharp
   public class MyClass
   {
       [Inject] private readonly IMyService myService;
   }
   ```

### How to Add a New Interaction (IChipInteractionLogic)

To add a new type of interaction between chips (other than merge and fill container):

1. Create a class that inherits `MonoBehaviour` and implements `IChipInteractionLogic`:
   ```csharp
   public class MyInteractionLogic : MonoBehaviour, IChipInteractionLogic
   {
       public bool CanInteract(Cell sourceCell, Cell targetCell)
       {
           // Check: is this interaction possible?
           return false;
       }

       public bool ExecuteInteraction(Cell sourceCell, Cell targetCell)
       {
           // Execute interaction
           return false;
       }
   }
   ```
2. Add the component to the same **GameObject** as `DraggableChipLogic`.
3. **Component order** in the inspector determines priority: the first strategy that returns `true` in `CanInteract` will be chosen.

---

## 3. Extending Chip

The base `Chip` class provides a set of virtual methods that can be overridden to create new types of chips. Existing examples: `ChipGenerator`, `ChipContainer`.

### Step 1 — Create a Derived Class

```csharp
public class MyChip : Chip
{
    // Custom fields and dependencies
    [Inject] private readonly IMyService myService;
}
```

### Step 2 — Override `Init` for Initialization

```csharp
public override void Init(ChipData data)
{
    base.Init(data); // Important! Initializes base effects and runtimeData
    
    // Custom initialization logic
    // For example, reading additional data from data.OtherData
}
```

### Step 3 — Add Specific Effects via `InitEffects`

```csharp
protected override void InitEffects()
{
    base.InitEffects(); // Initializes standard effects (Highlight, MergeAvailable, MoveLocked)
    
    // Add specific effects
    var myEffect = InstantiateEffect<IEffect>(myEffectPrefab);
    if (myEffect != null)
    {
        effects.Add(myEffect); // Add to the general list for automatic event broadcasting
    }
}
```

### Step 4 — Extend RuntimeData (as needed)

If a new chip needs additional runtime state:

```csharp
// 1. Create a runtime data class
public class MyChipRuntimeData : ChipRuntimeData
{
    public int CustomProperty;
}

// 2. Initialize in Init
public override void Init(ChipData data)
{
    runtimeData = new MyChipRuntimeData(); // Before call to base!
    base.Init(data);
}
```

### Key Virtual Methods of Chip

| Method | When to Use |
|---|---|
| `Init(ChipData)` | Initialization. Always call `base.Init(data)`. |
| `InitEffects()` | Adding specific effects. Always call `base.InitEffects()`. |
| `OnTap(Vector2)` | Reaction to tap (e.g., manual generation in `ChipGenerator`). |
| `OnDragStart/OnDrag/OnDragEnd` | Custom dragging behavior. |
| `UpdateVisual()` | Synchronizing visual state with `RuntimeData`. |
| `SetMoving(bool)` | Changing sorting order during movement. |
| `Destroy(Cell)` | Cleanup on destruction. |
| `CanMoving()` | Whether the chip can be moved. |

---

## 4. Extending Effects

The effects system is built on the `IEffect` interface and the `Effect` base class. Detailed description of all effects: [Visual Effects](Visuals/Effects.md).

> [!TIP]
> There is no strict binding to specific effect implementations. All built-in effects (Highlight, MergeAvailable, MoveLocked, etc.) are connected via **prefabs**, so they can be easily **replaced** with your own implementation or **extended** by creating a derived class. It is enough to create a new prefab with your component implementing `IEffect`, `IEffectContainer`, `IEffectGeneratorCharging`, or your own interface inheriting from `IEffect`.

### Option A — Inheriting from `Effect` (Recommended)

The base `Effect` class provides built-in support for Animator, triggers, and `effectForCell`.

```csharp
public class MyCustomEffect : Effect
{
    // Custom serialized fields
    [SerializeField] private Color glowColor;
    
    public override void Activate(Chip chip)
    {
        base.Activate(chip); // Sends "Activate" trigger if sendAnimatorTrigger = true
        // Additional logic
    }
    
    public override void Deactivate(Chip chip, bool force = false)
    {
        base.Deactivate(chip, force);
        // Additional logic
    }
    
    // Reaction to cell change during drag
    public override void OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell underCell)
    {
        // Update visual
    }
}
```

### Option B — Implementing `IEffect` Directly

For effects that do not require an Animator or standard `Effect` logic:

```csharp
public class MyPureEffect : MonoBehaviour, IEffect
{
    public void Init(Chip chip) { }
    public void Activate(Chip chip) { }
    public void Deactivate(Chip chip, bool force = false) { }
    public void SendTrigger(string triggerName, bool allowRepeat = false) { }
    public void OnChangedCell(Cell sourceCell, Cell targetCell) { }
    public void OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell underCell) { }
    public void OnInteractionUnderCellChanged(Cell underCell, Cell overCell) { }
}
```

### Specialized Effect Interfaces

For effects with an extended contract, there are specialized interfaces:

| Interface | Method | Purpose |
|---|---|---|
| `IEffectContainer` | `UpdateElements(...)` | Visualizing container contents |
| `IEffectGeneratorCharging` | `OnCharging(float progress)` | Displaying charging progress |

To create a new specialized effect:
1. Create an interface that inherits from `IEffect`.
2. Create an `InterfaceRef<T>` wrapper for inspector serialization:
   ```csharp
   [Serializable]
   public class MyEffectRef : InterfaceRef<IMyEffect> { }
   ```
3. Use `MyEffectRef` as a serialized field in the chip.

### Connecting an Effect to a Chip

1. **Via ChipData**: Set the effect prefab in the `ChipData` field (e.g., `CellHighlightPrefab`, `MergeAvailableEffectPrefab`, `MoveLockedEffectPrefab`).
2. **Via InitEffects**: In the derived `Chip` class, create the effect via `InstantiateEffect<T>(prefab)` and add it to `effects`.

Effects added to `effects` automatically receive notifications about:
- Cell change (`OnChangedCell`)
- Position change during drag (`OnInteractionOverCellChanged`, `OnInteractionUnderCellChanged`)
- Chip destruction (effects are also destroyed)

---

## 5. Recommendations for Creating New Mechanics

When you want to add new logic (for example, a chip that speeds up neighboring generators), it is recommended to follow this algorithm:

### Step 1 — ChipData Setup

Start by defining the data. It is advisable not to clutter the base `ChipData`.
1. Create a separate **ScriptableObject** for your mechanic parameters (e.g., `ExtensionChipData`).
2. Attach this SO to the **OtherData** field in your `ChipData`.
3. In the chip code, you can retrieve this data: `var settings = data.OtherData as ExtensionChipData;`.

### Step 2 — Creating Tests

Before writing the logic of the chip itself, write simple **tests** that describe the expected behavior. This will allow you to iterate much faster without restarting the entire game.

For the "Speeder" mechanic, it is important to check:
1. **Acceleration Over Time**: Measure the time it takes for a neighboring generator to charge with and without the speeder. it should be less according to your settings.
2. **Cancellation on Movement**: If the generator (or speeder) is moved to another part of the field where there is no speeder (or generator) — the acceleration should stop.

### Step 3 — Chip Implementation

Create your class, inheriting from `Chip` or existing specialized classes:
- `ChipGenerator` — if the chip is supposed to create something.
- `ChipContainer` — if the chip is supposed to store something inside.
- `Chip` — for unique base logic.

### Step 4 — Creating Effects

Develop the visual part:
1. Create effect prefabs implementing `IEffect` (or use `Effect`).
2. Connect them via `InitEffects` in your chip code.

### Step 5 — Continuous Verification

Throughout development, run tests. This ensures that the new mechanic hasn't broken existing ones (regression) and works according to your original design.
