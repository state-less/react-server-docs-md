# Server-Side Rendering (SSR) with Next.js

In this guide, we'll show you how to implement Server-Side Rendering (SSR) with React Server and Next.js. SSR allows you to generate fully-rendered HTML content on the server, which can improve performance and SEO.

## Overview

To implement SSR with React Server and Next.js, we'll use the `renderComponent` function provided by the `state-less/react-client` package. This function fetches a component using GraphQL and returns the rendered component, ready for SSR.

## Setup

1. First, make sure you have Next.js installed and set up in your project.
Refer to the [Next.js Docs](https://nextjs.org/docs/getting-started)
2. Make sure you have a running React Server, as well as both React Client and Apollo Client installed.


### Getting Started

The procedure to get started with next.js instead of vite is almost the same. Go ahead by creating a new project and installing the dependencies.

```bash
yarn create next-app --typescript my-app
cd my-app
yarn add @apollo/client state-less/react-client
yarn dev
```

Now you need to create a `lib/client.ts` file with the same content as the one on the [start page](/)

If you followed these steps, you can continuing implementing SSR with Next.js and React Server
## Implementing SSR with Next.js

1. In your Next.js project, create a new page that will use SSR.
4. In your Next.js page, use the `getServerSideProps` function to fetch the component's data during SSR using `renderComponent`. 
This function will ensure that the component is fetched and rendered on the server before sending the fully-rendered HTML to the client.
Return the data from the component as props.
3. Pass the props from `getServerSideProps` to the `useComponent` function in the data field of the options parameter, along with the component key and any required props (such as the client). This will ensure that your client subscribes to changes of the component, making it reactive.

Let's render our second hello world example which is a simple counter button which increases the count when you click on it.

*frontend/src/pages/test.tsx*
```tsx
import { renderComponent, useComponent } from '@state-less/react-client';
import { ssrClient, localClient } from '../lib/client';

import { Button } from '@mui/material';

export async function getServerSideProps() {
  const { data: helloWorldComponent } = await renderComponent('hello-world-2', {
    client: ssrClient,
  });

  return { props: { helloWorldComponent } };
}

export default function Test(props) {
  const [component, { error, loading }] = useComponent('hello-world-2', {
    client: localClient,
    data: props.helloWorldComponent,
  });
  return (
    <>
      <Button onClick={() => component?.props?.increase()}>
        Count is {component?.props?.count}
      </Button>
    </>
  );
}
```

If you visit [http://localhost:3000/test](http://localhost:3000/test) you will see that the count of the button get's rendered in the initial HTML sent by the next.js server. If you disable JavaScript and reload the page, the button still has the latest count. If you enable JavaScript, the button is interactive and reactive. If you click on it the count updates.

*Note: this assumes that you already have a react server with the hello-world-2 component running. It also assumes you have created the lib/client.ts file from the example on the start page. ([Home](/))*

With the above steps, you have successfully implemented SSR with React Server and Next.js. Your components will now be rendered on the server, providing improved performance and SEO benefits.