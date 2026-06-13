# Locked Areas and Deferred Cells

[← Back to Main](../Main.md)

This document describes the **Locked Areas** system — a mechanic that restricts player interaction with specific grid areas until they are unlocked. The system supports two types of locked cells: simple (with instantly loaded chips) and deferred (with chips that appear after unlocking).

## Overview

**Locked Areas** allow designers to create progressive levels where the player gradually gains access to new parts of the field. When an area is locked:

- Cells in the area have the `IsBlocked = true` flag, which blocks any input interaction (tap, drag).
- Visually, the area is covered by effects (e.g., fog, gates, chains) managed by `LockedAreaEffect`.
- Deferred chips do not spawn during level loading, but wait for unlocking.

When an area is unlocked (via gameplay code or completed conditions):

- Cells are unlocked (`IsBlocked = false`).
- `LockedAreaEffect` triggers a deactivation animation (e.g., fog fading out).
- For `DeferredCell` cells, the saved chips are instantly spawned.

---

## Architecture

### Key Components

#### 1. `LockedAreaManager` (ILockedAreaManager)
The central manager that controls the state of all locked areas on the field.

**Responsibilities**:
- **Initialization** (`Initialize`): Reads `FieldData.LockedAreas` and blocks all cells of locked areas.
- **Effect Registration** (`RegisterEffect`): Accepts registration from `LockedAreaEffect` components and instantly calls `Activate()` or `Deactivate()` depending on the current state.
- **Unlocking** (`UnlockArea`): Unlocks all cells in the area, spawns deferred chips, and triggers deactivation animations for effects.

**DI Injection**:
```csharp
// In Merge2Initializer or any gameplay component
[Inject] private readonly ILockedAreaManager lockedAreaManager;

// Initialization during scene start
lockedAreaManager.Initialize();

// Unlocking an area during gameplay
lockedAreaManager.UnlockArea(areaId: 1);
```

#### 2. `LockedAreaEffect`
A visual component representing the graphical overlay of a locked area.

**Structure**:
- **`lockedAreaId`** (int): Unique ID of the area the effect belongs to.
- **`Animator`**: An Animator component with triggers `"Activate"` (show) and `"Deactivate"` (hide).

**Methods**:
- **`Init(Chip chip, int effectId)`**: Called by `FieldInitializeCommand.CreateLevelVisual()` after level visualization instantiation. Stores `effectId` and registers itself in the manager:
  ```csharp
  this.effectId = effectId;
  lockedAreaManager.RegisterEffect(this);
  ```
  > **Important**: `base.Init()` **is not called** — the base implementation reads `chip.Data` and deactivates chip effects, which is not suitable for level-wide visuals.
- **`Activate(Chip chip)`**: Activates the GameObject and plays the animation via `Animator.SetTrigger("Activate")`. `chip` can be `null` (level effects are not bound to a chip).
- **`Deactivate(Chip chip, bool force)`**: Triggers the animation via `Animator.SetTrigger("Deactivate")` (usually with a fade-out). `chip` can be `null`.


#### 3. `DeferredCell` (cell addon component)
An additional `MonoBehaviour` component that stores chip configuration and spawns it after the area is unlocked. Added to the same GameObject as the regular `Cell` or `IsoCell`.

**Methods**:
- **`Init(ICell cell)`**: Binds the component to the owner (`Cell` or `IsoCell`). Called by `FieldGrid` right after adding the component.
- **`SetupDeferredChip(FieldData.CellData cellData, ChipData chipData)`**: Stores chip configuration for subsequent spawning (called during `FieldInitializeCommand.LoadChips`).
- **`SpawnDeferredChip()`**: Spawns the chip via `ChipFactory` with accumulated data. Called by `LockedAreaManager.UnlockArea`. Returns the created chip or `null`.
- **`TryGet(ICell cell, out DeferredCell deferredCell)`**: A static helper method to find the component on `ICell`.

**Features**:
- Does not implement `ICell` itself — acts only as a data holder.
- Chip data is stored internally until spawn; the chip does not reside in `ownerCell.Chip` before unlocking.
- On spawn, copies the `BlockerEffectIds` state from `FieldData` for consistency with regular loading.



## Data Structures

### `FieldData.LockedAreaData`
```csharp
[System.Serializable]
public struct LockedAreaData
{
    public int LockedAreaId;                      // Unique ID of the area
    public Vector2Int[] CellsToLock;             // Simple locked cells
    public Vector2Int[] CellsToLockAndDeferred;  // Deferred locked cells
}
```

**Fields**:
- **`LockedAreaId`**: Unique identifier to distinguish areas.
- **`CellsToLock`**: Cell coordinates that get locked, but chips on them load instantly (if defined in `FieldChipData`).
- **`CellsToLockAndDeferred`**: Cell coordinates where chips do not load until unlocked. `FieldGrid` automatically adds a `DeferredCell` component to the regular Cell GameObject.

**FieldData Integration**:
```csharp
public class FieldData : ScriptableObject
{
    [SerializeField] private LockedAreaData[] lockedAreas;
    public LockedAreaData[] LockedAreas => lockedAreas;

#if UNITY_EDITOR
    // Editor-only: called by LevelEditorWindow when saving a level
    public void SetLockedAreas(LockedAreaData[] newLockedAreas) => lockedAreas = newLockedAreas;
#endif
}
```

---

## Initialization and Lifecycle

### Scene Initialization Sequence (Merge2Initializer)

During scene launch, a sequence of steps occurs:

#### Step 1: `FieldInitializeCommand.CreateField()`
Creates the cell grid:

1. **Build Deferred Cells Set** (`BuildDeferredCellsSet`).
2. **Cell Instantiation**:
   - Spawns regular `Cell` or a custom prefab from `CellPrefabCollection`.
   - For coordinates in `deferredCellsSet`, **adds the `DeferredCell` component** to the same `Cell` GameObject.
   - Regular cells remain without this component.
3. **DI Injection**: Each cell (and `DeferredCell`) receives dependencies via `resolver.Inject()`.

#### Step 2: `LockedAreaManager.Initialize()`
Locks cells based on `FieldData.LockedAreas`:

```csharp
foreach (FieldData.LockedAreaData area in fieldData.LockedAreas)
{
    if (!unlockedAreaIds.Contains(area.LockedAreaId))
    {
        SetAreaBlocked(area, true);
    }
}
```

> **Important**: At this step, `LockedAreaEffect` components **are not yet registered**. Effects register later — in Step 3.

#### Step 3: `FieldInitializeCommand.CreateLevelVisual()`
Instantiates `LevelVisualPrefab` and initializes level visualization:

1. Spawns `fieldData.LevelVisualPrefab` via `resolver.Instantiate()`.
2. Calls `IVisualField.InitVisualField()` on the root component.
3. **Each `LockedAreaEffect`** during its initialization (`Init`) registers itself in the manager:
   ```csharp
   this.effectId = effectId;
   lockedAreaManager.RegisterEffect(this);
   ```
4. `RegisterEffect` **instantly synchronizes** the effect state — calling `Activate(null)` or `Deactivate(null, force: true)` based on the current `unlockedAreaIds` state.

#### Step 4: `FieldInitializeCommand.LoadChips()`
Loads chips from `FieldData`:

1. **For regular cells**: Spawns the chip instantly via `ChipFactory.CreateChip(cell, chipData)`.

2. **For cells with `DeferredCell` component**:
   ```csharp
   if (DeferredCell.TryGet(cell, out var deferredCell))
       deferredCell.SetupDeferredChip(cellData, chipData);
   else
       ChipFactory.CreateChip(cell, chipData);
   ```
   - `SetupDeferredChip()` stores configuration but does not create the chip.
   - The chip will be spawned later in `LockedAreaManager.UnlockArea()`.

---

## Unlocking and Spawning Deferred Chips

### `UnlockArea(int areaId, bool force = false)` Method

Unlocks the area and triggers all related actions:

```csharp
public void UnlockArea(int areaId, bool force = false)
{
    if (unlockedAreaIds.Contains(areaId) && !force)
        return;

    if (!TryGetArea(areaId, out FieldData.LockedAreaData area))
    {
        Debug.LogWarning($"LockedAreaManager: Area {areaId} not found");
        return;
    }

    // 1. Unlock all cells of the area
    unlockedAreaIds.Add(areaId);
    SetAreaBlocked(area, false);

    // 2. Spawn deferred chips (only for CellsToLockAndDeferred)
    SpawnDeferredChips(area);

    // 3. Deactivate visual effects
    DeactivateEffects(areaId, force);
}
```

**Parameters**:
- **`areaId`**: ID of the area to unlock.
- **`force`** (optional): Overrides unlock condition checks (if any).

**Sequence of Events**:

1. **Cell Unlocking** (`IsBlocked = false`):
   - `Cell.OnTap`, `Cell.OnDragStart`, `Cell.OnDrag`, `Cell.OnDragEnd` no longer ignore input.
   - `FieldGrid` allows placing chips on these cells.

2. **Deferred Chip Spawning** (`DeferredCell.SpawnDeferredChip()`):
   - Calls `ChipFactory.CreateChip(ownerCell, deferredChipData, ...)`
   - The chip is placed in the cell via the owner (`ownerCell`).
   - Copies `BlockerEffectIds` state from source `FieldData`.
   - Triggers all standard events (CellObserver notifications, Init, InitEffects).

3. **Effect Deactivation** (`LockedAreaEffect.Deactivate(null, immediate)`):
   - Animator trigger `"Deactivate"` is fired.
   - If `force=true` (passed from `UnlockArea` parameter), the animation can play instantly.
   - The GameObject remains in the scene (or gets destroyed upon animation completion, depending on implementation).

---

## Level Editor Integration

The configuration of locked grid areas, deferred chips (Deferred Cells), as well as corresponding editor modes, grid painting, and Undo/Redo commands are described in the general **[Editors](../Editors.md)** section.

---

## Data Validation

### Validation Rules (FieldData.ValidateLevel)

Before saving a level, the following are verified:

1. **Grid Bounds**: All cell coordinates in `LockedAreaData` are checked separately for `CellsToLock` and `CellsToLockAndDeferred`:
   ```csharp
   foreach (var cell in area.CellsToLock)
   {
       if (cell.x < 0 || cell.x >= fieldSize.x || cell.y < 0 || cell.y >= fieldSize.y)
           errors.Add($"Locked Area {area.LockedAreaId}: cell {cell} is outside field bounds {fieldSize}");
   }
   foreach (var cell in area.CellsToLockAndDeferred)
   {
       if (cell.x < 0 || cell.x >= fieldSize.x || cell.y < 0 || cell.y >= fieldSize.y)
           errors.Add($"Locked Area {area.LockedAreaId}: deferred cell {cell} is outside field bounds {fieldSize}");
   }
   ```

2. **Mutual Exclusivity**: No cell should reside simultaneously in both `CellsToLock` and `CellsToLockAndDeferred` of the same area:
   ```csharp
   foreach (var cell in area.CellsToLock)
   {
       if (Array.IndexOf(area.CellsToLockAndDeferred, cell) >= 0)
           errors.Add($"Locked Area {area.LockedAreaId}: cell {cell} exists in both CellsToLock and CellsToLockAndDeferred");
   }
   ```

3. **ID Uniqueness**: Verification of `LockedAreaId` uniqueness is performed at the editor level (auto-increment when adding a new area via `+`). `ValidateLevel` does not implement this check.

---

## VContainer Registration

### Registration in LifetimeScope

```csharp
// In Merge2LifetimeScope
public class Merge2LifetimeScope : LifetimeScope
{
    protected override void Configure(IContainerBuilder builder)
    {
        // ... other registrations ...
        
        builder.Register<LockedAreaManager>(Lifetime.Singleton)
            .As<ILockedAreaManager>();
        
        builder.RegisterEntryPoint<Merge2Initializer>();
    }
}
```

### Initialization in Merge2Initializer

```csharp
// Merge2Initializer implements IInitializable (VContainer), not MonoBehaviour
public class Merge2Initializer : IInitializable
{
    [Inject] private readonly ILockedAreaManager lockedAreaManager;
    [Inject] private readonly IFieldInitializeCommand fieldInitializeCommand;
    
    public void Initialize()
    {
        // 1. Create cell grid + DeferredCell components
        fieldInitializeCommand.CreateField();
        
        // 2. Block cells according to FieldData.LockedAreas
        lockedAreaManager.Initialize();
        
        // 3. Spawn level visualization and register LockedAreaEffects
        fieldInitializeCommand.CreateLevelVisual();
        
        // 4. Load chips (deferred are saved, not spawned)
        fieldInitializeCommand.LoadChips();
    }
}
```

**Important**: The step order is critical:
- `Initialize()` **after** `CreateField()` — cells must exist before blocking.
- `CreateLevelVisual()` **after** `Initialize()` — effects register with already known blocking state and instantly sync via `RegisterEffect`.
- `LoadChips()` **last** — deferred cells are already blocked and ready to receive `SetupDeferredChip`.

---

## Example: Unlocking an Area During Gameplay

```csharp
// In any gameplay component
public class LevelController : MonoBehaviour
{
    [Inject] private readonly ILockedAreaManager lockedAreaManager;
    
    public void OnQuestCompleted(int questAreaId)
    {
        // Unlock area after quest completion
        lockedAreaManager.UnlockArea(questAreaId);
        
        // Logging (optional)
        Debug.Log($"Area {questAreaId} unlocked!");
    }
}
```
