# MergeableChipLogic

[← Back to Main](../Main.md) | [← To ChipInteractions](ChipInteractions.md)

`MergeableChipLogic` is the main implementation of `IChipInteractionLogic` that handles merging two chips.

## Purpose
Handling the process of combining compatible chips to get a result of the next tier or type.

## How it Works

### Validation (CanInteract)
Before merging, the system checks:
1. Whether both cells have chips.
2. Whether they are not the same chip.
3. Whether the chip being dragged has merge data (`MergeData`).
4. Whether `MergeData` allows merging with the target chip via the `CanMerge` method.

### Execution (ExecuteInteraction)
When merging is confirmed:
1. **Determining Result**: Gets `MergeResult` based on source and target chip data.
2. **Position Check**: If the result chip is larger (e.g., [Multi-cell Chip](../Features/MultiCellChip.md)), the system checks if it fits within field bounds.
3. **Neighbor Relocation**: If the new chip needs more space, the [Chip Relocation](../Features/ChipRelocation.md) system is called to free up space.
4. **Field Update**:
   - Previous chips are destroyed.
   - New chip is created via `ChipFactory`.
   - `Merge` animation is started.
5. **Extra Result**: If `ExtraChip` is specified in settings, there is a chance of a bonus chip appearing on a neighboring free cell.

## Key Dependencies
- `ChipFactory`: Creation of new objects.
- `IFieldGrid`: Working with field coordinates.
- `IChipMovingLogic`: Moving neighboring chips when size changes.
- `IFreeCellFinder`: Finding space for bonus chips.

> [!NOTE]
> If `ExecuteInteraction` returns `false` (e.g., unable to push neighbors for a large chip), `DraggableChipLogic` will return the chip to its original position.
