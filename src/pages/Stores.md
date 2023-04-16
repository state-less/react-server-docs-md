# Stores

As mentioned earlier, stores provide an abstraction for storing states in various locations. They use a combination of a _key_ and a _scope_ to uniquely identify states within a store.

A _scope_ is a simple string passed into the options of a useState call, allowing for a straightforward yet powerful approach to manage state.

Each state is typically scoped to the component it resides in (with the scope derived from the component's unique key). This enables different components to use a state with the same key without any collisions.

## Scope

### Special Scopes

There are a few special scopes

- `Scopes.Client` - This enum represents a unique scope for each connecting client. It enables serving components with states that are unique to individual clients. In other words, each client sees their own version of a component that utilizes states scoped to the client.
- `Scopes.Component` - This enum scopes a state to its containing component, ensuring that its key will not collide with a state with the same key used in another component. This is the **default** scope.
- ~~`Scopes.User`~~ - This enum refers to another special scope for the currently authenticated user, making it extremely easy to deliver applications where each user sees their own version of the app. _Note: NYI_

Any other scope is simply a string and is globally available. This means that two components can share the same global state.

## Databases / Transportation

Currently, only an InMemory Store is available. Transportation of states to databases will be implemented soon.

### In Memory

By default, all states are stored in memory without persistence. This means that all states are lost after a server restart.

If you want to use React Server now and require states that persist across restarts, you would need to create your own Store or wait for the upcoming implementation.

### Postgresql (NYI)

This allows you to store your states in a Postgres database.

### Redis (NYI)

Redis will also be supported out of the box.

### DynamoDb (NYI)

As React Server aims to run fully serverless on AWS Lambda, you can of course use DynamoDb to store your states.

## Atomic States (NYI)

Some scenarios require you to have atomic writes to your database, such as multiple users increasing the count of a single state simultaneously.

If your database supports atomic writes, React Server can infer the operation from the updated and the current state, by providing it with an equation.

## _Store_

```tsx
const store = new Store(options);
const state = store.createState(0, { key: "key", scope: "scope" });
const hasState = store.hasState({ key: "key", scope: "scope" });
```


_store_ (the return value of `new Store()`)
| Property | Description |
|--|--|
|`_scopes: Map<string, Map<string, State<unknown>>>` | A map of states, in case you need to get all states of the same scope.  
|`_states: Map<string, State<any>>` | A flat map of all states. IDs are a concatenation of _scope_ and _key_.  
|`_options: StoreOptions` | Options passed to the store (NYI).
| createState(defaultValue: any, options: CreateStateOptions) | Creates a new state in the store under the specified key and scope |
| hasState(options: HasStateOptions) | Creates a new state in the store under the specified key and scope |

_StoreOptions_  
There are currently no options. (NYI)

_CreateStateOptions_   
see [/states](/states)

_HasStateOptions_
| Argument | Description |
|--|--|
|key|The key under which the state will be saved|
|scope|The scope under which the state will be saved. It's a convention to nest scopes with a dot. e.g. 'global.foo.bar'|


----
_state_ (the returnvalue of `store.createState`)  
see [/states](/states)
