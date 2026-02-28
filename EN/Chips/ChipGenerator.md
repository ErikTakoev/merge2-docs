# ChipGenerator (Generator)

[← Back to Main](../Main.md)

`ChipGenerator` is a specialized component that creates (spawns) new chips on the game field. It works as a state machine that switches between charging and readiness for generation.

## Architecture and Responsibility

### 1. `ChipGenerator.cs` (Main Component)
The `ChipGenerator` class manages the generator's lifecycle, combining update logic and events.
- **Input Data**:
  - **Manual**: Handles `OnTap` (player click).
  - **Auto**: Subscribes to `field.OnChangeField` to automatically spawn when free space appears.
- **Update Loop**: In the `Update` method, manages the recharge timer.
- **Dependencies**:
  - `IFreeCellFinder`: Logic for finding the nearest free cell.
  - `ChipFactory`: Factory for creating new objects.

### 2. `ChipGeneratorData.cs` (Configuration)
Data contains configuration and selection algorithms.
- **Properties**:
  - `Data`: Probability weights for different chip types.
  - `ChargeCount/ChargingTime`: Balance parameters.
  - `GenerationInterval`: Delay time between generations within one charge cycle.
  - `TotalRecharges`: Generator lifetime limit.
- **Logic (`GenerateChipData`)**: Method that performs weighted random selection (`Weighted Random`) to determine the next chip.

### 3. `ChipGeneratorRuntimeData.cs` (Runtime State)
Extends base `ChipRuntimeData` to store dynamic generator state.

**Inherited properties** (from `ChipRuntimeData`):
- **`IsMoveLocked`**: Determines whether the generator's movement is blocked. For details, see [Chip Runtime Data](Chip.md#runtime-data-in-game-state).

**Own properties**:
- **`IsCharged`**: Is the generator ready to create a new chip? When `true`, the generator can spawn (manually or automatically).
- **`ChargeCount`**: Number of chips the generator can create in the current charge cycle. Decreases with each generation.
- **`ChargingTimeLeft`**: Current charging time (in seconds). Increases in `Update()` until reaching `CurrentTargetChargingTime`.
- **`RechargesLeft`**: Number of recharge cycles remaining. When reaching 0, the generator evolves into `NextChipData` or is destroyed.
- **`IsWaitingForSpace`**: Waiting state (auto mode only). When `true`, the generator waits for free space to appear.
- **`CurrentTargetChargingTime`**: Current target time for charging completion. Can be equal to:
  - `ChargingTime` — during full recharge (when `ChargeCount` reached 0).
  - `GenerationInterval` — during short delay between generations in one cycle.

**Initialization**: Created in the constructor based on `ChipGeneratorData`, setting initial values from settings.

## Methods and Logic
- **`InitEffects()` (Override)**: Overrides the base method to initialize generator-specific effects. First calls `base.InitEffects()` to instantiate standard effects, then adds `chargedEffect` and `rechargeEffect` (via `InterfaceRef<IEffect>` and `InterfaceRef<IEffectGeneratorCharging>`) if assigned in the inspector. This allows the generator to extend base class behavior while maintaining loose coupling.
- **`UpdateVisual()` (Override)**: Overrides the base method to manage generator-specific effects. Activates `chargedEffect` and deactivates `rechargeEffect` when the generator is charged (`IsCharged`), and vice versa.
- **`SetMoving(bool)` (Override)**: Overrides the base method to deactivate `chargedEffect` during the start of movement to avoid cluttering the field's visuals.

## Flow

### Generation
1. **Trigger**: `OnTap` event or `Update` (if Auto).
2. **Check**: Check for charge (`IsCharged`) and search for free space (`IFreeCellFinder`). The `onlyAround` parameter is set to `true` only for automatic mode (`IsAutoGeneration`) so chips appear near the generator. In manual mode (tap), the search covers the entire field.
3. **Select**: Call `generatorData.GenerateChipData()` to select chip type.
4. **Spawn**: `ChipFactory.CreateChip` in the found cell.
5. **Consume**: Decrease `ChargeCount`.
    - If `ChargeCount > 0`: Set short recharge (`GenerationInterval`).
    - If `ChargeCount == 0`: Decrease `RechargesLeft` and set full recharge (`ChargingTime`).
6. **Update Visual**: Call `UpdateVisual()` to sync effects after charge state change.

### Recharge
1. **Update**: Each frame increases `ChargingTimeLeft` until reaching `CurrentTargetChargingTime`.
2. **Visuals**: `OnCharging` event is called -> `ChipGeneratorEffect` updates progress mask.
3. **Complete**: When time expires, readiness is restored (`IsCharged`). If this was a full cycle, charge count is restored. `UpdateVisual()` is called (if chip is not being dragged).

## Effects and Visualization
- **[ChipGeneratorRechargeEffect](../Visuals/Effects.md#3-chip-generator-progress-generator)**: Displays recharge progress. Implements `IEffectGeneratorCharging`. Active when `IsCharged` = `false`.
- **ChargedEffect (generator readiness)**: Implements `IEffect`. Active when `IsCharged` = `true` and chip is not being dragged.
- **Animator**: Uses triggers `Generate` (on spawn) and `Recharge` (on charge completion).

