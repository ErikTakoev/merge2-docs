# Quick Start

[← Back to Main](Main.md)

## Overview
This guide helps you quickly connect Merge Toolkit:
- connect the `VContainer` dependency;
- run the `ElementalMerge2` sample;
- run tests;
- review basic interactions.

## Dependencies
Merge Toolkit requires three dependencies:
- **VContainer**: https://github.com/hadashiA/VContainer
- **Input System**: `com.unity.inputsystem`
- **Shader Graph**: `com.unity.shadergraph`

> [!NOTE]
> Integrated tests require Unity version 6000.0 or newer.

Add them to `Packages/manifest.json` or via Package Manager.

## Getting Started
After connecting dependencies, make sure that:
- `Merge Toolkit` is present in the project at `Assets/Expecto/Merge2`;
- the project opens without critical compile errors;
- tests pass successfully: **Window > General > Test Runner** → run tests.

## Run Sample: ElementalMerge2
To run the demo sample:
1. Open scene `Assets/Expecto/Merge2/Examples/ElementalMerge2/Scenes/ElementalMerge2.unity`.
2. Press **Play**.
3. Verify the basic loop: chip movement, interactions, and merge.

---

## Create New Merge Game (Clean Project)

This section explains how to create a new merge game from scratch using the built-in **Chip Viewer** tool.

### Step 1 — Open Chip Viewer

In Unity menu: **Window → Merge2 → Chip Viewer**

### Step 2. Create a new game folder
Open `Window > Merge2 > Chip Viewer`.
1. In the **Settings** tab, click the **New Game Folder** button.
2. Choose a path for the new game (inside `Assets/`).

**The tool automatically:**
- Creates a folder structure (Data, Scripts, Scenes, Tests, etc.).
- Creates `ChipCreatorSettings` and `ChipDataCollection`.
- Generates an initial `{folderName}Scene.unity` scene with configured `Merge2LifetimeScope`.
- Creates test infrastructure: `.asmdef`, test template, and test assets (`TestSettings`, `FieldData`).

After confirmation, the following folders are created automatically:

| Folder | Purpose |
|---|---|
| `Data/` | Shared data |
| `Data/ChipData/` | `ChipData` ScriptableObjects |
| `Scripts/` | Your scripts |
| `Materials/` | Materials |
| `Prefabs/` | Prefabs |
| `Prefabs/Chips/` | Chip prefabs |
| `Prefabs/ChipsForContainer/` | Prefabs for container chips |
| `Prefabs/Effects/` | Effects |
| `Scenes/` | Scenes |
| `Textures/` | Textures |
| `Tests/Editor/` | EditMode tests |
| `Tests/Editor/Resources/` | Test resources |
| `Tests/FieldData/` | Test field data |
| `Tests/Integration/` | Integration tests |
| `Tests/Prefabs/` | Test prefabs |

Chip Viewer automatically switches to the new settings and is immediately ready to use.


