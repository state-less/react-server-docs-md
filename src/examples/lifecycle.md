## Lifecycle

When you update a server-side state using the setValue function, the component triggers an automatic re-rendering within the client's context. This mechanism ensures that the component promptly notifies subscribed clients about the change, leading to an immediate update in the component's state. Consequently, components rendered on the frontend become reactive, eliminating the necessity for manual refetching or polling of the state.

_backend/src/components/Examples.tsx_

```tsx
export const HelloWorldExample2 = () => {
  const [count, setState] = useState(0, { key: "count", scope: "global" });

  const increase = () => {
    setState(count + 1);
  };

  return <ServerSideProps count={count} increase={increase} />;
};
```

When you invoke the increase function from the client, the server updates the state and triggers a re-render on the client.

```tsx
export const HelloWorldExample2 = () => {
  const [component, { loading, error }] = useComponent("hello-world-1", {
    client: localClient,
  });
  console.log("HelloWorldExample1", error);
  return (
    <>
      {error && <Alert severity="error">{error.message}</Alert>}
      <Button onClick={() => component?.props?.increase()}>
        Count is {component?.props?.count}
      </Button>
    </>
  );
};
```

Click the button to observe the count increasing and remaining persistent, even after a page reload. You can also experiment by opening a second tab of the same page and clicking the button. You will notice the count updates simultaneously on both pages.
