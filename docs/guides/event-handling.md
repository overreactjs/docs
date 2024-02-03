---
sidebar_position: 3
---

# Event Handling

## Overview

Another knock-on effect of the [game loop](./game-loop) is how you'll handle events, such as key presses, mouse clicks, and touch events.

Traditionally, we write code that responds to _changes in the state_ of input devices. For, example, when a key is pressed down, and maybe again when that key is released. But in our update functions we want to query the _current state_ of input devices, such as "is a key currently being hand down.

We could achieve this by listening to both `keydown` and `keyup` events, then maintaining a list of all keys currently in the 'down' state. But since this is a common pattern, _Overreact_ takes care of it for us.

In the following example we're checking to see if the space bar is being held down, and if it is, the player blows bubbles!

```tsx
const { isKeyDown } = useKeyboard();

useUpdate(() => {
  if (isKeyDown('Space')) {
    fireBubbles();
  }
});
```

## Down vs pressed

In the example above we used the `isKeyDown` function to see whether a key is held down. 

## Keyboard

Coming soon...

## Pointer

Coming soon...

## Orientation and motion

Coming soon...