## Why React Server?

TLDR;

- Built with GraphQL and TypeScript for robustness and type safety
- Component-driven development for modularity and reusability
- Seamless isomorphic experience with React components as a unified interface
- ~~Automatic server-side rendering for fast and efficient page loads~~ (NYI)
- ~~Vertical scaling support for scalability and performance.~~ (NYI)

React Server allows for a seamless **isomorphic** experience, as it proposes React components as a unified interface to bridge the gap between server and client. This means that you can easily build and render components on the server-side and then send them to the client-side for further rendering and interactivity. This creates a smooth and uninterrupted user experience, where the server and client are working together in perfect harmony.

Another great benefit of React Server is its **robustness**, as it's built with GraphQL and TypeScript. These technologies ensure that your code is reliable, maintainable, and scalable. With GraphQL, you can easily manage your data queries and mutations, and with TypeScript, you have the added benefit of static typing, which catches errors early and improves code readability.

**Component-driven development** is another key feature of React Server. By breaking down your Backend / UI into smaller, reusable components, you can develop faster and more efficiently, without sacrificing quality. Components can be easily shared and reused across different parts of your app, making it easy to maintain a consistent design language and reduce duplication.

React Server also provides built-in support for **server-side rendering**, which greatly improves your app's performance and search engine optimization. By rendering your app on the server-side, you can reduce the amount of client-side rendering needed, resulting in faster load times and improved SEO rankings. (NYI*)

Finally, React Server offers a great developer experience, with hot reloading, code splitting, and other modern development tools. This makes it easy to iterate and test your app quickly, without having to worry about configuration and setup. With React Server, you can focus on building great user experiences, without getting bogged down in technical details.

<sub>*\*NYI - Not yet implemented*</sub>
## Installation

### Get a Server running

```
npx init state-less/react-server my-server
cd .my-server
npm start
```

#### Troubleshooting.

In case the initializiation did not work you can manually set up a server. It's a little more work but it's worth it.

```
git clone state-less/clean-starter -b react-server my-server
cd my-server
git remote remove origin
yarn install
yarn start
```

### Get a Client running

```
yarn create vite
yarn add @apollo/client state-less/react-client
```

#### Edit `src/App.tsx`

Import the `useServerState` hook and find and replace the `useState` call.

```
import { useServerState } from '@state-less/react-client'

// ...

const [count, setCount] = useServerState(0, {
key: 'count',
scope: 'global',
})
```

### Play around

This is all it needs to get a server and client running.
You can now manipulate the state from a graphql client.
