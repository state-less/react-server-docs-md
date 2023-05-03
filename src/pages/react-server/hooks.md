
# React Server Hooks
Hooks are a powerful addition to React, enabling developers to reuse stateful logic across components without creating complex class components. Hooks allow developers to "hook" into the React lifecycle and state management while working with functional components.

In React Server, hooks work in a similar manner, but with a key difference. They are designed to work specifically within the context of server-side React components, addressing the unique challenges and requirements of server-side component development. The hooks provided by React Server enable developers to manage server-side component state and lifecycle while leveraging the flexibility and reusability of functional components.

## Using Hooks in React Server
React Server provides a set of custom hooks to help you manage state and lifecycle in your server-side React components. These hooks enable you to write clean, functional components without the need for class components or complex state management libraries.

To use hooks in your React Server components, simply import them from the @state-less/react-server package, and call them within your component functions. Note that you can only use hooks within functional components, not class components.

Here is an overview of the hooks provided by React Server:

`useState`: A hook that allows you to manage server-side state within your component. It works similarly to React's useState, but specifically designed for server-side components.

`useEffect`: A hook that allows you to perform side effects, such as fetching data, in your server-side components. It works similarly to React's useEffect. It runs everytime a component is rendered on the server side in the context of the server.

`useClientEffect`: A hook that allows you to perform side effects, such as fetching data, in your server-side components. It works similarly to React's useEffect. It runs everytime a component is rendered on the server side in the context of a client.

`useContext`: A hook that allows you to access the context of a Provider somewhere up the tree. This is useful passing data to arbitrary children and for managing global state.

These hooks provide the foundation for developing server-side components in React Server. By using these hooks, you can build powerful, reusable components that can be easily integrated into your full-stack application.

Remember that hooks provided by React Server are specifically designed for server-side components and should not be used interchangeably with standard React hooks in client-side components. Always ensure that you are using the appropriate hooks for the context of your components.

## useState

| Signature                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| useState(defaultValue: any, options: CreateStateOptions) | Creates a new state in the store under the specified key and scope |

### CreateStateOptions
| Fields    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| key | The unique key of the state.
| scope | The scope of the state. |

## useEffect

| Signature                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| useEffect(fn: () => void, deps: any[]) | Schedules a side-effect that runs when the server renders the component in server context but only if a dependency changed. |
| fn: () => void | Any callback function. | 
| deps: any[] | An array of dependencies to monitor for changes in respect to the prior run.|

## useClientEffect (NYI*)

| Signature                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| useEffect(fn: () => void, deps: any[]) | Schedules a side-effect that runs when the server renders the component in server context but only if a dependency changed. |
| fn: () => void | Any callback function. | 
| deps: any[] | An array of dependencies to monitor for changes in respect to the prior run.|

## useContext

| Signature                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| useContext(context: ReactServerContext) | Consumes a provided context. |
| context: ReactServerContext | The context that has been created with [`createContext`](/react-server). It must provide a `<context.Provider>` higher up in the tree |
