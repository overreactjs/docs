# Properties

In _Overreact_ you'll work with props a little differently to how you would typically use props in a React app.

## Breaking from tradition

When we looked at the [Game Loop](./game-loop), we saw how we avoid parts of the React component lifecycle, in order to get greater control of when game state is updated, and when the DOM is synchronized with that state.

In this guide we'll see how that impacts how we work with props.

When building a React app, we mostly don't need to think about the component lifecycle, and how it synchronizes changes to the DOM. So long as we use state in accordance with its expectation of how it should be used, it holds up its end of the bargain to ensure that components are rerendered when the state changes.

Here's a simple component that contains a single piece of state, a count of how many times a button has been pressed. When the button is clicked, the count is increased by one, which triggers a rerender of the component, thus ensuring the new value is displayed. We can write this code without ever knowing the details of how React does this under the hood.

```tsx title="/src/components/MyComponent.js"
const MyComponent = () => {
  const [count, setCount] = useState(0);
  const onClick = () => setCount((count) => count + 1);

  return (
    <button onClick={onClick}>
      Clicked {count} times!
    </button>
  );
};
```

However, when we're making a game, we don't want React to rerender our components for us, because we need tighter control over when that happens. With _Overreact_, you'll use props that are more like refs. The example below is equivalent to the one above:

```tsx title="/src/components/MyComponent.js"
const MyComponent = () => {
  const element = useElement();
  const count = useProperty(0);
  const onClick = () => count.current += 1;

  useRender(() => {
    element.setText(`Clicked ${count.current} times!`);
  });

  return <button ref={element.ref} onClick={onClick} />;
};
```

## Passing properties

Working with properties that behave like refs introduces a problem: How do we pass them to child components?

Let's expand on the example above. Let's imagine we need access to the number of times the user clicked the button further up in our app. If this were a regular React app we'd hoist the state up to the parent component, and likely pass in an `onClick` callback.

We could do the equivalent with properties, like so, and it would work:

```tsx title="/src/components/Parent.js"
export const Parent = () => {
  const count = useProperty(0);
  return <MyComponent count={count} />;
};
```

```tsx title="/src/components/MyComponent.js"
type Props = {
  count: Property<number>;
}

const MyComponent: React.FC<Props> = ({ count }) => {
  const onClick = () => count.current += 1;

  useRender(() => /* ... */);

  return <button ref={element.ref} onClick={onClick} />;
};
```

However, we can do better. The `useProperty` hook not only initializes a new property, it can take an existing property as a parameter, and simply passes it through. This – along with the `Prop<T>` type – allows us to create components which can either initialize a property or reuse an existing property that something higher up the component tree also has access to.


```tsx title="/src/components/MyComponent.js"
type Props = {
  // highlight-start
  count: Prop<number>;
  // highlight-end
};

const MyComponent: React.FC<Props> = (props) => {
  // highlight-start
  const count = useProperty(props.count || 0);
  // highlight-end
  const onClick = () => count.current += 1;

  useRender(() => /* ... */);

  return <button ref={element.ref} onClick={onClick} />;
};
```

This can be particularly powerful, allowing us to create components that can either own their own properties, or use properties owned by others. And even when their own their own properties, they can be initialized by the parent component.

All of the following are valid:

```tsx
// The component owns the property.
return <MyComponent />;

// The component owns the property, initialized by the parent as '5'.
return <MyComponent count={5} />;

// The parent owns the property.
const count = useProperty(0);
return <MyComponent count={count} />;
```

## Invalidation

Remember when we said properties in _Overreact_ are a lot like refs, objects with a `current` property. They are a little more.

They also have an `invalidated` property, which is automatically set to `true` when a new value is assigned to `current`, or the value assigned to `current` changes. (This happens thanks to the magic of proxies).

The first example we showed above has a flaw, we aren't checking the `invalidated` flag (or clearing it) in the render function. As a result, we're setting the text content of the button ___every single frame___, even when it hasn't changed. Here's a better implementation:

```tsx
useRender(() => {
  // Check that the count value actually changed.
  if (count.invalidated) {
    element.setText(`Clicked ${count.current} times!`);

    // Clear the invalidated flag so that we don't render unnecessarily.
    count.invalidated = false;
  }
});
```

### Clear invalidation flags

When you clear an `invalidated` flag on a property, it does not change immediately. Instead, it is scheduled to change after the current render phase, once all registered render functions have completed. This is necessary since a property may be accessed in multiple render functions, and we wouldn't want the first of those to clear the invalidation flag that a subsequent render function will be checking.