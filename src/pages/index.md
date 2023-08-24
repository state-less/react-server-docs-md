## Why React Server?

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
Components on the server side.

```tsx
const server = <Server>
  {/* your components here */}
</Server>; 
```

Creating your own components is straight forward. This is the code that powers the button below.

```tsx
import { Scopes, useState, clientKey } from '@state-less/react-server';
import { ServerSideProps } from './ServerSideProps';

export const HelloWorldExample2 = () => {
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
