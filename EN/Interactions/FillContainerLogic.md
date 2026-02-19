# FillContainerLogic

[← Back to Main](../Main.md) | [← To ChipInteractions](ChipInteractions.md)

`FillContainerLogic` is an implementation of `IChipInteractionLogic` that allows chips to interact with containers.

## Purpose
Allows the player to drag an ingredient chip onto a container chip to fill it and complete tasks.

## How it Works

### Validation (CanInteract)
The system allows interaction if:
1. The target chip is a descendant of the `ChipContainer` class.
2. The source chip is compatible with this container (checked via `chipContainer.IsChipCompatible`).

### Execution (ExecuteInteraction)
Upon interaction completion:
1. Calls `TryAddChip` on the container.
2. If the container successfully accepts the chip (it meets quest conditions and the container has space):
   - Source chip is destroyed.
   - Container updates its internal state.

## More Details on Containers
The fill logic is quite simple, as it delegates most work to the container object itself. For information on how to configure ingredient types and rewards, see the main section:
- **[ChipContainer](../Chips/ChipContainer.md)**
