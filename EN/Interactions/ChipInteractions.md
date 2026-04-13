# ChipInteractions (Interactions)

[← Back to Main](../Main.md)

This section describes the mechanism by which general chip dragging logic (`DraggableChipLogic`) delegates execution of specific interactions (such as merge or container filling) through the `IChipInteractionLogic` interface.

## Architecture Overview

The interaction system is built on the decoupling principle. `DraggableChipLogic` is responsible for physical chip movement and for detecting target objects under it, but it does not know the details of what exactly should happen when one chip overlaps another.

Instead of hardcoding merge logic into the main dragging class, the system uses a list of handlers implementing `IChipInteractionLogic` interface.

## Transition Process

### 1. Initialization
On startup (`Awake`), `DraggableChipLogic` collects all components on the object that implement `IChipInteractionLogic`:
```csharp
IChipInteractionLogic[] interactionLogics = GetComponents<IChipInteractionLogic>();
```

### 2. Continuous Checking (OnDrag)
During dragging, in `UpdateInteractionState` method, the system continuously checks available interaction logics:
- List of cells occupied by chip is determined (considering chip size).
- `CanInteract(sourceCell, targetCell)` is called for each logic.
- If logic returns `true`, it is set as `currentMergableLogic`.

### 3. Interaction Completion (OnDragEnd)
When player releases chip:
- If active `currentMergableLogic` is found, `ExecuteInteraction(sourceCell, targetCell)` is called.
- If interaction is successful, processing completes.
- If interaction is not found or impossible, chip returns to place or is moved (Swap/Move) through `IChipMovingLogic`.

## IChipInteractionLogic Interface

Each implemented handler must have two main methods:
- `CanInteract`: Validity check (for example, whether chip identifiers are the same for merge).
- `ExecuteInteraction`: Direct game state change (destroy old chips, create new ones).

## Implementations
- **[MergeableChipLogic](MergeableChipLogic.md)** — Logic of merging two chips into one higher-level chip.
- **[FillContainerLogic](FillContainerLogic.md)** — Logic of adding a chip into a container.
