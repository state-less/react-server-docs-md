# States

States are the atomic unit of React Server. 
They are the building blocks of your backend components, and they can be used to create complex services with minimal code. 

## States on the server
States live on the server and are created using the `createState` method of the `Store` class.

### Authentication and Authorization
States do *not* handle authentication or authorization. To constraint access to a state you need to use a `Component` that handles who and how someone can operate on a state.

### Transport
In react-server v2, transportation of states is handled by GraphQl / Apollo. This makes it easier to build a robust framework by building on a proven solution, rather than implementing our own data transportation.

#### Reactivity
React Server uses `PubSub` to notify clients about changes to states. This allows clients to react to changes in states, without having to poll the server for changes.

### States vs. Data
It's _worth noting_ that `useServerState` should *not* be seen as a replacement for a database. Even though you could do so to some extent, you shouldn't. You wouldn't use `useState` to build a database in your client either. You should see `useServerState` as an extension of `useState` to the server side. It provides you with a state that lives on the server instead of the client. 

It's up to your imagination what use it for. Using a state provides you with a live and reactive property that's synchronized across all clients. However imagine you would collect big amounts of form data. Those shouldn't be stored in a state, but rather in a database. Why? Because it's difficult to query them. They are stored in a way that makes it easy to access and display it in a client, but in order to process and analyse the data later you would have to write complex queries. In a usual React Server application, data in states are usually tied to components which means the structure in the database is similar to the structure of your application which makes it easy to query data for display, but hard to query data for further manipulation or processing.

Thus it's recommended to use a traditional database to store data you will later need to access outside the scope of your application. If the data never leaves your react server application you can as well keep it in a state if it suits.

You can also query data from a database on the serverside and keep a copy of the data in a state in order to make it reactive, but you would need to implement the serverside logic to check the database for changes. e.g. using DynamoDb streams or Postgres triggers. You can also poll the database on the server and update the state with new data when it changed, but there are better techniques to make your data reactive.
## Stores

States are stored in `Stores`, which are responsible for persisting and retrieving states.
Externalizing the states into a database, allows your backend or BFF to run state-less by fetching the state from a database rather than memory.

This allows your whole backend to run on a serverless infrastructure, as no data needs to be kept in memory and everything can be handled on a per request basis.

To create a new state, you need to instantiate a store first. The store is responsible for persisting and retrieving states.
The store can transport states to any kind of database to persist them. e.g. Redis, Postgres, DynamoDb and so on.

By using a unified interface to access and manipulate states you can swap out the database that handles states at a later point without having to change your implementation.

This provides some benefits, but also some drawbacks.

* It is a very simplified interface to a database and it can't replace one. 
* Writing efficient queries that query data across components can be difficult and slow as it either involves application code or a relational database and custom SQL. 
* It's essentially only a key value store, as such it comes with limitations.

The good thing is, you can (and should) write custom data fetching logic any time you want. If you're creating 

It's thus recommended that you only use states where you need a reactive stateful variable on the server that is synced to clients in realtime. It's not a database and should be not treated as such. 

```
const store = new Store();
const state = store.createState(defaultValue, options);

state.value = 'Hello World';
```
### *Store*
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

### Optimistic

`setValue` calls are implemented optimistic and the client library returns the new value before the request finishes. Ongoing requests are cancelled if `setValue` is called repeatedly and after the last request succeeded, the optimistic value is replaced by the server side value.

### useServerState
```
const [value, setValue, {loading, error}] = useServerState(defaultValue, options);
```  

*useServerState*
| Argument    | Description |
|--|--|
|`defaultValue: any`    | The value of the state on the server, or the default value if the state is not yet available.  
|`options: UseServerStateOptions` | A function that updates the state on the server.  

*ServerState*
| Argument    | Description |
|--|--|
|`value`    | The value of the state on the server, or the default value if the state is not yet available.  
|`setValue` | A function that updates the state on the server.  
|`loading`  | A boolean that indicates if the state is currently loading.  
|`error`    | An error object that indicates if an error occurred while loading the state.

*UseServerStateOptions*
| Property    | Description |
|--|--|
|`key?`    | The key of the state on the server. If omitted a key is generated on the server.  
|`scope?`  | The scope of the state on the server. If omitted, 'global' is used.
