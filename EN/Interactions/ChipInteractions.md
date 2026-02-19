# ChipInteractions (Interactions)

[← Back to Main](../Main.md)

This section describes the mechanism by which general chip dragging logic (`DraggableChipLogic`) delegates execution of specific interactions (such as merging or filling a container) through the `IChipInteractionLogic` interface.

## Architecture Overview

The interaction system is built on the principle of decoupling. `DraggableChipLogic` is responsible for physical chip movement and detecting target objects under it, but it "does not know" the details of what exactly should happen when one chip overlaps another.

Instead of hardcoding merge logic into the main dragging class, the system uses a list of handlers implementing the `IChipInteractionLogic` interface.

## Transition Process

### 1. Initialization
On startup (`Awake`), `DraggableChipLogic` collects all components on the object that implement `IChipInteractionLogic`:
```csharp
IChipInteractionLogic[] interactionLogics = GetComponents<IChipInteractionLogic>();
```

### 2. Continuous Checking (OnDrag)
During dragging, in the `UpdateInteractionState` method, the system continuously checks available interaction logics:
- Defines a list of cells occupied by the chip (considering chip size).
- For each logic, calls `CanInteract(sourceCell, targetCell)`.
- If the logic returns `true`, it is set as `currentMergableLogic`.

### 3. Interaction Completion (OnDragEnd)
When the player releases the chip:
- If an active `currentMergableLogic` is found, `ExecuteInteraction(sourceCell, targetCell)` is called.
- If the interaction is successful, processing ends.
- If no interaction is found or is impossible, the chip is returned to its place or moved (Swap/Move) via `IChipMovingLogic`.

## IChipInteractionLogic Interface

Each implemented handler must have two main methods:
- `CanInteract`: Checks validity (e.g., whether chip IDs match for merging).
- `ExecuteInteraction`: Directly changes game state (destroys old chips, creates new ones).

## Implementations
- **[MergeableChipLogic](MergeableChipLogic.md)** — Logic for merging two chips into one of the next tier.
- **[FillContainerLogic](FillContainerLogic.md)** — Logic for adding a chip to a container.
