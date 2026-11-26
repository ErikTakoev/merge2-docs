# Merge Toolkit Documentation

Ласкаво просимо до документації Merge Toolkit. Цей файл є центральним навігаційним хабом по всьому фреймворку та його базовому модулю **Merge Toolkit Base**.

## Modules

Архітектура Merge Toolkit розділена на окремі модулі:

### Merge Toolkit Common
Спільні, переносні та розширювані модулі та механіки (еволюції, заблоковані зони, туторіали, візуальний скриптинг):
- **[Merge Toolkit Common](../../../MergeCommon/Docs/UA/Main.md)**

### Merge Toolkit Classic
Готова реалізація класичної 2D мерж-головоломки (Top-Down):
- **[Merge Toolkit Classic](../../../MergeClassic/Docs/UA/Main.md)**

### Merge Toolkit Isometric
Підтримка ізометричної проекції, механіка Merge-3 (BFS), ізометрична камера та візуальні ефекти глибини:
- **[Merge Toolkit Isometric](../../../MergeIsometric/Docs/UA/Main.md)**

---

## Contents

### Getting Started
Швидкий старт та керівництво з розширення проекту.

- **[Quick Start](Quick_Start.md)**
  > **Контекст**: Перший запуск модуля, підключення `VContainer`, запуск `MergeClassic` та практичний workflow через `Level Editor` і `Merge Toolkit`.

- **[Extension Guide](ExtensionGuide.md)**
  > **Контекст**: Покрокове керівництво з розширення проєкту: додавання нових типів фішок, ефектів, взаємодій та сервісів.

### Game Objects (Chips)
Розділ описує основні сутності ядра, що знаходяться на ігровому полі (Grid). Всі об'єкти успадковують або використовують базові принципи `Chip`.

- **[Chip (Базовий)](Chips/Chip.md)**
  > **Контекст**: Базовий ігровий об'єкт (фішка) та хост модулів `IChipModule`, що керує життєвим циклом, візуальними ефектами, сортуванням шарів і делегуванням подій вводу та взаємодій.

- **[Merge2Module](Chips/Merge2Module.md)**
  > **Контекст**: Модуль злиття двох фішок (збірка `MergeBase.Merge2`, дефайн `MERGE_BASE_MERGE2`). Описує **правила злиття (`ChipMerge2Data`)** як частину `moduleDatas`: партнерів `MergeCombination`, стратегії результатів `IChipEvolutionProvider` (`ConstantEvolutionProvider`, `RandomEvolutionProvider`) та делегування виконання в `IChipEvolutionService`.

- **[GeneratorModule](Chips/GeneratorModule.md)**
  > **Контекст**: Чіп-генератор, що створює (спавнить) нові фішки (збірка `MergeBase.Generator`, дефайн `MERGE_BASE_GENERATOR`). Працює як State Machine (Зарядка <-> Готовність). Підтримує ручний (Tap) та автоматичний режими, систему ваг (Weighted Random) для вибору результату та лімітовану кількість перезарядок.

- **[ContainerModule](Chips/ContainerModule.md)**
  > **Контекст**: Чіп-контейнер, який "споживає" інші фішки для виконання квестів або завдань. Після заповнення зазвичай видає винагороду.

- **[PowerBoosterModule](Chips/PowerBoosterModule.md)**
  > **Контекст**: Чіп-підсилювач, який посилює сусідні чіпи (наприклад, прискорює зарядку генераторів), підтримує `JoinPoints` у цілях підсилення та керує двома типами візуалізації: підсвіткою зони впливу і динамічними join-лінками через `IEffectPowerBoosterJoin`.

> [!NOTE]
> Додаткові модулі фішок (`WaitEvolutionModule`, `TapEvolutionModule`, `UnlockAreaModule`) входять до складу [Merge Toolkit Common](../../../MergeCommon/Docs/UA/Main.md#game-objects-chips).

### Key Features
Ключові архітектурні механіки ядра Merge Toolkit Base.

- **[Multi-cell Chips](Features/MultiCellChip.md)**
  > **Контекст**: Підтримка фішок розміром більше ніж 1x1. Описує складну систему "Якорів" (Anchor Cell), валідацію позицій та особливості рендерингу об'єктів, що займають кілька клітинок.

- **[Chip Relocation](Features/ChipRelocation.md)**
  > **Контекст**: Система автоматичного розштовхування сусідніх фішок. Коли гравець переміщує великий об'єкт або відбувається злиття, сусідні фішки намагаються знайти вільне місце, щоб звільнити простір, замість того щоб блокувати дію.

- **[Chip Effect Blockers](Features/ChipEffectBlockers.md)**
  > **Контекст**: Механіка блокування дій на фішках (переміщення, злиття тощо) та система поступового руйнування ефектів (chains, boxes).

- **[Cell Observer System](Features/CellObserverSystem.md)**
  > **Контекст**: Докладний опис системи спостереження за змінами клітинок.

> [!NOTE]
> Механіки `Locked Areas`, `Scenario System` та `Tutorial System` входять до складу [Merge Toolkit Common](../../../MergeCommon/Docs/UA/Main.md#features).

### Interactions
Розділ описує як фішки взаємодіють одна з одною при перетягуванні.

- **[Логіка взаємодій (ChipInteractions)](Interactions/ChipInteractions.md)**
  > **Контекст**: Опис того, як логіка перетягування фішок (`DraggableChipLogic`) делегує перевірку та виконання взаємодій модулям `IChipModule` (`Chip.CanInteract`, `Chip.OnInteract`, `DragInteractionResult`) і керує зворотним зв'язком `IDragFeedback`.

- **[Злиття фішок (Merge2Module)](Chips/Merge2Module.md)**
  > **Контекст**: Детальний опис механіки злиття: перевірка сумісності, вибір результатів через `IChipEvolutionProvider`, атомарний спавн додаткових фішок та автоматична релокація сусідів через `IChipEvolutionService`.

- **[Наповнення контейнерів (ContainerModule)](Chips/ContainerModule.md#drag-and-drop-interaction-caninteract--oninteract)**
  > **Контекст**: Як фішки-інгредієнти додаються до контейнерів через `ContainerModule.CanInteract` / `OnInteract` для виконання квестів.

### Visuals
Системи, що відповідають за "Game Feel" та візуальний зворотний зв'язок.

- **[Visual Effects](Visuals/Effects.md)**
  > **Контекст**: Система візуального зворотного зв'язку (Feedback). Перелік ефектів:
  > - **Chip**: Cell Highlight, Merge Available, Move Locked.
  > - **ChipGenerator**: Generator Recharge, Generator Charged.
  > - **ChipContainer**: Container Requirements.
  > - **ChipPowerBooster**: Connector Highlight, Particle Join Links (`PowerBoosterJoinEffect`).
  > Способи їх виклику через компоненти `Effect`.

### Technical Section
Інформація для розробників про архітектуру, патерни та інструменти.

- **[Technical Design](Technical/TechnicalDesign.md)**
  > **Контекст**: Опис Core-архітектури.
  > - **DI (VContainer)**: Як працює ін'єкція залежностей через `IMergeLifetimeScope` (`MergeClassicLifetimeScope`, `IsoMergeLifetimeScope`).
  > - **Interfaces**: Опис ключових абстракцій (`IFieldGrid`, `IChipMovingLogic`, `IFieldEventHandler` і т.д).

- **[Testing](Technical/Testing.md)**
  > **Контекст**: Гайд по написанню тестів. Фокус на **Integration Tests**, які використовують реальний префаб поля (`TestFieldBuilder`) для перевірки ігрових сценаріїв (Spawn -> Move -> Merge -> Validation), включно з pipeline підписок [Cell Observer System](Features/CellObserverSystem.md) та сценаріями `ChipPowerBooster` для manual+auto генераторів.

### Tools
Кастомні інструменти для полегшення розробки та налаштування контенту.

- **[Editors](Editors.md)**
  > **Контекст**: Опис кастомних інструментів редактора (Unity Editor) для налаштування рівнів, модульної системи редактора рівнів (`ILevelEditorContext`, `IFieldModuleEditor`), конфігурації фішок та балансу геймплею.

### Auto-generated API Reference
Автоматично згенерована довідка по API Merge Toolkit. Корисна для швидкого пошуку класів, інтерфейсів і методів.

- **[API Reference](../AutoGen/Expecto_MergeBase.md)**
  > **Контекст**: Повний агрегований огляд усіх виявлених сутностей (класи, інтерфейси, поля, методи) у просторі імен Expecto.MergeBase.
