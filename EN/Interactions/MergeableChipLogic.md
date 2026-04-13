# MergeableChipLogic

[← Back to Main](../Main.md) | [← To ChipInteractions](ChipInteractions.md)

`MergeableChipLogic` is the main `IChipInteractionLogic` implementation responsible for merging two chips.

## Purpose
Handles the process of combining compatible chips to get a new level or type result.

## How it Works

### Validation (CanInteract)
Before starting merge, system checks:
1. Whether both cells have chips.
2. Whether they are not the same chip.
3. Whether dragged chip has permissions to merge as source: `sourceChip.BlockingState.CanBeMergedAsSource`.
4. Whether target chip has permissions to merge as target: `targetChip.BlockingState.CanBeMergedAsTarget`.
5. Whether dragged chip has merge configuration: `sourceChip.MergeData != null`.
6. Whether `sourceChip.MergeData` allows merge with target chip through `CanMerge` method.

### Execution (ExecuteInteraction)
When merge is confirmed:
1. **Result definition**: `MergeResult` is obtained via `sourceCell.Chip.MergeData.GetNextChip(mainCell.Chip.Data)`.
2. **Position check**: If result chip is larger (for example, [Multi-cell Chip](../Features/MultiCellChip.md)), system checks whether it fits field bounds.
3. **Neighbor relocation**: If new chip requires more space, [Chip Relocation](../Features/ChipRelocation.md) system is called to free space. If freeing space on main position fails, system automatically checks all 8 neighboring cells to find an alternative placement.
4. **Field update**:
   - Previous chips are destroyed.
   - New chip is created through `ChipFactory`.
   - `Merge` animation is started.
5. **Extra result**: If `ExtraChip` is specified in settings, there is a chance to spawn a bonus chip on neighboring free cell.

## Key Dependencies
- `ChipFactory`: Creating new objects.
- `IFieldGrid`: Working with field coordinates.
- `IChipMovingLogic`: Moving neighboring chips when size changes.
- `IFreeCellFinder`: Finding place for bonus chips.

## Result Calculation: Weighted Random

`ChipMergeData` defines all possible results for each chip pair through structures:

1. **`MergeCombination`**: Rules for a chip pair
   - **`TargetChip`**: Which partner is required
   - **`Results`**: List of possible results with weights

2. **`MergeResult`**: Describes one possible result
   - **`Result`**: Which chip should be created
   - **`Weight`**: Weight of this result (int)
   - **`ExtraChip`**: Optional bonus chip

3. **`ExtraChip`**: Optional bonus chip
   - **`ChipData`**: What to spawn  
   - **`Chance`**: Appearance chance (0-100%)

### Algorithm

System uses **Weighted Random Selection** algorithm:
1. All `Results` for pair (sourceChip, targetChip) are collected
2. One result is randomly selected based on their `Weight`
3. If there is one result - it is guaranteed
4. If there are no results for this pair, merge is impossible (CanInteract returns `false`)

## Extra Chips

If `ExtraChip` is defined in `MergeResult`:
1. System generates random number (0-100)
2. If number ≤ `Chance`, chip is spawned
3. Spawn location: nearest free neighboring cell (via `IFreeCellFinder`)
4. If there are no free cells, bonus does not appear

> [!NOTE]
> If `ExecuteInteraction` returns `false` (for example, failed to push neighbors for a large chip), `DraggableChipLogic` will return chip to the original position.
