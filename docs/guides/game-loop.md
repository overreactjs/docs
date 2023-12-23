---
sidebar_position: 1
---

# Understanding the Game Loop

With React, we build apps at a high level of abstraction. We leave it to take care of the heavy lifting. Instead, we write code that responds to events, such as key presses, button clicks, and fetch request responses.

This works well for general web development, but when building a game we typically need to run some code frame-by-frame.

Take, for example, the humble platform game. In it, the player has a position, represented as coordinates. The player also has a velocity, such as if they are running or falling. Each frame, we must update the player's position based on their current position and their velocity. In addition, we must apply the effects of gravity to the player's velocity.

In other words, we take the current game state, and play it forwards by the amount of time that has elapsed since the previous frame. And once that is done, we can render all of the game objects to screen. Then the cycle repeats.

In _Overreact_ the [`Engine`](../components/engine) component initializes the game loop for us.

## Under the hood

Internally, the game loop uses [`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame). Here's a simplified version of what it looks like:

```ts
function loop(time: number) {
  const delta = previousTime - time;

  if (!paused) {
    update(delta);
  }

  render(delta);
}
```

Every frame, the engine invokes all of the registered update functions, followed by all of the render functions. Child components can register update and render functions using the [`useUpdate`](../hooks/use-update) and [`useRender`](../hooks/use-render) hooks respectively.

It is worth taking a moment to appreciate the game loop, as it is at the heart of everything you'll do as you build a game.

## See also

- MDN has an excellent article on game loops: https://developer.mozilla.org/en-US/docs/Games/Anatomy