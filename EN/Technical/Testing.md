# Testing

[← Back to Main](../Main.md)


Project uses a combination of Integration and Unit tests to ensure stability.

## Integration Tests
> [!IMPORTANT]
> Integration tests are supported starting from Unity version **6000.0**.

Located in `Modules/Merge2/Tests/Scripts/Integration/`. They verify system behavior as a whole on a real field (or its emulation).

### ChipBaseTests
Tests of base functionality for chip creation, movement, and merge.

#### CreateChip_SpawnChips
- **Goal**: Verifies correct spawning of chips of different sizes through `ChipFactory` and `IFreeCellFinder`.
- **Scenario**: Fills field with chips of different sizes from center, using `FindNearestFreeCell` to search free cells. Verifies integrity of references between chips and cells after spawn.

#### CreateChip_DraggingChip
- **Goal**: Verifies correct chip movement through drag-and-drop.
- **Scenario**: Creates several chips on field and simulates dragging one of them to another position. Verifies field integrity after movement.

#### DraggingChip_RandomMoves
- **Goal**: Verifies system stability under a large number of random movements.
- **Scenario**: Fills field with chips, then performs 2000 random drag operations between random cells. Verifies field integrity after all operations.

#### MergeableChip_NextChipIsBiggerThanParent
- **Goal**: Verifies correct chip merge when resulting chip is larger than source ones.
- **Scenario**: Creates chips of different sizes and simulates merge of a smaller chip with a larger one, resulting in an even larger chip. Verifies chip count and field integrity after merge.

### ChipGeneratorTests
Tests of manual chip generator functionality (`IsAutoGeneration = false`).

#### ManualGenerator_SpawnsChipOnTapAndRecharges
- **Goal**: Verifies that generator spawns chips on tap, consumes charges, and recharges correctly.
- **Scenario**: Creates a charged generator, simulates taps to consume all charges. Verifies chips were created, generator entered recharge state, and after waiting it is charged again with restored charge count.

#### ManualGenerator_EvolutionIntoNull
- **Goal**: Verifies that generator is destroyed after using all recharges when `NextChipData = null`.
- **Scenario**: Creates ChipData copy via `Clone()`, sets `TotalRecharges = 1` through `GetSpecialData<ChipGeneratorData>()`. Uses all generator charges. Verifies generator was destroyed (cell is empty).

#### ManualGenerator_EvolutionIntoNextChip
- **Goal**: Verifies that generator evolves into `NextChipData` after using all recharges.
- **Scenario**: Creates ChipData copy via `Clone()`, sets `TotalRecharges = 1` and `NextChipData` through `GetSpecialData<ChipGeneratorData>()`. Uses all charges. Verifies generator was replaced with `NextChipData`.

#### ManualGenerator_NoSpace
- **Goal**: Verifies generator behavior when trying to create chip while no free space exists on field.
- **Scenario**: Fills field completely, leaving only generator cell. Simulates tap on generator. Verifies that no new chip was created.

#### ManualGenerator_StartsUncharged_Recharges
- **Goal**: Verifies behavior of generator that starts uncharged (`IsStartCharged = false`).
- **Scenario**: Creates uncharged generator, waits recharge time, simulates tap. Verifies generator charges and creates a chip after tap.

#### ManualGenerator_StartsUncharged_Recharges
- **Goal**: Verifies behavior of generator that starts uncharged (`IsStartCharged = false`).
- **Scenario**: Creates ChipData copy via `Clone()`, sets `IsStartCharged = false` through `GetSpecialData<ChipGeneratorData>()`. Waits recharge time, simulates tap. Verifies generator charges and creates a chip after tap.

### AutoChipGeneratorTests
Tests of automatic chip generator functionality (`IsAutoGeneration = true`).

#### AutoGenerator_SpawnsChipAutomaticallyBeforeCharge
- **Goal**: Verifies that automatic generator creates chips automatically during the entire charge cycle.
- **Scenario**: Creates charged automatic generator, waits generation intervals. Verifies that all chips (according to `ChargeCount`) were created automatically before full discharge.

#### AutoGenerator_SpawnAround
- **Goal**: Verifies that automatic generator creates chips only around itself (in neighboring cells).
- **Scenario**: Creates ChipData copy via `Clone()`, sets `ChargeCount = 100` through `GetSpecialData<ChipGeneratorData>()`. Waits for generation of 10 chips. Verifies that all chips are created around generator (11 chips total on field).

#### AutoGenerator_EntersWaitingStateDuringDrag
- **Goal**: Verifies that generator enters waiting state during dragging.
- **Scenario**: Creates charged generator, starts dragging generator. Verifies generator behavior during and after dragging.

#### AutoGenerator_StartsUncharged_RechargesAndGenerates
- **Goal**: Verifies behavior of automatic generator that starts uncharged.
- **Scenario**: Creates ChipData copy via `Clone()`, sets `IsStartCharged = false` through `GetSpecialData<ChipGeneratorData>()`. Waits recharge time. Verifies that generator automatically created chip after recharge.

#### AutoGenerator_Evolution_IntoNull
- **Goal**: Verifies destruction of automatic generator after using all recharges when `NextChipData = null`.
- **Scenario**: Creates ChipData copy via `Clone()`, sets `ChargeCount = 2` and `TotalRecharges = 1` through `GetSpecialData<ChipGeneratorData>()`. Waits for completion of all generation cycles. Verifies generator was destroyed (cell is empty).

#### AutoGenerator_Evolution_IntoNextChip
- **Goal**: Verifies evolution of automatic generator into `NextChipData` after using all recharges.
- **Scenario**: Creates ChipData copy via `Clone()`, sets `ChargeCount = 2`, `TotalRecharges = 1`, and `NextChipData` through `GetSpecialData<ChipGeneratorData>()`. Waits for completion of all cycles. Verifies generator evolved into `NextChipData`.

### ChipContainerTests
Tests of chip container functionality (`ChipContainer`).

#### SpawnChipContainer_And_GoldGenerator
- **Goal**: Verifies interaction of container with gold generator, including chip consumption and automatic spawn.
- **Scenario**: Creates container and gold generator. Simulates dragging a gold chip into container. Verifies generator continues spawning new chips, and container accepts them successfully. Validates field integrity after all manipulations.

#### Container_Filling
- **Goal**: Verifies process of filling container with chips.
- **Scenario**: Creates container at position (1, 1). Creates 3 different chips (water_1_1, water_1_2, water_2) and sequentially drags them into container. Verifies chip count on field (should be 1 - container itself) and field consistency.

### ChipPowerBoosterTests
Tests of chip booster functionality ([ChipPowerBooster](../Chips/ChipPowerBooster.md)) and subscription system.

#### SpawnAndMovingCheckSubscribe
- **Goal**: Verifies correctness of booster subscriptions after spawn and a sequence of drag moves.
- **Scenario**: Creates booster at position (0,0), verifies expected `ObservedCellPositions`. Performs 3 sequential moves and after each validates subscribed cell list via `CellObserverAssertions.AssertObservedCells`.

#### AddGeneratorAndMoveBoosterCheckSubscribe
- **Goal**: Verifies subscriptions and virtual positions with a generator on field and booster movement.
- **Scenario**: Creates generator and booster. Verifies that cells under generator are split into direct and virtual. Performs 2 booster moves and validates subscription updates.

#### AddGeneratorAndMoveGeneratorCheckSubscribe
- **Goal**: Verifies apply/remove of modifiers when moving generator relative to booster.
- **Scenario**: Creates generator far from booster — verifies `BoostedTargets.Count == 0`. Moves generator near booster — verifies `BoostedTargets.Count == 1` and `AppliedBoosters.Count == 1`. Moves generator onto booster and away from it — verifies dynamic boost updates.

#### CheckBoosterPower
- **Goal**: Verifies that one `ChipPowerBooster` accelerates both manual and auto generators in influence zone at the same time.
- **Scenario**: Creates manual generator (`IsStartCharged = false`, `ChargingTime = 5f`) and auto generator (`ChargingTime = 5f`), places booster so both are in range. After waiting `ChargingTime / Power`, verifies manual generator became `IsCharged == true`, and auto generator entered charged cycle (`RuntimeDataOnlyEditor.ChargeCount != 0`).

#### CheckBoosterPower_MoveGenerator
- **Goal**: Verifies modifier is removed when generator is moved away from booster.
- **Scenario**: Creates uncharged generator near booster (`BoostedTargets.Count == 1`) with parameters through `GetSpecialData<ChipGeneratorData>()`. Moves generator far away. Waits `ChargingTime / Power - timeToMove`. Verifies generator did **not** charge (boost was removed).

#### CheckBoosterPower_Evolution
- **Goal**: Verifies correct boost cleanup during booster evolution (merge), which affects behavior of neighboring generators.
- **Scenario**:
  1. Places uncharged generator at position (0, 0) with `ChargingTime = 5f`.
  2. Creates two boosters at positions (4, 0) and (4, 6), both with `boosterEvolutionChipData` (configuration that has merge result).
  3. Generator is in range of first booster -> `AppliedBoosters.Count == 1`.
  4. Drags second booster onto first (drag from (4, 6) to (4, 0)) — this triggers booster **merge/evolution**.
  5. **Key check**: After merge, generator must have `AppliedBoosters.Count == 0` (old booster destroyed and cleaned boost).
  6. **Verification**: Waits `ChargingTime - timeToMove`. Generator remains UNCHARGED since boost was removed when first booster was destroyed.
- **Importance**: Guarantees that `PowerBoosterCellSubscriber.OnChipDestroy()` correctly removes all boosts before booster destruction, so it does not leave "ghost" active boosters on generators.

## Instrumentation (Helpers)

### TestSettings
Class configuration for test settings.
- **Purpose**: Stores path to test scene prefab and debug flags.
- **Updates**: 
  - Added menu item `Window/Merge2/Tests/Enable Debug Mode` for quick debug mode enabling.
  - Added reference to `ChipCreatorSettings` for automatic path setup for data and prefabs during test runs.

### Automated Scaffolding
"New Game Folder" tool automatically creates:
- `{folderName}Tests.cs`: Integration test template.
- `{folderName}.Tests.asmdef`: Configured Assembly Definition for tests.
- `Test{folderName}Settings.asset`: Settings for Resource loading.

### TestFieldBuilder
Helper class for creating test environment.
- **Usage**: Loads `TestElemental_Field` prefab, configures DI Container, and initializes field.
- **Configuration**: Uses `TestSettings` through Resource loading to determine scene prefab and debug modes.
- **Isolation**: `ClearField()` method allows clearing field between tests without scene reload.

### FieldAssertions
Set of methods for field state validation.
- `AssertFieldConsistency(field)`: Verifies reference integrity:
  - Whether each chip in `MainCell` correctly references its `SecondaryCells`.
  - Whether all cells under chip contain correct references to this chip.
  - Whether there are no "phantom" locks.

### TestHelper
Static class with helper methods for tests.
- `FindChipByChipId(field, chipId, exclude)`: Finds specified chip on field by its name (ID), allowing exclusion of already found chips.
- `WaitForSingleRun()`: Pause for visual control when running a single test manually.

## Test Writing Principles
1. **Clean Slate**: Each test should start with a clean field.
2. **Real Input Simulation**: Use `InputHelper` to emulate clicks/drag instead of direct logic method calls (where possible), to test full pipeline.
3. **Assert Consistency**: After each complex manipulation (Merge, Move), call `AssertFieldConsistency`.
