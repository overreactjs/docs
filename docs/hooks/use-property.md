# useProperty

Turn a `Prop` (either a `Property` or a literal value) into a `Property`. See the guide on [properties](../guides/properties) for a detailed explanation on how props are used in _Overreact_.

```tsx
const health = useProperty(100);
```

## Reference

### `useProperty(initial)`

#### Parameters

- `initial: T | Property<T>` : An existing property, or a literal value that will be used as the initial value for the property.

#### Returns

- `Property<T>` : If a property was passed in, it will be returned unchanged. If a literal value was passed in, it will be turned into a property.

## Examples

```tsx
type MyComponentProps = {
  pos: Prop<Position>;
};

const MyComponent: React.FC<> = (props) => {
  // props.pos might be a literal or a property, so ensure it's the latter.
  const pos = useProperty(props.pos);

  // initialize the score to zero.
  const score = useProperty(0);

  // ...
};
```
