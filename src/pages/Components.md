# Components

Server-side components are a concept in React Server. In this context, server-side components are reusable building blocks that are defined on the server and then rendered on the client.

The idea is similar to front-end React components, but with a few key differences.

They can be used to abstract away common functionality that you might want to reuse across multiple projects, such as authentication or database access, common microservices, or any other backend side logic.

In terms of implementation, server-side components are typically defined as plain JavaScript functions that return React Server elements (JSON objects).

These functions can use various techniques to handle data loading and other server-side concerns, such as making API requests, fetching data from a database, or authenticating a user.

When a server-side component is rendered on the client, it receives the props defined on the server. These props can be used to pass data to the component, or to pass functions that can be called from the client to modify server-side state.

The goal of server-side components is to make it easier to build modular, maintainable backends using familiar React syntax and patterns.

## Use Cases

In the context of React Server, server-side components are React components that can be executed and rendered on the server-side as well as on the client-side. These components are defined using TypeScript/TSX and can be easily shared between the client and the server.

The use case for server-side components is similar to that of server-side rendering, in that it allows for faster page loads and better search engine optimization. However, server-side components take this concept one step further by allowing you to define components that can execute on the server, and then be passed to the client for further manipulation.

By using TSX to define server-side logic, you can write modular and declarative code that can be easily reused across your application. This can be especially useful when building complex applications with many moving parts. Additionally, server-side components can help to reduce the amount of boilerplate code needed to write server-side logic, which can be a significant time-saver for developers.

In summary, the use case for server-side components is to create reusable components that can be executed on the server and passed to the client for further manipulation. Using TSX to define server-side logic allows for modular and declarative code that can be easily shared and reused throughout your application.

## Lifecycle

When you render a server side component, the component is first executed on the server. The server can render a `<ServerSideProps />` component which is used to send all the props passed to it to the client and make them available in frontend code.

When the component is rehydrated on the client, any functions that were passed will be mapped to callable functions that will call the function defined in the component on the serverside. This makes it a seamless experience to execute server side code from the client.

This means that any function defined on the server and passed as a prop can be called from the client, allowing the client to modify server-side state by invoking server-side functions.

It's important to note that server-side components not only define the data, but also the operations that can be performed on that data.

This means that any logic that is needed to modify the state of the component should be defined within the component itself.

Using this approach you can write fully server authoritative applications that require little to no client side logic. (This is not to say that you can't write client side logic, but it's not necessary to write a fully server authoritative application.)

Server-side components provide a powerful way to create dynamic, interactive web applications by allowing the client to interact with server-side data and logic using `<React />` as a seamless isomorphic interface.

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
