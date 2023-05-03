# @state-less/react-server

The @state-less/react-server package serves as the backbone of any React Server application, providing essential modules and utilities for developing server-side React components in full-stack applications. It exports a few built-in components, a memory store, various types, utility functions, and the core React Server functionality.

## Server

```tsx
import {Server} from '@state-less/react-server'

const server = <Server>
    {/* Your components */}
</Server>;
```
This is the entrypoint component for your react server. You need to render it using the provided `render` function.
| Props                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| children: ReactServerComponent\<unknown\>[] | The React Server components you want to serve. |

## render

The render function allows you to render React Server components on the server (in the context of a client request).
```tsx
import {render} from '@state-less/react-server'

const server = <Server>

</Server>

render(server);
```
| Function                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| render(component: ReactServerComponent\<unknown\>) | Renders a ReactServerComponent on the server. |
## destroy 

The destroy function destroys the current executing component including all its states. You should use it to clean up once a component is no longer needed.

```tsx
import {destroy} from '@state-less/react-server';

export const Comment = (props: CommentProps, { key }) => {
    const { remove } = props;
    const del = () => {
        // Destroy the component and all its states.
        destroy();
        // Remove the component from the array in the parent component.
        remove();
    };
    return (
        <ServerSideProps key={`${key}-props`} {...props} del={del}>
            <Votings
                key={`votings-${key}`}
                policies={[VotingPolicies.SingleVote]}
            />
        </ServerSideProps>
    );
};
```
| Function                                                    | Description                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| destroy(component?: ReactServerComponent\<unknown\>) | Destroys a component and all its states. |


<a name="createContext"></a>
## createContext
The `createContext` function allows you to create a context that can render a `Provider` which passes values down to child components where they can be consumed using the `useContext` hook.

```tsx
import { createContext } from '@state-less/react-server';

export const context = createContext();
export const Server = (props) => {
  const value = {
    version: VERSION,
    uptime: UPTIME,
    platform: process.platform,
  };
  return {
    ...value,
    children: (
      <context.Provider value={value}>{props.children}</context.Provider>
    ),
  };
};
```

Anyone child component of `Server` can consume the exported `context` to get access to the value provided by the `Provider`.

