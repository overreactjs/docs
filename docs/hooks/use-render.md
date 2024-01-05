# useRender

Register a callback function that will be invoked every frame, regardless of whether or not the game is paused. It is responsible for updating the appearance of elements in the DOM to reflect the current game state.

```tsx
useRender(callback);
```

## Examples

```tsx
// Set the position the player's sprite.
useRender(() => {
  element.setBaseStyles({ pos });
});
```

```tsx
// Display the player's score.
const score = useProperty(100);
useRender(() => {
  element.setText(score.current.toString());
});
```