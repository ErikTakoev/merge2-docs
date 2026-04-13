# Multi-cell Chips

[← Back to Main](../Main.md) | [Relocation Mechanics →](ChipRelocation.md)

A feature of the project is support for chips that occupy more than one grid cell (for example, 1x2, 2x2). This adds a significant layer of complexity to all game mechanics, from rendering to movement logic.

## Technical Structure

### Size Definition
Chip size is defined in `ChipData` by `Vector2Int Size` property.
- Minimum size: 1x1.
- Orientation: `x` - width (columns), `y` - height (rows).
- **Validation**: On load, `OnEnable` guarantees that size is not less than one.

### Anchor System (Top-Left Anchor)
For easier positioning, an anchor system is used:
- **Main Cell (Anchor)**: Logical chip position is always bound to the *top-left* cell of occupied area.
- **Secondary Cells**: Other cells covered by chip.
- `Chip` class has no list of all cells. Instead, ***Field Grid*** is responsible for having a reference to this `Chip` in all cells covered by chip.

### Grid Occupancy
When chip is placed at `(x, y)` with size `(w, h)`:
1. System checks grid bounds and occupancy of all cells in range `[x ... x+w-1, y ... y+h-1]`.
2. Reference to `Chip` instance is written into each cell of this range.
3. This allows instant access to chip when clicking any of its parts.

## Technical Challenges and Solutions

### 1. Position Validation
**Complexity**: Checking "is cell free" is trivial for 1x1. For NxM you need to check area.
**Solution**:
- `IFieldGrid.IsValidCellPos(Vector2Int pos, Vector2Int size)` method.
- Checks out-of-bounds for *all* potential cells.
- Checks that cells are not blocked (if obstacle mechanics exists).

### 2. Interaction (Input Redirection)
**Complexity**: Player may tap or start drag by the "tail" of a large chip (secondary cell).
**Solution**:
- Each `Cell` has `Chip` field.
- On any interaction with cell, logic works with obtained reference to `Chip`.
- For movement operations, **Main Cell** is always computed (as `currentCell.Position - relativeOffset`), or `Chip` stores its position relative to anchor itself.
- In current implementation, `Cell` knows its `Chip`, and movement methods (`MoveTo`) operate on target `MainCell`.

### 3. Space Conflicts (Relocation)
**Complexity**: When a large chip is moved, it may hit several smaller chips. A simple "swap" rule does not work here because there can be many chips and they may not fit into the old place of large chip.
**Solution**: Extracted into a separate complex **[Chip Relocation](ChipRelocation.md)** system, which searches free places for all affected neighbors.


## Interaction with Gameplay

### Merge into Larger Chip (Growth Merge)
Special merge case when result has larger size than source chips (for example, 1x1 + 1x1 -> 2x2).

**Algorithm (`MergeableChipLogic`):**
1. **Bounds check**: If new chip goes out of field bounds (for example, merge happens in rightmost column), system computes new anchor position (`newCellPosition`), effectively shifting chip within field.
2. **Relocation check**:
   - `chipMovingLogic.CanChipMoving` is called for new chip shape.
   - Both chips participating in merge are added to exclusion list (`chipsToExclude`) (because they will disappear).
3. **Result**:
   - **Success**: If space is found for new chip (and all neighbors that need to be moved), old chips are destroyed, neighbors are relocated, and new chip is created.
   - **Cancel (Rollback)**: If relocation is impossible (no space for neighbors), merge is canceled (`return false`), and chips stay in place. Player sees that merge did not happen.

- **Spawn**: Generators (`ChipGenerator`) must consider size of chip they spawn. If there is no output space for 2x2, spawn is blocked even if center cell is free.
- **Merge**: Merge result inherits anchor position of the chip *onto which* another was dropped. This is important for predictable results.
