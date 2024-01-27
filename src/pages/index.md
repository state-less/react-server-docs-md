## [Why React Server?](/why)

The main benefit of React is to implement and maintain a very clean codebase which can be scaled to very large projects. It's component driven approach naturally imposes a modular architecture which aids decoupling and modularization.

React has been proven to be an excellent tool in crafting re-usable high quality components. 

By adapting a successful approach to component driven development from the frontend world, React Server brings the same flexibility and reuseability of the frontend.

It's component based nature and reactive architecture allows you to enjoy the same developer experience as with React on the frontend. 
Enjoy the reactivity of `useState` hooks, manage side-effects using the familiar `useEffect` hooks.

React Server is declarative. It's a tool that allows you to easily prototype sophisticated server-side logic using a declarative approach powered by TSX. 

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
