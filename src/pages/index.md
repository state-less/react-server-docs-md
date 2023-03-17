## Why React Server?

- Simplified Data Management: React Server uses GraphQL as transportation, making it easy to query and manage data on both the server and client sides of your application. With GraphQL, you can specify exactly what data you need, reducing the amount of data transfer and improving performance.

- Easy Deployment: React Server can be easily deployed to AWS Lambda, making it a great choice for serverless applications. AWS Lambda provides automatic scaling and high availability, making it a reliable and cost-effective option for hosting your backend.

- Improved Type Safety: Since React Server is written entirely in TypeScript, you can take advantage of TypeScript's powerful type checking and static analysis features to catch errors before they make it to production. This improves code quality and makes it easier to maintain and refactor your codebase.

- Reusable Components: With React Server, you can use TSX on the server side to create reusable components that can be used across your entire application. This improves consistency and reduces development time by eliminating the need to write custom code for each feature.

- Improved Performance: By using React Server with GraphQL as transportation, you can reduce the amount of data transfer and improve the performance of your application. Plus, since React Server can be run in a mix of stateless and stateful servers, you can cache render requests and scale API calls indefinitely.

- Overall, React Server is a powerful tool for building modular and scalable backends with GraphQL, AWS Lambda, and TypeScript. With its streamlined data management, easy deployment, improved type safety, reusable components, and improved performance, React Server is a great choice for any project.

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
