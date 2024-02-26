# Scoped Stores

For seamless state management across various locations, stores provide a convenient solution, utilizing unique keys and scopes. Scopes, passed as options to a useState call, enable efficient state management in a straightforward manner.

By assigning each state to its respective component via the component's unique key, stores prevent state collisions and enable multiple components to share the same state key.

In essence, stores offer an efficient mechanism for managing state across different locations, employing unique keys and scopes to maintain separation between each component's state.

### Special Scopes

There are a few special scopes

- `Scopes.Client` - This enum represents a unique scope for each connecting client. It enables serving components with states that are unique to individual clients. In other words, each client sees their own version of a component that utilizes states scoped to the client.
- `Scopes.Component` - This enum scopes a state to its containing component, ensuring that its key will not collide with a state with the same key used in another component. This is the **default** scope.
- ~~`Scopes.User`~~ - This enum refers to another special scope for the currently authenticated user, making it extremely easy to deliver applications where each user sees their own version of the app. _Note: NYI_

Any other scope is simply a string and is globally available. This means that two components can share the same global state.

## Databases / Transportation

At Present, Only an InMemory Store is Available. Integration with Databases for State Transportation is Under Development.

### In Memory

By default, React Server stores all states in memory, without persistence. Consequently, all states are lost upon server restarts.

For those requiring persistent states across restarts, you have two options: either create your own custom Store or await the forthcoming implementation, which will offer this functionality.

### Postgresql (NYI)

This feature enables you to store your states in a PostgreSQL database.

### Redis (NYI)

Redis support will also be seamlessly integrated.

### DynamoDb (NYI)

As React Server is designed to operate seamlessly on AWS Lambda in a fully serverless environment, you can utilize DynamoDB for storing your states.

## Atomic States (NYI)

In certain scenarios, ensuring atomic writes to your database is crucial, especially when multiple users are simultaneously increasing the count of a single state.

If your database supports atomic writes, React Server can deduce the operation from the updated and current state by providing it with an equation.

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
