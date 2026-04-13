# ChipContainer (Container)

[← Back to Main](../Main.md)

`ChipContainer` is an interactive chip that accepts other chips to complete quests or orders. It inherits from the base `Chip` class and extends its functionality with item accumulation.

## Architecture and Responsibility

### 1. `ChipContainer.cs` (Container)
The `ChipContainer` class stores fill state and controls visual effects.
- **Properties (Data)**:
  - `ChipContainerData`: Container settings (list of required items `containers`, reward `NextChipData`), obtained via `data.GetSpecialData<ChipContainerData>()`.
- **Events and Delegates**:
  - `FillContainerDelegate`: Delegate for the container state update event.
  - `OnFillContainer`: Event invoked on initialization, item addition, or full completion.
- **Events and Delegates**:
  - `FillContainerDelegate`: Delegate for the container state update event.
  - `OnFillContainer`: Event invoked on initialization, item addition, or full completion.
- **Effects**:
  - **[ChipContainerEffect](../Visuals/Effects.md#3-container-requirements)**: A special effect implementing `IEffectContainer` that visualizes required items ("bubbles") above the container. Assigned through `EffectContainerRef` (InterfaceRef).

### 1.1. `ChipContainerRuntimeData.cs` (Runtime State)
Extends base `ChipRuntimeData` to store dynamic container state.

**Inherited properties** (from `ChipRuntimeData`):
- **`IsMoveLocked`**: Lock indicator (inherited from base state). Affects logic indirectly through effect activation, whose configuration later contributes to the global `BlockingState` (see [Chip Runtime Data](Chip.md#1-chipcs-base-class)).

**Own properties**:
- **`containers`**: Dictionary (`Dictionary<ContainerInfo, int>`) tracking current fill progress for each requirement.
  - **Key (`ContainerInfo`)**: Requirement definition (item type, ID, required count).
  - **Value (`int`)**: Current number of collected items for this requirement.
  - **Completion**: When a requirement is fully completed (counter reaches `ContainerInfo.Count`), it is removed from the dictionary. When the dictionary is empty, the container is considered filled.

**Initialization**: Created in `ChipContainer.Init()` based on `ChipContainerData` (via `GetSpecialData<ChipContainerData>()`). Initial counter values are set to 0.

### 2. `FillContainerLogic.cs` (Logic)
Controls the process of adding chips to the container through the `IChipInteractionLogic` mechanism.  
For detailed description (CanInteract, ExecuteInteraction, target objects), see [FillContainerLogic](../Interactions/FillContainerLogic.md).

## Key Methods (`ChipContainer.cs`)

### `IsChipCompatible(Chip chip)`
Checks whether the chip `Type` or `Name` matches at least one active requirement in `containerRuntimeData.containers`. Does not change state.

### `TryAddChip(Chip chip)`
Main method for adding a chip:
1. Finds the matching requirement in `containers` dictionary.
2. If the counter reaches required amount:
   - Removes the requirement from the dictionary.
   - Sends `Recharge` animation trigger.
   - Calls `OnFillContainer` with `isFull` flag.
   - **Completion**: If all requirements are completed, the container is destroyed via `this.Destroy(cell)`, and `NextChipData` is created in its place using `ChipFactory`.
3. If the counter is not full yet, simply increments its value.
4. **Result**: Returns `true` if the chip was accepted successfully.

## Visual Effects

### ChipContainerEffect.cs
A specialized effect implementing `IEffectContainer` to visualize container requirements. Controls a panel above the chip showing required items.

The effect is stored in `effects` dictionary with key `EffectConsts.ContainerRequirements` and is available through:
```csharp
var containerEffect = GetEffect<IEffectContainer>(EffectConsts.ContainerRequirements);
containerEffect?.UpdateElements(this, containers, false);
```

For detailed implementation of the effect, see [Effects.md](../Visuals/Effects.md#3-container-requirements).

## Flow

1. **Input**: Player starts dragging a chip.
2. **Move**: Through `FillContainerLogic.CanInteract`, the system continuously checks compatibility when hovering over containers.
3. **Check**: Through `IsChipCompatible`, the container confirms that it needs this chip.
4. **Drop**: On release, `ExecuteInteraction` -> `TryAddChip` is called.
5. **Update**: Container removes/changes requirements, `ChipContainerEffect` updates "bubbles" (or hides them if requirement is completed).
6. **Complete**: If no requirements remain, the container is replaced with a new chip (result).
