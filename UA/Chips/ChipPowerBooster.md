# ChipPowerBooster

[← На Головну](../Main.md)

Чіп-підсилювач (Power Booster) — спеціальний чіп, який модифікує параметри сусідніх чіпів. Наприклад, прискорює зарядку генераторів, що знаходяться поруч.

## Architecture and Responsibility

### 1. `ChipPowerBooster.cs`
Наслідує `Chip`. Основний клас бустера.
- **Дані (Configuration)**: Зберігає `ChipPowerBoosterData` з параметром `Power` (множник підсилення), читаючи його через `data.GetSpecialData<ChipPowerBoosterData>()`.
- **Підписки**: Вимагає компонент `PowerBoosterCellSubscriber` (`[RequireComponent]`), який відстежує сусідні клітинки та збирає активні модифікатори.
- **ModifiedEntities**: Делегує колекцію `HashSet<IPowerBoosterModifier>` до `PowerBoosterCellSubscriber.ModifiedEntities`.
- **Fail-fast перевірки**: В `Init(ChipData)` перевіряє наявність `PowerBoosterCellSubscriber` і `ChipPowerBoosterData`; при відсутності логіка бустера зупиняється з `Debug.LogError`.
- **Ефекти**: Ініціалізує `connectorCellsHighlightEffect` (див. [Visual Effects § 7](../Visuals/Effects.md#7-power-booster-connector-highlight)).

### 2. `ChipPowerBoosterData`
Серіалізований об'єкт налаштувань бустера.
- Реалізує `IChipSpecialData` і зберігається в `ChipData.specialDatas`.
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
- Трекає набір `modifiedEntities: HashSet<IPowerBoosterModifier>` — усі сусідні чіпи, що реалізують `IPowerBoosterModifier`.
- При появі нового чіпа (`OnObservedCellChipChanged`) — додає до `modifiedEntities` та викликає `ApplyPowerBoosterModifier`.
- При зникненні чіпа — видаляє з `modifiedEntities` та викликає `RemovePowerBoosterModifier`.
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
5. **Destroy**: 
   - Викликається `ChipPowerBooster.Destroy(Cell mainCell)` override.
   - Спочатку `cellSubscriber.OnChipDestroy(mainCell)` викликає `RemovePowerBoosterModifier()` для **всіх** елементів в `modifiedEntities`.
   - Потім очищує колекцію `modifiedEntities.Clear()`.
   - Завершує `base.Destroy(mainCell)`, який відписує бустер від `CellObserverManager`.
   - **Результат**: Всі пов'язані генератори отримують очищення модифікаторів перед знищенням бустера, забезпечуючи консистентність стану при merge/еволюції бустера (наприклад, при злитті двох бустерів в інший тип чіпа).
