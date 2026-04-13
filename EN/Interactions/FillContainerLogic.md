# FillContainerLogic

[← Back to Main](../Main.md) | [← To ChipInteractions](ChipInteractions.md)

`FillContainerLogic` is an `IChipInteractionLogic` implementation that allows chips to interact with containers.

## Purpose
Allows player to drag an ingredient chip onto a container chip to fill it and complete tasks.

## How it Works

### Validation (CanInteract)
System allows interaction if:
1. Target chip is a descendant of `ChipContainer` class.
2. Container has no fill restrictions (`targetChip.BlockingState.CanBeFilled`).
3. Source chip allows using itself as resource (`sourceChip.BlockingState.CanBeMergedAsSource`).
4. Source chip is compatible with this container (checked through `chipContainer.IsChipCompatible`).

### Execution (ExecuteInteraction)
When interaction is completed:
1. `TryAddChip` is called on container.
2. If container accepted the chip successfully (it matches quest conditions and container has space):
   - Source chip is destroyed.
   - Container updates its internal state.

## More Details on Containers
Fill logic itself is very simple because it delegates most work to container object itself. For information on how to configure ingredient types and rewards, see main section:
- **[ChipContainer](../Chips/ChipContainer.md)**
