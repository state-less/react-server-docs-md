# Components

In React Server, server-side components serve as reusable building blocks on the server, capable of rendering on the client. While bearing some similarities to front-end React components, they exhibit key distinctions.

Server-side components offer the ability to abstract common functionalities—like authentication, database access, or other backend logic—ideal for reuse across multiple projects. Typically defined as plain JavaScript functions returning React Server elements, they excel in handling server-side concerns such as data loading, making API requests, database interaction, or user authentication.

When a server-side component is rendered on the client, it receives the props defined on the server. These props serve to transmit data to the component or provide functions callable from the client to modify server-side state.

The essence of server-side components lies in simplifying the construction of modular, maintainable backends using familiar React syntax and patterns.

## Use Cases

In React Server, server-side components represent a unique breed of React components capable of execution and rendering on both server and client sides. Crafted using TypeScript/TSX, these components seamlessly bridge the gap between server and client, offering effortless sharing between the two realms.

These server-side components not only contribute to swifter page loads but also enhance search engine optimization efforts. They elevate the concept of server-side rendering by empowering developers to define components that execute on the server and subsequently undergo further manipulation on the client.

Employing TSX for server-side logic definition fosters the creation of modular, declarative code, highly reusable throughout the application. This approach proves especially beneficial for intricate applications housing numerous components. Moreover, server-side components streamline server-side logic implementation, reducing the need for extensive boilerplate code and ultimately saving developers valuable time.

In essence, server-side components empower developers to craft reusable components executable across server and client environments. Leveraging TSX for server-side logic definition facilitates the development of modular, reusable, and declarative code.

## Lifecycle

```github
https://raw.githubusercontent.com/state-less/react-server-docs-md/master/src/fragments/lifecycle.md
```

### Creation

Components are instantiated by rendering them using TSX syntax, such as `<Component prop="value" />`. Initially, when a component is created, little occurs. However, the true magic unfolds when you render the component on the server utilizing the render method from `@state-less/react-server`. This pivotal step unleashes the potential to employ hooks like `useState` and `useEffect`.

### Rendering

When a server-side component is rendered, it undergoes initial execution on the server. Subsequently, the server renders a `<ServerSideProps />` component, transmitting all passed props to the client. This process seamlessly integrates server-side code execution within frontend contexts, including functions, thereby enhancing the versatility of server-side components in React.

Upon rehydration on the client, any functions passed to the component are mapped to callable functions, executing the server-side function defined within the component. This mechanism enables the client to invoke server-side functions, thereby facilitating modifications to server-side state.

It's important to highlight that server-side components not only define data but also encapsulate the operations permissible on that data. Consequently, any logic required to alter the state of the component should be encapsulated within the component itself.

This approach empowers the development of fully server-authoritative applications, necessitating minimal client-side logic. While client-side logic remains possible, it's not imperative for creating a fully server-authoritative application.

### Destruction

Dynamic creation of components, such as mapping an array to components, is a common practice in React. However, rendering components dynamically can result in lingering states if not properly managed.

Consider a scenario like a comments section on a webpage. When an admin deletes a comment, the associated up and downvotes for that comment become obsolete. This is a situation that React Server doesn't handle automatically. Without a garbage collection mechanism, it's challenging to predict when a state might be needed again. Therefore, it's essential to actively delete states once they're no longer required.

Fortunately, you don't have to dispose of individual states manually. You can simply destroy a component, and it will remove any states created within its context. Destroying a component will effectively eliminate all associated states for all clients.

If you're utilizing an in-memory store, keep in mind that the value of the state is lost and cannot be recovered upon destruction.

To facilitate this process, you can use the destroy function exposed by @state-less/react-server. Calling this function will dismantle the current component. However, if you later render the same component again, all states will be recreated with their initial values. Any prior state values will be lost, even if the component shares the exact same key.

In summary, server-side components provide a robust mechanism for creating dynamic, interactive web applications. They enable client interaction with server-side data and logic seamlessly through React, offering a unified isomorphic interface.

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

`ServerSideProps` stands as a pivotal utility within React Server, facilitating the smooth transmission of server-side data or functions to client-side components. Serving as a wrapper around the server-side component, it simplifies the transfer of essential data and functions, ensuring seamless consumption on the client-side.

To leverage ServerSideProps, the standard approach involves crafting a server-side component that yields an instance of ServerSideProps, equipped with the necessary data and functions as props. These props undergo serialization and are subsequently dispatched to the client-side component. This enables effortless access and utilization, mirroring the familiar behavior of regular React components.

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

This example will pass both the value of the variable state as well as the function to update that state down to the client, where it can be consumed using the `useComponent` hook of _@state-less/react-client_.

### Manipulating Server State

In most cases, there's a need to modify the state of a server-side component from the client-side. This can be seamlessly accomplished by passing a function as a prop to the `ServerSideProps`.

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

In contrast to the previous example where the client could store an arbitrary value using the `useServerState` hook, this example distinctly outlines the permissible operations on the `count` state.

With the `setValue` function shielded from the client, the primary interface for interacting with the count state from the client's standpoint remains the `increase` function. This function singularly increments the count by one, providing a clearly defined operation on the state.

_backend/src/index.tsx_

```tsx
import { render } from "@state-less/react-server";
import { HelloWorld } from "./components/HelloWorld";

const server = render(<HelloWorld key="hello-world" />);
```

The showcased example above presents a component responsible for managing the state count and furnishing a function dubbed "increase," which the client can invoke to increment the count.

Despite the appearance of continuous server-side operation, the component does not persistently run on the server. Instead, it executes solely when rendered on the client. This setup grants the client the capability to invoke the "increase" function, thereby effecting modifications to the component's state.

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
const [props, { loading, error }] = useComponent(key, options);
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
|`data?` | Will be hydrated when passed. Passed data is synchronously available and the client will subscribe to further updates omitting the initial fetch.
