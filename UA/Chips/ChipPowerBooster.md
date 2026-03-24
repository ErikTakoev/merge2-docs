# ChipPowerBooster

[← На Головну](../Main.md)

Чіп-підсилювач (Power Booster) — спеціальний чіп, який модифікує параметри сусідніх чіпів. Наприклад, прискорює зарядку генераторів, що знаходяться поруч.

## Architecture and Responsibility

### 1. `ChipPowerBooster.cs`
Наслідує `Chip`. Основний клас бустера.
- **Дані (Configuration)**: Зберігає `ChipPowerBoosterData` з параметром `Power` (множник підсилення).
- **Підписки**: Вимагає компонент `PowerBoosterCellSubscriber` (`[RequireComponent]`), який відстежує сусідні клітинки та збирає активні модифікатори.
- **Generators**: Делегує колекцію `HashSet<IPowerBoosterModifier>` до `PowerBoosterCellSubscriber.Generators`.
- **Ефекти**: Ініціалізує `connectorCellsHighlightEffect` (див. [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-connector-highlight)).

### 2. `ChipPowerBoosterData`
Серіалізований об'єкт налаштувань бустера.
- **`Power`** (`float`): Множник підсилення, який передається через `IPowerBoosterModifier.ApplyPowerBoosterModifier`.

### 3. `IPowerBoosterModifier` (Interface)
Інтерфейс для чіпів, які можуть отримувати підсилення від бустера.
- **`PowerBoosterModifiers`** (`HashSet<ChipPowerBooster>`): Колекція активних бустерів.
- **`ApplyPowerBoosterModifier(ChipPowerBooster)`**: Застосувати підсилення. Повертає `false`, якщо модифікатор вже активний.
- **`RemovePowerBoosterModifier(ChipPowerBooster)`**: Зняти підсилення.

### 4. `ChipGenerator.PowerBoosterModifier.cs` (Реалізація)
Часткова реалізація `IPowerBoosterModifier` у `ChipGenerator` (partial class).
- **`powerMultiplier`** (`float`): Множник, що прискорює зарядку генератора.
- **Логіка**: При кількох бустерах використовується `Mathf.Max` — береться найбільше значення `Power`. При видаленні останнього бустера `powerMultiplier` скидається до `1f`.

## Subscriber System

Бустер використовує систему [CellSubscriber](../Technical/CellObserverManager.md#subscribers-cellsubscriber) для спостереження за сусідніми клітинками.

Спеціалізована реалізація `PowerBoosterCellSubscriber`:
- Трекає набір `generators: HashSet<IPowerBoosterModifier>` — усі сусідні чіпи, що реалізують `IPowerBoosterModifier`.
- При появі нового чіпа (`OnObservedCellChipChanged`) — додає до `generators` та викликає `ApplyPowerBoosterModifier`.
- При зникненні чіпа — видаляє з `generators` та викликає `RemovePowerBoosterModifier`.
- При переміщенні бустера (`OnChipChangedCell`) — спочатку знімає всі модифікатори, перепідписується на нові клітинки, збирає нові модифікатори через `GetAllChipsByType<IPowerBoosterModifier>`, та повторно застосовує їх.

Детальніше: [Cell Observer Pipeline — Subscribers](../Technical/CellObserverManager.md#subscribers-cellsubscriber).

## Effect Management

- **`connectorCellsHighlightEffect`**: Посилання на ефект `PowerBoosterConnectorCellsHighlightEffect`, що підсвічує клітинки, за якими спостерігає бустер. Ініціалізується в `InitEffects()`.
- **Visual State**:
  - `UpdateVisual()` — деактивує ефект під час руху (`IsMoving()`), активує коли бустер стоїть.
  - `SetMoving(true)` — негайно деактивує ефект на початку перетягування.

Детальніше: [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-connector-highlight).

## Process Flow

1. **Init**: `ChipFactory` створює бустер → `Init(ChipData)` → `InitEffects()` → ініціалізація `connectorCellsHighlightEffect`.
2. **Placement**: `FieldGrid.SetChipInCell` → `CellSubscriber.OnChipChangedCell` → `SubscribeToNeighbors` (обчислення bounding box + neighbors).
3. **Observation**: `CellObserverManager` нотифікує `PowerBoosterCellSubscriber.OnObservedCellChipChanged` → Apply/Remove модифікаторів.
4. **Move**: `SetMoving(true)` → ефект деактивується → після drop: `OnChipChangedCell` → re-subscribe → новий набір модифікаторів.
5. **Destroy**: `OnChipDestroy` → unsubscribe від `CellObserverManager`.
