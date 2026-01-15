# Namespace: Merge2.Input

## Table of Contents
- [IInputManager](#iinputmanager)
- [InputManager](#inputmanager)
- [Merge2Input](#merge2input)

---

## IInputManager

> - **Purpose**: Interface for input event handling system
> - defines events for tap and drag operations
> - **Usage**: Implemented by InputManager
> - used for dependency injection via VContainer to decouple input handling from consumers
> - **Notes**: Provides events (OnTap, OnDragStart, OnDrag, OnDragEnd) and editor-only simulation methods for testing
#### Methods
- `+ SimulateDrag(Vector2 position, Vector3 worldPosition): void`
- `+ SimulateDragEnd(Vector2 position, Vector3 worldPosition): void`
- `+ SimulateDragStart(Vector2 position, Vector3 worldPosition): void`
- `+ SimulateTap(Vector2 position, Vector3 worldPosition): void`
---

## InputManager
**Inherits**: `MonoBehaviour`

> - **Purpose**: Handles user input for the Merge2 game board, converting raw input into game events (Tap, Drag)
> - **Usage**: Attached to the board GameObject
> - raises events for other managers to subscribe to.
> - **Notes**: Uses the new Unity Input System via Merge2Input class. Preserves legacy thresholds for drag detection.
#### Fields
- `- dragging: bool`
- `- dragStartThreshold: float`
- `- input: Merge2Input`
- `- logEnable: bool`
- `- mainCamera: Camera`
- `- OnDrag: InputEvent`
- `- OnDragEnd: InputEvent`
- `- OnDragStart: InputEvent`
- `- OnTap: InputEvent`
- `- press: bool`
- `- remainingTapTime: float`
- `- spriteRenderer: SpriteRenderer`
- `- spriteTransform: Transform`
- `- startPos: Vector2`
- `- tapTimeThreshold: float`
#### Methods
- `+ SimulateDrag(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Simulates drag input for testing purposes in Unity Editor
    - **Usage**: Called from test helpers to simulate continuous drag without actual input
    - available only in editor
    - **Params**: position - local field position
    - worldPosition - world space position
- `+ SimulateDragEnd(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Simulates drag end input for testing purposes in Unity Editor
    - **Usage**: Called from test helpers to simulate drag end without actual input
    - available only in editor
    - **Params**: position - local field position
    - worldPosition - world space position
- `+ SimulateDragStart(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Simulates drag start input for testing purposes in Unity Editor
    - **Usage**: Called from test helpers to simulate drag start without actual input
    - available only in editor
    - **Params**: position - local field position
    - worldPosition - world space position
- `+ SimulateTap(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Simulates tap input for testing purposes in Unity Editor
    - **Usage**: Called from test helpers to simulate user tap without actual input
    - available only in editor
    - **Params**: position - local field position
    - worldPosition - world space position
- `- Awake(): void`
- `- OnDisable(): void`
- `- OnDragEndImpl(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Internal implementation for drag end event handling
    - **Usage**: Called internally when drag operation ends
    - logs and invokes OnDragEnd event. For testing, use SimulateDragEnd method instead.
    - **Params**: position - local field position
    - worldPosition - world space position
- `- OnDragImpl(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Internal implementation for drag event handling
    - **Usage**: Called internally continuously during drag
    - logs and invokes OnDrag event. For testing, use SimulateDrag method instead.
    - **Params**: position - local field position
    - worldPosition - world space position
- `- OnDragStartImpl(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Internal implementation for drag start event handling
    - **Usage**: Called internally when drag operation begins
    - logs and invokes OnDragStart event. For testing, use SimulateDragStart method instead.
    - **Params**: position - local field position
    - worldPosition - world space position
- `- OnEnable(): void`
- `- OnTapImpl(Vector2 position, Vector3 worldPosition): void`
    - **Purpose**: Internal implementation for tap event handling
    - **Usage**: Called internally when a tap is detected
    - logs and invokes OnTap event. For testing, use SimulateTap method instead.
    - **Params**: position - local field position
    - worldPosition - world space position
- `- Start(): void`
- `- Update(): void`
---

## Merge2Input
#### Fields
- `++ bindingMask: Nullable<InputBinding>`
- `++ devices: Nullable<ReadOnlyArray<InputDevice>>`
- `+- asset: InputActionAsset`
- `+- bindings: IEnumerable<InputBinding>`
- `+- controlSchemes: ReadOnlyArray<InputControlScheme>`
- `+- Game: GameActions`
- `- m_Game: InputActionMap`
- `- m_Game_Click: InputAction`
- `- m_Game_Point: InputAction`
- `- m_GameActionsCallbackInterfaces: List<IGameActions>`
#### Methods
- `+ Contains(InputAction action): bool`
- `+ Disable(): void`
- `+ Dispose(): void`
- `+ Enable(): void`
- `+ FindAction(string actionNameOrId, bool throwIfNotFound): InputAction`
- `+ FindBinding(InputBinding bindingMask, InputAction& action): int`
- `+ GetEnumerator(): IEnumerator<InputAction>`
- `~ Finalize(): void`
- `- System.Collections.IEnumerable.GetEnumerator(): IEnumerator`
---

