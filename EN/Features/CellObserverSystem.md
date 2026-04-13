[← Back to Main](../Main.md)

# Cell Observer System (Cell Observation System)

Cell observation system is a key component of Merge2 architecture that allows game objects (chips) and services to efficiently react to changes in game field state. It provides deferred event dispatching, which minimizes unnecessary calculations and guarantees state stability during logic processing.

## Main Components

System consists of three key entities:

### 1. [ChipChangedEvent](../../../Core/Scripts/Field/CellObserver/ChipChangedEvent.cs)
A lightweight structure (struct) describing a single change in a specific grid cell.

- **Properties**:
    - `Cell Cell`: Cell where change happened.
    - `Chip OldChip`: Chip that was in cell before change (null if cell was empty).
    - `Chip NewChip`: Chip that is in cell after change (null if cell is now empty).
- **Helpers**:
    - `ChipAdded`: true if chip appeared in a previously empty cell.
    - `ChipRemoved`: true if chip was removed from or moved out of the cell.
    - `ChipReplaced`: true if one chip was replaced with another within a single cell.

### 2. [DeferredChipChangeNotifier](../../../Core/Scripts/Field/CellObserver/DeferredChipChangeNotifier.cs)
Event aggregator that accumulates all changes during current frame.

- **Deferred processing**: Instead of immediate event dispatch on each change (which may lead to dozens of calls during complex moves/merges), events are collected in `pending` queue.
- **Event collapsing (Collapse)**: If same cell changes multiple times in one frame, system automatically merges these changes into one event, preserving earliest `OldChip` and final `NewChip`.
- **Flush**: Called once at frame end (usually in `LateUpdate`), publishing the whole event batch at once.

### 3. [CellObserverManager](../../../Core/Scripts/Field/CellObserver/CellObserverManager.cs)
Central manager that controls `ICellSubscriber` subscriptions and is responsible for precise event delivery.

- **Two-way indexing**:
    - `cellToSubscribers`: Allows instantly finding all subscribers for a specific cell.
    - `subscriberToCells`: Allows quickly removing all subscriptions of a specific object on destroy.
- **Working with Multi-cell chips**: During event dispatch, manager automatically computes area (footprint) occupied by old chip and occupied by new chip. Events are received by all subscribers that observe at least one cell from this expanded area.

---

## Workflow Pipeline

1. **State change**: `FieldGrid.SetChipInCell` is called to change cell content.
2. **Registration**: `FieldGrid` calls `IChipChangeNotifier.Enqueue`, passing change details.
3. **Accumulation**: `DeferredChipChangeNotifier` adds event into queue or updates existing event for this cell.
4. **Trigger**: At the end of game loop (LateUpdate), `notifier.Flush()` is called.
5. **Dispatch**:
    - `CellObserverManager` receives event list.
    - For each event, influence zone is determined (considering chip sizes).
    - `OnObservedCellChipChanged(evt)` is called for each subscriber in zone.

---

## Optimizations

System is designed to work under high change intensity:

1. **Batching**: Notification happens once per frame. This guarantees subscribers see the final stable field state after all logical manipulations.
2. **Subscriber deduplication**: `eventNotifiedSubscribers` (HashSet) is used so one subscriber does not receive same event multiple times even if it observes several neighboring cells in change zone.
3. **O(1) Operations**: Using dictionaries and hash sets provides instant subscriber lookup and event collapsing.
4. **Safe Callbacks**: `subscriberSnapshot` (List) is used for iteration. This allows safe subscription removal (Unsubscribe) directly during callback processing without breaking collection.
5. **Allocation minimization**: All helper collections (`snapshot`, `dedupe set`) are reused between frames and events, preventing unnecessary Garbage Collector load.

---

## Current Usage

Main usage mechanism is implementation of `ICellSubscriber` interface.

### CellSubscriber
Base component that allows chip to "see" its neighbors.
- Automatically subscribes to all cells around chip (considering its size).
- Re-subscribes when chip moves to a new position.

### PowerBoosterCellSubscriber
Specialized version for [ChipPowerBooster](../Chips/ChipPowerBooster.md).
- Uses events from observed cells to detect generators around itself.
- Dynamically applies or removes acceleration modifiers when neighboring chips appear, disappear, or change.

### Integration Tests
System is actively used in tests (`ChipPowerBoosterTests`) to verify connection integrity and correctness of effect calls during complex field manipulations.
