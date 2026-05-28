# Merge Toolkit - Isometric Extension

Welcome to the Merge Toolkit isometric extension documentation. This module adds support for an isometric grid, Merge-3 mechanics, and specific visual effects.

## Contents

### Getting Started
- **[Isometric Quick Start](Isometric_Quick_Start.md)** (Coming Soon)
  > **Context**: Quick setup of an isometric scene and initial component initialization.

### Field & Grid
This section describes field features in isometric view.

- **[IsoCell](Field/IsoCell.md)** (Coming Soon)
  > **Context**: A specialized cell that overrides chip positioning for correct rendering in isometric projection.

### Mechanics (Interactions)
Interaction logic developed specifically for isometric gameplay.

- **[IsoMergeableChipLogic](Interactions/IsoMergeableChipLogic.md)** (Coming Soon)
  > **Context**: Implementation of **Merge-3** mechanics. Uses Breadth-First Search (BFS) to find groups of identical chips. Supports spawning multiple results when merging large groups (e.g., 6 chips -> 2 chips of the next level) and an automatic neighbor relocation system.

- **[IsoDraggableChipLogic](Features/IsoDraggableChipLogic.md)** (Coming Soon)
  > **Context**: Extended chip dragging logic that integrates additional hint effects and group counters.

### Visuals
Visual feedback systems that enhance the gameplay experience in isometric view.

- **[IsoEffectConsts](Visuals/IsoEffectConsts.md)** (Coming Soon)
  > **Context**: List of effect IDs (MergeHint: 201, MergeGroupCounter: 202).

- **[Isometric Effects](Visuals/IsoEffects.md)** (Coming Soon)
  > **Context**: Description of specific effects:
  > - **MergeHint**: Highlights all compatible chips on the board when dragging starts.
  > - **MergeGroupCounter**: A floating UI counter showing the size of the current group above the chip.
  > - **MergeAvailable (Iso)**: Dynamic chip orientation (rotation) facing towards the center of the future merge.

### Camera
Camera control system for isometric fields.

- **[IsoMergeCamera](Camera/IsoMergeCamera.md)**
  > **Context**: Support for edge-scroll, pinch-to-zoom, inertia, and camera panning (pan).
