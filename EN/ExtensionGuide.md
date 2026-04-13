# Extension Guide

[← Back to Main](Main.md)

This document describes how to extend Merge Toolkit project: from creating a new game to adding new chip types, effects, and interactions.

---

## 1. Creating a New Game

Before extending, it is recommended to create a clean project structure. Detailed instructions are in [Quick Start -> Create New Merge Game (Clean Project)](Quick_Start.md#create-new-merge-game-clean-project).

**In short:**
1. Open **Window -> Merge2 -> Chip Viewer**.
2. In **Settings** tab click **New Game Folder**.
3. Tool will automatically create full folder structure, scene, `Merge2LifetimeScope`, `ChipCreatorSettings`, tests, and all required assets.

After that, your project is ready for extension.

---

## 2. Extension Through VContainer (Dependency Injection)

Project uses **VContainer** for dependency management. Configuration point is `Merge2LifetimeScope` class inheriting `LifetimeScope`.

### How It Works

In `Configure(IContainerBuilder builder)` method all services and data are registered:

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
| `IFieldEventHandler` | `FieldEventHandler` | Entry point for input events (Tap, Drag), interaction coordination between Input and field |
| `IFieldInitializeCommand` | `FieldInitializeCommand` | Level initialization: visual grid creation and initial chip loading |
| `IChipMovingLogic` | `ChipMovingLogic` | Movement validation and execution, chain relocation calculation for neighboring chips |
| `IFreeCellFinder` | `FreeCellFinder` | Search for nearest free cell (spiral algorithm) |
| `IChipFlyAnimation` | `ChipFlyAnimation` | Chip "fly" animation to target position |
| `IChipInteractionLogic` | `MergeableChipLogic`, `FillContainerLogic` | Interaction strategy between chips (merge, fill container) |

### Recommended Approach: Create Your Own LifetimeScope

> [!IMPORTANT]
> It is recommended **not to modify** `Merge2LifetimeScope` directly. Instead, create **your own class** inheriting `LifetimeScope` and copy configuration from `Merge2LifetimeScope` as a base. After that, replace or add implementations as needed.

This allows you to:
- Keep original `Merge2LifetimeScope` unchanged.
- Have full control over dependency registration.
- Replace specific implementations (for example, `IChipMovingLogic`, `IFreeCellFinder`) with your own.
- Since `Merge2LifetimeScope` is not used, it will not end up in build.

```csharp
public class MyGameLifetimeScope : LifetimeScope
{
    [SerializeField] private FieldData fieldData;

    protected override void Configure(IContainerBuilder builder)
    {
        // --- Copied from Merge2LifetimeScope ---
        _ = builder.RegisterInstance(fieldData);
        _ = builder.RegisterInstance(fieldData.ChipDataCollection);

        _ = builder.RegisterComponentInHierarchy<FieldEventHandler>().As<IFieldEventHandler>().AsSelf();
        _ = builder.RegisterComponentInHierarchy<FieldGrid>().As<IFieldGrid>().AsSelf();
        _ = builder.RegisterComponentInHierarchy<FieldInitializeCommand>().As<IFieldInitializeCommand>().AsSelf();
        _ = builder.RegisterComponentInHierarchy<InputManager>().AsSelf();
        
        _ = builder.Register<DeferredChipChangeNotifier>(Lifetime.Singleton).As<IChipChangeNotifier>();
        _ = builder.RegisterComponentInHierarchy<CellObserverManager>().AsSelf();

        _ = builder.Register<ChipFactory>(Lifetime.Singleton);
        _ = builder.Register<FreeCellFinder>(Lifetime.Singleton).As<IFreeCellFinder>();
        _ = builder.Register<ChipFlyAnimation>(Lifetime.Transient).As<IChipFlyAnimation>();
        _ = builder.Register<NeighborChipFinder>(Lifetime.Singleton).As<IChipFinder>();

        _ = builder.RegisterEntryPoint<Merge2Initializer>();

        // --- Replaced implementation ---
        _ = builder.Register<MyChipMovingLogic>(Lifetime.Singleton).As<IChipMovingLogic>();

        // --- Added new services ---
        _ = builder.Register<MyNewService>(Lifetime.Singleton).As<IMyNewService>();
    }
}
```

After that, replace `Merge2LifetimeScope` component with `MyGameLifetimeScope` on scene.

### Recommended Approach: Create Your Own Initializer

> [!IMPORTANT]
> Similar to `Merge2LifetimeScope`, it is recommended to create **your own initializer class** instead of modifying `Merge2Initializer`. Copy it as a base and extend as needed.

`Merge2Initializer` is an **Entry Point**, registered via `builder.RegisterEntryPoint<Merge2Initializer>()`. It implements VContainer `IInitializable` and is called automatically at game start. Its tasks:
- `ChipFactory` initialization.
- Binding input events (`OnTap`, `OnDrag`, etc.) to `IFieldEventHandler`.
- Creating game field and loading initial chips.

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

Do not forget to replace registration in your `LifetimeScope`:

```csharp
// Instead of: builder.RegisterEntryPoint<Merge2Initializer>();
builder.RegisterEntryPoint<MyGameInitializer>();
```

### How to Add Your Service

1. **Create interface** (if needed) and **implementation**.
2. **Register in `Merge2LifetimeScope`**:
   ```csharp
   // Pure C# service
   builder.Register<MyService>(Lifetime.Singleton).As<IMyService>();

   // Or MonoBehaviour on scene
   builder.RegisterComponentInHierarchy<MyComponent>().As<IMyComponent>();
   ```
3. **Inject** via `[Inject]` or constructor:
   ```csharp
   public class MyClass
   {
       [Inject] private readonly IMyService myService;
   }
   ```

### How to Add New Interaction (IChipInteractionLogic)

To add a new interaction type between chips (besides merge and fill container):

1. Create class inheriting `MonoBehaviour` and implementing `IChipInteractionLogic`:
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
2. Add component to the same **GameObject** as `DraggableChipLogic`.
3. **Component order** in inspector defines priority: first strategy returning `true` in `CanInteract` will be selected.

---

## 3. Chip Extension

Base `Chip` class provides a set of virtual methods that can be overridden to create new chip types. Existing examples: `ChipGenerator`, `ChipContainer`.

### Step 1 - Create Derived Class

```csharp
public class MyChip : Chip
{
    // Custom fields and dependencies
    [Inject] private readonly IMyService myService;
}
```

### Step 2 - Override `Init` for Initialization

```csharp
public override void Init(ChipData data, ChipRuntimeData runtimeData)
{
    base.Init(data, runtimeData); // Important! Initializes base effects and runtimeData
    
    // Custom initialization logic
    // For example, reading additional data from specialDatas
    var settings = data.GetSpecialData<ExtensionChipData>();
}
```

> [!TIP]
> Recommended to call `GetSpecialData<T>()` only once during `Init` and cache result in chip field. Do not call it repeatedly in `Update` or other hot logic areas.

### Step 3 - Add Specific Effects Through `InitEffects`

```csharp
protected override void InitEffects()
{
    base.InitEffects(); // Initializes standard effects and effects from ChipExtraEffectsData
    
    // Add specific effects programmatically
    var myEffect = InstantiateEffect<IEffect>(myEffectPrefab);
    AddEffect(myEffect, MyEffectId, true);



}
```

### Step 4 - Extend RuntimeData (If Needed)

If new chip needs additional runtime state:

```csharp
// 1. Create runtime data class
public class MyChipRuntimeData : ChipRuntimeData
{
    public int CustomProperty;
}

// 2. Initialize through InitRuntimeData
public override void InitRuntimeData(ChipData data, ref ChipRuntimeData runtimeData)
{
    if (runtimeData == null)
    {
        runtimeData = new MyChipRuntimeData();
    }
    base.InitRuntimeData(data, ref runtimeData);
}
```

### Key Virtual Chip Methods

| Method | When to Use |
|---|---|
| `Init(ChipData, ChipRuntimeData)` | Initialization. Always call `base.Init(data, runtimeData)`. |
| `InitRuntimeData(ChipData, ref ChipRuntimeData)` | Creation and initialization of `RuntimeData` object. |
| `InitEffects()` | Effect object creation. Always call `base.InitEffects()`. |
| `PostInitEffects()` | Setup after all effects are initialized (for example, for destruction systems). |
| `OnTap(Vector2)` | Reaction to tap (for example, manual generation in `ChipGenerator`). |
| `OnDragStart/OnDrag/OnDragEnd` | Custom behavior during dragging. |
| `UpdateVisual()` | Synchronization of visual effect state with `RuntimeData`. |
| `SetMoving(bool)` | Sorting order change and effect notification about movement start/end. |
| `OnNeighborsChipOfMerged()` | Reaction to neighboring chip merged (used for effect destruction systems). |
| `OnDraggingChipWithMoveLocked()` | Visual reaction to trying to move a locked chip. |
| `Destroy(Cell)` | Cleanup on destruction. |
| `CanMoving()` | Whether chip can be moved. |

---

## 4. Effect Extension

Effect systems are built on `IEffect` interface and base `Effect` class. Detailed description of all effects: [Visual Effects](Visuals/Effects.md).

> [!TIP]
> There is no hard binding to specific effect implementations. Chip binds to effect **interface**, not to its implementation. All built-in effects (Highlight, MergeAvailable, MoveLocked, etc.) are connected through **prefabs**, so they can be easily **replaced** with your own implementation or **extended** by creating a derived class. It is enough to create a new prefab with your component implementing `IEffect`, `IEffectContainer`, `IEffectGeneratorCharging`, or your own interface inheriting `IEffect`.

### Custom Effect IDs and Inspector Integration

To define your own custom effects without modifying the core `EffectConsts.cs` file, it is highly recommended to create a separate constants file in your project.

To avoid ID collisions with core effects during future updates, **start your custom IDs from 1000**.

Use the `[EffectDefinition]` attribute on your constants. The system will automatically collect them at startup via reflection and integrate them into the core registry.

```csharp
public static class MyEffectConsts
{
    // Start from 1000 to prevent overlap with core effect IDs!
    [EffectDefinition("FrostEffect")]
    public const int FrostEffect = 1000;
    
    [EffectDefinition("PoisonEffect")]
    public const int PoisonEffect = 1001;
}
```

If you need an inspector combobox to select one of the available effects (both core and custom) in your scripts, use the `[EffectSelector]` attribute on a serialized integer field:

```csharp
[EffectSelector]
[SerializeField] private int mySelectedEffectId;
```

### Option A - Inherit from `Effect` (Recommended)

Base `Effect` class is a powerful system that automates most common tasks for visual effects:
- **Auto scaling (`AutoSize`)**: Effect can automatically change its `localScale` to match chip size (1x1, 2x2, etc.), using different strategies (`ScaleByChipSize`, `ScaleByMaxChipSize`).
- **Hierarchy management**: Support for different binding types (`ParentChip`, `ParentCell`, `ParentChipAnimationNode`).
- **Lifecycle and state**: Automatic hiding during movement (`DeactivateOnMove`) and state restoration after movement ends.
- **Animator integration**: Built-in support for `Activate` and `Deactivate` triggers, and `dontRepeatTrigger` system.
- **Positioning**: Ability to automatically offset effect center depending on chip dimensions.
- **Blocking and destruction system**: Built-in support for `EffectBlockingSettings` and progressive destruction through `TryDestroyEffect`.

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

### Option B - Implement `IEffect` Directly

For effects that do not need Animator or standard `Effect` logic:

```csharp
public class MyPureEffect : MonoBehaviour, IEffect
{
    public int GetId() => 0;
    public GameObject gameObject => gameObject;
    public EffectBlockingSettings BlockingSettings => null;
    public EffectDestroyingSettings DestroyingSettings => null;

    public void Init(Chip chip, int effectHash) { }
    public bool Activate(Chip chip) => true;
    public void Deactivate(Chip chip, bool force = false) { }
    public void SendTrigger(string triggerName, bool allowRepeat = false) { }
    public void OnChangedCell(Cell sourceCell, Cell targetCell) { }
    public void OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell underCell) { }
    public void OnInteractionUnderCellChanged(Cell underCell, Cell overCell) { }
    public void OnMovingStateChanged(Chip chip, bool isMoving) { }
    public bool TryDestroyEffect(Chip chip, EffectDestroyingSettings settings, EffectDestroyingRuntimeData destroyingData) => false;
}
```

### Specialized Effect Interfaces

For effects with extended contract, there are specialized interfaces:

| Interface | Method | Purpose |
|---|---|---|
| `IEffectContainer` | `UpdateElements(...)` | Container content visualization |
| `IEffectGeneratorCharging` | `OnCharging(float progress)` | Charging progress display |
| `IEffectPowerBoosterJoin` | `OnJoin(IPowerBoosterTarget)` / `OnLeave(IPowerBoosterTarget)` | Booster connection visualization |


To create a new specialized effect:
1. Create an interface inheriting `IEffect`.
2. Create `InterfaceRef<T>` wrapper for inspector serialization:
   ```csharp
   [Serializable]
   public class MyEffectRef : InterfaceRef<IMyEffect> { }
   ```
3. Use `MyEffectRef` as serialized field in chip.

### Connecting Effect to Chip

1. **Through ChipData.specialDatas**: For blocker effects or other additional effects, add `ChipExtraEffectsData` into `specialDatas` and fill `Blockers` or `OtherEffects` list.
2. **Through InitEffects**: In derived `Chip` class, create effect via `InstantiateEffect<T>(prefab)` and add it using `AddEffect`.

### Effect Notifications

Effects added to `effects` automatically receive notifications through following `IEffect` methods:

| Event | Method(s) | Purpose |
|---|---|---|
| Cell changed | `OnChangedCell` | Called when parent cell of chip actually changes. |
| Movement during drag | `OnInteractionOverCellChanged`, `OnInteractionUnderCellChanged` | Updates effect visuals (for example, target highlight) during drag. |
| Movement state change | `OnMovingStateChanged` | Start or end of visual movement (drag or relocation). |
| Self-destruction request | `TryDestroyEffect` | Handles incoming events (for example, neighboring merges) for effect destruction systems. |
| Chip destruction | - | Effects are automatically destroyed with chip GameObject. |

---

## 5. Recommendations for Creating New Mechanics

When you want to add a new chip type (for example, a chip that accelerates neighboring generators), it is recommended to follow this algorithm:

### Step 1 - Configure ChipData

Start with data definition. It is better not to clutter base `ChipData`.
1. Create a separate serializable class for your mechanic parameters (for example, `ExtensionChipData`) that implements `IChipSpecialData`.
2. Add this type into `ChipData.specialDatas` through `Chip Creator` -> **Special Data** section.
3. In chip code, get data in `Init` as follows: `var settings = data.GetSpecialData<ExtensionChipData>();` (recommended to call once and store in field).

```csharp
[Serializable]
public class ChipExtensionData : IChipSpecialData
{
    public float Power = 1.2f;
}
```

> [!TIP]
> `SpecialData` classes can be extended through inheritance. For example, if you make your own chip based on `ChipGenerator`, it is convenient to inherit from `ChipGeneratorData` and add only new parameters.

```csharp
[Serializable]
public class ChipGeneratorDataEx : ChipGeneratorData
{
    ...
}
```

### Step 2 - Create Tests

Before writing chip logic itself, write simple **tests** that describe expected behavior. This lets you iterate much faster without restarting the whole game.

For "Accelerator" mechanic it is important to verify:
1. **Time acceleration**: Measure charge time of a neighboring generator with accelerator and without it. It should be lower according to your settings.
2. **Cancel on movement**: If generator (or accelerator) is moved to another part of field where there is no accelerator (or generator) - acceleration should stop.

### Step 3 - Implement Chip

Create your class inheriting from `Chip` or existing specialized classes:
- `ChipGenerator` - if chip should create something.
- `ChipContainer` - if chip should store something inside.
- `Chip` - for unique base logic.

### Step 4 - Create Effects

Implement visual part:
1. Create effect prefabs implementing `IEffect` (or use `Effect`).
2. Connect them through `InitEffects` in your chip code.

### Step 5 - Continuous Verification

Throughout development, run tests continuously. This guarantees that new mechanics did not break existing ones (regression) and works according to your initial design.
