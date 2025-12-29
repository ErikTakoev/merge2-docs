# Technical Design (Технічний Дизайн)

[← На Головну](../README.md)


Архітектура побудована на принципах модульності, Dependency Injection та чіткого розділення відповідальності.

## Dependency Injection (VContainer)
Ми використовуємо **VContainer** для керування залежностями.
- **LifetimeScope**: `Merge2LifetimeScope` — точка конфігурації для сцени Merge2.
- **Initialization**: `Merge2Initializer` виступає як Entry Point. Він отримує через конструктор ключові інтерфейси (`IFieldGrid`, `IFieldEventHandler`, `ChipFactory`, `IInputManager`) і запускає гру.
- **Component Injection**: Всі ігрові сервіси та логічні класи отримують залежності через `[Inject]` або конструктор.

## Core Interfaces & Implementations
Основні абстракції системи та їх реалізації:

### Grid & Data
- **`IFieldGrid`** -> `FieldGrid`
  - **Призначення**: Управління станом сітки (2D масив `Cell`).
  - **Відповідальність**: Створення комірок, валідація координат, низькорівневі операції розміщення чіпів (`SetChipInCell`).

- **`IFieldInitializeCommand`** -> `FieldInitializeCommand`
  - **Призначення**: Команда ініціалізації рівня.
  - **Відповідальність**: Завантаження конфігурації поля (`FieldData`), створення візуальної сітки та спавн початкових чіпів.

### Logic & Interaction
- **`IInputManager`** -> `InputManager`
  - **Призначення**: Абстракція системи вводу для Dependency Injection.
  - **Відповідальність**: Визначення подій вводу (OnTap, OnDragStart, OnDrag, OnDragEnd) та методів симуляції для тестування. Реалізація `InputManager` обробляє Unity Input System та трансформує події вводу в C# події.

- **`IFieldEventHandler`** -> `FieldEventHandler`
  - **Призначення**: Точка входу для подій системи вводу.
  - **Відповідальність**: Обробка Tap/Drag подій, координація взаємодії між IInputManager та логікою поля.

- **`IFreeCellFinder`** -> `FreeCellFinder`
  - **Призначення**: Алгоритмічний пошук місця.
  - **Відповідальність**: Знаходження найближчої вільної комірки (спіральний пошук) для спавну або переміщення чіпів.

- **`IChipMovingLogic`** -> `ChipMovingLogic`
  - **Призначення**: Комплексна логіка переміщення.
  - **Відповідальність**: Валідація переміщень, обробка колізій та розрахунок ланцюгових переміщень (relocation) інших чіпів, щоб звільнити місце.

## Interaction Strategies
Щоб уникнути "God Object" у класі `Chip`, логіка взаємодій винесена в стратегії:
- **Інтерфейс**: `IChipInteractionLogic`.
- **Реалізації**:
  - `MergeInteractionLogic`
  - `FillContainerLogic`
  - `GeneratorInteractionLogic`
- Це дозволяє комбінувати поведінку фішок, просто додаючи відповідні компоненти логіки.

## Input System
Система вводу базується на **Unity Input System (New Package)**.
- **Asset**: `Modules/Merge2/Input/Merge2Input.inputactions` — містить визначення Action Maps (Game, UI).
- **Architecture**:
  - **Інтерфейс**: `IInputManager` — визначає контракт для системи вводу, дозволяє Dependency Injection через VContainer.
  - **Реалізація**: `InputManager` — клас-адаптер, що ініціалізує Generated Class `Merge2Input` та трансформує події вводу (Press, Drag) у C# події (`OnTap`, `OnDragStart`, `OnDrag`, `OnDragEnd`). Надає методи симуляції (`SimulateTap`, `SimulateDrag*`) для тестування в Unity Editor.
  - **Інтеграція**: `FieldEventHandler` підписується на події `IInputManager` і делегує їх обробку відповідним компонентам (фішкам або полю).

## Animation System
Анімації відокремлені від логіки даних.
- **Інтерфейс**: `IChipFlyAnimation`.
- **Призначення**: Керує візуальним переміщенням (Tweening) об'єктів.
- **Перевага**: Логіка `Cell` або `Chip` каже "перемістись туди", а система анімації вирішує "як" це зробити (швидкість, крива, ефекти), не блокуючи логічний стан гри.

## Context Awareness
Для полегшення розуміння коду (особливо для AI) використовується атрибут `[ContextCodeAnalyzer]`.
- **Поля**: `@purpose`, `@usage`, `@params`, `@notes`.
- **Використання**: Документування нетривіальної логіки прямо в коді. Це допомагає генерувати актуальну документацію та дає контекст при LLM-аналізі.
