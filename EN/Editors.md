# Editors

[← Back to Main](Main.md)

# Level Editor

This tool is intended for creating and editing Merge Toolkit game levels directly in Unity Editor.

## Main Features
- **Palette Panel**: Select chips to place on the field (left panel).
- **Grid Editor**: Visual field for placing chips with Drag-and-Drop (center panel).
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
- Changing chip lock state (Move Lock).
- Changing grid size (Apply Grid Resize).
- Clearing the whole field (Clear All Chips).

## Technical Implementation
The Undo/Redo system is built on the **Command** pattern.

### Key Components
- **`IEditorCommand`**: Interface for all editor commands. Each command implements execution (`Execute`) and rollback (`Undo`) logic.
- **`EditorCommandHistory`**: Manager class that controls two stacks (Undo and Redo).
- **`LevelEditorWindow.UndoRedo.cs`**: Partial class containing definitions of all concrete commands:
    - `PlaceChipCommand` — place a new chip.
    - `MoveChipCommand` — move an existing chip.
    - `RemoveChipCommand` — remove a chip.
    - `ToggleMoveLockCommand` — toggle the lock flag.
    - `ResizeGridCommand` — resize the grid with removal of chips outside bounds.
    - `ClearAllChipsCommand` — full field clear.

### Workflow (Flow)
Instead of directly changing `placedChips` or `gridSize`, editor methods (for example, `PlaceChip`) create the corresponding command object and call `RecordAndExecute(command)`. This method delegates execution to `EditorCommandHistory`, which pushes the command to the stack and calls `Repaint()`.

---

# Chip Creator (Chip Viewer)

This tool provides a centralized interface for viewing, creating, and modifying `ChipData` assets.

## Main Features
- **Chip List**: View all chips in the project with icons (left panel).
- **Properties Editor**: View and edit serialized chip data (right panel).
- **Special Data Section**: Manage polymorphic `ChipData.specialDatas` data (`IChipSpecialData` types) with add/remove operations.
- **Default Special Data (Create Only)**: In the Settings tab, you can define `defaultSpecialDatas` templates that are automatically applied only during `Create Chip`.
- **Merge Configuration**: Merge rules are set manually through `ChipMergeData` in the `Special Data` section (without auto-creating a self-merge combination).
- **Undo/Redo System**: Full support for undoing property changes and rename operations.
- **Asset Management**: Buttons for creating new chips, saving changes, and refreshing the list; if template-prefab is missing, a warning is shown and creation is blocked.

## Undo/Redo System
The chip editor uses the same **Command** pattern as Level Editor to track changes.

### Hotkeys
- **Ctrl + Z**: Undo the last change.
- **Ctrl + Y** or **Ctrl + Shift + Z**: Redo the undone change.

### Supported Actions
- **Property Editing**: All changes made in `SerializedObject` during inspector editing.
- **Special Data Editing**: Add/remove entries in `specialDatas`, edit fields of a specific type (`ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, etc.).
- **Renaming**: Rename a chip asset (also renames the related prefab).
- **Stable Undo/Redo after rename/type-change**: After name or `Type` changes, the chip list and filters synchronize correctly.

## Technical Implementation

### Key Components
- **`IChipCreatorCommand`**: Interface for commands that manage `ChipData` state.
- **`ChipCreatorCommandHistory`**: Manages undo/redo stacks specifically for chip editing context.
- **`ChipCreatorWindow.SpecialDataSection.cs`**: Separate partial for `Special Data` UI section.
    - Lazily finds all `IChipSpecialData` implementations in loaded assemblies.
    - Shows each `specialDatas` element as a foldout with field editing via `SerializeReference`.
    - Allows adding only types not yet present in the current `ChipData` (no type duplicates).
    - Allows removing an element from the list.
- **`ChipCreatorWindow.RightPanel.cs` (Settings)**:
    - Contains the **Default Special Data (Create Only)** section to configure `defaultSpecialDatas` in `ChipCreatorSettings`.
    - Allows adding only unique `IChipSpecialData` types (one type -> one entry), editing their fields, and removing entries.
- **`ChipCreatorSettings`**:
    - Stores `defaultSpecialDatas` instead of a separate field for the default lock-effect prefab.
    - Recommended lock-effect scenario: add `ChipMoveLockedData` to `defaultSpecialDatas` and set `Prefab`.
- **`ChipCreatorWindow.UndoRedo.cs`**: Contains command definitions:
    - `SetChipPropertyCommand` — stores JSON snapshots of `ChipData` asset state "before" and "after".
    - `RenameChipCommand` — handles asset rename for both `ChipData` and prefab.
    - During Undo/Redo of changes affecting the list (name/type), `SerializedObject` sync, list sorting, and filter update are performed.
- **`ChipCreatorWindow.CreateChip.cs`**:
    - Caches the list of template prefabs for creating new chips.
    - Correctly handles an empty template folder (shows warning instead of incorrect creation).
    - During `Create Chip`, clones all elements from `DefaultSpecialDatas` into the new `ChipData` (`ApplyDefaultSpecialDataToNewChip` + `CloneSpecialData`).

### Workflow (Flow)
When a property changes in `DrawRightPanel`, the window creates `SetChipPropertyCommand` with captured asset states. This includes both regular `ChipData` fields and the `Special Data` section (including `ChipMergeData`). `RenameChipCommand` is used for renaming. These commands are passed to `RecordAndExecute(command)`, which updates history and ensures `SerializedObject` refresh. During new chip creation (`Create Chip`), templates from `defaultSpecialDatas` are applied, so default `MoveLocked` settings are also transferred through `ChipMoveLockedData`, not through a separate prefab field.
