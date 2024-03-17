# FAQ

## Answers to frequently asked questions.

> Is it a framework ? Something like NextJs ?<sup>[1](https://discord.com/channels/1150117667093626930/1150117667550789739/1218682412356206754)</sup>

It's a framework but it has a different purpose than Next.js, see the answer below.

> Can I do SSR with React Server?

Yes, [SSR](/SSR) is fully supported using a bundler like [Vite](https://vitejs.dev/guide/) or [Webpack](https://webpack.js.org/) or a framework like [Next.js](https://nextjs.org/).  
Please note that, React Server is more than a [BFF](/SSR#bff-vs-backend) used for SSR.
You can write your whole backend business logic using components, states, hooks and effects.

> How does it compare to Next.js?

Next.js is essentially a [BFF](/SSR#bff-vs-backend) which allows you to do Server Side Rendering. React Server on the other hand allows you to build a **backend** using components that have their own lifecycle on the server side.  
React Server allows you to use familiar paradigms and a reactive pattern known from the frontend. React Server can - but not neccessarily has to - be used as BFF in order to facilitate [SSR](/SSR).

> Is there a recommended way to handle authentication and authorization in React Server?

Authentication and Authorization is currently seamless with Google Oauth. Read more about [authentication](/authentication).

> How can I create custom scopes for managing state, and are there any best practices for doing so?

You can pass any string as scope. There is currently no way to define special scopes that have there value replaced during runtime. You can however compute a string dynamically.

> Can you provide more examples of using atomic states once the feature is implemented?

Atomic states will be defined using an equation in the form x=n+1 which allows React Server to infer the correct operation. This feature will be implemented after release. The docs will be updated.

> How do you handle server-side rendering and SEO optimization with React Server?

There is currently no implementation for server side rendering. You can render React components to html, but there is currently no http server serving rendered components. There's also no optimization to pass serverside state to SSR react components.

> Are there any performance considerations or limitations when using React Server, especially when dealing with a large number of users and components?

The architecture has only been tested in small to medium projects where it performed well. So there's currently no known limit to performance.

> How do you handle error boundaries and fallback UIs when a component fails to render or encounters an error?

If a component fails to render during a client side request, the error will be passed to the client and can be handled in the frontend.

> Is there any built-in support for handling component caching, and if so, how can it be configured?

Any stateful server can cache components by using an in memory Store.

> How can I implement real-time functionality or web sockets with React Server?

Every state / component is automatically transported in real-time using Graph Ql subscriptions. You don't need to worry about it.
