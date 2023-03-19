# Stores

As previously noted, stores abstract the mechanism of storing states in different locations. 

They use a *key* + *scope* identifier to uniquely identify states in a store.

Where *scope* is a simple string passed into the options of a `useState` call.

## Scope

Scoping is a simple yet powerful tool that enables you to do a lot of neat things.

Each state is usually scoped to the component it's contained in (the scope is derived from the components unique key). This allows different components to use a state with the same key without them colliding.


### Special Scopes

There are a few special scopes

* `Scopes.Client` - This enum is a special scope that gets replaced by the connecting clients unique id.
This makes it straightforward to serve components with states that are unique to each client. In other words, every client see their own version of a component that using states scoped to the client.
* `Scopes.Component` - This enum scopes a state to its containing component. Which means that its *key* won't collide with a state with the same key used in another component. This is the **default** scope.
* ~~`Scopes.User`~~ - This enum is yet another special scope and refers to the currently *authenticated* user. This makes it super easy to deliver applications where each user sees their own version of your app. *Note: **NYI***

Every other scope is simple a string and is globally available. This means that two components can share the same global state.

## Databases / Transportation

Currently there is only an InMemory Store. Transportation of states to databases will come soon.

### In Memory
The default operation is to store all states in memory without persisting them. This means that all states are gone after a server restart.

If you want to use React Server now and need states that persist over restarts, you would need to write your own Store or wait for the implementation.
### Postgresql (NYI)
This allows you to store your states in a Postgres database. 
### Redis (NYI)
Redis will also be supported out of the box.
### DynamoDb (NYI)
As React Server aims to run fully serverless on AWS Lambda, you can of course use DynamoDb to store your states.

## Atomic States (NYI)
Some scenarios require you to have atomic writes to your database, such as multiple users increasing the count of a single state simultaneously. 

If your database supports atomic writes, React Server can infer the operation from the updated and the current state, by providing it with an equation.

## *Store*
```tsx
const store = new Store();
const state = store.createState(0, {key: 'key', scope: 'scope'});
const hasState = store.hasState({key: 'key', scope: 'scope'});
```
*store*
| Argument    | Description |
|--|--|
|`_scopes: Map<string, Map<string, State<unknown>>>`    | A map of states, in case you need to get all states of the same scope.  
|`_states: Map<string, State<any>>` | A flat map of all states. IDs are a concatenation of *scope* and *key*.  
|`_options: StoreOptions` | Options passed to the store (NYI).  

*StoreOptions*  
There are currently no options. (NYI)