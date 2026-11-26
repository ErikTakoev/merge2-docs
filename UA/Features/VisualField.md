# Visual Field

[← На Головну](../Main.md)

Модуль візуального поля (`FieldModules/VisualField`) відповідає за візуальне оформлення ігрового поля (фон, рамку сітки, дочірні візуальні елементи рівня) та його інтеграцію з [Level Editor](../Editors.md) і системою ініціалізації рівня.

---

## Overview

`VisualField` реалізовано як опційний модуль поля на базі `IFieldModuleData` та `IFieldModuleEditor`:
- **Збірки**: runtime-код знаходиться у збірці `MergeBase.VisualField` (`FieldModules/VisualField/Scripts`), редакторська частина — у `MergeBase.VisualField.Editor` (`FieldModules/VisualField/Editor`).
- **Умовна компіляція**: обидві збірки визначають символ `MERGE_BASE_VISUAL_FIELD` (`versionDefines`), що дозволяє іншим модулям (`MergeBase.Editor`, `MergeClassic`, `MergeCommon.LockedArea`) опційно взаємодіяти з `VisualFieldData`, `IVisualField` та `IVisualFieldElement` без жорсткої залежності.
- **Вирівнювання до сітки**: візуальне поле вирівнюється за позицією та обертанням `IFieldGrid` як у runtime, так і в прев'ю редактора рівнів.

---

## Architecture and Key Components

### Data and Contracts
- **[VisualFieldData.cs](../../../FieldModules/VisualField/Scripts/Data/VisualFieldData.cs)** (`IFieldModuleData`): Конфігурація модуля в `FieldData.ModuleDatas`. Зберігає серіалізоване посилання `VisualFieldRef` (`[InterfacePrefab]`) на префаб із компонентом `IVisualField` та реалізує `Validate(FieldData fieldData, List<string> errors)`.
- **[IVisualField.cs](../../../FieldModules/VisualField/Scripts/Interfaces/IVisualField.cs)**: Контракт кореневого компонента префаба візуального поля (`InitVisualField(FieldData fieldData)` та `#if UNITY_EDITOR OnFieldSizeChanged(Vector2Int newSize)`). Тут також оголошено типізоване посилання `VisualFieldRef : InterfaceRef<IVisualField>`.
- **[IVisualFieldElement.cs](../../../FieldModules/VisualField/Scripts/Interfaces/IVisualFieldElement.cs)**: Контракт дочірніх візуальних елементів рівня, які потребують ін'єкції залежностей та ініціалізації через `Init()` під час створення візуалу поля (наприклад, `ILockedAreaEffect` у [Locked Areas](../../../../MergeCommon/Docs/UA/Features/LockedAreas.md)).

### Runtime Implementation
- **[VisualField.cs](../../../FieldModules/VisualField/Scripts/VisualField.cs)** (`MonoBehaviour, IVisualField`): Базова 2D-реалізація візуального поля.
  - **`InitVisualField(FieldData fieldData)`**: Виконує `ApplyFieldTransform()`, `ApplyEffects()` та `ApplySize(fieldData.FieldSize)`.
  - **`ApplyFieldTransform()`**: Синхронізує позицію та обертання `VisualField` із `fieldGrid.transform`, а за наявності `invertTransform` встановлює його світову позицію в `Vector3.zero`.
  - **`ApplyEffects()`**: Знаходить усіх нащадків `IVisualFieldElement` (включно з неактивними), виконує `resolver.Inject(element)` та викликає `element.Init()`.
  - **`ApplySize(Vector2Int newSize)`**: Масштабує `fieldSpriteRenderer.size` та `fieldBorderSpriteRenderer.size` з урахуванням відступів рамки.
  - **`OnFieldSizeChanged(Vector2Int newSize)`** (`#if UNITY_EDITOR`): Оновлює розміри спрайтів і повторно застосовує вирівнювання до `IFieldGrid` у редакторі.

### Level Editor Integration
- **[VisualFieldLevelEditorModule.cs](../../../FieldModules/VisualField/Editor/VisualFieldLevelEditorModule.cs)** (`IFieldModuleEditor<VisualFieldData>`): Модуль редактора рівнів із вкладкою `"Visual Field"` (`Order = 10`).
  - **`DrawPanelGUI()`**: Відображає поле вибору префаба `Visual Field` через `InterfaceRefEditorGUI.DrawLayoutPrefabField` із підтримкою Undo/Redo та запитом на перебудову прев'ю.
  - **`OnUpdatePreview(...)` / `OnDestroyPreview()`**: Інстанціює префаб `VisualField` в ізольовану сцену прев'ю `LevelEditorPreviewScene` за координатами `ILevelEditorContext.GridPosition` та `ILevelEditorContext.GridRotation`, синхронізує розмір (`OnFieldSizeChanged`) та видимість дочірніх ефектів (`OnBoardEffectsVisibilityChanged`).
  - **`OnFieldSizeChanged(Vector2Int newSize)`**: Передає новий розмір сітки в `IVisualField.OnFieldSizeChanged(newSize)` для прев'ю-екземпляра.
  - **`OnBoardEffectsVisibilityChanged(bool isEnabled)`**: Перемикає активність `GameObject` усіх дочірніх `IVisualFieldElement` відповідно до тугла видимості ефектів дошки у Scene View.

---

## Initialization Flow

1. Під час ініціалізації рівня `FieldInitializeCommand.CreateLevelVisual()` перевіряє наявність модуля `VisualFieldData` через `fieldData.GetModuleData<VisualFieldData>()` (за `#if MERGE_BASE_VISUAL_FIELD`).
2. Якщо `VisualFieldData` і префаб `VisualField` задані, префаб інстанціюється через VContainer (`resolver.Instantiate(...)`) та прикріплюється до батька `FieldInitializeCommand`.
3. Трансформ створеного об'єкта вирівнюється за `fieldGrid.transform.position` та `fieldGrid.transform.rotation` (або встановлюється в локальний нуль, якщо `fieldGrid == null`).
4. Через `resolver.Inject(visualField)` виконується ін'єкція залежностей і викликається `visualField.InitVisualField(fieldData)`, що налаштовує розмір спрайтів та ініціалізує всі дочірні `IVisualFieldElement`.
