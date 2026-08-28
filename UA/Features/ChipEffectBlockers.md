# Chip Effect Blockers & Destroying

[← На Головну](../Main.md)

Цей документ описує систему блокувань дій на фішках (Blocking) та механіку їх поступового руйнування (Effect Destroying). Ці системи дозволяють створювати пазл-елементи, де гравець має взаємодіяти з полем, щоб розблокувати певні об'єкти.

## Overview

Блокування в Merge — це стан фішки, який забороняє певні ігрові дії (переміщення, злиття, генерацію тощо). Найчастіше блокування накладається через **Extra-ефекти** (павутина, коробки, комбіновані ефекти), які мають власні правила руйнування.

---

## Blocking System

### `IEffectBlockingSettings`
Контракт конфігурації блокувань, що визначає дозволені дії на чіпі:
- **`CanBeMergedAsSource` / `CanBeMergedAsTarget`**: Дозвіл злиття як джерело/ціль.
- **`CanBeFilled`**: Дозвіл наповнення (для `ChipContainer`).
- **`CanGenerate`**: Дозвіл генерації (для `ChipGenerator`).
- **`CanBeTaped`**: Дозвіл натиснення.
- **`CanApplyModifiers`**: Дозвіл впливу на інших (для `ChipPowerBooster`).
- **`CanReceiveModifiers`**: Дозвіл прийому підсилень (для `ChipGenerator`, `ChipPowerBooster`).
- **`CanBeMoved`**: Дозвіл переміщення.
- **`IsLittleChip`**: Зменшення візуалу чіпа, коли ефект активний.
- **`HideEffectIds`** (`HashSet<int>`): Набір ID ефектів, які повинні бути приховані при активному блокуванні.

**Реалізації**:
- **`EffectBlockingSettings`** (`ScriptableObject`): Серіалізована конфігурація, що налаштовується в Inspector. Поле `hideEffectIds` (int[]) напряму конфігурується через атрибут `[EffectSelector]` і кешується у `HideEffectIds`.
- **`CombinedBlockingState`**: Runtime-агрегат усіх активних `IEffectBlockingSettings` на чіпі. `ApplyBlock` використовує AND-логіку для bool-прапорців та OR для `IsLittleChip`. `RemoveBlock` тригерить повний `Recalculate` з усіх залишених блоків. `HideEffectIds` — union усіх активних.

---

## Effect Destroying System

Система руйнування ефектів дозволяє ефектам поступово руйнуватися при сусідніх злиттях.

### Configuration
- **`EffectDestroyingSettings`** (`ScriptableObject`): Налаштування руйнування ефекту.
  - `AdditionalStates` (`string[]`): Масив назв додаткових станів/тригерів, що послідовно відтворюються при сусідніх злиттях перед повним руйнуванням ефекту.
  - `Priority` (int): Пріоритет серед активних destroying-ефектів; найвищий обробляється першим.

### Runtime
- **`EffectDestroyingRuntimeData`**: Зберігає поточний `NeighboringMergeCount` для кожного destroying-ефекту.
- **`ChipRuntimeData.EffectDestroyingData`** (`Dictionary<int, EffectDestroyingRuntimeData>`): Словник прогресу руйнування per-effect.

### Lifecycle
1. `Chip.InitDestroyingEffectsData()` — сканує всі ефекти з `DestroyingSettings`, створює `EffectDestroyingRuntimeData` записи.
2. `Chip.UpdatePrioritizingDestroyingEffect()` — обирає ефект з найвищим `Priority` як `effectOfPrioritizingDestroying`.
3. Коли на сусідній клітинці відбувається взаємодія (`OnNeighborsChipOfInteraction`), `HandleDestroyingEffects` інкрементує `NeighboringMergeCount` і викликає `TryDestroyEffect`.
4. Якщо `TryDestroyEffect` повертає `true`, `RemoveEffect` деактивує ефект, видаляє з словника, прибирає блок із `BlockingState`, і обирає наступний пріоритетний ефект.

---

## Blocker Effects (Move Locked, Web, Box, Box And Web)

Extra-ефекти — це набір опціональних візуальних ефектів, що конфігуруються через `ChipExtraEffectsData` (реалізує `IChipSpecialData`).

- **Конфігурація**: `ChipExtraEffectsData` містить масив `ExtraEffectData[]`, де кожен елемент визначає `effectName` (рядок, що резолвиться в ID через `EffectConsts.GetIdByName`) та `Prefab` (ефект-префаб).
- **Активація**:
  - Ефект активується, коли його ID є в `ChipRuntimeData.EffectEnables`.
  - При ініціалізації (`Chip.InitEffects`) ітерується масив `ChipExtraEffectsData.Blockers`. Для кожного, чий `EffectId` є в `runtimeData.EffectEnables`, інстантіюється префаб і додається в словник ефектів.
  - Метод `Chip.UpdateVisual()` активує ефекти, чиї ID є в `EffectEnables`.
- **Деактивація**: Коли ID видаляється з `EffectEnables`.
- **Blocking**: Кожен extra-ефект має `EffectBlockingSettings`, які при активації передаються в `CombinedBlockingState` чіпа.
- **Знищення**: Extra-ефекти підтримують систему руйнування (див. [Effect Destroying System](#effect-destroying-system)).

---

## Technical Implementation (Chip Locking)

Механіка **Chip Locking** (Move Locked) є найбільш поширеним застосуванням системи блокувань.

### Data (FieldData)
Стан блокування зберігається у структурі `ChipSpawnData` всередині `FieldData`:

```csharp
public struct ChipSpawnData
{
    [ChipSelector]
    public string ChipId;
    [EffectBlockerSelector]
    public int[] BlockerEffectIds; // Містить IDs ефектів (наприклад, EffectConsts.Blockers.MoveLockedEffect = 103)
}
```

Конфігурація lock-ефекту зберігається в `ChipData.specialDatas` через `ChipExtraEffectsData`.

### Runtime (Chip)
Клас `Chip` містить `ChipRuntimeData`, який синхронізується з даними рівня:
- **`runtimeData.EffectEnables`**: Набір активних blocker-ефектів.
- **`CombinedBlockingState`**: Агрегований стан блокувань, перевіряється через `BlockingState.CanBeMoved`.
- **`OnDraggingChipWithMoveLocked()`**: Віртуальний метод для зворотного зв'язку при спробі перетягнути заблоковану фішку. Спочатку намагається відправити тригер `"MoveLocked"` у `effectOfPrioritizingDestroying`; якщо його немає — у ефект з ключем `EffectConsts.Blockers.MoveLockedEffect`.

### Level Editor
Система блокувань повністю інтегрована у візуальний редактор рівнів:
1. **Контекстне меню**: Правий клік по фішці на сітці відкриває меню з переліком усіх доступних blocker-ефектів. Імена ефектів отримуються через `EffectConsts.GetNameByIdEditorOnly`. Активні на цей момент ефекти позначаються галочкою «✓».
2. **Візуальна індикація**: У режимі редактора активні ефекти відображаються у вигляді іконок (прев'ю префабів відповідних ефектів) у нижньому правому куті клітинки. Це дозволяє одразу бачити всі накладені блоки.
3. **Збереження**: Стан зберігається через масив `BlockerEffectIds` в `ChipSpawnData` при збереженні ассету рівня.

### Use in Gameplay
Блокування через `EffectBlockingSettings` напряму впливає на ігрові механіки:
- **Переміщення**: Якщо `CanBeMoved = false`, гравець не може почати перетягування фішки. При спробі перетягнути спрацьовує візуальний фідбек (`OnDraggingChipWithMoveLocked`).
- **Слот підсилення**: Якщо `CanReceiveModifiers = false`, фішка ігнорує підсилення від бустерів (Power Boosters).
- **Relocation (Релокація)**: Заблоковані фішки виступають як **нерухомі перешкоди**. Система автоматичної релокації не може змістити таку фішку. Якщо для виконання дії (наприклад, злиття або переміщення іншої великої фішки) потрібно звільнити місце, де знаходиться заблокований об'єкт, вся дія відміняється, і об'єкт гравця повертається на місце.
