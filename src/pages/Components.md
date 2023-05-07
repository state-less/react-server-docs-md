# Components

In React Server, server-side components are a way to create reusable building blocks on the server that can be rendered on the client. They have some similarities with front-end React components, but there are a few key differences.

Server-side components can help you abstract away common functionality that you might want to reuse across multiple projects, such as authentication, database access, or any other backend logic. They are typically defined as plain JavaScript functions that return React Server elements, and they can handle data loading and other server-side concerns, such as making API requests, fetching data from a database, or authenticating a user.

When a server-side component is rendered on the client, it receives the props that were defined on the server. These props can be used to pass data to the component or to pass functions that can be called from the client to modify server-side state.

The goal of server-side components is to make it easier to build modular, maintainable backends using familiar React syntax and patterns.

## Use Cases

In React Server, server-side components are special React components that can be executed and rendered both on the server-side and the client-side. These components are defined using TypeScript/TSX and can be shared effortlessly between the client and server.

Server-side components are useful for achieving faster page loads and improving search engine optimization. They take the concept of server-side rendering one step further, allowing developers to define components that can execute on the server and then be passed to the client for further manipulation.

By using TSX to define server-side logic, developers can create modular and declarative code that is easily reusable across their application. This approach is especially helpful for complex applications with multiple components. Furthermore, server-side components can help reduce the amount of boilerplate code required to write server-side logic, saving developers time.

In summary, server-side components enable developers to build reusable components that can be executed on both the server and client. Using TSX to define server-side logic promotes the creation of modular, reusable, and declarative code.

## Lifecycle

\`\`\`mermaid
sequenceDiagram
participant Browser as Browser
participant Server as Server
participant RSC as React Server
participant SSP as Lifecycle

Browser->>Server: Request component with clientProps
Server->>RSC: Instantiate and render component with clientProps
RSC->>SSP: Render component on the server
RSC->>Server: Return serverSideProps and rendered component
Server->>Browser: Send serverSideProps and rendered component
Browser->>Browser: Render component with serverSideProps
Browser->>Server: Subscribe to component updates

Note over Browser, Server: User interacts with component (e.g. button click)

Browser->>Server: Request execution of server-side function
Server->>RSC: Execute server-side function and re-render component if needed
RSC->>SSP: Update serverSideProps if needed
RSC->>Server: Return updated serverSideProps and rendered component (if updated)
Server->>Browser: Send updated serverSideProps and rendered component (if updated)
Browser->>Browser: Update component with new serverSideProps (if needed)
\`\`\`         
### Creation

Components are created by rendering them using TSX `<Component prop="value" />`. Once a component is created nothing much happens. The magic begins when you render the component on the server using the `render` method of `@state-less/react-server`. Rendering a component allows you to make use of **hooks** such as `useState` and `useEffect`.



### Rendering

When a server-side component is rendered, it's first executed on the server. The server then renders a  `<ServerSideProps />` component that sends all the props passed to it to the client, making them available in frontend code. Including functions which make server-side components in React a seamless experience for executing server-side code from the client.

Once the component is rehydrated on the client, any functions that were passed to it will be mapped to callable functions that execute the function defined in the component on the server-side. This allows any function defined on the server and passed as a prop to be called from the client, enabling the client to modify server-side state by invoking server-side functions.

It's worth noting that server-side components define not only the data but also the operations that can be performed on that data. Therefore, any logic needed to modify the state of the component should be defined within the component itself.

Using this approach, you can write fully server authoritative applications that require little to no client-side logic. While it's possible to write client-side logic, it's not necessary to create a fully server authoritative application.



### Destruction

There are occasions where components are being created dynamically, such as mapping an array to components which is very common in React. 
Rendering components dynamically will lead to dangling states if you don't clean them up.

Suppose you have a comments section like on this page. When an admin deletes a comment, the up and downvotes for the comment are no longer needed. This is something React Server does not automatically take care of. There is no garbage collection mechanism because we can't now when a state might be needed again. So you need to actively delete states once they are no longer needed. 

You don't have to dispose of single states manually, you can destroy a component and it will delete any state that has been created within its context. Destroying a component will delete all states that have been created for any client. 

If you are using an in memory store, the value of the state is lost and can not be recovered.

You can simply call the `destroy` function exposed by `@state-less/react-server` to destroy the current component. Note that you somehow render it again, all states will be recreated with their initial values. Any prior value is lost. Even if the component shares the exact same key.

In summary, server-side components offer a powerful way to create dynamic, interactive web applications. They allow the client to interact with server-side data and logic using React as a seamless isomorphic interface.
## Usage

To render a serverside component, all you need to do is write a function.

_/backend/src/components/HelloWorld.tsx_

```tsx
const HelloWorld = () => {
  useEffect(() => {
    console.log("Hello World");
  });
  return null;
};
```

_index.tsx_

```tsx
import { render } from "@state-less/react-server";
import { HelloWorld } from "./components/HelloWorld";

const server = render(<HelloWorld key="test" />);
```

This example demonstrates a simple component that logs "Hello World" to the console when rendered on the server. It showcases the ease of creating a server-side component. Naturally, you can create more sophisticated components that are rendered on the server and subsequently passed to the client for additional manipulation.

### ServerSideProps
`ServerSideProps` is a utility in React Server that enables you to pass server-side data or functions to client-side components seamlessly. It acts as a wrapper around the server-side component, making it easy to transfer the necessary data and functions to be consumed on the client-side.

To use `ServerSideProps`, you would typically create a server-side component that returns an instance of `ServerSideProps` with the required data and functions as props. These props will be serialized and sent to the client-side component, allowing you to access and use them just as you would with regular React components.

```
import { ServerSideProps, useState} from '@state-less/react-server';

export const MyServerComponent = () => {
  const [state, setState] = useState('foo')
  return (
    <ServerSideProps
      key="my-server-component-props"
      state={state}
      setState={setState}
    />
  );
};
```
This example will pass both the value of the variable state as well as the function to update that state down to the client, where it can be consumed using the `useComponent` hook of *@state-less/react-client*.
### Manipulating Server State

Typically, you'll want the ability to alter the state of a server-side component from the client-side. This can be achieved by passing a function as a prop to the `ServerSideProps`.

_backend/src/components/helloworld.tsx_

```tsx
const HelloWorld = () => {
  const [count, setCount] = useState(0);

  const increase = () => {
    setCount(count + 1);
  };

  return <ServerSideProps count={count} increase={increase} />;
};
```

In contrast to the previous example, where the client could store an arbitrary value using the `useServerState` hook, this example clearly defines the possible operations on the `count` state.

Since the `setValue` function is not exposed to the client, the sole available interface for interacting with the count state from the client's perspective is the `increase` function, which increments the count by one.

_backend/src/index.tsx_

```tsx
import { render } from "@state-less/react-server";
import { HelloWorld } from "./components/HelloWorld";

const server = render(<HelloWorld key="hello-world" />);
```

This example above demonstrates a component that maintains the state count and provides a function called increase that can be invoked from the client to increment the count.

Although it may seem like the component is continuously running on the server, it isn't. The component is executed on the server only when it is rendered (on the client). Allowing the client to call the increase function to modify the state of the component.

_frontend/src/components/HelloWorld.tsx_

```tsx
const HelloWorld = () => {
  const [props] = useComponent("hello-world");

  return (
    <div>
      <h1>{props.count}</h1>
      <button onClick={props.increase}>Increase</button>
    </div>
  );
};
```

## useComponent

```tsx
const [props, {loading, error}] = useComponent(key, options);
```

_useComponent_
| Argument | Description |
|--|--|
|`key: any` | The serverside key of the component.  
|`options: UseComponentOptions` | Additional options.

_Component_
| Argument | Description |
|--|--|
|`props` | The serverside props passed to the `ServerSideProps` component.  
|`loading` | A boolean that indicates if the component is currently loading.  
|`error` | An error object that indicates if an error occurred while loading the component.

_UseComponentOptions_
| Property | Description |
|--|--|
|`key?` | The key of the state on the server. If omitted a key is generated on the server.  
|`scope?` | The scope of the state on the server. If omitted, 'global' is used.
|`data?`  | Will be hydrated when passed. Passed data is synchronously available and the client will subscribe to further updates omitting the initial fetch.
