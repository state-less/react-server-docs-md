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