# IsoMergeCamera

## Description
`IsoMergeCamera` — is a camera control system specifically designed for an isometric game board (XY plane). It provides smooth and intuitive movement across the field during gameplay, taking into account both direct interaction with the camera and indirect interaction (e.g., when dragging objects).

## Features

The camera system supports four main mechanics:
1. **Pan**: Free camera movement across the board.
2. **Zoom**: Zooming in and out.
3. **Edge-scroll**: Automatic camera movement when a player drags an object near the screen edge.
4. **Inertia**: Smooth movement damping after active movement (edge-scroll).

---

## Detailed Description of Mechanics

### 1. Pan (Free Movement)
Allows the player to freely explore the playing field.
- **Activation**: Player touches the screen (or holds the mouse button) on an empty spot or an inactive object and begins dragging.
- **Logic**: The camera tracks the pointer (finger/cursor) movement and shifts the view so that the field point pressed by the player remains under the pointer.
- **Limitation**: Movement is only possible **when the player is not dragging a game chip**.

### 2. Zoom
Allows changing the field view scale.
- **Input Sources**:
  - **Pinch-to-zoom**: Pinching or spreading two fingers on a touchscreen.
  - **Mouse Wheel**: Scrolling the wheel.
- **Logic**: Zooming occurs relative to the point (focus) where the pointer or the center between two fingers is located. This means that the camera does not just zoom in/out from the screen center, but zooms specifically into the area of player interest.
- **Limitation**: There are minimum (`minOrthoSize`) and maximum (`maxOrthoSize`) zoom bounds.

### 3. Edge-scroll (Automatic Screen-Edge Scrolling)
Helps the player move a chip a long distance when the target location is outside the current screen view.
- **Activation**: Occurs **only during chip dragging**.
- **Logic**: When the chip dragged by the player approaches the screen edge (falls into a special invisible `edgeScrollZone`), the camera starts moving automatically in that direction.
- **Speed**: The closer the chip is to the edge itself, the faster the camera moves. Movement can happen simultaneously along multiple axes (e.g., diagonally into a screen corner).
- **Field Interaction**: To prevent the chip from "detaching" from the cursor during camera movement, the system continuously compensates for the camera shift, moving the chip in world coordinates.

### 4. Inertia
Makes the camera stop more natural and pleasant.
- **Activation**: Triggers automatically when the player releases the chip after using Edge-scroll.
- **Logic**: Instead of a sharp stop, the camera receives a speed "impulse" it had during Edge-scroll and continues sliding, gradually slowing down (depends on the `inertiaDamping` parameter).

---

## Bounds
The camera is always kept within the playing field bounds, defined by two extreme points (bottom-left and top-right corners).
- The bounds logic accounts for the current zoom scale and screen aspect ratio, guaranteeing that the player will never see "emptiness" beyond the game world if the field is large enough.
- If the field is smaller than the screen, the camera is centered.

---

## High-level Architectural Overview

- **Input**: All gestures (touch, drag, zoom) are aggregated through a common `InputManager`. The camera does not read input devices directly.
- **Field Synchronization (`IsoFieldEventHandler`)**: The camera works in close connection with the field system. When the player drags a chip, the field notifies the camera about the drag state. The camera, in turn, notifies the field if it shifts due to Edge-scroll, so the field can correct the chip position.
- **Smoothing**: All target camera movement values (target position and zoom) are applied via linear interpolation in the `LateUpdate` phase. This makes camera movement smooth and independent of minor input jitters.
