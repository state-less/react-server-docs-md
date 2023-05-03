## Why React Server?

TLDR;

- Built with GraphQL and TypeScript for robustness and type safety
- Component-driven development for modularity and reusability
- Seamless isomorphic experience with React components as a unified interface
- [Server-side rendering](/SSR) for fast and efficient page loads
- ~~Vertical scaling support for scalability and performance.~~ (NYI)


React Server streamlines development by enabling seamless integration of server and client-side React components. This facilitates building server-rendered components that transfer to the client-side for further rendering and interaction, ensuring a smooth user experience.

Leveraging GraphQL and TypeScript, React Server promotes reliable, maintainable, and scalable code. GraphQL simplifies data query and mutation management, while TypeScript's static typing enhances error detection and code readability.

The component-driven approach of React Server fosters rapid development of reusable components, promoting consistent design language and reduced duplication. Additionally, server-side rendering support enhances app performance and SEO through reduced client-side rendering reliance, improving load times and search rankings.

React Server also offers an excellent developer experience with features like hot reloading and code splitting, allowing for rapid iteration and testing without configuration hassles. Focus on crafting exceptional user experiences with React Server, unencumbered by technical minutiae.

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