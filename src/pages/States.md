# States

States form the cornerstone of React Server, empowering your backend components with robust functionality. With states, you can effortlessly construct intricate services while keeping your codebase concise and efficient.

## States on the server

States, residing on the server, are effortlessly created through the createState method within the Store class.

### Authentication and Authorization

States alone do not handle authentication or authorization. To control access to a state, you must employ a Component that governs user interaction and permissions.

### Validation

States do not inherently include validation capabilities. To validate user input or function calls, Components responsible for managing such processes should be utilized.

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

This method provides a flexible and intuitive means of imposing constraints on your states.

On the client side, you can utilize the useComponent hook to access the server-side props and invoke the provided setCount method.

In the event of invalid input, the server-side function will throw an error, which can then be managed and handled on the client side.

### Transport

In React Server v2, state transportation is facilitated by GraphQL/Apollo. This strategy leverages a proven solution, simplifying the development process by avoiding the need to implement a custom data transportation system.

#### Reactivity

React Server utilizes `PubSub` to seamlessly notify clients of state changes. This approach enables clients to respond to updates in real-time, eliminating the need for continuous polling of the server.

### States vs. Data

It's crucial to understand that `useServerState` is not designed to function as a replacement for a database. While it might seem tempting to utilize it in such a manner, it's not advisable. Just as you wouldn't rely on `useState` to construct a database on the client side, `useServerState` should be viewed as an extension of `useState` tailored for the server side, offering a state residing on the server rather than the client.

Utilizing a state provides you with a dynamic, reactive property that remains synchronized across all clients. However, when handling substantial amounts of form data, storing such data in a database is imperative. This is because navigating through such data can pose challenges. While data within states are structured for easy access and display within a client, complex queries become necessary for processing and analyzing the data at a later stage. Typically, data within states in a React Server application are linked to components, implying that the database structure reflects your application's architecture, simplifying data retrieval queries but complicating queries for data manipulation or processing.

It's recommended to employ a traditional database for storing data that requires access beyond your application's confines. If the data remains within your React Server application without being transmitted elsewhere, storing it in a state is acceptable as long as it's appropriate.

Alternatively, you can retrieve data from a database on the server side and maintain a copy of the data within a state for reactivity purposes. However, you'll need to implement server-side logic to monitor the database for changes (e.g., using DynamoDB streams or PostgreSQL triggers). Although you can poll the database on the server and update the state with fresh data when alterations occur, there exist more efficient techniques for ensuring your data remains reactive.

## Stores
In React Server, states are stored within Stores, responsible for persisting and retrieving them. By externalizing states into a database, your backend or BFF (Backend for Frontend) can operate in a stateless manner, retrieving state data from a database rather than relying on memory. This strategy enables your entire backend to function on serverless infrastructure, as no data needs to be stored in memory, and each request can be handled independently.

To create a new state, you instantiate a Store. This Store manages the persistence and retrieval of states, capable of transporting them to various databases for persistence, such as Redis, PostgreSQL, DynamoDB, and more. Employing a unified interface for accessing and manipulating states empowers you to change the database handling states in the future without requiring modifications to your current implementation.

However, while this approach offers several benefits, it also presents some drawbacks:

* It provides a simplified interface to a database, but it cannot entirely replace one.
* Crafting efficient queries to access data across components can prove challenging and slow, involving application code, a relational database, and potentially custom SQL.
* Given its nature as a key-value store, inherent limitations are present.

It's important to emphasize that States should not be regarded as a replacement for a database. They are not intended to function as such. Custom data fetching logic should be implemented whenever necessary. It's advisable to utilize States only when a reactive, stateful variable on the server that synchronizes with clients in real-time is required.

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

States are accessed by the client through the useServerState hook, which accepts a default value and an options object as arguments. This hook closely resembles React's useState hook, with the only distinction being the options object.

### Optimistic Updates

When setValue is invoked, optimistic updates are employed, allowing the client library to return the new value before the request completes. If setValue is called multiple times, ongoing requests are canceled. Upon the successful completion of the final request, the optimistic value is replaced with the server-side value. This approach guarantees a responsive user experience while upholding data consistency.

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
