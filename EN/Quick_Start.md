# Quick Start

[← Back to Main](Main.md)

## Overview
This guide will help you quickly connect Merge Toolkit:
- connect the `VContainer` dependency;
- run the `ElementalMerge2` example;
- run tests
- review the basic interactions.

## Dependencies
Merge Toolkit has three dependencies:
- **VContainer**: https://github.com/hadashiA/VContainer
- **Input System**: `com.unity.inputsystem`
- **Shader Graph**: `com.unity.shadergraph`

> [!NOTE]
> Integration tests depend on Unity version 6000.0.

Add them to `Packages/manifest.json` or through Package Manager.

## Getting Started
After connecting dependencies, make sure that:
- the project contains `Merge Toolkit` at `Assets/Expecto/Merge2`;
- the project opens without critical compile errors;
- tests pass successfully: **Window > General > Test Runner** -> run tests.

## Run Sample: ElementalMerge2
To run the demo example:
1. Open the scene `Assets/Expecto/Merge2/Examples/ElementalMerge2/Scenes/ElementalMerge2.unity`.
2. Press **Play**.
3. Check the basic loop: chip movement, interactions, and merge.

---

## Create New Merge Game (Clean Project)

This section explains how to create a new merge game from scratch using the built-in **Chip Viewer** tool.

### Step 1 - Open Chip Viewer

In the Unity menu: **Window -> Expecto -> MergeBase -> Chip Viewer**

### Step 2. Create a new game folder
Open `Window -> Expecto -> MergeBase -> Chip Viewer`.
1. In the **Settings** tab, click the **New Game Folder** button.
2. Choose a path for the new game (inside `Assets/`).

**The tool automatically:**
- Creates the folder structure (Data, Scripts, Scenes, Tests, etc.).
- Creates `ChipCreatorSettings` and `ChipDataCollection`.
- Generates the initial scene `{folderName}Scene.unity` with configured `Merge2LifetimeScope`.
- Creates test infrastructure: `.asmdef`, test template, and test assets (`TestSettings`, `FieldData`).

After confirmation, the following folders are created automatically:

| Folder | Purpose |
|---|---|
| `Data/` | General data |
| `Data/ChipData/` | `ChipData` ScriptableObjects |
| `Scripts/` | Your scripts |
| `Materials/` | Materials |
| `Prefabs/` | Prefabs |
| `Prefabs/Chips/` | Chip prefabs |
| `Prefabs/ChipsForContainer/` | Chip prefabs for containers |
| `Prefabs/Effects/` | Effects |
| `Scenes/` | Scenes |
| `Textures/` | Textures |
| `Tests/Editor/` | EditMode tests |
| `Tests/Editor/Resources/` | Resources for tests |
| `Tests/FieldData/` | Data for test fields |
| `Tests/Integration/` | Integration tests |
| `Tests/Prefabs/` | Prefabs for tests |

Chip Viewer automatically switches to the new settings and is immediately ready to work.


