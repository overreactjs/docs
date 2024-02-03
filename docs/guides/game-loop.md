---
sidebar_position: 1
---

# Game Loop

## Overview 

React takes care of a lot for us. It provides a component lifecycle which ensures that when application state changes, the components that own that state get rerendered, as a result of which the DOM is updated.

This works well for general web app development, where a change in state causes a data table to change, or a transition to a different screen, but for games we need the consistency and guarantee for running code every frame, 60 times per second.

One thing all game engines have in common, is how they achieve this. Typically it is referred to as the __"game loop"__, or "core loop", and it consists of two things:

1. The update phase, which is responsible for updating the state of the entire game, taking into account the amount of time that has elapsed since the previous frame.

2. The render phase, which is responsible for rendering the next image that is sent to the screen, taking into account everything that just changed in the game state.

Let's take a look at an example.

Image a simple platform game. In it, the player has a position, represented as coordinates. The player also has a velocity, describing the direction and speed with which the player is currently moving. Each frame, in the "update" phase we update the set a new position for the player, based on their current position and their velocity. Then, once that is done, we render the player to the screen in their new location.

> In _Overreact_ the [`Engine`](../components/engine) component initializes the game loop for us.

## An example

Let's create a component that renders a multicolored square on screen.

### The update phase

Our component outputs an empty div, with no styles, and no classes. It has a `hue` ref, and each frame the hue value is increased by a small amount. If we were to run this we'd see nothing, because we aren't 'rendering' anything yet.

```tsx
const MyComponent = () => {
  const ref = useRef<HTMLDivElement>(null);
  const hue = useRef(0);

  useUpdate((delta) => {
    hue.current = hue.current + (delta * 0.1);
  });

  return <div ref={ref} style={{ width: '100px', height: '100px' }} />;
}
```

### The render phase

Let's add a render function, which updates the background color of the element, based on the current `hue` value.

```tsx
const MyComponent = () => {
  const ref = useRef<HTMLDivElement>(null);
  const hue = useRef(0);

  useUpdate((delta) => {
    hue.current = hue.current + (delta * 0.1);
  });

  // highlight-start
  useRender(() => {
    ref.style.background = `hsl(${hue.current}deg 100% 50%)`;
  });
  // highlight-end

  return <div ref={ref} style={{ width: '100px', height: '100px' }} />;
}
```

That's better, now we see the square, cycling gradually through all of the colors of the rainbow.

## Under the hood

The _Overreact_ game loop uses [`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) to request that the browser runs one iteration of the loop each time it is ready to draw to the screen. Here's a simplified version of what it looks like:

```tsx
let previous = 0;

function loop(next) {
  const delta = next - previous;
  previous = next;

  update(delta);
  render();
}
```

## See also

- MDN has an excellent article on game loops: https://developer.mozilla.org/en-US/docs/Games/Anatomy