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
- **Ctrl + Y** or **Ctrl + Shift+ Z**: Redo the undone action.

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

---

# Chip Creator (Chip Viewer)

This tool provides a centralized interface for browsing, creating, and modifying `ChipData` assets.

## Main Features
- **Chip List**: Browse all chips in the project with thumbnails (left panel).
- **Properties Editor**: Inspect and edit serialized chip data (right panel).
- **Undo/Redo System**: Full support for reversing property changes and renaming.
- **Asset Management**: Buttons for creating new chips, saving changes, and refreshing the list.

## Undo/Redo System
The Chip Creator uses the same **Command** pattern as the Level Editor to track modifications.

### Hotkeys
- **Ctrl + Z**: Undo the last change.
- **Ctrl + Y** or **Ctrl + Shift + Z**: Redo the undone change.

### Supported Actions
- **Property Edits**: All changes made to the `SerializedObject` during an inspector edit.
- **Renaming**: Renaming a chip asset (also renames the linked prefab).

## Technical Implementation

### Key Components
- **`IChipCreatorCommand`**: Interface for commands that manage `ChipData` state.
- **`ChipCreatorCommandHistory`**: Manages the undo/redo stacks specifically for the chip editing context.
- **`ChipCreatorWindow.UndoRedo.cs`**: Contains the command definitions:
    - `SetChipPropertyCommand` — records before/after JSON snapshots of the `ChipData` asset.
    - `RenameChipCommand` — handles asset renaming for both the `ChipData` and its prefab.

### Workflow (Flow)
When a property is modified in the `DrawRightPanel` method, the window creates a `SetChipPropertyCommand` with the capture of the asset states. For renaming, a `RenameChipCommand` is used. These are passed to `RecordAndExecute(command)`, which updates the history and ensures the `SerializedObject` is refreshed.
