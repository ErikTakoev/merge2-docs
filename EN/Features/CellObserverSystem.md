[← Back to Main](../Main.md)

# Cell Observer System

The cell observation system is a key component of the Merge2 architecture, allowing game objects (chips) and services to effectively react to field state changes. It provides deferred event dispatching, which minimizes redundant calculations and ensures state stability during logic processing.

## Core Components

The system consists of three key entities:

### 1. [ChipChangedEvent](../../../Core/Scripts/Field/CellObserver/ChipChangedEvent.cs)
A lightweight structure (struct) describing a single change in a specific grid cell.

- **Properties**:
    - `ICell Cell`: The cell where the change occurred.
    - `Chip OldChip`: The chip that was in the cell before the change (null if the cell was empty).
    - `Chip NewChip`: The chip that is in the cell after the change (null if the cell is now empty).
- **Helpers**:
    - `ChipAdded`: true if a chip appeared in a previously empty cell.
    - `ChipRemoved`: true if a chip was removed or moved from the cell.
    - `ChipReplaced`: true if one chip was replaced by another within the same cell.

### 2. [DeferredChipChangeNotifier](../../../Core/Scripts/Field/CellObserver/DeferredChipChangeNotifier.cs)
An event aggregator that accumulates all changes during the current frame.

- **Deferred Processing**: Instead of immediate event dispatch upon every change (which could lead to dozens of calls during complex moves/merges), events are gathered in a `pending` queue.
- **Event Collapse**: If the same cell (`ICell` implementation) changes multiple times in one frame, the system automatically merges these changes into one event, keeping the very first `OldChip` and the final `NewChip`.
- **Flush**: Called once at the end of the frame (usually in `LateUpdate`), publishing the entire batch of events at once.

### 3. [CellObserverManager](../../../Core/Scripts/Field/CellObserver/CellObserverManager.cs)
Central manager that controls `ICellSubscriber` subscriptions and is responsible for accurate event delivery.

- **Two-way Indexing**:
    - `cellToSubscribers`: Allows instantly finding all subscribers for a specific cell (`ICell`).
    - `subscriberToCells`: Allows quickly removing all subscriptions of a specific object when it is destroyed.
- **Multi-cell Chip Support**: When dispatching events, the manager automatically calculates the footprint occupied by the old chip and the new one. Events are received by all subscribers observing at least one cell (`ICell`) within this expanded area.

---

## Workflow

1. **State Change**: `FieldGrid.SetChipInCell` is called to change cell content.
2. **Registration**: `FieldGrid` calls `IChipChangeNotifier.Enqueue`, passing change details.
3. **Accumulation**: `DeferredChipChangeNotifier` adds the event to the queue or updates an existing one for this cell.
4. **Trigger**: At the end of the game loop (LateUpdate), `notifier.Flush()` is called.
5. **Dispatch**:
    - `CellObserverManager` receives the list of events.
    - For each event, an influence zone is determined (considering chip sizes).
    - For each subscriber in the zone, `OnObservedCellChipChanged(evt)` is called.

---

## Optimizations

The system is designed to work under high-intensity change conditions:

1. **Batching**: Notification happens once per frame. This ensures that subscribers see the final stable state of the field after all logical manipulations.
2. **Subscriber Deduplication**: `eventNotifiedSubscribers` (HashSet) is used so a subscriber doesn't receive the same event multiple times, even if they observe several neighboring cells in the change zone.
3. **O(1) Operations**: Use of dictionaries and hash sets ensures instant subscriber lookup and event merging.
4. **Safe Callbacks**: `subscriberSnapshot` (List) is used for iteration. This allows safely unsubscribing directly during callback processing without breaking the collection.
5. **Allocation Minimization**: All auxiliary collections (`snapshot`, `dedupe set`) are reused between frames and events, preventing excessive Garbage Collector load.

---

## Current Usage

The primary usage mechanism is implementing the `ICellSubscriber` interface.

### CellSubscriber
Base component that allows a chip to "see" its neighbors.
- Automatically subscribes to all cells around the chip (considering its size).
- Resubscribes when the chip moves to a new position.

### PowerBoosterCellSubscriber
Specialized version for [ChipPowerBooster](../Chips/ChipPowerBooster.md).
- Uses events from observed cells to detect generators around itself.
- Dynamically applies or removes acceleration modifiers when neighboring chips appear, disappear, or change.

### Integration Tests
The system is actively used in tests (`ChipPowerBoosterTests`) to verify link integrity and correct effect triggers during complex field manipulations.
