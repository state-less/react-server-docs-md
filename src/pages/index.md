## Why React Server?

React Server aims to adapt proven concepts of the frontend world to the backend. By introduing React's coding paradigms such as hooks, providers, and components to the backend, it allows you to rethink your backend architecture and pursue a component driven approach with all the benefits it brings. 

A component driven and reactive architecture allows you to build an ecosystem of **reusable** components. It allows you to utilize a unified interface in the form of React components. 

By aligning the component and data structure of your backend and frontend you can simplify data transportation, reactivity, prerendering. You could mitigate the waterfall problem by mirroring the component structure on the backend providing pre computed data matching the frontend structure which can be passed down to the client in a single API call.

React provides means of maintaining a high quality code base with a beautiful architecture. With React on the frontend you usually end up with a clean, readable and maintainable code base.

By using the same techniques on the backend you can unify your development experience and build self suistained reusable packages that contain backend and frontend logic. 
This could help the community to build a rich ecosystem of selfcontained buisiness logic for the server side, ready to be plugged into your running React Server. 

TSX and hooks have proven to be successful and I think what worked on the frontend might also work on the backend.

On a personal note, it's really easy and straightforward to develope with React Server if you already know React. 

You can spin up full-stack applications in no time and you only need one server to host a multitude of services. 

### Next.js
React Server is not a replacement for [Next.js](/faq). You can combine it, to have realtime server authoritative fullstack apps with SSR for better SEO.

### React Server

React Server streamlines development by enabling seamless integration of server and client. This facilitates building server-rendered components that transfer to the client-side for further interaction, ensuring a smooth and **realtime** user experience.

Leveraging *GraphQL and TypeScript*, React Server promotes **reliable, maintainable, and scalable code**. GraphQL simplifies data query and mutation management, while TypeScript's static typing enhances error detection and code readability.

The **component-driven** approach of React Server fosters rapid development of reusable components, promoting consistent design language and reduced duplication. Additionally, **server-side rendering** support enhances app performance and SEO through reduced client-side rendering reliance, improving load times and search rankings.

Discover component driven backend development and leverage the power of TypeScript and JSX to build **your own ecosystem** of reusable components.

```tsx
const server = <Server>
  {/* your components here */}
</Server>; 
```

Creating your own components is straight forward. This is the code that powers the button below.

```tsx
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
      key="hello-world-2-props"
      count={count}
      increase={increase}
    />
  );
};
```
