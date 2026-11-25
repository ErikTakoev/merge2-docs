# Editors

[← На Головну](Main.md)

# Level Editor

Цей інструмент призначений для створення та редагування ігрових рівнів Merge Toolkit безпосередньо в Unity Editor.

## Main Features
- **Palette Panel**: Вибір фішок для розміщення на полі (ліва панель).
- **Scene View**: Основний робочий простір. Весь функціонал редагування — розміщення фішок, перетягування та контекстне меню — перенесено безпосередньо у **Scene View** Unity для прямої та інтуїтивної взаємодії з полем.
- **Scene View Overlay Toolbar**: Верхня панель інструментів [`LevelEditorOverlay`](../../Core/Editor/Scripts/LevelEditor/LevelEditorOverlay.cs) безпосередньо у Scene View для швидкого керування режимом редактора, ефектами, прев'ю VScripting-префабу, прев'ю фішок, маскою заблокованих клітинок, перехопленням вводу та відображенням міток координат клітинок `(x,y)`.
- **Properties Panel**: Налаштування параметрів рівня (розмір сітки, колекція фішок, префаб візуального поля, префаб Visual Scripting, вибір активного рівня) та валідація (права панель).
- **Locked Areas Editor**: Спеціальний режим для створення, редагування та видалення заблокованих ділянок та відкладених фішок (Deferred Cells) безпосередньо на сітці.
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
- **Редагування заблокованих ділянок**: Додавання/видалення ділянок, зміна ID, малювання заблокованих та відкладених клітинок (див. розділ "Редагування заблокованих ділянок").

## Editing Locked Areas

Для налаштування заблокованих ділянок та відкладених фішок використовується спеціальний режим редагування.

### Editor Modes

Level Editor підтримує два режими редагування:

#### Mode 1: "Edit Level" (Standard Editing)
- Стандартне розміщення фішок та редагування сітки.
- Палітра фішок видима та активна.

#### Mode 2: "Edit Locked Areas" (Locked Areas Editing)
- Відключені операції перетягування фішок.
- Замість палітри фішок — панель управління заблокованими ділянками:
  - **Додати ділянку (+)**: Новий `LockedAreaData` з унікальним ID.
  - **Видалити ділянку (-)**: Видалення вибраної ділянки.
  - **Вибір активної ділянки**: Dropdown або список для вибору, яку ділянку редагувати.
  - **ID ділянки**: IntField для зміни ID (наприклад, перейменування).
  - **Очистити клітинки**: Кнопка для видалення всіх відмічених клітинок у поточній ділянці.

### Grid Controls

У режимі "Edit Locked Areas" взаємодія із сіткою змінюється:

- **LMB (ліва кнопка миші)**: Малювання клітинок у `CellsToLock` (прості заблоковані клітинки для активної ділянки).
  - Клітинки рисуються **зеленим** напівпрозорим кольором.
  - На кожній клітинці показується текст `L - Area {ID}`.
- **RMB (права кнопка миші)**: Малювання клітинок у `CellsToLockAndDeferred` (заблоковані клітинки з відкладеними фішками для активної ділянки).
  - Клітинки рисуються **синім** напівпрозорим кольором.
  - На кожній клітинці показується текст `D - Area {ID}`.

**Взаємна виключність**: Клітинка не може одночасно перебувати в обох списках однієї чи різних ділянок. Якщо клітинка додається до `CellsToLock`, вона автоматично видаляється з `CellsToLockAndDeferred` (і навпаки).

**Smart Brush**: При натисненні LMB або RMB визначається, чи комірка додається чи видаляється з відповідного списку. Ця операція (додавання або видалення) фіксується на весь період утримання кнопки миші (drag-малювання), забезпечуючи зручне та передбачуване малювання без хаотичних змін.

Детальніше про життєвий цикл та логіку роботи заблокованих ділянок див. у розділі [Заблоковані ділянки (Locked Areas)](../../../MergeCommon/Docs/UA/Features/LockedAreas.md).

## Technical Implementation

### Core Architecture and Synchronization
- **`IMergeLifetimeScope` як єдине джерело правди**: Редактор активується та рендериться лише за наявності компонента `IMergeLifetimeScope` у поточній ієрархії активної сцени. Зв'язок між редактором та сценою є прямим: поле `FieldData` в активному scope виступає єдиним джерелом правди. Будь-які дії створення рівнів (`OnNewLevel`), завантаження або автоматичного збереження (`EnsureAutosaveTarget`, `AutosaveCurrentField`) записують асет безпосередньо в active scope та позначають його зміненим (`EditorUtility.SetDirty`).
- **Інтеграція з VContainer**: Сітка поля (`IFieldGrid`) першочергово розділяється через DI-контейнер (`scope.Container.Resolve<IFieldGrid>()`), що забезпечує повну сумісність з runtime-залежностями та конфігурацією сцени. Якщо контейнер не побудовано в режимі редагування, використовується резервний пошук компонента прямо у сцені.
- **Динамічний тип клітинки для прев'ю (`CellType`)**: Для обчислення локального зміщення фішок (`CalculateChipLocalPosition`) редактор бере `IMergeLifetimeScope.CellType` (за замовчуванням `Cell`) і створює/кешує прихований тимчасовий об'єкт `[LevelEditor_PreviewCell]` через `GetOrCreatePreviewCell()` (`HideFlags.HideAndDontSave`), без потреби у окремому префабі клітинки.
- **Скидання сесії при створенні проєкту (`ResetSessionForProjectCreation`)**: Перед генерацією нового проєкту `NewMergeProjectWindow` викликає `LevelEditorWindow.ResetSessionForProjectCreation()`, який зберігає змінені `FieldData`, очищає стан відкритих вікон редактора рівнів і скидає `EditorContext.ActiveFieldData`, не перезаписуючи асети з розсинхронізованих словників.
- **Ізоляція та придушення в Prefab Mode**: Редактор відстежує стан Prefab Mode за допомогою подій `PrefabStage` та `PrefabStageUtility`. При переході в режим редагування префабів взаємодія з редактором призупиняється, а візуальне прев'ю сцени негайно знищується. Після закриття Prefab Mode редактор автоматично відновлює свій стан та виконує ресинхронізацію.
- **Стійкість до перезавантаження домену (Domain Reload)**: Після перезавантаження домену Unity, коли несеріалізований словник розміщених фішок (`placedChips`) скидається, редактор автоматично виявляє розсинхронізацію та повторно наповнює словник на основі серіалізованого асета `FieldData` з активного scope.
- **Ізольована сцена візуалізації сцени**: Для відображення префабу поля та розміщених фішок редактор динамічно створює окрему тимчасову адитивну сцену (`LevelEditorPreviewScene`). Це запобігає забрудненню або випадковій модифікації основної відкритої сцени проекту. Усі прев'ю-об'єкти створюються з прапорцем `HideFlags.DontSave`.
- **Синхронізація GridAnchor та візуального зміщення (FieldOffset)**: Тимчасовий якір сітки `tempGridAnchor` синхронізує свою позицію та обертання з активною сіткою `gridTransform` та враховує зміщення візуального поля (`IVisualField.FieldOffset` через `GetFieldOffset` і `SyncGridAnchorTransform`). Це забезпечує точне позиціонування розміщених фішок, гріда, міток координат та оверлеїв заблокованих зон у Scene View при наявності зміщень на префабах візуального поля.
- **Scene View Overlay (`LevelEditorOverlay`)**: Клас `LevelEditorOverlay` (транзієнтний оверлей `ITransientOverlay`), що інтегрує елементи керування [`LevelEditorToolbarContent`](../../Core/Editor/Scripts/LevelEditor/LevelEditorOverlay.cs) прямо у верхню тулбар-стрічку Scene View. Відображається лише за умови наявності `IMergeLifetimeScope` у сцені та поза режимами Play Mode і Prefab Mode. Містить іконки перемикання режимів відображення ефектів, прев'ю VScripting-префабу (динамічно з'являється за наявності `VScriptingFieldPrefab` в активному `FieldData`), фішок, заблокованих клітинок, тугл відображення міток координат клітинок (`(x,y)`) та перемикач `Input Interception`, який регулює обробку вводу миші та малювання на сітці.
- **Модульна архітектура розширення (`ILevelEditorContext` / `IFieldModuleEditor`)**:
  - `ILevelEditorContext`: Інтерфейс контексту, що надає доступ до `FieldData`, `ChipDataCollection`, `SerializedFieldData`, сітки (координати ↔ Rect через `GetCellRect` та `TryGetCellAtGUIPosition`, виділення клітинок `SelectedCells`, `Repaint()`) та методів управління змінами (`RecordUndo`, `MarkDirty`, `RequestGridRebuild`). `LevelEditorWindow` реалізує цей інтерфейс безпосередньо.
  - `IFieldModuleEditor<TData>`: Контракт модульних редакторів із життєвим циклом `OnAttach` / `OnDetach`, панеллю `DrawPanelGUI()`, оверлеями `OnDrawGridOverlay()` / `OnDrawCellOverlay()`, перехопленням інпуту `OnCellInput()`, хуками валідації та збереження.
  - `FieldModuleEditorRegistry`: Реєстр на базі `TypeCache`, який автоматично зіставляє типи `IFieldModuleData` з реалізаціями `IFieldModuleEditor<TData>` і надає fallback `MissingFieldModuleEditor` із попередженням `HelpBox`.
  - **Панель вкладок та керування модулями**: Вкладка 0 відповідає за базовий рівень (Level Controls, розмір сітки, палітра фішок). Вкладки 1..N динамічно генеруються для кожного `IFieldModuleData` в `FieldData.ModuleDatas`. Кнопка `+ Add Module` відображає список доступних типів модулів, яких ще немає на поточному полі. Кнопка `Remove Module` всередині вкладки модуля дозволяє видалити його після підтвердження користувачем. Обидві операції підтримують повну історію Undo/Redo через `AddModuleCommand` та `RemoveModuleCommand`.

### Undo/Redo System
Система Undo/Redo побудована на патерні **Command**.

#### Key Components
- **`IEditorCommand`**: Інтерфейс для всіх команд редактора. Кожна команда реалізує логіку виконання (`Execute`) та відкату (`Undo`).
- **`EditorCommandHistory`**: Клас-менеджер, що керує двома стеками (Undo та Redo).
- **`LevelEditorWindow.UndoRedo.cs`**: Частковий клас (partial class), що містить визначення всіх конкретних команд:
    - `PlaceChipCommand` — розміщення нової фішки (інкрементально створює прев'ю-об'єкт нової фішки без перестворення сцени).
    - `MoveChipCommand` — переміщення існуючої фішки (інкрементально змінює позицію transform існуючого прев'ю-об'єкта).
    - `RemoveChipCommand` — видалення фішки (інкрементально знищує прев'ю-об'єкт).
    - `ToggleMoveLockCommand` — перемикання прапорця блокування.
    - `ResizeGridCommand` — зміна розміру сітки з видаленням фішок поза межами (виконує повне оновлення прев'ю-сцени).
    - `ClearAllChipsCommand` — повне очищення поля.
    - `AddLockedAreaCommand` — додавання нової заблокованої ділянки.
    - `RemoveLockedAreaCommand` — видалення заблокованої ділянки (зберігає стан для Undo).
    - `ChangeLockedAreaIdCommand` — зміна ID заблокованої ділянки.
    - `PaintLockedAreaCellsCommand` — редагування `CellsToLock` та `CellsToLockAndDeferred` (малювання заблокованих/відкладених клітинок).
    - `AddModuleCommand` — додавання модуля конфігурації поля (виконує оновлення модульних прев'ю).
    - `RemoveModuleCommand` — видалення модуля конфігурації поля (зберігає стан та індекс для Undo, виконує оновлення модульних прев'ю).

#### Workflow (Flow)
Замість прямої зміни полів `placedChips` або `gridSize`, методи редактора (наприклад, `PlaceChip`), що викликаються через події Scene View або UI, створюють відповідний об'єкт команди та викликають `RecordAndExecute(command)`. Цей метод делегує виконання `EditorCommandHistory`, який виконує команду (зі збереженням в стек Undo), викликає точкове оновлення прев'ю для операцій із фішками (`SpawnChipPreview`, `MoveChipPreview`, `DestroyChipPreview`), синхронізує `FieldData` з позначкою `SetDirty` через `AutosaveCurrentField` (без виклику повного перестворення прев'ю-сцени `UpdateLevelVisualPreview`) та викликає `Repaint()` Scene View і вікна редактора. Повне оновлення сцени візуалізації (`UpdateLevelVisualPreview`) зарезервовано виключно для структурних змін (зміна розмірів сітки, зміна префабу візуального поля, додавання/видалення модулів, завантаження рівня). Запис асетів на диск (`AssetDatabase.SaveAssets`) відкладено до подій втрати фокусу (`OnLostFocus`), закриття вікна або переходу в Play Mode.

---

# Merge Toolkit

Цей інструмент (`MergeToolkitWindow`, відкривається через **Window → Expecto → Merge Toolkit**) надає централізований інтерфейс для перегляду, створення та модифікації асетів `ChipData`, керування конфігурацією проєкту (`Project Settings`) та запуску створення нового мерж-проєкту (`New Project`).

## Main Features
- **Chip List**: Перегляд усіх фішок у проекті з іконками (ліва панель).
- **Properties Editor**: Перегляд та редагування серіалізованих даних фішки (права панель).
- **Base Sprite Validation**: Автоматична валідація наявності спрайта у базового рендерера (`IChipSortingLayer.SortingLayers[0].Renderer`). Якщо рендерер призначений, але поле `sprite` порожнє, відображається `HelpBox` з рівнем `Error` та кнопкою `Open`, яка відкриває префаб у Prefab Mode та виділяє відповідний `GameObject` зі `SpriteRenderer`.
- **Module Data Validation**: Автоматична валідація відповідності модулів префаба конфігураційним даним. Якщо на зв'язаному префабі присутні компоненти `IChipModule` з атрибутом `[RequireModuleData]`, але відповідний тип даних відсутній у `moduleDatas`, під заголовком секції відображається попередження `HelpBox` із зазначенням відсутніх типів.
- **Module Data Section**: Керування поліморфними даними `ChipData.moduleDatas` (типи `IChipModuleData`) з додаванням/видаленням елементів.
- **Default Module Data (Create Only)**: У вкладці **Project Settings** можна задати шаблони `defaultModuleDatas`, які автоматично застосовуються лише під час `Create Chip`.
- **Merge Configuration**: Merge-правила задаються вручну через `ChipMergeData` у секції `Module Data` (без авто-створення self-merge комбінації).
- **Undo/Redo System**: Повна підтримка скасування змін властивостей та перейменування з автоматичним перемиканням на відповідну фішку при глобальному відкаті.
- **Asset Management**: Створення нових фішок та оновлення списку; автоматичне збереження змін на диск (Save on Switch/Close/Blur) при перемиканні між фішками, відкритті вкладок **Project Settings** / **New Chip**, натисканні **New Project**, закритті або втраті фокусу вікна; при відсутності template-prefab показується попередження і створення блокується.

## Undo/Redo System
Редактор фішок використовує той самий патерн **Command**, що і Редактор Рівнів, для відстеження змін.

### Hotkeys
- **Ctrl + Z**: Скасувати останню зміну.
- **Ctrl + Y** або **Ctrl + Shift + Z**: Повернути скасовану зміну.

### Supported Actions
- **Редагування властивостей**: Усі зміни, зроблені в `SerializedObject` під час редагування в інспекторі.
- **Редагування Module Data**: Додавання/видалення записів у `moduleDatas`, редагування полів конкретного типу (`ChipMergeData`, `ChipGeneratorData`, `ChipContainerData`, `ChipPowerBoosterData`, тощо).
- **Перейменування**: Перейменування асета фішки (також перейменовує пов'язаний префаб).
- **Стабільний Undo/Redo після rename/type-change**: Після змін імені або `Type` список фішок і фільтри синхронізуються коректно.

## Technical Implementation

### Key Components
- **`MergeToolkitWindow`**: Головне вікно редактора (`Window → Expecto → Merge Toolkit`) з тулбаром (`New Chip`, `Refresh`, `Project Settings`, `New Project`, `Undo/Redo`, `Asset Store`, `Discord`), лівим списком фішок та перемиканням режимів правої панелі (`Default`, `CreateChip`, `ProjectSettings`). Кнопка **New Project** (`OnNewProject()`) зберігає поточні правки фішки та відкриває `NewMergeProjectWindow`.
- **`EditorContext`**: Централізований провайдер контексту редактора, що надає доступ до активного `FieldData`, налаштувань `MergeProjectSettings` (`ActiveMergeProjectSettings`) та поточної `ChipDataCollection`. Зберігає шляхи до асетів в `EditorPrefs` для забезпечення стабільності при роботі в Unity з вимкненим перезавантаженням домену (Enter Play Mode Options) і мігрує збережений шлях зі старого ключа `ChipEditorWindow.SettingsPath`.
- **`ChipEvolutionProviderDrawer`**: Кастомний `PropertyDrawer` для поліморфних полів `IChipEvolutionProvider` (`[SerializeReference]`). Використовує `TypeCache` для швидкого вибору конкретної реалізації стратегії еволюції (`ConstantEvolutionProvider`, `RandomEvolutionProvider`) та рендерингу її внутрішніх полів в інспекторі.
- **`ChipSelectorDrawer`**: Кастомний `PropertyDrawer` для атрибута `[ChipSelector]`. Відображає випадаючий список із пошуком для швидкого вибору фішки з активної колекції `EditorContext.ActiveChipDataCollection`.
- **`IChipEditorCommand`**: Інтерфейс для команд, що керують станом `ChipData`.
- **`ChipEditorCommandHistory`**: Керує стеками undo/redo спеціально для контексту редагування фішок.
- **`MergeToolkitWindow.ModuleDataSection.cs`**: Окремий partial для UI секції `Module Data`.
    - Ліниво знаходить всі реалізації `IChipModuleData` в завантажених assembly.
    - Реалізує валідацію `DrawMissingModuleDataWarning(SerializedProperty listProp)`, яка перевіряє `RequireModuleDataAttribute` на компонентах `IChipModule` префаба та виводить попередження при відсутності необхідних конфігурацій.
    - Показує кожен елемент `moduleDatas` як foldout з редагуванням полів через `SerializeReference`.
    - Дозволяє додати лише ті типи, яких ще немає в поточному `ChipData` (без дублювань типів).
    - Дозволяє видалення елемента зі списку.
- **`MergeToolkitWindow.RightPanel.cs`**:
    - Відображає деталі чіпа, прев'ю, заголовок, властивості та секції даних.
    - Здійснює валідацію `DrawMissingBaseSpriteWarning()`: перевіряє `IChipSortingLayer.SortingLayers[0].Renderer.sprite`. Якщо рендерер призначений, але спрайт відсутній — виводить `HelpBox` (Error) з кнопкою `Open`.
    - Метод `OpenPrefabAndSelectRenderer()` завантажує префаб через `PrefabStageUtility.OpenPrefab()`, знаходить відповідний `GameObject` зі `SpriteRenderer` за шляхом трансформації або через сортувальний шар у сцені префабу, виділяє його (`Selection.activeGameObject`) та фокусує через `EditorGUIUtility.PingObject()`.
    - У вкладці **Project Settings** (`DrawProjectSettingsPanel`) містить вибір `MergeProjectSettings` та секцію **Default Module Data (Create Only)** для налаштування `defaultModuleDatas` у `MergeProjectSettings`.
    - Дозволяє додавати тільки унікальні типи `IChipModuleData` (один тип — один запис), редагувати їх поля та видаляти записи.
- **`MergeProjectSettings`**:
    - Зберігає `defaultModuleDatas` замість окремого поля для default-префаба lock-ефекту.
    - Рекомендований сценарій для lock-ефекту: додати `ChipMoveLockedData` у `defaultModuleDatas` і задати `Prefab`.
- **`MergeToolkitWindow.UndoRedo.cs`**: Містить визначення команд:
    - `SetChipPropertyCommand` — записує JSON-знімки стану асета `ChipData` "до" та "після".
    - `RenameChipCommand` — обробляє перейменування асета як для `ChipData`, так і для префаба.
    - При Undo/Redo змін, що впливають на список (name/type), виконується синхронізація `SerializedObject`, сортування списку та оновлення фільтрів.
- **`MergeToolkitWindow.ContextMenu.cs`**:
    - Реалізує контекстне меню (правий клік по чіпу в лівій панелі).
    - Містить пункти `Duplicate`, розділювач (сепаратор) та `Delete`.
    - `Duplicate` переводить редактор у режим створення чіпа (`State.CreateChip`), призначає обраний `ChipData` як джерело (`duplicateSourceChip`) та залишає поле назви порожнім для введення користувачем.
    - `Delete` запитує підтвердження та безпечно видаляє асет чіпа і пов'язаний префаб (якщо префаб не належить іншим чіпам).
- **`MergeToolkitWindow.CreateChip.cs`**:
    - Кешує список template-префабів для створення нових чіпів.
    - Коректно обробляє порожню папку шаблонів (показує warning замість некоректного створення).
    - Під час `Create Chip` клонує всі елементи з `DefaultModuleDatas` у новий `ChipData` (`ApplyDefaultModuleDataToNewChip` + `CloneModuleData`).
    - У секції `Create Next Level Chip` відображає лише `Chip Name:` (тільки для читання) та кнопку `Create`. При натисканні на `Create` відкриває вкладку `New Chip` (`State.CreateChip`) і попередньо заповнює назву наступного рівня як для дублювання (`duplicateChipName`), так і для створення за шаблоном (`newChipName`), залишаючи `duplicateSourceChip` порожнім (`null`).

### Workflow (Flow)
Коли властивість змінюється в методі `DrawRightPanel`, вікно створює `SetChipPropertyCommand` із захопленням станів асета. Це включає як звичайні поля `ChipData`, так і секцію `Module Data` (зокрема `ChipMergeData`). Для перейменування використовується `RenameChipCommand`. Ці команди передаються в `RecordAndExecute(command)`, що оновлює історію та забезпечує оновлення `SerializedObject`. Під час створення нового чіпа (`Create Chip`) застосовуються шаблони з `defaultModuleDatas`, тому default-налаштування `MoveLocked` також переносяться через `ChipMoveLockedData`, а не через окреме поле префаба.

---

# New Merge Project

Цей інструмент призначений для автоматичного створення нової структури папок, C# скриптів, префабів поля, інтеграційних тестів та початкової сцени для нової гри з нуля на основі конфігураційних шаблонів. Відкривається кнопкою **New Project** на панелі інструментів вікна **Merge Toolkit** (`MergeToolkitWindow`).

## Main Features
- **Template Configuration**: Можливість створення та вибору довільних конфігурацій шаблонів за допомогою ScriptableObject `MergeTemplateSettings`. Шаблон визначає структуру підкаталогів, шаблони `.Tests.asmdef` та інтеграційних тестів, вихідні `MonoScript` для `LifetimeScope` і `Initializer`, папку `TemplateChips` (`ChipTemplatesFolder`) та базовий префаб поля (`FieldPrefab`). `VisualFieldPrefab` і `DefaultFieldSize` автоматично зчитуються з `IMergeLifetimeScope.FieldData` на призначеному `FieldPrefab`.
- **Dynamic Script Generation**: Генерація скриптів `{Prefix}MergeLifetimeScope` та `{Prefix}MergeInitializer` із заміною namespace, імен класів і плейсхолдерів на вказані користувачем **Namespace** та **Prefix**.
- **Automated Copy & Setup**: Копіювання папки шаблонних фішок у `Prefabs/TemplateChips` (`CopyTemplateChips` із записом у `MergeProjectSettings.ChipsTemplateFolder`), копіювання базових префабів поля та візуального відображення поля з додаванням префіксу користувача, підготовка тестових асетів (`TestFieldData`, `Test{FolderName}_Field.prefab`, `TestSettings`) та відкладене створення і відкриття початкової сцени після компіляції.
- **Smart Data Migration (DidReloadScripts)**: Автоматична міграція компонентів на префабах поля та тестів після компіляції згенерованого C# коду. Старий базовий компонент scope замінюється на новий згенерований тип зі збереженням раніше налаштованих полів (через тимчасову JSON-серіалізацію).

## Technical Implementation

### Key Components
- **`NewMergeProjectWindow`**: Клас вікна редактора (`EditorWindow`), розділений на чотири `partial`-файли:
  - `NewMergeProjectWindow.cs` — UI вікна (відкривається через кнопку **New Project** у `MergeToolkitWindow`, вибір шаблону, введення Namespace/Prefix, валідація наявності `IMergeLifetimeScope` і `FieldData` на `FieldPrefab` через `ValidateProjectConfiguration`, кнопка старту) та створення дефолтного `DefaultMergeTemplateSettings` (`CreateDefaultTemplateSettingsAsset`).
  - `NewMergeProjectWindow.Generation.cs` — валідація цільової папки, скидання сесії `LevelEditorWindow.ResetSessionForProjectCreation()` і перехід на порожню сцену (`CreateNewProject`), оркестрація створення контенту (`CreateProjectContents`), копіювання `TemplateChips` (`CopyTemplateChips`) і префабів, генерація `MergeProjectSettings`, `.Tests.asmdef` і тестових асетів (`CreateTestAssets`).
  - `NewMergeProjectWindow.ScriptGeneration.cs` — генерація `{Prefix}MergeLifetimeScope.cs` і `{Prefix}MergeInitializer.cs` (`GenerateProjectScripts`, `ApplyScriptReplacements`, `ResolveScriptTypeInfo`).
  - `NewMergeProjectWindow.PostCompile.cs` — обробка завершення компіляції після Domain Reload (`OnScriptsReloaded`, `FinalizeProjectAfterReload`, `ReplaceLifetimeScopeInPrefab`, `CreateInitialScene`).
- **`MergeTemplateSettings`**: Серіалізовані налаштування шаблону, які містять:
  - Список підкаталогів для створення.
  - Текстові шаблони для `.asmdef` і тесту, а також посилання на вихідні `MonoScript` для `LifetimeScope` і `Initializer`.
  - Посилання на вихідний префаб поля (`FieldPrefab`) та папку `ChipTemplatesFolder` (`TemplateChips`), а також обчислювані властивості `VisualFieldPrefab` і `DefaultFieldSize`, що зчитуються з `FieldPrefab`.
- **`PendingProjectSetup`**: Допоміжний серіалізований клас для збереження параметрів незавершеного налаштування проєкту в `EditorPrefs` на час компіляції коду.

### Workflow (Flow)
1. Користувач вказує налаштування у вікні `NewMergeProjectWindow` та обирає цільову папку всередині `Assets/` (не кореневу папку `Assets`).
2. Вікно перевіряє збереження змінених сцен, скидає активну сесію редактора рівнів (`LevelEditorWindow.ResetSessionForProjectCreation()`), відкриває порожню сцену, створює структуру каталогів, копіює `TemplateChips`, префаби поля й візуального поля, генерує `MergeProjectSettings`, тестові асети й текстові файли скриптів на диску.
3. Щоб обійти процесор шаблонів VContainer (`ScriptTemplateProcessor`), який може затирати файли при генерації, вікно вручну створює `.meta` файли із заздалегідь згенерованим GUID.
4. Шлях до папки проєкту, префабів, скрипту, розмір поля за замовчуванням та ім'я нового згенерованого типу `MergeLifetimeScope` записуються в `EditorPrefs` за ключем `NewMergeProjectWindow.PendingProjectSetup` (`PendingProjectSetupPrefKey`).
5. Після завершення генерації файлів Unity запускає компіляцію скриптів (Domain Reload).
6. Метод `OnScriptsReloaded`, помічений атрибутом `[DidReloadScripts]`, зчитує дані з `EditorPrefs` і через `EditorApplication.delayCall` планує `FinalizeProjectAfterReload`: динамічно знаходить новий тип (`FindCompiledType`), замінює `IMergeLifetimeScope` на префабах поля й тестів (`ReplaceLifetimeScopeInPrefab`) зі збереженням даних через JSON, створює `FieldData`, оновлює `EditorContext.ActiveFieldData`, створює й відкриває початкову сцену (`CreateInitialScene`) та закриває вікно `NewMergeProjectWindow`.
