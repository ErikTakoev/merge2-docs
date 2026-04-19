# Chip Relocation

[← Back to Main](../Main.md) | [← Multi-cell Chips](MultiCellChip.md)

Relocation is a mechanism of automatically moving blocking chips into neighboring free cells to free space for a new or moved chip. This is a critically important system for supporting gameplay with multi-cell chips.

## The Problem
In a game with chips of different sizes (1x1, 1x2, 2x2, etc.), space conflict situations often occur:
1. **Merge**: Two 1x1 chips merge into a 1x2 chip that needs more space.
2. **Drag**: Player tries to place a large chip into a space partially occupied by other chips.

Without relocation, player would have to clear space manually, which worsens UX.

## Technical Implementation

Relocation logic is handled by `ChipMovingLogic` class implementing `IChipMovingLogic` interface.

### `CanChipMoving` Algorithm

The process of checking movement possibility and planning relocation works as follows:

1. **Target Position Validation**:
   - System checks whether target cell (`leftTopCell`) is valid at all for placing a chip of given size (`fieldGrid.IsValidCellPos`).

2. **Conflict Detection**:
   - `GetOccupiedCellsInArea` method finds all chips currently occupying cells in the area where new chip wants to be.
   - For each conflicting chip, **`chip.CanMoving()`** is checked — if the chip is MoveLocked (e.g., has a `MoveLockedEffect` blocking effect), the entire process is immediately cancelled (`return false`): neither movement nor relocation occurs.
   - A unique list of chips for potential movement is created (`chipsToPotentiallyMove`).

3. **Priority Sorting**:
   - Chips for movement are sorted by size (area) in descending order: `(bSize.x * bSize.y) - (aSize.x * aSize.y)`.
   - **Reason**: Large chips are harder to place, so space is searched for them first to minimize field fragmentation.

4. **Movement Planning (Simulation)**:
   - `plannedRelocations` list of planned actions is created.
   - `unavailableCellsForRelocation` set is created, and cells that will be occupied by new chip are added immediately.
   - `freeCellFinder.FindNearestFreeCell` is called for each conflicting chip.
   - **Important**: If a new place is found, its cells are also added to `unavailableCellsForRelocation` so that next chips in the same cycle do not try to occupy the same place.

5. **Atomicity**:
   - If free place is not found for at least one conflicting chip, whole process is considered impossible (`return false`), and no changes happen on the field.

### Relocation Execution (`ChipsRelocate`)

If `CanChipMoving` returned `true`, `ChipsRelocate` method runs:
1. **Cleanup**: References to moving chip (Source) and all chips to be relocated are removed from cells.
2. **Movement**:
   - Relocated chips receive new coordinates.
   - They are temporarily set to `SetDragging(true)` for visual correctness (layer sorting).
   - Grid is updated (`fieldGrid.SetChipInCell`).
3. **Main Chip Placement**: Chip that initiated process is placed to its new location.

## Interaction with Other Systems

### 1. Drag & Drop
At drag end (`OnDragEnd` in `DraggableChipLogic`):
- `ChipMovingLogic.ChipMoving` is called.
- If relocation is possible, chips are "pushed apart".
- If not — chip dragged by player returns to previous location ("snap back").

### 2. Merge (Merging)
During merge, when chip evolves and increases in size:
- System checks whether there is enough space for new shape.
- If not — search for space for neighbors starts.
- This allows chip to "grow" by moving surrounding objects.

## Used Structures
```csharp
public struct ChipMoveAction
{
    public Chip ChipToMove;
    public Cell FromCell; // Initial MainCell
    public Cell ToCell;   // Target MainCell
}
```
