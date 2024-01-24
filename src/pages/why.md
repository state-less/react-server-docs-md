# Why?

## Reactive Coding
React Server provides an abstraction layer for the asynchronous flow of stateful requests through a *reactive* interface. It externalizes states into databases, enabling a **stateless** architecture which can be run on e.g. AWS Lambda. 
By utilizing a *reactive* coding style featuring *hooks* such as `useState` and server / client effects e.g. `useEffect`  and `useClientEffect` as know from React, React Server provides a reactive interface with a linear semantic flow of code which abstracts the complex data fetching and transportation logic on the server side. It uses GraphQL's PubSub mechanism allowing you to reflect the serverside state of a component on the client in realtime, simply by consuming a *component* on the frontend. 

The reactive nature of *React Server* theorethically allows you to minimize TTFB as a cached response will be fetched from the server and once the live data has been fetched from the database it publishes the data to all connected clients, making it available on the frontend as soon as it arrived from the database at the backend.

The benefits of a reactive coding style allow for a clean representation of code flow abstracting complex data fetching and transportation logic promoting readable and maintainable code bases. 

## A new way to consume data.

Just like *GraphQL* provides a new layer of flexibility over REST by allowing you to consume safely typed shapes of data, *React Server* provides a new layer above GraphQL which allows you to consume *components* instead of an arbitrary shape. Which of course is way less flexible than consuming custom shapes. However in the context of component based *micro services* where you already modularize your frontend code into reusable components you can couple these to backend component which provide full server authoritative backend buisiness logic for your frontend components. This allows you to craft highly reusabel microservices which can be hosted on any NodeJS runtime which supports *React Server*. The neat part is that communication between the server and client is seamless. You can obtain *function references* through a single *useComponent* call which directly calls the function defined in your serverside source code. This means no more HTTP requests or queries which fetch data or perform actions. You instead consume a component which provides the props for your frontend component. Any function reference you render on the server side will automatically be passed to the frontend as callable function which passes all arguments to the serverside function. This allows for seamless two way communication as the serverside response will be sent back to the client resolving the promise. 

## Declarative and Modular

React Server has borrowed a few of the concepts from React which makes it possible to design modular backend code in a declarative fashion. Its component driven approach makes it ideal for building complex applications or microservices that work together. 

You can easily nest components on the backend and mirror the component structure of the frontend.  
**Note:** This allows you to mitigate the waterfall problem by *precomputing* all the nested data your frontend components need and send it down to the client in one single response. 

## Data transportation

When using serverside components to provide serverside state management and buisiness logic to your frontend components, you don't need to worry about transporting data at all. All that's needed is rendering the component on the *backend* `<MyComponent key="my-key" />` and a hook on the frontend does the rest `useComponent('my-key)`.

## Components instead of APIs

Instead of consuming REST APIs which maps a given *path* to a specific functionality, you can now consume **components** all data needed for a component with a single hook. 

### React on the backend 
Coming from React, you are used to *states* and `props` on the frontend. Now simply extend that concept to the server side. 

A component on the server side is a collection of *states* (which store your current serverside state), *functions* (which operate on your states), and a rendered TSX components which describes the *properties* you expose to the public. You can render any JavaScript value as `props`. Functions get mapped to a serializable representation and passed to the frontend where they can be called with any serializable argument, directly invoking the function of your currently mounted component. 

By consuming a component on the frontend (or any other GraphQL client), you get access to all properties rendered by the component. The frontend automatically subscribes to any changes to the component on the serverside so it's properties are always up to date. 

Did we mention that a component is simply a function which returns a TSX component? It doesn't get any easier.

## State management

Using *states* and *effects* on a component base makes it very easy to build large reactive backend applications. It's easy to break down buisiness logic of a large scale project into multiple components and use states to provide reactive behaviour to parts of its data. 

