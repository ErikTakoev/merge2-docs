# Merge2 Module Documentation

Welcome to the Merge2 module documentation. This file is a navigation hub for all project documentation, describing mechanics, architecture, and development processes.

## Contents

### Game Objects (Chips)
This section describes the main objects on the game field (Grid). All objects inherit from or use the basic principles of `Chip`.

- **[Chip (Base)](Chips/Chip.md)**
  > **Context**: This is the basic game object (chip) that supports merge mechanics. The section includes a description of **merge rules (ChipMergeData)**: partner configurations, weights (Weighted Random), and automatic initialization in the editor.
  
- **[ChipGenerator](Chips/ChipGenerator.md)**
  > **Context**: A chip-generator that creates (spawns) new chips. Works as a State Machine (Charging <-> Ready). Supports manual (Tap) and automatic modes, a weight system (Weighted Random) for result selection, and a limited number of recharges.

- **[ChipContainer](Chips/ChipContainer.md)**
  > **Context**: A chip-container that "consumes" other chips to complete quests or tasks. After filling, it usually provides a reward.

### Key Features
Mechanics that distinguish this project from classic Merge games.

- **[Multi-cell Chips](Features/MultiCellChip.md)**
  > **Context**: Support for chips larger than 1x1. Describes the complex "Anchor Cell" system, position validation, and rendering features for objects occupying multiple cells.

- **[Chip Relocation](Features/ChipRelocation.md)**
  > **Context**: A system for automatically pushing neighboring chips. When a player moves a large object or a merge occurs, neighboring chips attempt to find free space to vacate, instead of blocking the action.

- **[Chip Locking](Features/ChipLocking.md)**
  > **Context**: A mechanic for blocking chip movement. Locked chips cannot be moved by the player, adding puzzle elements to level design. Supports visual indicators in the editor and special effects in-game.

### Interactions
This section describes how chips interact with each other during dragging.

- **[Interaction Logic (ChipInteractions)](Interactions/ChipInteractions.md)**
  > **Context**: Description of how the chip dragging logic (`DraggableChipLogic`) delegates specific actions to the `IChipInteractionLogic` interface.
- **[Chip Merging (MergeableChipLogic)](Interactions/MergeableChipLogic.md)**
  > **Context**: Detailed description of merge mechanics: compatibility checking, result creation, and automatic neighbor relocation.
- **[Filling Containers (FillContainerLogic)](Interactions/FillContainerLogic.md)**
  > **Context**: How ingredient chips are added to containers to complete quests.

### Visuals
Systems responsible for "Game Feel" and visual feedback.

- **[Visual Effects](Visuals/Effects.md)**
  > **Context**: Visual feedback system. Effects list:
  > - **Chip**: Cell Highlight, Merge Available, Move Locked.
  > - **ChipGenerator**: Generator Recharge, Generator Charged.
  > - **ChipContainer**: Container Requirements.
  > How to trigger them through `Effect` components.

### Technical Section
Information for developers about architecture, patterns, and tools.

- **[Technical Design](Technical/TechnicalDesign.md)**
  > **Context**: Description of core architecture.
  > - **DI (VContainer)**: How dependency injection works through `Merge2LifetimeScope`.
  > - **Interfaces**: Description of key abstractions (`IFieldGrid`, `IChipMovingLogic`, `IFieldEventHandler`, etc).

- **[Testing](Technical/Testing.md)**
  > **Context**: Testing guide. Focus on **Integration Tests**, which use a real field prefab (`TestFieldBuilder`) to verify game scenarios (Spawn -> Move -> Merge -> Validation).

### Tools
Custom tools for facilitating development and content configuration.

- **[Editors](Editors.md)**
  > **Context**: Description of custom editor tools (Unity Editor) for level configuration, chip setup, and gameplay balance.
