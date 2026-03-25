# Editors

[← На Головну](Main.md)

# Level Editor

Цей інструмент призначений для створення та редагування ігрових рівнів Merge Toolkit безпосередньо в Unity Editor.

## Main Features
- **Palette Panel**: Вибір фішок для розміщення на полі (ліва панель).
- **Grid Editor**: Візуальне поле для розташування фішок за допомогою Drag-and-Drop (центральна панель).
- **Properties Panel**: Налаштування параметрів рівня (розмір сітки, колекція фішок) та валідація (права панель).
- **Undo/Redo System**: Можливість скасовувати та повертати будь-які дії редагування.

## Undo/Redo System
Редактор підтримує повну історію дій, що дозволяє безпечно експериментувати з дизайном рівня.

### Hotkeys
- **Ctrl + Z**: Скасувати останню дію (Undo).
- **Ctrl + Y** або **Ctrl + Shift + Z**: Повернути скасовану дію (Redo).

### Supported Actions
- Розміщення фішок на полі.
- Переміщення фішок між комірками.
- Видалення фішок.
- Зміна стану блокування фішки (Move Lock).
- Зміна розміру сітки (Apply Grid Resize).
- Очищення всього поля (Clear All Chips).

## Technical Implementation
Система Undo/Redo побудована на патерні **Command**.

### Key Components
- **`IEditorCommand`**: Інтерфейс для всіх команд редактора. Кожна команда реалізує логіку виконання (`Execute`) та відкату (`Undo`).
- **`EditorCommandHistory`**: Клас-менеджер, що керує двома стеками (Undo та Redo).
- **`LevelEditorWindow.UndoRedo.cs`**: Частковий клас (partial class), що містить визначення всіх конкретних команд:
    - `PlaceChipCommand` — розміщення нової фішки.
    - `MoveChipCommand` — переміщення існуючої фішки.
    - `RemoveChipCommand` — видалення фішки.
    - `ToggleMoveLockCommand` — перемикання прапорця блокування.
    - `ResizeGridCommand` — зміна розміру сітки з видаленням фішок поза межами.
    - `ClearAllChipsCommand` — повне очищення поля.

### Workflow (Flow)
Замість прямої зміни полів `placedChips` або `gridSize`, методи редактора (наприклад, `PlaceChip`) створюють відповідний об'єкт команди та викликають `RecordAndExecute(command)`. Цей метод делегує виконання `EditorCommandHistory`, який додає команду в стек і викликає `Repaint()`.

---

# Chip Creator (Chip Viewer)

Цей інструмент надає централізований інтерфейс для перегляду, створення та модифікації асетів `ChipData`.

## Main Features
- **Chip List**: Перегляд усіх фішок у проекті з іконками (ліва панель).
- **Properties Editor**: Перегляд та редагування серіалізованих даних фішки (права панель).
- **Special Data Section**: Керування поліморфними даними `ChipData.specialDatas` (типи `IChipSpecialData`) з додаванням/видаленням елементів.
- **Undo/Redo System**: Повна підтримка скасування змін властивостей та перейменування.
- **Asset Management**: Кнопки для створення нових фішок, збереження змін та оновлення списку.

## Undo/Redo System
редактор фішок використовує той самий патерн **Command**, що і Редактор Рівнів, для відстеження змін.

### Hotkeys
- **Ctrl + Z**: Скасувати останню зміну.
- **Ctrl + Y** або **Ctrl + Shift + Z**: Повернути скасовану зміну.

### Supported Actions
- **Редагування властивостей**: Усі зміни, зроблені в `SerializedObject` під час редагування в інспекторі.
- **Редагування Special Data**: Додавання/видалення записів у `specialDatas`, редагування полів конкретного типу (`ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, тощо).
- **Перейменування**: Перейменування асета фішки (також перейменовує пов'язаний префаб).

## Technical Implementation

### Key Components
- **`IChipCreatorCommand`**: Інтерфейс для команд, що керують станом `ChipData`.
- **`ChipCreatorCommandHistory`**: Керує стеками undo/redo спеціально для контексту редагування фішок.
- **`ChipCreatorWindow.SpecialDataSection.cs`**: Окремий partial для UI секції `Special Data`.
    - Ліниво знаходить всі реалізації `IChipSpecialData` в завантажених assembly.
    - Показує кожен елемент `specialDatas` як foldout з редагуванням полів через `SerializeReference`.
    - Дозволяє додати лише ті типи, яких ще немає в поточному `ChipData` (без дублювань типів).
    - Дозволяє видалення елемента зі списку.
- **`ChipCreatorWindow.UndoRedo.cs`**: Містить визначення команд:
    - `SetChipPropertyCommand` — записує JSON-знімки стану асета `ChipData` "до" та "після".
    - `RenameChipCommand` — обробляє перейменування асета як для `ChipData`, так і для префаба.

### Workflow (Flow)
Коли властивість змінюється в методі `DrawRightPanel`, вікно створює `SetChipPropertyCommand` із захопленням станів асета. Це включає як звичайні поля `ChipData`, так і секцію `Special Data`. Для перейменування використовується `RenameChipCommand`. Ці команди передаються в `RecordAndExecute(command)`, що оновлює історію та забезпечує оновлення `SerializedObject`.
