# useProperty

> Coalesce a `Prop` (either a `Property` or a literal value) into a `Property`. 

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

## See also

- Guide on [passing props](../guides/passing-props).