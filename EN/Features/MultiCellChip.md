# Multi-cell Chips

[← Back to Main](../Main.md) | [Chip Relocation →](ChipRelocation.md)

A distinctive feature of the project is support for chips occupying multiple grid cells (e.g., 1x2, 2x2). This adds a significant layer of complexity to all game mechanics, from rendering to movement logic.

## Technical Structure

### Size Definition
Chip size is defined in `ChipData` by the `Vector2Int Size` property.
- Minimum size: 1x1.
- Orientation: `x` - width (columns), `y` - height (rows).
- **Validation**: On load `OnEnable`, it is guaranteed that sizes are not less than one unit.

### Anchor System (Top-Left Anchor)
To simplify positioning, an "anchor" system is used:
- **Main Cell (Anchor)**: The chip's logical position is always bound to the *top-left* cell of the occupied area.
- **Secondary Cells**: The remaining cells covered by the chip.
- The `Chip` class does not have a list of all cells. Instead, the ***Field Grid*** is responsible for storing a reference to this `Chip` in all cells it covers.

### Grid Occupancy
When a chip is placed at position `(x, y)` with size `(w, h)`:
1. The system checks field bounds and occupancy of all cells in the range `[x ... x+w-1, y ... y+h-1]`.
2. A reference to the `Chip` instance is written to each cell in this range.
3. This allows instant access to the chip by clicking on any of its parts.

## Technical Challenges and Solutions

### 1. Position Validation
**Challenge**: Checking "is a cell free" for 1x1 is trivial. For NxM, you need to check an area.
**Solution**:
- Method `IFieldGrid.IsValidCellPos(Vector2Int pos, Vector2Int size)`.
- Checks array bounds for *all* potential cells.
- Checks whether cells are not blocked (if there is an obstacle mechanic).

### 2. Interaction (Input Redirection)
**Challenge**: Player can tap or start dragging the "tail" of a large chip (secondary cell).
**Solution**:
- Each `Cell` has a `Chip` field.
- With any cell interaction, the logic works with the obtained `Chip` reference.
- For movement operations, the **Main Cell** is always calculated (as `currentCell.Position - relativeOffset`), or the `Chip` stores its position relative to the anchor.
- In the current implementation, `Cell` knows its `Chip`, and movement methods (`MoveTo`) operate on the target `MainCell`.

### 3. Space Conflicts (Relocation)
**Challenge**: When moving a large chip, it can touch multiple smaller chips. A simple "swap" rule does not work here because there are many chips and they may not fit in the large chip's old space.
**Solution**: Extracted into a separate complex system **[Chip Relocation](ChipRelocation.md)**, which finds free space for all affected neighbors.


## Interaction with Gameplay

### Merge into Larger Chip (Growth Merge)
A special merging case where the result is larger than the original chips (e.g., 1x1 + 1x1 -> 2x2).

**Algorithm (`MergeableChipLogic`):**
1. **Bounds Check**: If the new chip goes out of bounds (e.g., merge occurs in the far-right column), the system calculates a new anchor position (`newCellPosition`), effectively shifting the chip within bounds.
2. **Relocation Check**:
   - Calls `chipMovingLogic.CanChipMoving` for the new chip shape.
   - Adds both merging chips to the exclusion list (`chipsToExclude`) because they will disappear.
3. **Result**:
   - **Success**: If space is found for the new chip (and for all neighbors that need to be pushed), old chips are destroyed, neighbors are relocated, and a new chip is created.
   - **Rollback**: If relocation is impossible (no space for neighbors), the merge is canceled (`return false`), and chips remain in place. The player sees that the merge did not happen.

- **Spawn**: Generators (`ChipGenerator`) must account for the size of the chip they spawn. If there is no space for a 2x2, spawning is blocked even if the center cell is free.
- **Merge**: The merge result inherits the position (Anchor) of the chip *onto which* the other was thrown. This is important for predictability.
