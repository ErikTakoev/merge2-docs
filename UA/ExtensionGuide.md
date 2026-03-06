# Extension Guide

[← На Головну](Main.md)

Цей документ описує, як розширювати проєкт Merge Toolkit: від створення нової гри до додавання нових типів фішок, ефектів та взаємодій.

---

## 1. Створення нової гри

Перед розширенням рекомендується створити чисту структуру проєкту. Детальна інструкція знаходиться у [Quick Start → Create New Merge Game (Clean Project)](Quick_Start.md#create-new-merge-game-clean-project).

**Коротко:**
1. Відкрийте **Window → Merge2 → Chip Viewer**.
2. У вкладці **Settings** натисніть **New Game Folder**.
3. Інструмент автоматично створить повну структуру папок, сцену, `Merge2LifetimeScope`, `ChipCreatorSettings`, тести та всі необхідні ассети.

Після цього ваш проєкт готовий до розширення.

---

## 2. Розширення через VContainer (Dependency Injection)

Проєкт використовує **VContainer** для керування залежностями. Точка конфігурації — клас `Merge2LifetimeScope`, який наслідує `LifetimeScope`.

### Як це працює

У методі `Configure(IContainerBuilder builder)` реєструються всі сервіси та дані:

```csharp
protected override void Configure(IContainerBuilder builder)
{
    // Static data (instances)
    builder.RegisterInstance(fieldData);
    builder.RegisterInstance(fieldData.ChipDataCollection);

    // MonoBehaviour components (scene hierarchy)
    builder.RegisterComponentInHierarchy<FieldEventHandler>().As<IFieldEventHandler>();
    builder.RegisterComponentInHierarchy<FieldGrid>().As<IFieldGrid>();
    builder.RegisterComponentInHierarchy<FieldInitializeCommand>().As<IFieldInitializeCommand>();
    builder.RegisterComponentInHierarchy<InputManager>().AsSelf();

    // Pure C# services (singletons)
    builder.Register<ChipFactory>(Lifetime.Singleton);
    builder.Register<ChipMovingLogic>(Lifetime.Singleton).As<IChipMovingLogic>();
    builder.Register<FreeCellFinder>(Lifetime.Singleton).As<IFreeCellFinder>();
    builder.Register<ChipFlyAnimation>(Lifetime.Transient).As<IChipFlyAnimation>();

    // Entry Point
    builder.RegisterEntryPoint<Merge2Initializer>();
}
```

### Ключові інтерфейси

| Інтерфейс | Реалізація | Призначення |
|---|---|---|
| `IFieldGrid` | `FieldGrid` | Управління сіткою (2D масив `Cell`): створення комірок, валідація координат, розміщення чіпів |
| `IFieldEventHandler` | `FieldEventHandler` | Точка входу для подій вводу (Tap, Drag), координація взаємодії між Input та полем |
| `IFieldInitializeCommand` | `FieldInitializeCommand` | Ініціалізація рівня: створення візуальної сітки та завантаження початкових чіпів |
| `IChipMovingLogic` | `ChipMovingLogic` | Валідація та виконання переміщень, розрахунок ланцюгових релокацій сусідніх чіпів |
| `IFreeCellFinder` | `FreeCellFinder` | Пошук найближчої вільної комірки (спіральний алгоритм) |
| `IChipFlyAnimation` | `ChipFlyAnimation` | Анімація "польоту" чіпа до цільової позиції |
| `IChipInteractionLogic` | `MergeableChipLogic`, `FillContainerLogic` | Стратегія взаємодії між фішками (merge, fill container) |

### Рекомендований підхід: створення власного LifetimeScope

> [!IMPORTANT]
> Рекомендується **не змінювати** `Merge2LifetimeScope` напряму. Замість цього створіть **власний клас**, який наслідує `LifetimeScope`, та скопіюйте конфігурацію з `Merge2LifetimeScope` як базу. Після цього замінюйте або додавайте реалізації за потребою.

Це дозволяє:
- Зберегти оригінальний `Merge2LifetimeScope` без змін.
- Мати повний контроль над реєстрацією залежностей.
- Замінювати окремі реалізації (наприклад, `IChipMovingLogic`, `IFreeCellFinder`) на власні.
- Так як `Merge2LifetimeScope` не використовується — він не потрапить у білд.

```csharp
public class MyGameLifetimeScope : LifetimeScope
{
    [SerializeField] private FieldData fieldData;

    protected override void Configure(IContainerBuilder builder)
    {
        // --- Скопійовано з Merge2LifetimeScope ---
        builder.RegisterInstance(fieldData);
        builder.RegisterInstance(fieldData.ChipDataCollection);

        builder.RegisterComponentInHierarchy<FieldEventHandler>().As<IFieldEventHandler>();
        builder.RegisterComponentInHierarchy<FieldGrid>().As<IFieldGrid>();
        builder.RegisterComponentInHierarchy<FieldInitializeCommand>().As<IFieldInitializeCommand>();
        builder.RegisterComponentInHierarchy<InputManager>().AsSelf();

        builder.Register<ChipFactory>(Lifetime.Singleton);
        builder.Register<FreeCellFinder>(Lifetime.Singleton).As<IFreeCellFinder>();
        builder.Register<ChipFlyAnimation>(Lifetime.Transient).As<IChipFlyAnimation>();

        builder.RegisterEntryPoint<Merge2Initializer>();

        // --- Замінена реалізація ---
        builder.Register<MyChipMovingLogic>(Lifetime.Singleton).As<IChipMovingLogic>();

        // --- Додані нові сервіси ---
        builder.Register<MyNewService>(Lifetime.Singleton).As<IMyNewService>();
    }
}
```

Після цього замініть компонент `Merge2LifetimeScope` на `MyGameLifetimeScope` на сцені.

### Рекомендований підхід: створення власного Initializer

> [!IMPORTANT]
> Аналогічно до `Merge2LifetimeScope`, рекомендується створити **власний клас ініціалізатора** замість зміни `Merge2Initializer`. Скопіюйте його як базу та розширюйте за потребою.

`Merge2Initializer` — це **Entry Point** (точка входу), зареєстрований через `builder.RegisterEntryPoint<Merge2Initializer>()`. Він реалізує `IInitializable` від VContainer і автоматично викликається при старті гри. Його задачі:
- Ініціалізація `ChipFactory`.
- Підключення подій вводу (`OnTap`, `OnDrag`, тощо) до `IFieldEventHandler`.
- Створення ігрового поля та завантаження початкових фішок.

```csharp
public class MyGameInitializer : IInitializable
{
    [Inject] private readonly InputManager inputManager;
    [Inject] private readonly ChipFactory chipFactory;
    [Inject] private readonly IObjectResolver resolver;
    [Inject] private readonly IFieldEventHandler field;
    [Inject] private readonly IFieldGrid fieldGrid;
    [Inject] private readonly IFieldInitializeCommand fieldInitializeCommand;

    // --- Додані залежності ---
    [Inject] private readonly IMyNewService myNewService;

    public void Initialize()
    {
        // --- Скопійовано з Merge2Initializer ---
        chipFactory.Init(resolver, fieldGrid);

        inputManager.OnTap += field.OnTap;
        inputManager.OnDragStart += field.OnDragStart;
        inputManager.OnDrag += field.OnDrag;
        inputManager.OnDragEnd += field.OnDragEnd;

        fieldInitializeCommand.CreateField();
        fieldInitializeCommand.LoadChips();

        // --- Додана власна ініціалізація ---
        myNewService.Init();
    }
}
```

Не забудьте замінити реєстрацію у вашому `LifetimeScope`:

```csharp
// Замість: builder.RegisterEntryPoint<Merge2Initializer>();
builder.RegisterEntryPoint<MyGameInitializer>();
```

### Як додати свій сервіс

1. **Створіть інтерфейс** (за потреби) та **реалізацію**.
2. **Зареєструйте у `Merge2LifetimeScope`**:
   ```csharp
   // Pure C# service
   builder.Register<MyService>(Lifetime.Singleton).As<IMyService>();

   // Або MonoBehaviour на сцені
   builder.RegisterComponentInHierarchy<MyComponent>().As<IMyComponent>();
   ```
3. **Ін'єктуйте** через `[Inject]` або конструктор:
   ```csharp
   public class MyClass
   {
       [Inject] private readonly IMyService myService;
   }
   ```

### Як додати нову взаємодію (IChipInteractionLogic)

Щоб додати новий тип взаємодії між фішками (крім merge та fill container):

1. Створіть клас, що наслідує `MonoBehaviour` та реалізує `IChipInteractionLogic`:
   ```csharp
   public class MyInteractionLogic : MonoBehaviour, IChipInteractionLogic
   {
       public bool CanInteract(Cell sourceCell, Cell targetCell)
       {
           // Перевірка: чи можлива ця взаємодія?
           return false;
       }

       public bool ExecuteInteraction(Cell sourceCell, Cell targetCell)
       {
           // Виконання взаємодії
           return false;
       }
   }
   ```
2. Додайте компонент на той самий **GameObject**, що і `DraggableChipLogic`.
3. **Порядок компонентів** в інспекторі визначає пріоритет: перша стратегія, що повертає `true` у `CanInteract`, буде обрана.

---

## 3. Розширення Chip

Базовий клас `Chip` надає набір віртуальних методів, які можна перекрити для створення нових типів фішок. Існуючі приклади: `ChipGenerator`, `ChipContainer`.

### Крок 1 — Створити похідний клас

```csharp
public class MyChip : Chip
{
    // Власні поля та залежності
    [Inject] private readonly IMyService myService;
}
```

### Крок 2 — Перекрити `Init` для ініціалізації

```csharp
public override void Init(ChipData data)
{
    base.Init(data); // Важливо! Ініціалізує базові ефекти та runtimeData
    
    // Власна логіка ініціалізації
    // Наприклад, читання додаткових даних з data.OtherData
}
```

### Крок 3 — Додати специфічні ефекти через `InitEffects`

```csharp
protected override void InitEffects()
{
    base.InitEffects(); // Ініціалізує стандартні ефекти (Highlight, MergeAvailable, MoveLocked)
    
    // Додати специфічні ефекти
    var myEffect = InstantiateEffect<IEffect>(myEffectPrefab);
    if (myEffect != null)
    {
        effects.Add(myEffect); // Додати до загального списку для автоматичної розсилки подій
    }
}
```

### Крок 4 — Розширити RuntimeData (за потреби)

Якщо новому чіпу потрібен додатковий runtime стан:

```csharp
// 1. Створити клас рантайм-даних
public class MyChipRuntimeData : ChipRuntimeData
{
    public int CustomProperty;
}

// 2. Ініціалізувати у Init
public override void Init(ChipData data)
{
    runtimeData = new MyChipRuntimeData(); // До виклику base!
    base.Init(data);
}
```

### Ключові віртуальні методи Chip

| Метод | Коли використовувати |
|---|---|
| `Init(ChipData)` | Ініціалізація. Завжди викликайте `base.Init(data)`. |
| `InitEffects()` | Додавання специфічних ефектів. Завжди викликайте `base.InitEffects()`. |
| `OnTap(Vector2)` | Реакція на тап (наприклад, ручна генерація у `ChipGenerator`). |
| `OnDragStart/OnDrag/OnDragEnd` | Кастомна поведінка при перетягуванні. |
| `UpdateVisual()` | Синхронізація візуального стану з `RuntimeData`. |
| `SetMoving(bool)` | Зміна sorting order під час руху. |
| `Destroy(Cell)` | Очищення при знищенні. |
| `CanMoving()` | Чи може фішка бути переміщена. |

---

## 4. Розширення ефектів

Системи ефектів побудована на інтерфейсі `IEffect` та базовому класі `Effect`. Детальний опис всіх ефектів: [Visual Effects](Visuals/Effects.md).

> [!TIP]
> Немає жорсткої прив'язки до конкретних реалізацій ефектів. Чіп прив'язується до **інтерфейсу** ефекту, а не до його реалізації. Усі вбудовані ефекти (Highlight, MergeAvailable, MoveLocked тощо) підключаються через **префаби**, тому їх можна легко **замінити** власною реалізацією або **розширити**, створивши похідний клас. Достатньо створити новий префаб з вашим компонентом, що реалізує `IEffect`, `IEffectContainer`, `IEffectGeneratorCharging` або власний інтерфейс, що наслідує `IEffect`.

### Варіант A — Наслідування від `Effect` (рекомендований)

Базовий клас `Effect` надає вбудовану підтримку Animator, тригерів та `effectForCell`.

```csharp
public class MyCustomEffect : Effect
{
    // Власні serialized поля
    [SerializeField] private Color glowColor;
    
    public override void Activate(Chip chip)
    {
        base.Activate(chip); // Відправляє тригер "Activate" якщо sendAnimatorTrigger = true
        // Додаткова логіка
    }
    
    public override void Deactivate(Chip chip, bool force = false)
    {
        base.Deactivate(chip, force);
        // Додаткова логіка
    }
    
    // Реакція на зміну комірки під час drag
    public override void OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell underCell)
    {
        // Оновити візуал
    }
}
```

### Варіант B — Реалізація `IEffect` напряму

Для ефектів, які не потребують Animator або стандартної логіки `Effect`:

```csharp
public class MyPureEffect : MonoBehaviour, IEffect
{
    public void Init(Chip chip) { }
    public void Activate(Chip chip) { }
    public void Deactivate(Chip chip, bool force = false) { }
    public void SendTrigger(string triggerName, bool allowRepeat = false) { }
    public void OnChangedCell(Cell sourceCell, Cell targetCell) { }
    public void OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell underCell) { }
    public void OnInteractionUnderCellChanged(Cell underCell, Cell overCell) { }
}
```

### Спеціалізовані інтерфейси ефектів

Для ефектів з розширеним контрактом існують спеціалізовані інтерфейси:

| Інтерфейс | Метод | Призначення |
|---|---|---|
| `IEffectContainer` | `UpdateElements(...)` | Візуалізація вмісту контейнерів |
| `IEffectGeneratorCharging` | `OnCharging(float progress)` | Відображення прогресу зарядки |

Щоб створити новий спеціалізований ефект:
1. Створіть інтерфейс, що наслідує `IEffect`.
2. Створіть `InterfaceRef<T>` обгортку для серіалізації в інспекторі:
   ```csharp
   [Serializable]
   public class MyEffectRef : InterfaceRef<IMyEffect> { }
   ```
3. Використовуйте `MyEffectRef` як серіалізоване поле у чіпі.

### Підключення ефекту до чіпа

1. **Через ChipData**: Задайте префаб ефекту в полі `ChipData` (наприклад, `CellHighlightPrefab`, `MergeAvailableEffectPrefab`, `MoveLockedEffectPrefab`).
2. **Через InitEffects**: У похідному класі `Chip` створіть ефект через `InstantiateEffect<T>(prefab)` та додайте до `effects`.

Ефекти, додані до `effects`, автоматично отримують сповіщення про:
- Зміну комірки (`OnChangedCell`)
- Зміну позиції під час drag (`OnInteractionOverCellChanged`, `OnInteractionUnderCellChanged`)
- Знищення чіпа (ефекти також знищуються)

---

## 5. Рекомендації по створенню нової механіки

Коли ви хочете додати нову логіку (наприклад, чіп, що прискорює сусідні генератори), рекомендується дотримуватися наступного алгоритму:

### Крок 1 — Налаштування ChipData

Починайте з визначення даних. Бажано не захаращувати базовий `ChipData`.
1. Створіть окремий **ScriptableObject** для параметрів вашої механіки (наприклад, `ExtensionChipData`).
2. Прикріпіть цей SO до поля **OtherData** у вашому `ChipData`.
3. В коді чіпа ви зможете отримати ці дані: `var settings = data.OtherData as ExtensionChipData;`.

### Крок 2 — Створення тестів

Перш ніж писати логіку самого чіпа, напишіть прості **тести**, які описують очікувану поведінку. Це дозволить вам ітерувати набагато швидше, не перезапускаючи всю гру.

Для механіки "Прискорювач" важливо перевірити:
1. **Прискорення по часу**: Засікти час, за який заряджається сусідній генератор з прискорювачем і без нього. Він має бути меншим згідно з вашими налаштуваннями.
2. **Скасування при переміщенні**: Якщо перенести генератор (або прискорювач) в іншу частину поля, де немає прискорювача (або генератора) — прискорення повинно зупинитися.

### Крок 3 — Реалізація Chip

Створіть свій клас, наслідуючись від `Chip` або вже існуючих спеціалізованих класів:
- `ChipGenerator` — якщо чіп має щось створювати.
- `ChipContainer` — якщо чіп має щось зберігати всередині.
- `Chip` — для унікальної базової логіки.

### Крок 4 — Створення ефектів

Розробіть візуальну частину:
1. Створіть префаби ефектів, реалізуючи `IEffect` (або використовуйте `Effect`).
2. Підключіть їх через `InitEffects` у вашому коді чіпа.

### Крок 5 — Постійна перевірка

Протягом усієї розробки запускайте тести. Це гарантує, що нова механіка не зламала існуючі (регресія) і працює згідно з вашим початковим дизайном.
