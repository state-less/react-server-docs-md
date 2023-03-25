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

React Server also provides built-in support for **server-side rendering**, which greatly improves your app's performance and search engine optimization. By rendering your app on the server-side, you can reduce the amount of client-side rendering needed, resulting in faster load times and improved SEO rankings. (NYI\*)

Finally, React Server offers a great developer experience, with hot reloading, code splitting, and other modern development tools. This makes it easy to iterate and test your app quickly, without having to worry about configuration and setup. With React Server, you can focus on building great user experiences, without getting bogged down in technical details.

_\*NYI - Not yet implemented_

## Installation
### Get a Server running

```bash
git clone https://github.com/state-less/clean-starter.git -b react-server my-server
cd my-server
git remote remove origin
yarn install
yarn start
```

### Get a Client running

Create a new vite project and choose *React* as framework and *TypeScript* as variant.
```bash
yarn create vite
```

Now change to the newly created folder, install the dependencies and add `@apollo/client` and `state-less/react-client` to your project and start the server.

```bash
cd vite-project
yarn
yarn add @apollo/client state-less/react-client
yarn dev
```

#### Add a GraphQl client

In order to connect to our backend, we need to create a GraphQl client. Create a new file under `/src/lib/client.ts` and paste the following content.

```ts
import { ApolloClient, InMemoryCache, split, HttpLink } from '@apollo/client';
import { WebSocketLink } from '@apollo/client/link/ws';
import { getMainDefinition } from '@apollo/client/utilities';

// Create an HTTP link
const localHttp = new HttpLink({
  uri: 'http://localhost:4000/graphql',
});

// Create a WebSocket link
const localWs = new WebSocketLink({
  uri: `ws://localhost:4000/graphql`,
  options: {
    reconnect: true,
  },
});

// Use the split function to direct traffic between the two links
const local = split(
  ({ query }) => {
    const definition = getMainDefinition(query);
    return (
      definition.kind === 'OperationDefinition' &&
      definition.operation === 'subscription'
    );
  },
  localWs,
  localHttp
);

// Create the Apollo Client instance
export const localClient = new ApolloClient({
  link: local,
  cache: new InMemoryCache(),
});

export default localClient;

```

This creates a new GraphQl client instance which will be used by the React Server client.
For now you need to manually create this file, but it will later be created by an initializer or react-client will provide a way to bootstrap the graphql client by providing a *react server url*. For now you need to manually create a GraphQl client.

### Edit `src/App.tsx`

It's been a long way, but all that's left to do is import the `client` and `useServerState` hook and find and replace the `useState` call with a `useServerState` call.

```ts
import { useServerState } from "@state-less/react-client";
import client from "./lib/client";

// ...

const [count, setCount] = useServerState(0, {
  key: "count",
  scope: "global",
  client
});
```

![screenshot](../../images/screenshot.jpg)

If you don't want to pass a client object to each query, you can wrap your application in an `<Apolloprovider client={client} />`. React Server will use the provided client.
*Note: You can still override the provided client if you pass one in the options*
### Play around
That's all. **Make sure the backend react server is running** and click the button.


