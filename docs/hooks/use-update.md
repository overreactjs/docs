# useUpdate

> Register a callback function that will be invoked once per frame, whilst the game is not paused. It is responsible for updating some of the game state.

## Example

```tsx
// Update the player's position, based on their velocity.
useUpdate((delta) => {
  pos.current[0] += velocity.current[0] * delta;
  pos.current[1] += velocity.current[1] * delta;
});
```

## Using `delta`

In your update functions, it is important to think about when the `delta` value should be used. It is the number of milliseconds that have elapsed since the previous frame, so on higher framerate devices it will be a smaller number.

In the example above the player's velocity is being multiplied by `delta` to ensure that the player's movement is agnostic of the framerate. Otherwise, on higher framerate devices, the player should run faster and jump higher! 