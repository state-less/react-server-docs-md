## [Why React Server?](/why)

The main benefit of React is to write highly maintainable and *reusable*  codebase which can be scaled to very large projects. It's component driven approach naturally imposes a modular architecture which aids decoupling and modularization while the reactive coding style favors readability and a clean architecture.

This principle has been proven in the real world by React being a state of the art solution to SPA's being deployed in production ever so often. Based on experience it has been an excellent tool in crafting re-usable high quality components. 

By adapting a successful approach to component driven development from the frontend world, React Server brings the some of the flexibility of reactive programming.

It's component based nature allows you to enjoy promotes developer experience. 
Enjoy the power of hooks such as  `useState` and manage serverside lifecycle of components using the familiar concept of effects such as `useEffect`.

React Server is declarative. It's a tool that allows you to easily prototype sophisticated server-side logic using a declarative approach powered by TSX.

A side effect of using the same lifecycles and component hierarchy is that you get realtime state updates out of the box. If one client mutates the state on the server the component rerenders and publishes the rerendered component to all subscribed clients.

You don't need to worry about data transportation or fetching *at all*. Just render *props* on the server and consume them on the client with a simple `useComponent` hook. 

Did we mention that you can even pass function references and call serverside functions on the frontend as if they were right there? 

React Server *bridges the gap* between server and client in the react world, bringing you a real fullstack experience with a unified interface towards a component driven architecture. 

If you are used to microservices, check out React Server. Components / Services can be plugged into any React Server instance allowing you to reuse your backend code in multiple platforms.

The magic is mostly in behind the  `useState` hook which makes it exceptionally simple to seperate data operations by user. It's as simple as including a unique *user ID* in the `scope` parameter and the same *component* renders a seperate *view* into your data based on the currently logged in user. 

If you love React you will also love React Server. Try it now and discover how easy it is to build modern and reliable fullstack applications.
### Next.js
React Server is not a replacement for [Next.js](/faq). You can combine it, to have realtime server authoritative fullstack apps with SSR for better SEO.

### React Server
#### What is React Server?
* TSX on the backend
  * React's syntax
  * declarative
  * modular
  * composable
* Component driven development
  * modular and declarative fullstack microservices
  * deliver both, frontend and backend code using components as API
* Reactive coding style
  * synchronous *hooks* abstract complex async / await patterns. 
  * linear semantic flow
  * lifecycle managed by *effects*
  * *states* are externalized to a database.
* Bridging the gap between server and client
  * seamless transport of serverside *props*. 
  * seamless *function* invocation. **serverside** functions are callable from the client
* A component driven abstraction layer over GraphQL
  * consume components instead of arbitrarily shaped data
  * utilizes PubSub for realtime state updates
* A reactive database interface
* Rapid prototyping



You probably wonder how components on the server side look like. That's easy. Just as they do on the Frontend. If you're familiar with *React* you should spot the similarities immediately.

```tsx
const server = <Server>
  {/* your components here */}
</Server>; 
```

Creating your own components is straight forward. This is the code that powers the button below.

```tsx
import { Scopes, useState, clientKey } from '@state-less/react-server';
import { ServerSideProps } from './ServerSideProps';

export const HelloWorldExample2 = (props, { key, context })  => {
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
