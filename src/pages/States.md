# States

States serve as the fundamental unit of React Server, acting as the backbone of your backend components. They enable you to build complex services with minimal code.

## States on the server

States reside on the server and are created using the createState method from the Store class.

### Authentication and Authorization

States, by themselves, do not handle authentication or authorization. To restrict access to a state, you must use a Component that manages who and how someone can interact with a state.

### Validation

States do not inherently provide validation. To validate user input or function calls, you need to utilize Components that handle these validation processes.

```tsx
const MyComponent = () => {
    const [count, _setCount] = useState(0);

    const setCount = (n) => {
        if (typeof n !== 'number') {
            throw new Error('Invalid input');
        }
        _setCount(n);
    }

    return <ServerSideProps
        count={count}
        setCount={setCount}
     >
}
```

This approach offers a flexible and intuitive way to impose constraints on your states.

On the client side, you would utilize the `useComponent` hook to obtain the server-side props and call the passed `setCount` method.

If the input is invalid, the server-side function will throw an error, which can be handled on the client side.

### Transport

In react-server v2, state transportation is managed by GraphQL / Apollo. This approach builds on a proven solution, making it easier to develop a robust framework instead of implementing a custom data transportation system.

#### Reactivity

React Server employs `PubSub` to notify clients about changes to states. This allows clients to respond to state changes without having to poll the server for updates.

### States vs. Data

It's important to keep in mind that use `useServerState`  is not intended to replace a database. While it may seem like a good idea to use it as such, it is not recommended. Just as you wouldn't rely on `useState` to create a database on the client side, `useServerState` should be thought of as an extension of `useState` for the server side, providing a state that exists on the server rather than the client.

Using a state provides you with a live, reactive property that is synchronized across all clients. However, when dealing with large amounts of form data, it's important to store that data in a database rather than a state. This is because querying such data can become challenging. Data in states are structured in a way that makes it easy to access and display in a client, but complex queries are needed for processing and analyzing the data later. Typically, data in states within a React Server application are tied to components, meaning the database structure mirrors your application's structure, simplifying data display queries but complicating data manipulation or processing queries.

It is recommended to use a traditional database to store data that you need to access outside of your application's scope. If the data never leaves your React Server application, you can store it in a state as long as it is appropriate.

You can also query data from a database on the server side and maintain a copy of the data in a state for reactivity. However, you would need to implement server-side logic to check the database for changes (e.g., using DynamoDB streams or PostgreSQL triggers). Although you can poll the database on the server and update the state with new data when it changes, there are more efficient techniques available for making your data reactive.

## Stores
States in React Server are housed in Stores, which are responsible for persisting and retrieving states. By externalizing states into a database, your backend or BFF can operate in a stateless manner, fetching the state from a database instead of relying on memory. This approach enables your entire backend to run on serverless infrastructure, as no data needs to be stored in memory and every request can be handled individually.

To create a new state, you need to instantiate a Store. The Store persists and retrieves states and can transport them to various databases for persistence (e.g., Redis, PostgreSQL, DynamoDB, etc.). Utilizing a unified interface for accessing and manipulating states allows you to change the database handling states later without altering your implementation.

However, this approach comes with some benefits as well as drawbacks:

* It provides a simplified interface to a database, but it cannot replace one.
* Crafting efficient queries to access data across components can be challenging and slow, as it may involve application code, a relational database, and custom SQL.
* As it is essentially a key-value store, it comes with inherent limitations.

It's important to note that States should not be mistaken for a database and should not be treated as such. You can (and should) write custom data fetching logic whenever necessary. It's recommended to use States only when you require a reactive, stateful variable on the server that syncs with clients in real-time.

```tsx
const store = new Store();
const state = store.createState(defaultValue, options);

state.value = "Hello World";
```

### _Store_

| Argument                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| createState(defaultValue: any, options: CreateStateOptions) | Creates a new state in the store under the specified key and scope |

CreateStateOptions
| Argument | Description |
|--|--|
|key|The key under which the state will be saved|
|scope|The scope under which the state will be saved. It's a convention to nest scopes with a dot. e.g. 'global.foo.bar'|

## States on the client

States are consumed by the client using the `useServerState` hook, which takes a default value and an options object as arguments. The hook is very similar to the `useState` hook from React and only differs in the options object.

### Optimistic Updates

`setValue` calls employ optimistic updates, where the client library returns the new value before the request completes. If `setValue` is called repeatedly, ongoing requests are canceled. Once the final request succeeds, the optimistic value is replaced with the server-side value. This approach ensures a responsive user experience while maintaining data consistency.

### useServerState

```tsx
const [value, setValue, { loading, error }] = useServerState(
  defaultValue,
  options
);
```

_useServerState_
| Argument | Description |
|--|--|
|`defaultValue: any` | The value of the state on the server, or the default value if the state is not yet available.  
|`options: UseServerStateOptions` | An object with `key` and `scope`.

_ServerState_
| Argument | Description |
|--|--|
|`value` | The value of the state on the server, or the default value if the state is not yet available.  
|`setValue` | A function that updates the state on the server.  
|`loading` | A boolean that indicates if the state is currently loading.  
|`error` | An error object that indicates if an error occurred while loading the state.

_UseServerStateOptions_
| Property | Description |
|--|--|
|`key?` | The key of the state on the server. If omitted a key is generated on the server.  
|`scope?` | The scope of the state on the server. If omitted, 'global' is used.
