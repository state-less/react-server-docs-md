# States

States are the atomic unit of React Server. 
They are the building blocks of your backend components, and they can be used to create complex services with minimal code. 

## States on the server
States live on the server and are created using the `createState` method of the `Store` class.

### Stores
States are stored in `Stores`, which are responsible for persisting and retrieving states.
Externalizing the states into a database, allows your backend to run state-less, by fetching the state from a database rather than memory.

This allows your whole backend to run on a serverless infrastructure, as no data needs to be kept in memory and everything can be handled on a per request basis.

The most basic scenario is a single `State` instantiated on the server. 
This state can can be exposed to and consumed by a client.

By consuming a *state*, the clients gain access to a `setValue` function which allows them to update the state with an arbitrary value.

### Authentication and Authorization
States do *not* handle authentication or authorization. To constraint access to a state you need to use a `Component` that handles who and how someone can operate on a state.

### Transport
In react-server v2, transportation of states is handled by GraphQl / Apollo. This makes it easier to build a robust framework by building on a proven solution, rather than implementing own data transportation mechanisms.

#### Reactivity
React Server uses `PubSub` to notify clients about changes to states. This allows clients to react to changes in states, without having to poll the server for changes.

### `Store`
To create a new state, you need to instantiate a store first. The store is responsible for persisting and retrieving states.
```
const store = new Store();
const state = store.createState(defaultValue, options);

state.value = 'Hello World';
```
Store
| Argument | Description |
|--|--|
|createState(defaultValue: any, options: CreateStateOptions)|Creates a new state in the store under the specified key and scope|

CreateStateOptions
| Argument | Description |
|--|--|
|key|The key under which the state will be saved|
|scope|The scope under which the state will be saved. It's a convention to nest scopes with a dot. e.g. 'global.foo.bar'|

## States on the client
States are consumed by the client using the `useServerState` hook, which takes a default value and an options object as arguments. The hook is very similar to the `useState` hook from React and only differs in the options object.

### `useServerState`
```
const [value, setValue, {loading, error}] = useServerState(defaultValue, options);
```  

| Argument    | Description |
|--|--|
|`value`    | The value of the state on the server, or the default value if the state is not yet available.  
|`setValue` | A function that updates the state on the server.  
|`loading`  | A boolean that indicates if the state is currently loading.  
|`error`    | An error object that indicates if an error occurred while loading the state.