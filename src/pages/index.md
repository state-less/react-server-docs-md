## Why React Server?

It's a fullstack framework with a serverside and a clientside library that makes it straight forward to use the familiar syntax of React to build your backend code as components in a way that makes it easy to transfer any serverside state to the client as TSX props. 

It's a fun and intuitive way to build and rapidly prototype sophisticated services with minimal effort. 

React Server is a fullstack solution for web applications that offers several key benefits, including:

* **Components**: React Server fosters a component driven architecture, which proved a successful pattern for clean and scalable code.
* **Server-side rendering**: React Server can render your application on the server using Next.js, which can improve performance and SEO.
* **Real-time updates**: React Server can update your application in real time, which can improve the user experience.
* **Scalability**: React Server is designed to be scalable, so you can easily handle large numbers of users.
* **Simplicity**: React Server is familiar to React and is easy to use, even for developers who are new to React.

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
