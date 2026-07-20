# Extension Guide

[← На Головну](Main.md)

Цей документ описує, як розширювати проєкт Merge Toolkit: від створення нової гри до додавання нових типів фішок, ефектів та взаємодій.

---

## 1. Creating a New Game

Перед розширенням рекомендується створити чисту структуру проєкту. Детальна інструкція знаходиться у [Quick Start → Create New Merge Game (Clean Project)](Quick_Start.md#create-new-merge-game-clean-project).

**Коротко:**
1. Відкрийте **Window → Expecto → Clean Merge Project**.
2. Оберіть потрібний шаблон конфігурації у полі **Template Settings** (використовує ScriptableObject асети типу `MergeTemplateSettings`).
3. Вкажіть бажаний **Namespace**, унікальний **Prefix** та натисніть кнопку **Select Folder For Clean Project**, обравши або створивши папку нової гри.
4. Інструмент автоматично створить повну структуру папок на основі конфігурації шаблону, сцену, згенерує C# класи `{Prefix}MergeLifetimeScope` та `{Prefix}MergeInitializer`, скопіює та перейменує базові префаби поля, підготує інтеграційні тести (`.Tests.asmdef`, тестові ассети та шаблон класу тесту) та автоматично перемкне відкрите вікно **Chip Viewer** на використання нових налаштувань.

Після цього ваш проєкт готовий до розширення.

---

## 2. Extension via VContainer (Dependency Injection)

Проєкт використовує **VContainer** для керування залежностями. Точка конфігурації — клас `Merge2LifetimeScope`, який наслідує `LifetimeScope`.

### How It Works

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

### Key Interfaces

| Інтерфейс | Реалізація | Призначення |
|---|---|---|
| `IFieldGrid` | `FieldGrid` | Управління сіткою (2D масив `Cell`): створення комірок, валідація координат, розміщення чіпів |
| `IFieldEventHandler` | `FieldEventHandler` | Точка входу для подій вводу (Tap, Drag), координація взаємодії між Input та полем |
| `IFieldInitializeCommand` | `FieldInitializeCommand` | Ініціалізація рівня: створення візуальної сітки та завантаження початкових чіпів |
| `IChipMovingLogic` | `ChipMovingLogic` | Валідація та виконання переміщень, розрахунок ланцюгових релокацій сусідніх чіпів |
| `IFreeCellFinder` | `FreeCellFinder` | Пошук найближчої вільної комірки (спіральний алгоритм) |
| `IChipFlyAnimation` | `ChipFlyAnimation` | Анімація "польоту" чіпа до цільової позиції |
| `IChipInteractionLogic` | `MergeableChipLogic`, `FillContainerLogic` | Стратегія взаємодії між фішками (merge, fill container) |

### Recommended Approach: Creating Custom LifetimeScope

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
        _ = builder.RegisterInstance(fieldData);
        _ = builder.RegisterInstance(fieldData.ChipDataCollection);

        _ = builder.RegisterComponentInHierarchy<FieldEventHandler>().As<IFieldEventHandler>().AsSelf();
        _ = builder.RegisterComponentInHierarchy<FieldGrid>().As<IFieldGrid>().AsSelf();
        _ = builder.RegisterComponentInHierarchy<FieldInitializeCommand>().As<IFieldInitializeCommand>().AsSelf();
        _ = builder.RegisterComponentInHierarchy<InputManager>().AsSelf();
        
        _ = builder.Register<DeferredChipChangeNotifier>(Lifetime.Singleton).As<IChipChangeNotifier>();
        _ = builder.RegisterComponentInHierarchy<CellObserverManager>().AsSelf();

        _ = builder.Register<ChipFactory>(Lifetime.Singleton);
        _ = builder.Register<FreeCellFinder>(Lifetime.Singleton).As<IFreeCellFinder>();
        _ = builder.Register<ChipFlyAnimation>(Lifetime.Transient).As<IChipFlyAnimation>();
        _ = builder.Register<NeighborChipFinder>(Lifetime.Singleton).As<IChipFinder>();

        _ = builder.RegisterEntryPoint<Merge2Initializer>();

        // --- Замінена реалізація ---
        _ = builder.Register<MyChipMovingLogic>(Lifetime.Singleton).As<IChipMovingLogic>();

        // --- Додані нові сервіси ---
        _ = builder.Register<MyNewService>(Lifetime.Singleton).As<IMyNewService>();
    }
}
```

Після цього замініть компонент `Merge2LifetimeScope` на `MyGameLifetimeScope` на сцені.

### Recommended Approach: Creating Custom Initializer

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

### How to Add Custom Service

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

### How to Add Custom Interaction (IChipInteractionLogic)

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

## 3. Extending Chip

Базовий клас `Chip` надає набір віртуальних методів, які можна перекрити для створення нових типів фішок. Існуючі приклади: `ChipGenerator`, `ChipContainer`.

### Step 1 — Create Derived Class

```csharp
public class MyChip : Chip
{
    // Власні поля та залежності
    [Inject] private readonly IMyService myService;
}
```

### Step 2 — Override `Init` for Initialization

```csharp
public override void Init(ChipData data, ChipRuntimeData runtimeData)
{
    base.Init(data, runtimeData); // Важливо! Ініціалізує базові ефекти та runtimeData
    
    // Власна логіка ініціалізації
    // Наприклад, читання додаткових даних з specialDatas
    var settings = data.GetSpecialData<ExtensionChipData>();
}
```

> [!TIP]
> Рекомендовано викликати `GetSpecialData<T>()` лише один раз під час `Init` і кешувати результат у полі чіпа. Не викликайте його повторно в `Update` або інших гарячих ділянках логіки.

### Step 3 — Add Specific Effects via `InitEffects`

```csharp
protected override void InitEffects()
{
    base.InitEffects(); // Ініціалізує стандартні ефекти та ефекти з ChipExtraEffectsData
    
    // Додати специфічні ефекти програмно
    var myEffect = InstantiateEffect<IEffect>(myEffectPrefab);
    AddEffect(myEffect, MyEffectId, true);



}
```

### Step 4 — Extend RuntimeData (Optional)

Якщо новому чіпу потрібен додатковий runtime стан:

```csharp
// 1. Створити клас рантайм-даних
public class MyChipRuntimeData : ChipRuntimeData
{
    public int CustomProperty;
}

// 2. Ініціалізувати через InitRuntimeData
public override void InitRuntimeData(ChipData data, ref ChipRuntimeData runtimeData)
{
    if (runtimeData == null)
    {
        runtimeData = new MyChipRuntimeData();
    }
    base.InitRuntimeData(data, ref runtimeData);
}
```

### Key Virtual Methods of Chip

| Метод | Коли використовувати |
|---|---|
| `Init(ChipData, ChipRuntimeData)` | Ініціалізація. Завжди викликайте `base.Init(data, runtimeData)`. |
| `InitRuntimeData(ChipData, ref ChipRuntimeData)` | Створення та ініціалізація об'єкта `RuntimeData`. |
| `InitEffects()` | Створення об'єктів ефектів. Завжди викликайте `base.InitEffects()`. |
| `PostInitEffects()` | Налаштування після ініціалізації всіх ефектів (наприклад, для систем знищення). |
| `OnTap(Vector2)` | Реакція на тап (наприклад, ручна генерація у `ChipGenerator`). |
| `OnDragStart/OnDrag/OnDragEnd` | Кастомна поведінка при перетягуванні. |
| `UpdateVisual()` | Синхронізація візуального стану ефектів з `RuntimeData`. |
| `SetMoving(bool)` | Зміна sorting order та сповіщення ефектів про початок/кінець руху. |
| `OnNeighborsChipOfInteraction()` | Реакція на те, що на сусідній клітинці відбулася взаємодія (використовується для систем знищення ефектів). |
| `OnDraggingChipWithMoveLocked()` | Візуальна реакція на спробу перемістити заблокований чіп. |
| `Destroy(Cell)` | Очищення при знищенні. |
| `CanMoving()` | Чи може фішка бути переміщена. |

---

## 4. Extending Effects

Системи ефектів побудована на інтерфейсі `IEffect` та базовому класі `Effect`. Детальний опис всіх ефектів: [Visual Effects](Visuals/Effects.md).

> [!TIP]
> Немає жорсткої прив'язки до конкретних реалізацій ефектів. Чіп прив'язується до **інтерфейсу** ефекту, а не до його реалізації. Усі вбудовані ефекти (Highlight, MergeAvailable, MoveLocked тощо) підключаються через **префаби**, тому їх можна легко **замінити** власною реалізацією або **розширити**, створивши похідний клас. Достатньо створити новий префаб з вашим компонентом, що реалізує `IEffect`, `IEffectContainerHint`, `IEffectGeneratorCharging` або власний інтерфейс, що наслідує `IEffect`.

### Custom Effect IDs & Inspector Integration

Щоб визначити власні кастомні ефекти без зміни базового файлу `EffectConsts.cs`, настійно рекомендується створити окремий файл з константами у вашому проєкті.

Щоб уникнути конфліктів ID з базовими ефектами під час майбутніх оновлень рушія, **починайте власні ID з 1000**.

Використовуйте атрибут `[EffectDefinition]` над вашими константами. Система автоматично збере їх під час запуску через Reflection та інтегрує до загального реєстру.

```csharp
public static class MyEffectConsts
{
    // Починайте з 1000, щоб уникнути конфліктів з базовими ID!
    [EffectDefinition("FrostEffect")]
    public const int FrostEffect = 1000;
    
    [EffectDefinition("PoisonEffect")]
    public const int PoisonEffect = 1001;
}
```

Якщо у ваших скриптах потрібен випадаючий список (комбобокс) в інспекторі для вибору одного з доступних ефектів (як базових, так і кастомних), використовуйте атрибут `[EffectSelector]` над серіалізованим `int` полем:

```csharp
[EffectSelector]
[SerializeField] private int mySelectedEffectId;
```

### Option A — Inheriting from `Effect` (Recommended)

Базовий клас `Effect` — це потужна система, яка автоматизує більшість типових задач для візуальних ефектів:
- **Автоматичне масштабування (`AutoSize`)**: Ефект може автоматично змінювати свій `localScale` під розмір чіпа (1x1, 2x2 тощо), використовуючи різні стратегії (`ScaleByChipSize`, `ScaleByMaxChipSize`).
- **Керування ієрархією**: Підтримка різних типів прив'язки (`ParentChip`, `ParentCell`, `ParentChipAnimationNode`).
- **Життєвий цикл та стан**: Автоматичне приховування під час руху (`DeactivateOnMove`) та відновлення стану після завершення переміщення.
- **Інтеграція з Animator**: Вбудована підтримка тригерів `Activate` та `Deactivate`, а також система `dontRepeatTrigger`.
- **Позиціонування**: Можливість автоматичного зміщення центру ефекту залежно від розмірності чіпа.
- **Система блокування та знищення**: Вбудована підтримка `EffectBlockingSettings` та прогресивного знищення через `TryDestroyEffect`.

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

### Option B — Implementing `IEffect` Directly

Для ефектів, які не потребують Animator або стандартної логіки `Effect`:

```csharp
public class MyPureEffect : MonoBehaviour, IEffect
{
    public int GetId() => 0;
    public GameObject gameObject => gameObject;
    public EffectBlockingSettings BlockingSettings => null;
    public EffectDestroyingSettings DestroyingSettings => null;

    public void Init(Chip chip, int effectHash) { }
    public bool Activate(Chip chip) => true;
    public void Deactivate(Chip chip, bool force = false) { }
    public void SendTrigger(string triggerName, bool allowRepeat = false) { }
    public void OnChangedCell(Cell sourceCell, Cell targetCell) { }
    public void OnInteractionOverCellChanged(Cell prevCell, Cell currentCell, Cell underCell) { }
    public void OnInteractionUnderCellChanged(Cell underCell, Cell overCell) { }
    public void OnMovingStateChanged(Chip chip, bool isMoving) { }
    public bool TryDestroyEffect(Chip chip, EffectDestroyingSettings settings, EffectDestroyingRuntimeData destroyingData) => false;
}
```

### Specialized Effect Interfaces

Для ефектів з розширеним контрактом існують спеціалізовані інтерфейси:

| Інтерфейс | Метод | Призначення |
|---|---|---|
| `IEffectHint` | `Hint(bool force)` | Відображення візуальних підказок |
| `IEffectContainerHint` | `UpdateElements(...)` | Візуалізація вмісту контейнерів з підказками |
| `IEffectGeneratorCharging` | `OnCharging(float progress)` | Відображення прогресу зарядки |
| `IEffectPowerBoosterJoin` | `OnJoin(IPowerBoosterTarget)` / `OnLeave(IPowerBoosterTarget)` | Відображення зв'язку з бустером |


Щоб створити новий спеціалізований ефект:
1. Створіть інтерфейс, що наслідує `IEffect`.
2. Створіть `InterfaceRef<T>` обгортку для серіалізації в інспекторі:
   ```csharp
   [Serializable]
   public class MyEffectRef : InterfaceRef<IMyEffect> { }
   ```
3. Використовуйте `MyEffectRef` як серіалізоване поле у чіпі.

### Connecting Effect to Chip

1. **Через ChipData.specialDatas**: Для blocker-ефектів або інших додаткових ефектів додайте дані `ChipExtraEffectsData` у `specialDatas` і заповніть список `Blockers` або `OtherEffects`.
2. **Через InitEffects**: У похідному класі `Chip` створіть ефект через `InstantiateEffect<T>(prefab)` та додайте його за допомогою `AddEffect`.

### Effect Notifications

Ефекти, додані до `effects`, автоматично отримують сповіщення через наступні методи `IEffect`:

| Подія | Метод(и) | Призначення |
|---|---|---|
| Зміна комірки | `OnChangedCell` | Викликається при фактичній зміні батьківської комірки чіпа. |
| Рух під час drag | `OnInteractionOverCellChanged`, `OnInteractionUnderCellChanged` | Оновлення візуалу ефектів (наприклад, підсвічування цілі) під час перетягування. |
| Зміна стану руху | `OnMovingStateChanged` | Початок або кінець візуального переміщення (drag або relocation). |
| Вимога на самознищення | `TryDestroyEffect` | Обробка вхідних подій (наприклад, мердж сусідів) для систем знищення ефектів. |
| Знищення чіпа | — | Ефекти автоматично знищуються разом із Game-об'єктом чіпа. |

---

## 5. Guidelines for Creating New Mechanics

Коли ви хочете додати новий тип чіпу (наприклад, чіп, що прискорює сусідні генератори), рекомендується дотримуватися наступного алгоритму:

### Step 1 — Setting Up ChipData

Починайте з визначення даних. Бажано не захаращувати базовий `ChipData`.
1. Створіть окремий serializable-клас для параметрів вашої механіки (наприклад, `ExtensionChipData`), який реалізує `IChipSpecialData`.
2. Додайте цей тип у `ChipData.specialDatas` через `Chip Creator` -> секцію **Special Data**.
3. В коді чіпа отримуйте дані в `Init` так: `var settings = data.GetSpecialData<ExtensionChipData>();` (рекомендовано викликати один раз і зберігати в полі).

```csharp
[Serializable]
public class ChipExtensionData : IChipSpecialData
{
    public float Power = 1.2f;
}
```

> [!TIP]
> `SpecialData`-класи можна розширювати через наслідування. Наприклад, якщо ви робите власний чіп на базі `ChipGenerator`, зручно наслідуватися від `ChipGeneratorData` і додати тільки нові параметри.

```csharp
[Serializable]
public class ChipGeneratorDataEx : ChipGeneratorData
{
    ...
}
```

### Step 2 — Creating Tests

Перш ніж писати логіку самого чіпа, напишіть прості **тести**, які описують очікувану поведінку. Це дозволить вам ітерувати набагато швидше, не перезапускаючи всю гру.

Для механіки "Прискорювач" важливо перевірити:
1. **Прискорення по часу**: Засікти час, за який заряджається сусідній генератор з прискорювачем і без нього. Він має бути меншим згідно з вашими налаштуваннями.
2. **Скасування при переміщенні**: Якщо перенести генератор (або прискорювач) в іншу частину поля, де немає прискорювача (або генератора) — прискорення повинно зупинитися.

### Step 3 — Implementing Chip

Створіть свій клас, наслідуючись від `Chip` або вже існуючих спеціалізованих класів:
- `ChipGenerator` — якщо чіп має щось створювати.
- `ChipContainer` — якщо чіп має щось зберігати всередині.
- `Chip` — для унікальної базової логіки.

### Step 4 — Creating Effects

Розробіть візуальну частину:
1. Створіть префаби ефектів, реалізуючи `IEffect` (або використовуйте `Effect`).
2. Підключіть їх через `InitEffects` у вашому коді чіпа.

### Step 5 — Continuous Verification

Протягом усієї розробки запускайте тести. Це гарантує, що нова механіка не зламала існуючі (регресія) і працює згідно з вашим початковим дизайном.
