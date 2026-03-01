# Testing

[← Back to Main](../Main.md)


The project uses a combination of Integration and Unit tests to ensure stability.

## Integration Tests
Located in `Modules/Merge2/Tests/Scripts/Integration/`. They verify system operation in assembly on a real field (or its emulation).

### ChipBaseTests
Tests for basic functionality of creating, moving, and merging chips.

#### CreateChip_SpawnChips
- **Goal**: Verifies correct spawning of chips of various sizes via `ChipFactory` and `IFreeCellFinder`.
- **Scenario**: Fills the field with chips of various sizes from the center using `FindNearestFreeCell` to find free cells. Verifies the integrity of references between chips and cells after spawning.

#### CreateChip_DraggingChip
- **Goal**: Verifies correct chip movement via drag-and-drop.
- **Scenario**: Creates several chips on the field and simulates dragging one of them to another position. Verifies field integrity after movement.

#### DraggingChip_RandomMoves
- **Goal**: Verifies system stability with a large number of random movements.
- **Scenario**: Fills the field with chips, then performs 2000 random drag operations between random cells. Verifies field integrity after all operations.

#### MergeableChip_NextChipIsBiggerThanParent
- **Goal**: Verifies correct merging of chips when the result chip is larger than the originals.
- **Scenario**: Creates chips of various sizes and simulates merging a smaller chip with a larger one, resulting in an even larger chip. Verifies chip count and field integrity after merging.

### ChipGeneratorTests
Tests for manual chip generator functionality (`IsAutoGeneration = false`).

#### ManualGenerator_SpawnsChipOnTapAndRecharges
- **Goal**: Verifies that the generator creates chips on tap, uses charges, and recharges correctly.
- **Scenario**: Creates a charged generator, simulates taps to use all charges. Verifies that chips were created, the generator entered recharge state, and after waiting, is charged again with restored charge count.

#### ManualGenerator_EvolutionIntoNull
- **Goal**: Verifies that the generator is destroyed after using all recharges when `NextChipData = null`.
- **Scenario**: Creates a copy of ChipData via `Clone()`, sets `TotalRecharges = 1`. Uses all generator charges. Verifies the generator was destroyed (cell is empty).

#### ManualGenerator_EvolutionIntoNextChip
- **Goal**: Verifies that the generator evolves into `NextChipData` after using all recharges.
- **Scenario**: Creates a copy of ChipData via `Clone()`, sets `TotalRecharges = 1` and `NextChipData`. Uses all charges. Verifies the generator was replaced with `NextChipData`.

#### ManualGenerator_NoSpace
- **Goal**: Verifies generator behavior when attempting to create a chip with no free space on the field.
- **Scenario**: Fills the field completely, leaving only the generator's cell. Simulates a tap on the generator. Verifies the new chip was not created.

#### ManualGenerator_StartsUncharged_Recharges
- **Goal**: Verifies generator behavior that starts without charge (`IsStartCharged = false`).
- **Scenario**: Creates an uncharged generator, waits for recharge time, simulates tap. Verifies the generator charges and creates a chip after tap.

#### ManualGenerator_StartsUncharged_Recharges
- **Goal**: Verifies generator behavior that starts without charge (`IsStartCharged = false`).
- **Scenario**: Creates a copy of ChipData via `Clone()`, sets `IsStartCharged = false`. Waits for recharge time, simulates tap. Verifies the generator charges and creates a chip after tap.

### AutoChipGeneratorTests
Tests for automatic chip generator functionality (`IsAutoGeneration = true`).

#### AutoGenerator_SpawnsChipAutomaticallyBeforeCharge
- **Goal**: Verifies that automatic generator creates chips automatically throughout the charge cycle.
- **Scenario**: Creates a charged automatic generator, waits for generation intervals. Verifies all chips (according to `ChargeCount`) were created automatically before full discharge.

#### AutoGenerator_SpawnAround
- **Goal**: Verifies that automatic generator creates chips only around itself (in neighboring cells).
- **Scenario**: Creates a copy of ChipData via `Clone()`, sets `ChargeCount = 100`. Waits for 10 chips to spawn. Verifies all chips are created around the generator (total 11 chips on field).

#### AutoGenerator_EntersWaitingStateDuringDrag
- **Goal**: Verifies that the generator enters waiting state during dragging.
- **Scenario**: Creates a charged generator, starts dragging the generator. Verifies generator behavior during and after dragging.

#### AutoGenerator_StartsUncharged_RechargesAndGenerates
- **Goal**: Verifies automatic generator behavior that starts without charge.
- **Scenario**: Creates a copy of ChipData via `Clone()`, sets `IsStartCharged = false`. Waits for recharge time. Verifies the generator automatically created a chip after recharging.

#### AutoGenerator_Evolution_IntoNull
- **Goal**: Verifies automatic generator destruction after using all recharges when `NextChipData = null`.
- **Scenario**: Creates a copy of ChipData via `Clone()`, sets `ChargeCount = 2` and `TotalRecharges = 1`. Waits for completion of all generation cycles. Verifies the generator was destroyed (cell is empty).

#### AutoGenerator_Evolution_IntoNextChip
- **Goal**: Verifies automatic generator evolution into `NextChipData` after using all recharges.
- **Scenario**: Creates a copy of ChipData via `Clone()`, sets `ChargeCount = 2`, `TotalRecharges = 1`, and `NextChipData`. Waits for completion of all cycles. Verifies the generator evolved into `NextChipData`.

### ChipContainerTests
Tests for container functionality (`ChipContainer`).

#### SpawnChipContainer_And_GoldGenerator
- **Goal**: Verifies container interaction with gold generator, including chip consumption and automatic spawning.
- **Scenario**: Creates a container and gold generator. Simulates dragging gold chip into container. Verifies the generator continues spawning new chips while the container successfully accepts them. Validates field integrity after all manipulations.

#### Container_Filling
- **Goal**: Verifies the process of filling a container with chips.
- **Scenario**: Creates a container at position (1, 1). Creates 3 different chips (water_1_1, water_1_2, water_2) and sequentially drags them into the container. Verifies chip count on field (should be 1 - container itself) and field consistency.

## Instrumentation (Helpers)

### TestSettings
Configuration class for test settings.
- **Purpose**: Stores the path to the test scene prefab and debug flags.
- **Update**:
  - Added menu item `Window/Merge2/Tests/Enable Debug Mode` for quick debug mode enabling.
  - Added a link to `ChipCreatorSettings` for automatic path configuration for data and prefabs during test execution.

### Automated Scaffolding
The "New Game Folder" tool automatically creates:
- `{folderName}Tests.cs`: Integration test template.
- `{folderName}.Tests.asmdef`: Preconfigured Assembly Definition for tests.
- `Test{folderName}Settings.asset`: Settings for Resource loading.

### TestFieldBuilder
Helper class for creating test environment.
- **Usage**: Loads the `TestElemental_Field` prefab, configures DI Container, and initializes field.
- **Configuration**: Uses `TestSettings` through Resource loading to determine scene prefab and debug modes.
- **Isolation**: `ClearField()` method allows clearing the field between tests without reloading the scene.

### FieldAssertions
Set of methods for validating field state.
- `AssertFieldConsistency(field)`: Verifies reference integrity:
  - Whether each chip in `MainCell` correctly references its `SecondaryCells`.
  - Whether all cells under a chip have correct references to that chip.
  - Whether there are no "phantom" blocks.

### TestHelper
Static class with helper methods for tests.
- `FindChipByChipId(field, chipId, exclude)`: Finds the specified chip on field by its name (ID), allowing already-found chips to be excluded.
- `WaitForSingleRun()`: Pause for visual control when running a single test manually.

## Test Writing Principles
1. **Clean Slate**: Each test should start with a clean field.
2. **Real Input Simulation**: Use `InputHelper` to emulate clicks/drag instead of directly calling logic methods (where possible) to test the full pipeline.
3. **Assert Consistency**: After each complex manipulation (Merge, Move), call `AssertFieldConsistency`.
