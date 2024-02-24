## Errors

Errors thrown within components can propagate back to the client, providing transparency and facilitating efficient debugging processes.

_backend/src/components/Examples.tsx_

```tsx
export const HelloWorldExample1 = () => {
  const [count] = useState(0, { key: "count", scope: "global" });

  const increase = () => {
    throw new Error("Not implemented");
  };

  return <ServerSideProps count={count} increase={increase} />;
};
```

Calling the `increase` function from the client may result in a server error, which you can handle gracefully in your client-side code. Once a successful function call is made subsequent to the error, the error state is cleared automatically.

_frontend/src/server-components/Examples.tsx_

```tsx
const HelloWorldExample1 = () => {
  const [{ count, increase }, { loading, error }] =
    useComponent("hello-world-1");

  return (
    <>
      {error && <Alert severity="error">{error.message}</Alert>}
      <Button onClick={() => increase()} />
    </>
  );
};
```
