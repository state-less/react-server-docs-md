## Why React Server?

TLDR;

- Built with GraphQL and TypeScript for robustness and type safety
- Component-driven development for modularity and reusability
- Seamless isomorphic experience with React components as a unified interface
- [Server-side rendering](/SSR) for fast and efficient page loads
- ~~Vertical scaling support for scalability and performance.~~ (NYI)

React Server simplifies the development process by offering a seamless experience through React components that connect the server and client. This means you can easily build and render components on the server-side and then transfer them to the client-side for further rendering and interaction, creating a smooth, uninterrupted user experience.

React Server is also built with GraphQL and TypeScript, making your code reliable, maintainable, and scalable. GraphQL makes it easy to manage your data queries and mutations, while TypeScript's static typing helps catch errors early and improves code readability.

Using React Server's component-driven development, you can create smaller, reusable components that allow you to develop faster without sacrificing quality. You can also share and reuse these components across different parts of your app, resulting in a consistent design language and reduced duplication.

React Server's built-in support for server-side rendering improves your app's performance and search engine optimization by reducing the need for client-side rendering. This leads to faster load times and improved SEO rankings.

Finally, React Server offers a great developer experience, with hot reloading, code splitting, and other modern development tools. This makes it easy to iterate and test your app quickly, without worrying about configuration and setup. With React Server, you can focus on building great user experiences without getting bogged down in technical details.

_\*NYI - Not yet implemented_

Discover component driven backend development and leverage the power of TypeScript and JSX to build your own ecosystem of components.
```tsx
const server = <Server>
    // your components here
</Server>;
```

Creating your own components is straight forward. This is the code that powers the button below.
```tsx
export const HelloWorldExample2 = () => {
    // The useState hook looks familiar?
    const [count, setState] = useState(0, {
        key: 'count',
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