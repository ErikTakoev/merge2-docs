# Editors

[← Home](Main.md)

# Level Editor

This tool is designed for creating and editing Merge2 game levels directly in the Unity Editor.

## Main Features
- **Palette Panel**: Select chips to place on the field (left panel).
- **Grid Editor**: Visual area for placing chips using Drag-and-Drop (center panel).
- **Properties Panel**: Configure level parameters (grid size, chip collection) and validation (right panel).
- **Undo/Redo System**: Ability to undo and redo any editing actions.

## Undo/Redo System
The editor supports a full action history, allowing you to safely experiment with level design.

### Hotkeys
- **Ctrl + Z**: Undo the last action.
- **Ctrl + Y** or **Ctrl + Shift + Z**: Redo the undone action.

### Supported Actions
- Placing chips on the field.
- Moving chips between cells.
- Removing chips.
- Toggling chip "Move Lock" status.
- Changing grid size (Apply Grid Resize).
- Clearing the entire field (Clear All Chips).

## Technical Implementation
The Undo/Redo system is built on the **Command** pattern.

### Key Components
- **`IEditorCommand`**: Interface for all editor commands. Each command implements execution (`Execute`) and rollback (`Undo`) logic.
- **`EditorCommandHistory`**: Manager class that controls two stacks (Undo and Redo).
- **`LevelEditorWindow.UndoRedo.cs`**: A partial class containing definitions for all concrete commands:
    - `PlaceChipCommand` — placing a new chip.
    - `MoveChipCommand` — moving an existing chip.
    - `RemoveChipCommand` — removing a chip.
    - `ToggleMoveLockCommand` — toggling the lock flag.
    - `ResizeGridCommand` — changing grid size with out-of-bounds chip removal.
    - `ClearAllChipsCommand` — full field clearing.

### Workflow (Flow)
Instead of directly changing the `placedChips` or `gridSize` fields, editor methods (e.g., `PlaceChip`) create the corresponding command object and call `RecordAndExecute(command)`. This method delegates execution to `EditorCommandHistory`, which adds the command to the stack and calls `Repaint()`.
