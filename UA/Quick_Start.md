# Quick Start

[← На Головну](Main.md)

## Overview
Цей гайд допоможе швидко підключити Merge Toolkit:
- підключити залежність `VContainer`;
- запустити приклад `ElementalMerge2`;
- запустити тести
- переглянути базові взаємодії.

## Dependencies
Merge Toolkit потребує двох залежностей:
- **VContainer**: https://github.com/hadashiA/VContainer
- **Input System**: `com.unity.inputsystem`

Додайте їх у `Packages/manifest.json` або через Package Manager.

## Getting Started
Після підключення залежностей переконайтесь що:
- у проєкті присутній `Merge Toolkit` за шляхом `Assets/Expecto/Merge2`;
- проєкт відкривається без критичних compile errors;
- тести проходять успішно: **Window > General > Test Runner** → запустіть тести.

## Run Sample: ElementalMerge2
Щоб запустити демонстраційний приклад:
1. Відкрийте сцену `Assets/Expecto/Merge2/Examples/ElementalMerge2/Scenes/ElementalMerge2.unity`.
2. Натисніть **Play**.
3. Перевірте базовий цикл: переміщення фішок, взаємодії та merge.

---

## Create New Merge Game (Clean Project)

Цей розділ пояснює, як створити нову мерж-гру з нуля за допомогою вбудованого інструменту **Chip Viewer**.

### Крок 1 — Відкрийте Chip Viewer

У меню Unity: **Window → Merge2 → Chip Viewer**

### Крок 2 — Створіть папку гри

1. У тулбарі вікна натисніть **Settings**.
2. У правій панелі з'явиться кнопка **New Game Folder** — натисніть її.
3. У діалозі вибору папки вкажіть або створіть нову папку **всередині** `Assets/` (наприклад, `Assets/MyGame`).

> ⚠️ Папка обов'язково має знаходитись у межах `Assets/`. Якщо обрати зовнішню папку — з'явиться помилка.

Після підтвердження автоматично створюються такі папки:

| Папка | Призначення |
|---|---|
| `Data/` | Загальні дані |
| `Data/ChipData/` | ScriptableObject-и `ChipData` |
| `Scripts/` | Ваші скрипти |
| `Materials/` | Матеріали |
| `Prefabs/` | Префаби |
| `Prefabs/Chips/` | Префаби фішок |
| `Prefabs/ChipsForContainer/` | Префаби фішок для контейнерів |
| `Prefabs/Effects/` | Ефекти |
| `Scenes/` | Сцени |
| `Textures/` | Текстури |
| `Tests/Editor/` | EditMode тести |
| `Tests/Editor/Resources/` | Ресурси для тестів |
| `Tests/FieldData/` | Дані для тестових полів |
| `Tests/Integration/` | Інтеграційні тести |
| `Tests/Prefabs/` | Префаби для тестів |

Разом зі структурою папок генеруються два ключові assets:

- **`ChipCreatorSettings`** — головний конфіг Chip Creator;
- **`ChipDataCollection`** — колекція фішок, одразу прив'язана до `ChipCreatorSettings.ChipDataBase`

Chip Viewer автоматично перемикається на нові settings і одразу готовий до роботи.

### Крок 5 — Що далі?

- **Chips** — відкрийте Chip Viewer і натисніть **Create Chip**, щоб додати першу фішку.
- **Scene** — додайте сцену до `Scenes/` та налаштуйте поле згідно з документацією [Editors.md](Editors.md).
- **Tests** — створіть perші тести у `Tests/Editor/` та запустіть їх через **Window → General → Test Runner**.

