# ChipGenerator (Generator)

[← Back to Main](../Main.md)

`ChipGenerator` is a specialized component that creates (spawns) new chips on the game field. It works as a state machine that switches between charging and readiness to generate.

## Architecture and Responsibility

### 1. `ChipGenerator.cs` (Main Component)
The `ChipGenerator` class controls generator lifecycle, combining update logic and events.
- **Input data (Input)**:
  - **Manual**: Handles `OnTap` (player click).
  - **Auto**: Subscribes to `field.OnChangeField` to auto-spawn when free space appears.
- **Data initialization**: In `Init(ChipData)`, reads configuration via `data.GetSpecialData<ChipGeneratorData>()`. If `ChipGeneratorData` is missing, generator logic does not start.
- **Update Loop**: In `Update` method, controls recharge timer.
- **Dependencies**:
  - `IFreeCellFinder`: Logic for finding the nearest free cell.
  - `ChipFactory`: Factory for creating new objects.

### 2. `ChipGeneratorData.cs` (Configuration)
Data with settings and selection algorithms. Implements `IChipSpecialData` and is added to `ChipData.specialDatas`.
- **Parameters**: `ChargeCount`, `ChargingTime`, `GenerationInterval`, `TotalRecharges`
- **Chip selection**: Uses **Weighted Random** mechanism to choose type (see [MergeableChipLogic](../Interactions/MergeableChipLogic.md#result-calculation-weighted-random))

### 3. `ChipGeneratorRuntimeData.cs` (Runtime State)
Extends base `ChipRuntimeData` to store dynamic generator state.

**Inherited properties** (from `ChipRuntimeData`):
- **`EffectEnables`** (`HashSet<int>`): Set of active blocker effects (from `EffectConsts`/`EffectConsts.Blockers`). Affects logic indirectly through activation of relevant effects, whose configuration later contributes to global `BlockingState` (see [Chip Runtime Data](Chip.md#1-chipcs-base-class)).

**Own properties**:
- **`IsCharged`**: Whether the generator is ready to create a new chip. When `true`, generator can spawn a chip (manually or automatically).
- **`ChargeCount`**: Number of chips the generator can create in the current charge cycle. Decreases with each generation.
- **`ChargingTimeLeft`**: Current charging time (in seconds). Increases in `Update()` until reaching `CurrentTargetChargingTime`.
- **`RechargesLeft`**: Number of recharge cycles remaining. When it reaches 0, generator evolves into `NextChipData` or is destroyed.
- **`IsWaitingForSpace`**: Waiting-for-space state (Auto mode only). When `true`, generator waits for a free cell to appear.
- **`CurrentTargetChargingTime`**: Current target time to complete charging. Can be equal to:
  - `ChargingTime` — on full recharge (when `ChargeCount` reached 0).
  - `GenerationInterval` — on short delay between generations within one cycle.

**Initialization**: Created in constructor based on `ChipGeneratorData` (obtained via `GetSpecialData<ChipGeneratorData>()`), setting initial values from settings.
6. **Update Visual**: Call `UpdateVisual()` to synchronize effects after charge state change.

### Recharge
1. **Update**: Every frame, `ChargingTimeLeft` increases until `CurrentTargetChargingTime` is reached.
2. **Visuals**: `OnCharging` event is invoked -> `ChipGeneratorEffect` updates progress mask.
3. **Complete**: When time is over, readiness is restored (`IsCharged`). If it was a full cycle, charge count is restored. `UpdateVisual()` is called (if chip is not being dragged).

## Effects and Visualization
Generator controls two effects stored in `effects` dictionary with keys from `EffectConsts`:

- **`EffectConsts.GeneratorCharging`**: Accessed through `GetEffect<IEffectGeneratorCharging>(EffectConsts.GeneratorCharging)`
  - Implementation: [ChipGeneratorRechargeEffect](../Visuals/Effects.md#5-generator-charging)
  - Displays recharge progress. Active when `IsCharged` = `false`.
  - Implements `IEffectGeneratorCharging`, its method `OnCharging(float progress)` updates recharge mask.
  
- **`EffectConsts.GeneratorCharged`**: Accessed through `GetEffect(EffectConsts.GeneratorCharged)`
  - Implementation: base `Effect`
  - Active when `IsCharged` = `true` and chip is not being dragged.
  - Usually this is a cyclic idle animation (glow, pulse) that shows the player the object is ready.


- **Animator**: Uses `Generate` trigger (on spawn) and `Recharge` trigger (on charge completion).
