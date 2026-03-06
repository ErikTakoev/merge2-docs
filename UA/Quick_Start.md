# Quick Start

[← На Головну](Main.md)

## Overview
Цей гайд допоможе швидко підключити Merge Toolkit:
- підключити залежність `VContainer`;
- запустити приклад `ElementalMerge2`;
- запустити тести
- переглянути базові взаємодії.

## Dependencies
Merge Toolkit має три залежності:
- **VContainer**: https://github.com/hadashiA/VContainer
- **Input System**: `com.unity.inputsystem`
- **Shader Graph**: `com.unity.shadergraph`

> [!NOTE]
> Для інтегрованих тестів є залежність від 6000.0 версії юніті.

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

### Крок 2. Створення нового папки гри
Відкрийте `Window > Merge2 > Chip Viewer`.
1. У вкладці **Settings** натисніть кнопку **New Game Folder**.
2. Виберіть шлях для нової гри (всередині `Assets/`).

**Інструмент автоматично:**
- Створить структуру папок (Data, Scripts, Scenes, Tests тощо).
- Створить `ChipCreatorSettings` та `ChipDataCollection`.
- Згенерує початкову сцену `{folderName}Scene.unity` з налаштованим `Merge2LifetimeScope`.
- Створить інфраструктуру для тестів: `.asmdef`, шаблон тесту та тестові ассети (`TestSettings`, `FieldData`).

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

Chip Viewer автоматично перемикається на нові settings і одразу готовий до роботи.


