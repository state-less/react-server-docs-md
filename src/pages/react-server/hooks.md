
# React Server Hooks
Hooks represent a significant advancement in React, empowering developers to efficiently reuse stateful logic across components without resorting to intricate class structures. These hooks seamlessly integrate into functional components, allowing developers to tap into React's lifecycle and state management effortlessly.

In React Server, hooks operate in a similar fashion but with a crucial distinction. Tailored to the context of server-side React components, these hooks are specifically crafted to tackle the distinctive intricacies and demands of server-side development. Equipped with React Server's suite of hooks, developers gain the ability to proficiently manage server-side component state and lifecycle, all while harnessing the inherent flexibility and reusability inherent in functional components.

## Using Hooks in React Server
React Server equips you with a collection of tailor-made hooks to streamline state management and lifecycle handling within your server-side React components. These hooks liberate you from the complexities of class components and intricate state management libraries, allowing you to craft sleek, functional components with ease.

Integrating hooks into your React Server components is straightforward. Simply import them from the @state-less/react-server package and invoke them within your component functions. It's worth noting that hooks are exclusively applicable within functional components, not class components.

Here's a brief overview of the hooks furnished by React Server:

`useState`: Facilitates server-side state management within your components, mirroring React's useState but fine-tuned for server-side use.

`useEffect`: Enables execution of side effects, like data fetching, in your server-side components, akin to React's useEffect. It triggers upon each rendering of a component on the server.

`useClientEffect`: Executes side effects, such as data fetching, in your server-side components, mirroring React's useEffect. It activates upon each rendering of a component on the server within a client's context.

`useContext`: Grants access to the context of a Provider higher up the component tree, facilitating data propagation to diverse children and managing global state.

These hooks form the cornerstone of server-side component development in React Server. Leveraging their capabilities empowers you to construct robust, reusable components seamlessly integrated into your full-stack application.

Keep in mind that the hooks offered by React Server are purpose-built for server-side components and should be utilized exclusively within this context. Always exercise caution to ensure the appropriate utilization of hooks aligned with your component's context.

## useState

| Signature                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| useState(defaultValue: any, options: CreateStateOptions) | Creates a new state in the store under the specified key and scope |

### CreateStateOptions
| Fields    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| key | The unique key of the state.
| scope | The scope of the state. |

## useEffect

| Signature                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| useEffect(fn: () => void, deps: any[]) | Schedules a side-effect that runs when the server renders the component in server context but only if a dependency changed. |
| fn: () => void | Any callback function. | 
| deps: any[] | An array of dependencies to monitor for changes in respect to the prior run.|

## useClientEffect (NYI*)

| Signature                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| useEffect(fn: () => void, deps: any[]) | Schedules a side-effect that runs when the server renders the component in server context but only if a dependency changed. |
| fn: () => void | Any callback function. | 
| deps: any[] | An array of dependencies to monitor for changes in respect to the prior run.|

## useContext

| Signature                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| useContext(context: ReactServerContext) | Consumes a provided context. |
| context: ReactServerContext | The context that has been created with [`createContext`](/react-server#createcontext). It must provide a `<context.Provider>` higher up in the tree |
