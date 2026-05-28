# Merge Toolkit Documentation

Welcome to the Merge Toolkit documentation. This file is a navigation hub for all project documentation that describes mechanics, architecture, and development processes.

## Contents

### Getting Started
Quick start and extension guide for the project.

- **[Quick Start](Quick_Start.md)**
  > **Context**: First module launch, connect `VContainer`, run `ElementalMerge2` and practical workflow through `Level Editor` and `Chip Viewer`.

- **[Extension Guide](ExtensionGuide.md)**
  > **Context**: Step-by-step guide for extending the project: adding new chip types, effects, interactions, and services.

### Extensions
Additional modules and specialized solutions built on top of the Merge Toolkit.

- **[Merge Toolkit - Isometric](Extensions/Isometric/IsometricMain.md)**
  > **Context**: Support for an isometric grid, depth sorting, and specific visual effects for isometric games.
- **Merge Toolkit - Classic**
  > **Context**: Core mechanics set for classic 2D or Top-Down merge puzzles.
- **Merge Toolkit - Tower Defense**
  > **Context**: An extension combining the chip merge system with tower defense mechanics.



### Game Objects (Chips)
This section describes the core entities located on the game field (Grid). All objects inherit from or use the base `Chip` principles.

- **[Chip (Base)](Chips/Chip.md)**
  > **Context**: This is the base game object (chip) that supports Merge mechanics. The section includes a description of **merge rules (ChipMergeData)** as part of `specialDatas`: partner setup, weights (Weighted Random), and runtime access through `Chip.MergeData`.
  
- **[ChipGenerator](Chips/ChipGenerator.md)**
  > **Context**: A chip generator that creates (spawns) new chips. Works as a State Machine (Charging <-> Ready). Supports manual (Tap) and automatic modes, a weight system (Weighted Random) for result selection, and a limited number of recharges.

- **[ChipContainer](Chips/ChipContainer.md)**
  > **Context**: A chip container that "consumes" other chips to complete quests or tasks. After filling, it usually gives a reward.

- **[ChipPowerBooster](Chips/ChipPowerBooster.md)**
  > **Context**: A chip booster that strengthens neighboring chips (for example, speeds up generator charging), supports `JoinPoints` on boost targets, and controls two visualization types: influence zone highlighting and dynamic join links through `IEffectPowerBoosterJoin`.

### Key Features
Mechanics that distinguish this project from classic Merge games.

- **[Locked Areas](Features/LockedAreas.md)**
  > **Context**: A system for locking specific grid areas until they are unlocked. Supports two types of locked cells: simple (with instant chip loading) and deferred (with chips appearing after unlocking).

- **[Multi-cell Chips](Features/MultiCellChip.md)**
  > **Context**: Support for chips larger than 1x1. Describes the complex Anchor Cell system, position validation, and rendering specifics for objects occupying multiple cells.

- **[Chip Relocation](Features/ChipRelocation.md)**
  > **Context**: A system for automatically pushing neighboring chips away. When the player moves a large object or a merge happens, neighboring chips try to find free space to clear room instead of blocking the action.

- **[Chip Effect Blockers](Features/ChipEffectBlockers.md)**
  > **Context**: The mechanic of blocking actions on chips (movement, merge, etc.) and the system of progressive effect destruction (chains, boxes).

- **[Cell Observer System](Features/CellObserverSystem.md)**
  > **Context**: A detailed description of the cell change observation system.

### Interactions
This section describes how chips interact with each other during dragging.

- **[Interaction Logic (ChipInteractions)](Interactions/ChipInteractions.md)**
  > **Context**: Description of how chip dragging logic (`DraggableChipLogic`) delegates specific actions to the `IChipInteractionLogic` interface.
- **[Chip Merge (MergeableChipLogic)](Interactions/MergeableChipLogic.md)**
  > **Context**: Detailed description of merge mechanics: compatibility checks, result creation, and automatic neighbor relocation.
- **[Container Filling (FillContainerLogic)](Interactions/FillContainerLogic.md)**
  > **Context**: How ingredient chips are added to containers to complete quests.

### Visuals
Systems responsible for game feel and visual feedback.

- **[Visual Effects](Visuals/Effects.md)**
  > **Context**: Visual feedback system. List of effects:
  > - **Chip**: Cell Highlight, Merge Available, Move Locked.
  > - **ChipGenerator**: Generator Recharge, Generator Charged.
  > - **ChipContainer**: Container Requirements.
  > - **ChipPowerBooster**: Connector Highlight, Particle Join Links (`PowerBoosterJoinEffect`).
  > Ways to trigger them through `Effect` components.

### Technical Section
Information for developers about architecture, patterns, and tools.

- **[Technical Design](Technical/TechnicalDesign.md)**
  > **Context**: Core architecture description.
  > - **DI (VContainer)**: How dependency injection works through `Merge2LifetimeScope`.
  > - **Interfaces**: Description of key abstractions (`IFieldGrid`, `IChipMovingLogic`, `IFieldEventHandler`, etc.).


  
- **[Testing](Technical/Testing.md)**
  > **Context**: A guide for writing tests. Focus on **Integration Tests** that use a real field prefab (`TestFieldBuilder`) to verify gameplay scenarios (Spawn -> Move -> Merge -> Validation), including the subscription pipeline of [Cell Observer System](Features/CellObserverSystem.md) and `ChipPowerBooster` scenarios for manual+auto generators.

### Tools
Custom tools to simplify development and content setup.

- **[Editors](Editors.md)**
  > **Context**: Description of custom editor tools (Unity Editor) for level setup, chip configuration, and gameplay balance.

### Auto-generated API Reference
Automatically generated API reference for Merge Toolkit. Useful for quickly finding classes, interfaces, and methods.

- **[API Reference](../AutoGen/Expecto_MergeBase.md)**
  > **Context**: Full aggregated overview of all discovered entities (classes, interfaces, fields, methods) within the Expecto.MergeBase namespace.
