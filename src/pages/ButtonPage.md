# Code
You probably wonder how components on the server side look like. That's easy: Just as they do on the Frontend. If you're familiar with _React_ you should immediately notice the similarities.

```tsx
const server = <Server>{/* your components here */}</Server>;
```

Creating your own components is straight forward. This is the code that powers the button below.

```tsx
import { Scopes, useState, clientKey } from "@state-less/react-server";
import { ServerSideProps } from "./ServerSideProps";

export const HelloWorldExample = (props, { key, context }) => {
  // The useState hook looks familiar?
  const [count, setState] = useState(0, {
    key: "count",
    scope: Scopes.Global,
  });

  // A simple function that can be executed from the client side.
  const increase = () => {
    setState(count + 1);
  };

  return (
    // Simply pass down props to the client side.
    <ServerSideProps
      key={clientKey(`${key}-props`, context)}
      count={count}
      increase={increase}
    />
  );
};
```
