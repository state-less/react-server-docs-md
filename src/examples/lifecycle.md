## Lifecycle

If you change a state on the server using the `setValue` function, the component will be rerendered in the context of the client.

The component will automatically notify the subscribed client about the change where it updates the component.

This makes components rendered by the frontend reactive and you don't need to bother refetching or polling the state yourself.


*backend/src/components/Examples.tsx*
```tsx
export const HelloWorldExample2 = () => {
    const [count, setState] = useState(0, { key: 'count', scope: 'global' });

    const increase = () => {
        setState(count + 1);
    };

    return <ServerSideProps count={count} increase={increase} />;
};
```

If you call the `increase` function from the client, the server updates the state and issues a rerender on the client.

```tsx
export const HelloWorldExample2 = () => {
  const [component, { loading, error }] = useComponent('hello-world-1', {
    client: localClient,
  });
  console.log('HelloWorldExample1', error);
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

Try and click the button. As you can see the count increases and stays persistent, even if you reload the page. You can also try to open a second tab to the same page and click the button. You will see that it get's updated in both pages.
