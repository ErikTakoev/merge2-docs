# Quick Start

[← На Головну](Main.md)

## Overview
Цей гайд допоможе швидко підключити Merge Toolkit:
- підключити залежність `VContainer`;
- запустити приклад `MergeClassic`;
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
- у проєкті присутній `Merge Toolkit` у папці `Assets/Expecto/MergeBase` (та модулі `MergeCommon`, `MergeClassic`, `MergeIsometric`);
- проєкт відкривається без критичних compile errors;
- тести проходять успішно: **Window > General > Test Runner** → запустіть тести.

## Run Sample: MergeClassic
Щоб запустити демонстраційний приклад:
1. Відкрийте сцену `Assets/Expecto/MergeClassic/Scenes/MergeClassicScene.unity`.
2. Натисніть **Play**.
3. Перевірте базовий цикл: переміщення фішок, взаємодії та merge.

---

## Create New Merge Game (New Merge Project)

Цей розділ пояснює, як створити нову мерж-гру з нуля за допомогою вбудованого інструменту **New Merge Project**.

### Step 1 — Open New Merge Project

У меню Unity відкрийте **Window → Expecto → Merge Toolkit** та натисніть кнопку **New Project** на панелі інструментів.

### Step 2 — Configuration and New Game Folder Creation
1. У вікні **New Merge Project** виберіть потрібний шаблон конфігурації у полі **Template Settings** (використовує ScriptableObject асети типу `MergeTemplateSettings`).
2. Вкажіть бажаний **Namespace** та унікальний **Prefix** для ваших згенерованих файлів (вони будуть використані під час генерації скриптів `{Prefix}MergeLifetimeScope` та `{Prefix}MergeInitializer`).
3. Натисніть кнопку **Create New Merge Project** та виберіть або створіть порожню папку для нової гри (обов'язково всередині папки `Assets/` вашого проекту, окрім самої кореневої папки `Assets`).

**Інструмент автоматично:**
- Створить структуру папок на основі конфігурації у вибраному шаблоні (Data, Scripts, Scenes, Tests тощо).
- Скопіює папку шаблонних фішок у `Prefabs/TemplateChips/` (якщо задано `ChipTemplatesFolder` у шаблоні).
- Згенерує `MergeProjectSettings` (із прив'язкою `ChipsTemplateFolder`) та `ChipDataCollection` і автоматично перемкне відкрите вікно **Merge Toolkit** на використання нових налаштувань.
- Скопіює базові префаби поля (`FieldPrefab`) та візуального відображення поля (`VisualFieldPrefab`, визначений із модуля `VisualFieldData` у `IMergeLifetimeScope.FieldData` на `FieldPrefab`), додавши до них ваш **Prefix**.
- Згенерує скрипти `{Prefix}MergeLifetimeScope` та `{Prefix}MergeInitializer` із вказаним namespace та prefix.
- Створить інфраструктуру для інтеграційних тестів: `.Tests.asmdef`, шаблон класу тесту та тестові асети (`TestSettings`, `FieldData`, тестовий префаб поля).
- Після успішної компіляції нових скриптів автоматично виконає `DidReloadScripts`-міграцію даних у префабах поля й тестів, створить `FieldData` із розмірами поля з `FieldPrefab`, збереже й відкриє початкову сцену `{folderName}Scene.unity` з інстансом вашого поля та закриє вікно генерації.

Після завершення процесу автоматично створюються такі папки та файли:

| Папка / Файл | Призначення |
|---|---|
| `Data/` | Загальні дані гри |
| `Data/ChipData/` | ScriptableObject асети фішок (`ChipData`) |
| `Scripts/` | Ваші C# скрипти (зокрема згенеровані `{Prefix}MergeLifetimeScope.cs` та `{Prefix}MergeInitializer.cs`) |
| `Materials/` | Матеріали та шейдери |
| `Prefabs/` | Префаби гри |
| `Prefabs/Chips/` | Префаби фішок |
| `Prefabs/ChipsForContainer/` | Префаби фішок для контейнерів |
| `Prefabs/Effects/` | Префаби візуальних ефектів |
| `Prefabs/Field/` | Префаби поля та візуального поля з вашим префіксом |
| `Prefabs/TemplateChips/` | Шаблонні префаби для створення нових фішок у Merge Toolkit |
| `Scenes/` | Початкова сцена `{folderName}Scene.unity` |
| `Textures/` | Текстури та спрайти |
| `Tests/Editor/` | EditMode тести |
| `Tests/Editor/Resources/` | Ресурси для тестів |
| `Tests/FieldData/` | Дані для тестових полів (зокрема тестове `FieldData`) |
| `Tests/Integration/` | Інтеграційні тести (зокрема шаблон `{folderName}Tests.cs`) |
| `Tests/Prefabs/` | Префаби поля для тестів з вашим `FieldData` |


