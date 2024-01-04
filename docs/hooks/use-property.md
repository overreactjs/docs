# useProperty

Coalesce a `Prop` (either a `Property` or a literal value) into a `Property`. See the guide on [passing props](../guides/passing-props) for a detailed explanation on how props are used in _Overreact_.

## Example

```tsx
type MyComponentProps = {
  pos: Prop<Position>;
};

const MyComponent: React.FC<> = (props) => {
  // highlight-start
  const pos: Property<Position> = useProperty(props.pos)
  const score: Property<number> = useProperty(0);
  // highlight-end

  // ...
};
```
