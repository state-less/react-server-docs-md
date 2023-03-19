## Errors

Functions in components can throw errors which get propagated back to the client.

*backend/src/components/Examples.tsx*
```tsx
const HelloWorldExample1 = () => {
    const [count, setCount] = useState(0);

    const increase = () => {
        throw new Error('Not implemented')
    }

    return <ServerSideProps {count} increase={increase}/>
}
```

If you call the `increase` function from the client, the server throws an error which you can handle in your client.
The error will be cleared after a successful call to a function was made.

```tsx
const HelloWorldExample1 = () => {
    const [{count, increase}, {loading, error}] = useComponent('hello-world-1');

    return <>
        {error && <Alert severity="error">{error.message}</Alert> }
        <Button onClick={() => increase()} />
    </>
}
```