# Setting up a Forum

This is a guide on manually building a Forum with _@state-less/leap-frontend_

There's also a boilerplate repo which you can fork in order to spin up a Forum as hosted on [javascript.forum](https://javascript.forum).

You can either clone the Forum repo and connect it to a running React Server instance, or you can build the Forum using the _Components_ and _hooks_ provided by _@state-less/leap-frontend_.

In both cases you will need the backend Forum running on React Server. Follow the instructions to spin up a Forum.

## Setup

Before you can drop the Forum component into your server you first need to
[setup a new React Server instance](/installation). This only takes a few minutes.

## Backend

Once your server is running you can install the backend repo which contains the Forum [@state-less/leap-backend](https://npmjs.com/package/@state-less/leap-backend).

```bash
cd my-server
```

```bash
yarn add @state-less/leap-backend
```

Find the following section of code in [_src/index.tsx_](https://github.com/state-less/clean-starter/blob/e3dadbdd3ca3b3268a1158e21ffd746b027b6782/src/index.tsx#L87)

```tsx
export const reactServer = (
  <Server key="server">
    <HelloWorldExample2 key="button" />
  </Server>
);
```

Import the Forum and place it inside the `Server` component.

```tsx
import { Forum, ForumPolicies } from "@state-less/leap-backend";
```

```tsx
export const reactServer = (
  <Server key="server">
    <Forum
      key="my-forum"
      id="my-forum"
      name="My Forum"
      policies={[ForumPolicies.PostsNeedApproval]}
      users={["moritz.roessler@gmail.com"]} //Use your gmail adress
    />
  </Server>
);
```

That's all it needs to provide the Forum services on the backend.
You can start your server with `yarn start`.

## Frontend

You have two options of connecting a frontend to the backend. 1. Use the boilerplate repo to connect your backend. 2. Create your own frontend from scratch with [Leap](https://npmjs.com/package/@state-less/leap-backend)'s frontend components.

### Boilerplate

To connect the whole SPA at [state-less/javascript-forum](https://github.com/state-less/javascript-forum/) to the backend, simply clone the repo and follow the README instructions in the github repo.

```bash
git clone https://github.com/C5H8NNaO4/javascript-forum.git my-forum
cd my-forum
cp .env.template .env
yarn install
yarn dev
```

If you set up your env variables correctly, the Forum should connect to your server and you can login and start to post questions.

### Manual

In case you want to build a site with a different layout you can use the _@state-less/leap-frontend_ to build a frontend yourself. _Note: It currently provides plug and play components that connect to the Forum. However we will also provide hooks in order to simply fetch data._

Open a split terminal and start your backend server.

```bash
cd my-server
```

```
yarn start
```

### Dependencies

Install our open source frontend components [@state-less/leap-frontend](https://npmjs.com/package/@state-less/leap-frontend) along with their peer dependencies.

```bash
yarn add @state-less/leap-frontend
yarn add react-router react-router-dom
```

In order for your App to render, you need to wrap it in an `ApolloProvider` `AuthProvider` and `Router`.
The Forum App needs to be routed in userland. Which means you will have to setup some basic routing.

Paste the following code into your _App.tsx_.

```tsx
import { ApolloProvider } from "@apollo/client";
import { BrowserRouter as Router, Routes } from "react-router-dom";

import client from "./lib/client";
import { routes } from "./routes";
import { AuthProvider } from "@state-less/react-client";

function App() {
  return (
    <ApolloProvider client={client}>
      <AuthProvider client={client}>
        <Router>
          <Routes>{routes}</Routes>
        </Router>
      </AuthProvider>
    </ApolloProvider>
  );
}

export default App;
```

Create a new file called _src/routes.tsx_ and render the `ForumPage` and `ForumPost` routes.

```tsx
import { ForumPage, PostsPage } from "@state-less/leap-frontend";
import { Route } from "react-router";

const CLIENT_ID = import.meta.env.REACT_APP_GOOGLE_ID;
export const routes = [
  <Route
    path="/"
    Component={() => {
      return (
        <ForumPage
          basePath=""
          forumKey="my-forum"
          ghSrc={{
            rules:
              "https://raw.githubusercontent.com/state-less/blogs.state-less.cloud/main/Rules.md",
            qa: "https://raw.githubusercontent.com/state-less/blogs.state-less.cloud/main/Q&A.md",
          }}
          clientId={CLIENT_ID}
        />
      );
    }}
  />,
  <Route
    path="/:post"
    Component={() => (
      <PostsPage basePath="" forumKey="my-forum" clientId={CLIENT_ID} />
    )}
  />,
];
```

Note, you need to obtain a valid google client id and pass it to the forum in order to be able to login and approve posts. Right now the only supported login method is Google.
