# useElement

Create a reference to an HTML element, along with a handful of helper functions for modifying that element.

```tsx
const { ref, setText, setAttribute, setStyle, ...more } = useElement();
```

## Reference

### `setStyle` function

Set a CSS style on the element.

```tsx
const { setStyle } = useElement();

useRender(() => {
  setStyle('background-color', 'hotpink');
});
```

#### Parameters

- `key: string` : The CSS property to set.
- `value: CSSStyleValue | string | number` : The CSS property value.

---

### `setBaseStyles` function

Set commonly used styles, including position, rotation, size, and opacity.

```tsx
const { setBaseStyles } = useElement();

const pos = useProperty([50, 100]);
const size = useProperty([200, 200]);
const angle = useProperty(45);

useRender(() => {
  setBaseStyles({ pos, size, angle });
});
```

#### Parameters

- `options` : An object consisting of the following optional properties:
  - `pos : Property<Position>` : The absolute position of the element.
  - `size : Property<Size>` : The width and height of the element.
  - `angle : Property<number>` : The angle of rotation, in degrees.
  - `flip : Property<boolean>` : Flag indicating whether the element is flipped horizontally.

---

### `setText` function

Sets the text content (inner HTML) of the element.

```tsx
const { setText } = useElement();

const score = useProperty(props.score);

useRender(() => {
  setText(`Score: ${score.current}`);
});
```

---

### `setAttribute` function

Sets an attribute on the element.

---

### `setData` function

Set a data attribute (`data-?`) on the element.

