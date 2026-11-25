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

### Core Architecture and Synchronization
- **`IMergeLifetimeScope` as the Single Source of Truth**: The editor is activated and rendered only when an `IMergeLifetimeScope` component is present in the current hierarchy of the active scene. The connection between the editor and the scene is direct: the `FieldData` field in the active scope acts as the single source of truth. Any actions like creating levels (`OnNewLevel`), loading, or auto-saving (`EnsureAutosaveTarget`, `AutosaveCurrentField`) write the asset directly to the active scope and mark it as dirty (`EditorUtility.SetDirty`).
- **VContainer Integration**: The field grid (`IFieldGrid`) is resolved primarily via the DI container (`scope.Container.Resolve<IFieldGrid>()`), ensuring full compatibility with runtime dependencies and scene configuration. If the container is not built in Edit mode, a fallback search for the component directly in the scene is used.
- **Isolation and Suppression in Prefab Mode**: The editor tracks the Prefab Mode state using `PrefabStage` and `PrefabStageUtility` events. When switching to Prefab Mode, interaction with the editor is suspended, and the scene visual preview is instantly destroyed. After closing Prefab Mode, the editor automatically restores its state and performs a re-synchronization.
- **Domain Reload Resilience**: After a Unity domain reload, when the non-serialized dictionary of placed chips (`placedChips`) is reset, the editor automatically detects the desynchronization and repopulates the dictionary based on the serialized `FieldData` asset from the active scope.
- **Isolated Scene Visual Preview**: To render the field prefab and placed chips, the editor dynamically creates a separate temporary additive scene (`LevelEditorPreviewScene`). This prevents cluttering or accidental modification of the main open scene of the project. All preview objects are created with the `HideFlags.DontSave` flag.

### Undo/Redo System
The Undo/Redo system is built on the **Command** pattern.

#### Key Components
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

#### Workflow (Flow)
Instead of directly changing `placedChips` or `gridSize`, editor methods (for example, `PlaceChip`) called via Scene View events or UI create the corresponding command object and call `RecordAndExecute(command)`. This method delegates execution to `EditorCommandHistory`, which pushes the command to the stack and calls `Repaint()` of Scene View and editor windows.

---

# Merge Toolkit

This tool (`MergeToolkitWindow`, opened via **Window -> Expecto -> Merge Toolkit**) provides a centralized interface for viewing, creating, and modifying `ChipData` assets, configuring project settings (**Project Settings**), and launching new merge project creation (**New Project**).

## Main Features
- **Chip List**: View all chips in the project with icons (left panel).
- **Properties Editor**: View and edit serialized chip data (right panel).
- **Base Sprite Validation**: Automatically validates the presence of a sprite on the base renderer (`IChipSortingLayer.SortingLayers[0].Renderer`). If the renderer is assigned but its `sprite` is null, an `Error` level `HelpBox` is displayed along with an `Open` button that opens the prefab in Prefab Mode and selects the corresponding `GameObject` with the `SpriteRenderer`.
- **Module Data Section**: Manage polymorphic `ChipData.moduleDatas` data (`IChipModuleData` types) with add/remove operations.
- **Default Module Data (Create Only)**: In the **Project Settings** tab, you can define `defaultModuleDatas` templates that are automatically applied only during `Create Chip`.
- **Merge Configuration**: Merge rules are set manually through `ChipMergeData` in the `Module Data` section (without auto-creating a self-merge combination).
- **Undo/Redo System**: Full support for undoing property changes and rename operations with automatic switching to the target chip during global undo/redo.
- **Asset Management**: Create new chips and refresh the list; automatic saving of modifications to disk (Save on Switch/Close/Blur) when switching chips, opening **Project Settings** / **New Chip** tabs, clicking **New Project**, closing the window, or on focus lost; if template-prefab is missing, a warning is shown and creation is blocked.

## Undo/Redo System
The chip editor uses the same **Command** pattern as Level Editor to track changes.

### Hotkeys
- **Ctrl + Z**: Undo the last change.
- **Ctrl + Y** or **Ctrl + Shift + Z**: Redo the undone change.

### Supported Actions
- **Property Editing**: All changes made in `SerializedObject` during inspector editing.
- **Module Data Editing**: Add/remove entries in `moduleDatas`, edit fields of a specific type (`ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, etc.).
- **Renaming**: Rename a chip asset (also renames the related prefab).
- **Stable Undo/Redo after rename/type-change**: After name or `Type` changes, the chip list and filters synchronize correctly.

## Technical Implementation

### Key Components
- **`EditorContext`**: Centralized editor context provider holding the active `FieldData`, `MergeProjectSettings` (`ActiveMergeProjectSettings`), and active `ChipDataCollection`.
- **`IChipEditorCommand`**: Interface for commands that manage `ChipData` state.
- **`ChipEditorCommandHistory`**: Manages undo/redo stacks specifically for chip editing context.
- **`MergeToolkitWindow.ModuleDataSection.cs`**: Separate partial for `Module Data` UI section.
    - Lazily finds all `IChipModuleData` implementations in loaded assemblies.
    - Shows each `moduleDatas` element as a foldout with field editing via `SerializeReference`.
    - Allows adding only types not yet present in the current `ChipData` (no type duplicates).
    - Allows removing an element from the list.
- **`MergeToolkitWindow.RightPanel.cs`**:
    - Displays chip details, preview thumbnail, header, properties, and data sections.
    - Performs validation in `DrawMissingBaseSpriteWarning()`: checks `IChipSortingLayer.SortingLayers[0].Renderer.sprite`. If renderer is assigned but sprite is missing, displays an `Error` `HelpBox` with an `Open` button.
    - `OpenPrefabAndSelectRenderer()` loads the prefab via `PrefabStageUtility.OpenPrefab()`, locates the target `GameObject` with the `SpriteRenderer` via transform path or via the opened prefab stage's sorting layer, selects it (`Selection.activeGameObject`), and pings it (`EditorGUIUtility.PingObject()`).
    - In the **Project Settings** tab (`DrawProjectSettingsPanel`), provides the `MergeProjectSettings` field and the **Default Module Data (Create Only)** section to configure `defaultModuleDatas` in `MergeProjectSettings`.
    - Allows adding only unique `IChipModuleData` types (one type -> one entry), editing their fields, and removing entries.
- **`MergeProjectSettings`**:
    - Stores `defaultModuleDatas` instead of a separate field for default lock-effect prefab.
    - Recommended lock-effect scenario: add `ChipMoveLockedData` to `defaultModuleDatas` and set `Prefab`.
- **`MergeToolkitWindow.UndoRedo.cs`**: Contains command definitions:
    - `SetChipPropertyCommand` — stores JSON snapshots of `ChipData` asset state "before" and "after".
    - `RenameChipCommand` — handles asset rename for both `ChipData` and prefab.
    - During Undo/Redo of changes affecting the list (name/type), `SerializedObject` sync, list sorting, and filter update are performed.
- **`MergeToolkitWindow.ContextMenu.cs`**:
    - Implements the context menu (right-click on a chip in the left panel).
    - Contains `Duplicate`, a separator, and `Delete` menu items.
    - `Duplicate` switches the editor into chip creation mode (`State.CreateChip`), sets the chosen `ChipData` as the duplication source (`duplicateSourceChip`), and leaves the name field blank for user input.
    - `Delete` prompts for confirmation and safely deletes the chip asset and associated prefab (if not owned by other chips).
- **`MergeToolkitWindow.CreateChip.cs`**:
    - Caches the list of template prefabs for creating new chips.
    - Correctly handles empty template folder (shows warning instead of incorrect creation).
    - During `Create Chip`, clones all elements from `DefaultModuleDatas` into the new `ChipData` (`ApplyDefaultModuleDataToNewChip` + `CloneModuleData`).
    - In the `Create Next Level Chip` section, displays only `Chip Name:` (read-only) and a `Create` button. Clicking `Create` opens the `New Chip` tab (`State.CreateChip`) and pre-fills the next-level name for both duplication (`duplicateChipName`) and template creation (`newChipName`), leaving `duplicateSourceChip` empty (`null`).

### Workflow (Flow)
When a property changes in `DrawRightPanel`, the window creates `SetChipPropertyCommand` with captured asset states. This includes both regular `ChipData` fields and the `Module Data` section (including `ChipMergeData`). `RenameChipCommand` is used for renaming. These commands are passed to `RecordAndExecute(command)`, which updates history and ensures `SerializedObject` refresh. During new chip creation (`Create Chip`), templates from `defaultModuleDatas` are applied, so default `MoveLocked` settings are also transferred through `ChipMoveLockedData`, not through a separate prefab field.

---

# New Merge Project

Opened via the **New Project** button on the **Merge Toolkit** toolbar (`MergeToolkitWindow`), `NewMergeProjectWindow` generates a complete project folder structure, `MergeProjectSettings`, `ChipDataCollection`, field prefabs, scripts (`{Prefix}MergeLifetimeScope`, `{Prefix}MergeInitializer`), integration test setup, and initial scene from a `MergeTemplateSettings` asset.
