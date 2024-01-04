# Device

```tsx
<Engine>
  <Device bg="black" mode="mobile" showFPS>
    <MyGame />
  </Device>
</Engine>
```

This component provides access to the dimensions of the device on which it's running. Nested component can access this information using the `useDevice` hook.

Not only that, but when your app is running in a browser, in development, if the `mode` prop is set to `"mobile"` your game runs inside of simulated mobile frame. In addition, you can rotate and shake the simulated device, and the APIs that report device events (orientation and motion) report appropriate values.

## Props

| Prop | Type | Default | Description |
|-|-|-|-|
| `bg` | `string` | `"white"` | Background color of the device. |
| `mode` | `"mobile" \| "desktop"` | `"desktop"` | Size of the device in development. |
| `hideClose` | `boolean` | `false` | Hide the "close" icon in the top left corner of the device. |
| `showFPS` | `boolean` | `false` | Display an FPS counter in the top right corner of the device. |
| `allowShake` | `boolean` | `false` | When enabled, in development, pressing "S" causes the device to shake, and the values returned by the `useMotion` hook reflect this. |
| `allowTilt` | `boolean` | `false` | When enabled, in development, pressing "G" and "H" rotates the simulated device, and the values returned by the `useOrientation` hook reflect this. |