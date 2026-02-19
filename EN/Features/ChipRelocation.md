# Chip Relocation

[← Back to Main](../Main.md) | [← Multi-cell Chips](MultiCellChip.md)

Relocation is a mechanism for automatically moving chips that are in the way to neighboring free cells to clear space for a new or moved chip. It is a critically important system for supporting gameplay with multi-cell chips.

## The Problem
In a game with chips of different sizes (1x1, 1x2, 2x2, etc.), space conflict situations frequently arise:
1. **Merge**: Two 1x1 chips merge into a 1x2 chip, which needs more space.
2. **Dragging**: Player tries to place a large chip in a space partially occupied by other chips.

Without relocation, the player would have to manually clear space, which worsens UX.

## Technical Implementation

The `ChipMovingLogic` class is responsible for relocation logic, implementing the `IChipMovingLogic` interface.

### `CanChipMoving` Algorithm

The process of checking the possibility of movement and planning relocation is as follows:

1. **Target Position Validation**:
   - The system checks whether the target cell (`leftTopCell`) is even valid for placing a chip of a given size (`fieldGrid.IsValidCellPos`).

2. **Determining Conflicts**:
   - The `GetOccupiedCellsInArea` method finds all chips currently occupying cells in the zone where the new chip wants to go.
   - A list of unique chips for potential movement is created (`chipsToPotentiallyMove`).

3. **Priority Sorting**:
   - Chips for movement are sorted by size (area) in descending order: `(bSize.x * bSize.y) - (aSize.x * aSize.y)`.
   - **Reason**: Large chips are harder to place, so space is found for them first to minimize field fragmentation.

4. **Movement Planning (Simulation)**:
   - `plannedRelocations` is created — a list of planned actions.
   - A set `unavailableCellsForRelocation` is created, immediately adding cells that will be occupied by the new chip.
   - For each conflicting chip, `freeCellFinder.FindNearestFreeCell` is called.
   - **Important**: If a new location is found, its cells are also added to `unavailableCellsForRelocation` so subsequent chips in this cycle do not try to occupy the same space.

5. **Atomicity**:
   - If space is not found for at least one conflicting chip, the entire process is considered impossible (`return false`), and no changes occur on the field.

### Relocation Execution (`ChipsRelocate`)

If `CanChipMoving` returned `true`, the `ChipsRelocate` method is executed:
1. **Clearing**: References to the moving chip (Source) and all chips being relocated are removed from cells.
2. **Movement**:
   - Relocating chips receive new coordinates.
   - They are temporarily set to `SetDragging(true)` state for visual correctness (layer ordering).
   - The grid is updated (`fieldGrid.SetChipInCell`).
3. **Main Chip Placement**: The chip that initiated the process is placed in its new location.

## Interaction with Other Systems

### 1. Drag & Drop
When dragging ends (`OnDragEnd` in `DraggableChipLogic`):
- `ChipMovingLogic.ChipMoving` is called.
- If relocation is possible, chips are "pushed apart".
- If not — the chip the player was dragging is returned to its previous position ("snap back").

### 2. Merge (Merging)
When merging, when a chip evolves and increases in size:
- The system checks if there is enough space for the new form.
- If not — starts searching for space for neighbors.
- This allows a chip to "grow" by pushing its surroundings.

## Used Structures
```csharp
public struct ChipMoveAction
{
    public Chip ChipToMove;
    public Cell FromCell; // Starting MainCell
    public Cell ToCell;   // Target MainCell
}
```
