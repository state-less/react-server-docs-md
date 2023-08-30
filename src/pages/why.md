# Why?

## Declarative and Modular

React Server has borrowed a few of the concepts from React which makes it possible to design modular backend code in a declarative fashion. Its component driven approach makes it ideal for building complex applications or microservices that work together. 

You can easily nest components on the backend and mirror the component structure of the frontend.  
**Note:** This allows you to mitigate the waterfall problem by *precomputing* all the nested data your frontend components need. 

## Data transportation

When using serverside components to provide serverside state management to your frontend components you don't need to worry about transporting data at all. All that's needed is rendering the component on the *backend* `<MyComponent key="my-key" />` and a hook on the frontend does the rest `useComponent('my-key)`.

## Components instead of APIs

Instead of consuming REST APIs which maps a given *path* to a specific functionality, you can now consume **components** as easy as calling one function. 

Coming from React, you are used to *states* and `props` on the frontend. Now simply extend that concept to the server side. 

A component on the server side is a collection of *states* (which store your current serverside state), *functions* (which operate on your states), and a rendered TSX components which describes the *properties* you expose to the public. You can render any JavaScript value as `props`. Functions get mapped to a serializable representation and passed to the frontend where they can be called with any serializable argument, directly invoking the function of your currently mounted component. 

By consuming a component on the frontend (or any other GraphQL client), you get access to all properties rendered by the component. The frontend automatically subscribes to any changes to the component on the serverside so it's properties are always up to date. 

Did we mention that a component is simply a function which returns a TSX component? It doesn't get any easier.

## State management

Using *states* and *effects* on a component base makes it very easy to build large reactive backend applications. It's easy to break down buisiness logic of a large scale project into multiple components and use states to provide reactive behaviour to parts of its data. 

