# Editors

[← Back to Main](Main.md)

# Level Editor

This tool is intended for creating and editing Merge Toolkit game levels directly in Unity Editor.

## Main Features
- **Palette Panel**: Select chips to place on the field (left panel).
- **Scene View**: Main workspace. All editing functionality — chip placement, dragging, and context menu — is moved directly into Unity **Scene View** for direct and intuitive field interaction.
- **Properties Panel**: Configure level parameters (grid size, chip collection, active level selection) and validation (right panel).
- **Locked Areas Editor**: A special mode for creating, editing, and deleting locked areas and deferred chips (Deferred Cells) directly on the grid.
- **Undo/Redo System**: Ability to undo and redo any editing actions.

## Undo/Redo System
The editor supports a full action history, allowing you to safely experiment with level design.

### Hotkeys
- **Ctrl + Z**: Undo the last action.
- **Ctrl + Y** or **Ctrl + Shift + Z**: Redo the undone action.

### Supported Actions (Scene View)
- **Chip Placement**: Drag-and-drop chips from palette directly onto the grid in Scene View.
- **Chip Movement**: Drag existing chips across the grid.
- **Context Menu**: Right-click on a cell in Scene View opens a quick actions menu (delete, lock, etc.).
- **Lock State Change**: Via context menu in Scene View.
- **Grid Size Change**: Via Properties Panel (Apply Grid Resize).
- **Level Selection**: Select `FieldData` asset directly in Scene View for quick switching between levels.
- **Field Clear**: Via Properties Panel or context menu.
- **Locked Areas Editing**: Adding/removing areas, changing IDs, painting locked and deferred cells (see the "Locked Areas Editing" section).

## Locked Areas Editing (Locked Areas)

A special editing mode is used to configure locked areas and deferred chips.

### Editor Modes

The Level Editor supports two editing modes:

#### Mode 1: "Edit Level" (Standard Editing)
- Standard chip placement and grid editing.
- The chip palette is visible and active.

#### Mode 2: "Edit Locked Areas" (Locked Areas Editing)
- Chip dragging operations are disabled.
- Instead of the chip palette, a control panel for locked areas is shown:
  - **Add Area (+)**: A new `LockedAreaData` with a unique ID.
  - **Remove Area (-)**: Deletes the selected area.
  - **Select Active Area**: A dropdown or list to choose which area to edit.
  - **Area ID**: An IntField to change the ID (e.g., renaming).
  - **Clear Cells**: A button to clear all painted cells in the current area.

### Grid Control

In "Edit Locked Areas" mode, interaction with the grid changes:

- **LMB (left mouse button)**: Painting cells in `CellsToLock` (simple locked cells for the active area).
  - Cells are painted **green** semi-transparently.
  - Each cell displays the text `L - Area {ID}`.
- **RMB (right mouse button)**: Painting cells in `CellsToLockAndDeferred` (locked cells with deferred chips for the active area).
  - Cells are painted **blue** semi-transparently.
  - Each cell displays the text `D - Area {ID}`.

**Mutual Exclusivity**: A cell cannot simultaneously belong to both lists of the same or different areas. If a cell is added to `CellsToLock`, it is automatically removed from `CellsToLockAndDeferred` (and vice versa).

**Smart Brush**: Clicking LMB or RMB determines whether a cell is added or removed from the respective list. This operation (addition or removal) is locked for the entire duration of holding the mouse button (drag painting), providing convenient and predictable painting without chaotic changes.

For more details on the lifecycle and logic of locked areas, see the [Locked Areas (Locked Areas)](Features/LockedAreas.md) section.

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
    - `AddLockedAreaCommand` — adds a new locked area.
    - `RemoveLockedAreaCommand` — removes a locked area (preserves state for Undo).
    - `ChangeLockedAreaIdCommand` — changes a locked area ID.
    - `PaintLockedAreaCellsCommand` — edits `CellsToLock` and `CellsToLockAndDeferred` (painting locked/deferred cells).

### Workflow (Flow)
Instead of directly changing `placedChips` or `gridSize`, editor methods (for example, `PlaceChip`) called via Scene View events or UI create the corresponding command object and call `RecordAndExecute(command)`. This method delegates execution to `EditorCommandHistory`, which pushes the command to the stack and calls `Repaint()` of Scene View and editor windows.

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
    - Stores `defaultSpecialDatas` instead of a separate field for default lock-effect prefab.
    - Recommended lock-effect scenario: add `ChipMoveLockedData` to `defaultSpecialDatas` and set `Prefab`.
- **`ChipCreatorWindow.UndoRedo.cs`**: Contains command definitions:
    - `SetChipPropertyCommand` — stores JSON snapshots of `ChipData` asset state "before" and "after".
    - `RenameChipCommand` — handles asset rename for both `ChipData` and prefab.
    - During Undo/Redo of changes affecting the list (name/type), `SerializedObject` sync, list sorting, and filter update are performed.
- **`ChipCreatorWindow.CreateChip.cs`**:
    - Caches the list of template prefabs for creating new chips.
    - Correctly handles empty template folder (shows warning instead of incorrect creation).
    - During `Create Chip`, clones all elements from `DefaultSpecialDatas` into the new `ChipData` (`ApplyDefaultSpecialDataToNewChip` + `CloneSpecialData`).

### Workflow (Flow)
When a property changes in `DrawRightPanel`, the window creates `SetChipPropertyCommand` with captured asset states. This includes both regular `ChipData` fields and the `Special Data` section (including `ChipMergeData`). `RenameChipCommand` is used for renaming. These commands are passed to `RecordAndExecute(command)`, which updates history and ensures `SerializedObject` refresh. During new chip creation (`Create Chip`), templates from `defaultSpecialDatas` are applied, so default `MoveLocked` settings are also transferred through `ChipMoveLockedData`, not through a separate prefab field.
