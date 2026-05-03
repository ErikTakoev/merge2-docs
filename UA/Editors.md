# Editors

[← На Головну](Main.md)

# Level Editor

Цей інструмент призначений для створення та редагування ігрових рівнів Merge Toolkit безпосередньо в Unity Editor.

## Main Features
- **Palette Panel**: Вибір фішок для розміщення на полі (ліва панель).
- **Scene View**: Основний робочий простір. Весь функціонал редагування — розміщення фішок, перетягування та контекстне меню — перенесено безпосередньо у **Scene View** Unity для прямої та інтуїтивної взаємодії з полем.

- **Properties Panel**: Налаштування параметрів рівня (розмір сітки, колекція фішок, вибір активного рівня) та валідація (права панель).
- **Undo/Redo System**: Можливість скасовувати та повертати будь-які дії редагування.

## Undo/Redo System
Редактор підтримує повну історію дій, що дозволяє безпечно експериментувати з дизайном рівня.

### Hotkeys
- **Ctrl + Z**: Скасувати останню дію (Undo).
- **Ctrl + Y** або **Ctrl + Shift + Z**: Повернути скасовану дію (Redo).

### Supported Actions (Scene View)
- **Розміщення фішок**: Drag-and-drop фішки з палітри прямо на сітку в Scene View.
- **Переміщення фішок**: Перетягування існуючих фішок по сітці.
- **Контекстне меню**: Правий клік по клітинці в Scene View відкриває меню швидких дій (видалення, блокування тощо).
- **Зміна стану блокування**: Через контекстне меню у Scene View.
- **Зміна розміру сітки**: Через Properties Panel (Apply Grid Resize).
- **Вибір рівня**: Вибір асета `FieldData` безпосередньо в Scene View для швидкого перемикання між рівнями.
- **Очищення всього поля**: Через Properties Panel або контекстне меню.

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
Замість прямої зміни полів `placedChips` або `gridSize`, методи редактора (наприклад, `PlaceChip`), що викликаються через події Scene View або UI, створюють відповідний об'єкт команди та викликають `RecordAndExecute(command)`. Цей метод делегує виконання `EditorCommandHistory`, який додає команду в стек і викликає `Repaint()` Scene View та вікна редактора.

---

# Chip Creator (Chip Viewer)

Цей інструмент надає централізований інтерфейс для перегляду, створення та модифікації асетів `ChipData`.

## Main Features
- **Chip List**: Перегляд усіх фішок у проекті з іконками (ліва панель).
- **Properties Editor**: Перегляд та редагування серіалізованих даних фішки (права панель).
- **Special Data Section**: Керування поліморфними даними `ChipData.specialDatas` (типи `IChipSpecialData`) з додаванням/видаленням елементів.
- **Default Special Data (Create Only)**: У вкладці Settings можна задати шаблони `defaultSpecialDatas`, які автоматично застосовуються лише під час `Create Chip`.
- **Merge Configuration**: Merge-правила задаються вручну через `ChipMergeData` у секції `Special Data` (без авто-створення self-merge комбінації).
- **Undo/Redo System**: Повна підтримка скасування змін властивостей та перейменування.
- **Asset Management**: Кнопки для створення нових фішок, збереження змін та оновлення списку; при відсутності template-prefab показується попередження і створення блокується.

## Undo/Redo System
редактор фішок використовує той самий патерн **Command**, що і Редактор Рівнів, для відстеження змін.

### Hotkeys
- **Ctrl + Z**: Скасувати останню зміну.
- **Ctrl + Y** або **Ctrl + Shift + Z**: Повернути скасовану зміну.

### Supported Actions
- **Редагування властивостей**: Усі зміни, зроблені в `SerializedObject` під час редагування в інспекторі.
- **Редагування Special Data**: Додавання/видалення записів у `specialDatas`, редагування полів конкретного типу (`ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, тощо).
- **Перейменування**: Перейменування асета фішки (також перейменовує пов'язаний префаб).
- **Стабільний Undo/Redo після rename/type-change**: Після змін імені або `Type` список фішок і фільтри синхронізуються коректно.

## Technical Implementation

### Key Components
- **`IChipCreatorCommand`**: Інтерфейс для команд, що керують станом `ChipData`.
- **`ChipCreatorCommandHistory`**: Керує стеками undo/redo спеціально для контексту редагування фішок.
- **`ChipCreatorWindow.SpecialDataSection.cs`**: Окремий partial для UI секції `Special Data`.
    - Ліниво знаходить всі реалізації `IChipSpecialData` в завантажених assembly.
    - Показує кожен елемент `specialDatas` як foldout з редагуванням полів через `SerializeReference`.
    - Дозволяє додати лише ті типи, яких ще немає в поточному `ChipData` (без дублювань типів).
    - Дозволяє видалення елемента зі списку.
- **`ChipCreatorWindow.RightPanel.cs` (Settings)**:
    - Містить секцію **Default Special Data (Create Only)** для налаштування `defaultSpecialDatas` у `ChipCreatorSettings`.
    - Дозволяє додавати тільки унікальні типи `IChipSpecialData` (один тип — один запис), редагувати їх поля та видаляти записи.
- **`ChipCreatorSettings`**:
    - Зберігає `defaultSpecialDatas` замість окремого поля для default-префаба lock-ефекту.
    - Рекомендований сценарій для lock-ефекту: додати `ChipMoveLockedData` у `defaultSpecialDatas` і задати `Prefab`.
- **`ChipCreatorWindow.UndoRedo.cs`**: Містить визначення команд:
    - `SetChipPropertyCommand` — записує JSON-знімки стану асета `ChipData` "до" та "після".
    - `RenameChipCommand` — обробляє перейменування асета як для `ChipData`, так і для префаба.
    - При Undo/Redo змін, що впливають на список (name/type), виконується синхронізація `SerializedObject`, сортування списку та оновлення фільтрів.
- **`ChipCreatorWindow.CreateChip.cs`**:
    - Кешує список template-префабів для створення нових чіпів.
    - Коректно обробляє порожню папку шаблонів (показує warning замість некоректного створення).
    - Під час `Create Chip` клонує всі елементи з `DefaultSpecialDatas` у новий `ChipData` (`ApplyDefaultSpecialDataToNewChip` + `CloneSpecialData`).

### Workflow (Flow)
Коли властивість змінюється в методі `DrawRightPanel`, вікно створює `SetChipPropertyCommand` із захопленням станів асета. Це включає як звичайні поля `ChipData`, так і секцію `Special Data` (зокрема `ChipMergeData`). Для перейменування використовується `RenameChipCommand`. Ці команди передаються в `RecordAndExecute(command)`, що оновлює історію та забезпечує оновлення `SerializedObject`. Під час створення нового чіпа (`Create Chip`) застосовуються шаблони з `defaultSpecialDatas`, тому default-налаштування `MoveLocked` також переносяться через `ChipMoveLockedData`, а не через окреме поле префаба.
