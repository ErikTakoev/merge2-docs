# Merge Toolkit Documentation

Ласкаво просимо до документації Merge Toolkit. Цей файл є навігаційним хабом по всій документації проекту, яка описує механіки, архітектуру та процеси розробки.

## Contents

### Getting Started
Швидкий старт та керівництво з розширення проекту.

- **[Quick Start](Quick_Start.md)**
  > **Контекст**: Перший запуск модуля, підключення `VContainer`, запуск `GardenMerge` та практичний workflow через `Level Editor` і `Chip Viewer`.

- **[Extension Guide](ExtensionGuide.md)**
  > **Контекст**: Покрокове керівництво з розширення проєкту: додавання нових типів фішок, ефектів, взаємодій та сервісів.

### Extensions
Додаткові модулі та спеціалізовані рішення, побудовані на базі Merge Toolkit.

- **[Merge Toolkit - Isometric](Extensions/Isometric/IsometricMain.md)**
  > **Контекст**: Підтримка ізометричної сітки, сортування за глибиною та специфічні візуальні ефекти для ізометричних ігор.
- **Merge Toolkit - Classic**
  > **Контекст**: Базовий набір механік для класичних мерж-головоломок у форматі 2D або Top-Down.
- **Merge Toolkit - Tower Defense**
  > **Контекст**: Розширення, що поєднує систему злиття фішок з механіками захисту веж (Tower Defense).



### Game Objects (Chips)
Розділ описує основні сутності, що знаходяться на ігровому полі (Grid). Всі об'єкти успадковують або використовують базові принципи `Chip`.

- **[Chip (Базовий)](Chips/Chip.md)**
  > **Контекст**: Це базовий ігровий об'єкт (фішка), що підтримує механіку злиття (Merge). Розділ включає опис **правил злиття (ChipMergeData)** як частини `specialDatas`: налаштування партнерів, ваг (Weighted Random) і runtime-доступ через `Chip.MergeData`.
  
- **[GeneratorModule](Chips/GeneratorModule.md)**
  > **Контекст**: Чіп-генератор, що створює (спавнить) нові фішки. Працює як State Machine (Зарядка <-> Готовність). Підтримує ручний (Tap) та автоматичний режими, систему ваг (Weighted Random) для вибору результату та лімітовану кількість перезарядок.

- **[WaitEvolutionModule](Chips/WaitEvolutionModule.md)**
  > **Контекст**: Модуль еволюції, що автоматично замінює фішку на іншу випадковим чином згідно з заданими ймовірнісними вагами після закінчення таймера очікування. Підтримує підсилення швидкості еволюції за допомогою сусідніх Power Boosters.

- **[ContainerModule](Chips/ContainerModule.md)**
  > **Контекст**: Чіп-контейнер, який "споживає" інші фішки для виконання квестів або завдань. Після заповнення зазвичай видає винагороду.

- **[PowerBoosterModule](Chips/PowerBoosterModule.md)**
  > **Контекст**: Чіп-підсилювач, який посилює сусідні чіпи (наприклад, прискорює зарядку генераторів), підтримує `JoinPoints` у цілях підсилення та керує двома типами візуалізації: підсвіткою зони впливу і динамічними join-лінками через `IEffectPowerBoosterJoin`.

### Key Features
Механіки, що відрізняють цей проект від класичних Merge-ігор.

- **[Locked Areas](Features/LockedAreas.md)**
  > **Контекст**: Система блокування окремих ділянок сітки до їх розблокування. Підтримує два типи заблокованих клітинок: прості (з миттєвим завантаженням фішок) та відкладені (з фішками, що з'являються після розблокування).

- **[Multi-cell Chips](Features/MultiCellChip.md)**
  > **Контекст**: Підтримка фішок розміром більше ніж 1x1. Описує складну систему "Якорів" (Anchor Cell), валідацію позицій та особливості рендерингу об'єктів, що займають кілька клітинок.

- **[Chip Relocation](Features/ChipRelocation.md)**
  > **Контекст**: Система автоматичного розштовхування сусідніх фішок. Коли гравець переміщує великий об'єкт або відбувається злиття, сусідні фішки намагаються знайти вільне місце, щоб звільнити простір, замість того щоб блокувати дію.

- **[Chip Effect Blockers](Features/ChipEffectBlockers.md)**
  > **Контекст**: Механіка блокування дій на фішках (переміщення, злиття тощо) та система поступового руйнування ефектів (chains, boxes).

- **[Cell Observer System](Features/CellObserverSystem.md)**
  > **Контекст**: Докладний опис системи спостереження за змінами клітинок.

- **[Scenario System](Features/Scenario.md)**
  > **Контекст**: Базова архітектура для створення сюжетних сценаріїв, туторіалів та квестів у Merge з використанням Unity Visual Scripting (UVS) та івент-орієнтованого підходу.

### Interactions
Розділ описує як фішки взаємодіють одна з одною при перетягуванні.

- **[Логіка взаємодій (ChipInteractions)](Interactions/ChipInteractions.md)**
  > **Контекст**: Опис того, як логіка перетягування фішок (`DraggableChipLogic`) делегує специфічні дії інтерфейсу `IChipInteractionLogic`.
- **[Злиття фішок (MergeableChipLogic)](Interactions/MergeableChipLogic.md)**
  > **Контекст**: Детальний опис механіки злиття: перевірка сумісності, створення результату та автоматична релокація сусідів.
- **[Наповнення контейнерів (FillContainerLogic)](Interactions/FillContainerLogic.md)**
  > **Контекст**: Як фішки-інгредієнти додаються до контейнерів для виконання квестів.

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
  > - **DI (VContainer)**: Як працює ін'єкція залежностей через `Merge2LifetimeScope`.
  > - **Interfaces**: Опис ключових абстракцій (`IFieldGrid`, `IChipMovingLogic`, `IFieldEventHandler` і т.д).



- **[Testing](Technical/Testing.md)**
  > **Контекст**: Гайд по написанню тестів. Фокус на **Integration Tests**, які використовують реальний префаб поля (`TestFieldBuilder`) для перевірки ігрових сценаріїв (Spawn -> Move -> Merge -> Validation), включно з pipeline підписок [Cell Observer System](Features/CellObserverSystem.md) та сценаріями `ChipPowerBooster` для manual+auto генераторів.

### Tools
Кастомні інструменти для полегшення розробки та налаштування контенту.

- **[Editors](Editors.md)**
  > **Контекст**: Опис кастомних інструментів редактора (Unity Editor) для налаштування рівнів, конфігурації фішок та балансу геймплею.

### Auto-generated API Reference
Автоматично згенерована довідка по API Merge Toolkit. Корисна для швидкого пошуку класів, інтерфейсів і методів.

- **[API Reference](../AutoGen/Expecto_MergeBase.md)**
  > **Контекст**: Повний агрегований огляд усіх виявлених сутностей (класи, інтерфейси, поля, методи) у просторі імен Expecto.MergeBase.
