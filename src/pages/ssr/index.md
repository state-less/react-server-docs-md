# SSR

## Bundler

React Server is unopiniated about SSR, you need to use a [Bundler]() like Vite or Webpack in order to really leverage SSR.

Even if you don't need SSR, a bundler makes things much easier.

React Server can do different things out of the box.

## BFF vs. Backend

```github
https://raw.githubusercontent.com/state-less/react-server-docs-md/master/src/fragments/server-vs-bff.md
```

## React Server

React Server makes more sense in a Backend context, but you can use it instead of e.g. _express_ in order to provide serverside rendering which seamlessly integrates with the frontend library.

You just need to pass a `suspend` and `ssr` flag to `useComponent` and it will automatically suspend during SSR and CSR. This seamlessly integrates with Apollo clients's SSR support and allows us to use the same `useComponent` hook during SSR as on the client. You basically don't need to change anything besides a few flags in order to enable serverside rendering.

This of course assumes you have a working SSR setup with your bundler and all your dependencies support SSR in a production builld.

You can read more on how to implement SSR with popular frameworks like [Vite](/ssr/vite) and [Next.js](/ssr/next.js).

React Server is compatible with both frameworks but it doesn't abstract the differences between them. So you would still need to adapt your code if you want to switch from Next.js to Vite or vice versa.
