## Dynamic, Reactive, Full-Stack Framework

A cutting-edge framework designed for crafting highly responsive full-stack services. Leveraging the renowned syntax of [React](https://react.dev/), this innovative solution seamlessly merges frontend familiarity with powerful server-side capabilities.

With React Server, you can construct server-side components using JSX/TSX, blending declarative structures with a reactive lifecycle—a hallmark of the beloved React library. Harnessing TSX under the hood, you can effortlessly embrace a component-driven approach on the server side.

Key Features:

- **GraphQL Integration**: Utilize GraphQL for dependable transport and a modern API interface, enhancing data exchange efficiency and flexibility.
- **Real-Time Updates**: Employ PubSub for real-time state updates, ensuring seamless synchronization across all client applications.
- **Modular Architecture**: Embrace inherent modularity, simplifying the creation of a robust ecosystem of reusable components—a foundation for scalable and adaptable projects.

Experience the future of full-stack development with React Server—where innovation meets efficiency, and frontend prowess extends seamlessly to the server side.

**Effortlessly Prototype Advanced Full-Stack Services with Minimal Complexity. Get Started Today!**

## [Why React Server?](/why)

React's strength lies in its ability to create maintainable and scalable components, making it a cornerstone of large-scale projects. Its declarative nature enhances readability, while its component-driven approach naturally fosters modular architecture and facilitates decoupling. Additionally, its reactive lifecycle ensures a streamlined flow of data, fostering clean architectural designs.

This proven principle has solidified React's position as a state-of-the-art solution for single-page applications (SPAs), with frequent production deployments attesting to its effectiveness. Its reputation for crafting reusable, high-quality components is well-established.

Drawing from the success of component-driven development in the frontend realm, React Server extends the flexibility of reactive programming to the backend environment.

Benefits of React Server:

- **Developer Experience**: Its component-based nature offers an exceptional developer experience. Enjoy the power of familiar hooks like useState and manage server-side component lifecycles effortlessly using concepts like useEffect.

- **Declarative Approach**: React Server enables you to prototype sophisticated server-side logic using a declarative approach powered by TSX, enhancing productivity and code clarity.

- **Real-Time Updates**: By utilizing the same lifecycles and component structure on both backend and frontend, React Server mitigates the waterfall problem and provides real-time state updates seamlessly across all subscribed clients.

- **Effortless Data Handling**: Forget about data transportation or fetching concerns. Simply render props on the server and consume them on the client with the straightforward useComponent hook.

- **Seamless Integration**: React Server bridges the gap between server and client in the React ecosystem, delivering a unified interface for a component-driven architecture and offering a real-time full-stack experience.

For those accustomed to microservices, React Server offers a compelling solution. Components or services can be seamlessly plugged into any React Server instance, facilitating code reuse across multiple platforms. Develop a service once and run it on any Node.js server, simplifying deployment and enhancing scalability.

Experience the seamless synergy between frontend and backend development with React Server, empowering you to create dynamic and responsive applications with ease.

### Passionate about React? You're going to adore React Server. Dive in now and unlock the seamless creation of cutting-edge, dependable full-stack applications.

Maximize developer efficiency by accelerating the delivery of robust full-stack services. Designed for rapid prototyping of intricate business processes, React Server doubles as a platform for open-source services, fostering an expansive ecosystem of components for universal enjoyment.

### Next.js

React Server is _not_ a replacement for [Next.js](/faq). You can combine it, to have realtime server authoritative fullstack apps with [SSR](/SSR).

### React Server

#### What is React Server?

- TSX on the backend
  - React's syntax
  - Declarative
  - Modular
  - Composable
- Component driven development
  - Modular and declarative fullstack microservices
  - Deliver frontend _and_ backend code
  - Consume _components_ instead of REST APIs
- Reactive coding style
  - Synchronous _hooks_ abstract complex async / await patterns.
  - Linear semantic flow of data / states
  - Lifecycle managed by _effects_ and _states_
  - _States_ are externalized to a relational database.
- Bridging the gap between server and client
  - Render props on the _server_, access them on the _client_.
  - Seamless _function_ invocation.  
    **serverside** functions are callable from the client
- A component based abstraction layer on top of GraphQL
  - Consume reactive components instead of arbitrarily shaped data
  - Utilizes PubSub for realtime state updates
- A reactive database interface
- Rapid prototyping

You probably wonder how components on the server side look like. That's easy: Just as they do on the Frontend. If you're familiar with _React_ you should immediately notice the similarities.

```tsx
const server = <Server>{/* your components here */}</Server>;
```

Creating your own components is straight forward. This is the code that powers the button below.

```tsx
import { Scopes, useState, clientKey } from "@state-less/react-server";
import { ServerSideProps } from "./ServerSideProps";

export const HelloWorldExample = (props, { key, context }) => {
  // The useState hook looks familiar?
  const [count, setState] = useState(0, {
    key: "count",
    scope: Scopes.Global,
  });

  // A simple function that can be executed from the client side.
  const increase = () => {
    setState(count + 1);
  };

  return (
    // Simply pass down props to the client side.
    <ServerSideProps
      key={clientKey(`${key}-props`, context)}
      count={count}
      increase={increase}
    />
  );
};
```
