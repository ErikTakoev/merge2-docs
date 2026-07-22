# Quick Start

[← На Головну](Main.md)

## Overview
Цей гайд допоможе швидко підключити Merge Toolkit:
- підключити залежність `VContainer`;
- запустити приклад `GardenMerge`;
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

## Run Sample: GardenMerge
Щоб запустити демонстраційний приклад:
1. Відкрийте сцену `Assets/Expecto/MergeBase/Examples/GardenMerge/Scenes/GardenMerge.unity`.
2. Натисніть **Play**.
3. Перевірте базовий цикл: переміщення фішок, взаємодії та merge.

---

## Create New Merge Game (Clean Project)

Цей розділ пояснює, як створити нову мерж-гру з нуля за допомогою вбудованого інструменту **Clean Merge Project**.

### Крок 1 — Відкрийте Clean Merge Project

У меню Unity: **Window → Expecto → Clean Merge Project**

### Крок 2. Налаштування та створення нової папки гри
1. У вікні **Clean Merge Project** виберіть потрібний шаблон конфігурації у полі **Template Settings** (використовує ScriptableObject асети типу `MergeTemplateSettings`).
2. Вкажіть бажаний **Namespace** та унікальний **Prefix** для ваших згенерованих файлів (вони будуть використані під час генерації скриптів `MergeLifetimeScope` та `MergeInitializer`).
3. Натисніть кнопку **Select Folder For Clean Project** та виберіть або створіть порожню папку для нової гри (обов'язково всередині папки `Assets/` вашого проекту).

**Інструмент автоматично:**
- Створить структуру папок на основі конфігурації у вибраному шаблоні (Data, Scripts, Scenes, Tests тощо).
- Згенерує `ChipCreatorSettings` та `ChipDataCollection` і автоматично перемкне відкрите вікно **Chip Viewer** на використання нових налаштувань.
- Скопіює базові префаби поля (`FieldPrefab`) та візуального відображення поля (`VisualFieldPrefab`) з шаблону, додавши до них ваш **Prefix**.
- Згенерує скрипти `MergeLifetimeScope` та `MergeInitializer` із вказаним namespace та prefix.
- Створить інфраструктуру для інтеграційних тестів: `.Tests.asmdef`, шаблон класу тесту та тестові ассети (`TestSettings`, `FieldData`).
- Збереже початкову сцену `{folderName}Scene.unity` з інстансом вашого поля.
- Після успішної компіляції нових скриптів автоматично виконає DidReloadScripts-міграцію даних у префабах поля та сцени, замінивши старі базові класи на ваші нові згенеровані компоненти із повним збереженням серіалізованих даних.

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
| `Scenes/` | Початкова сцена `{folderName}Scene.unity` |
| `Textures/` | Текстури та спрайти |
| `Tests/Editor/` | EditMode тести |
| `Tests/Editor/Resources/` | Ресурси для тестів |
| `Tests/FieldData/` | Дані для тестових полів (зокрема тестове `FieldData`) |
| `Tests/Integration/` | Інтеграційні тести (зокрема шаблон `{folderName}Tests.cs`) |
| `Tests/Prefabs/` | Префаби поля для тестів з вашим `FieldData` |


