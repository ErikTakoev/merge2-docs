# Chip Locking

[← Back to Main](../Main.md)

## Overview
**Chip Locking** is a mechanic that allows you to fix chips on the game field, preventing the player from moving them. This creates an additional level of difficulty on levels where the player has to work around locked objects.

## Technical Implementation

### Data (FieldData)
The lock state is stored directly in the `CellData` structure inside `FieldData`:

```csharp
public struct CellData
{
    public FieldChipData FieldChipData; // Contains ChipId and effects (IsMoveLocked)
    public Vector2Int Position;
}
```

### Runtime (Chip)
The `Chip` class contains `ChipRuntimeData` which is synchronized with level data on initialization:

- **`runtimeData.IsMoveLocked`**: Boolean value determining whether dragging is possible.
- **`UpdateRuntimeData()`**: Method for visual sync of lock effect.
- **`CanMoving()`**: Validation method that returns `false` if the chip is locked.

### Visualization
To visually display the locked state, **MoveLockedEffect** is used:
- When activated (via `UpdateRuntimeData`), it displays a special visual element (e.g., chains or a padlock).
- If the player tries to drag a locked chip, a "DontMove" animation is triggered, providing negative feedback.

## Level Editor
Lock mechanics are integrated into the visual level editor:

1.  **Context Menu**: Right-click on a chip in the grid opens a menu with the option **"Lock Move"** or **"Unlock Move"**.
2.  **Visual Indicator**: In editor mode, locked chips are marked with a 🔒 icon in the lower right corner of the cell.
3.  **Saving**: Lock state is automatically saved to the field Asset when pressing "Save".

## Use in Gameplay
Locked chips:
- Cannot be moved by the player (DRAG is blocked).
- Can participate in merging (if provided by specific chip type logic), but do not initiate movement themselves.
- Will be ignored by the **Chip Relocation** system, as their position is considered static ("pinned").
