# @state-less/react-server

The @state-less/react-server package serves as the backbone of any React Server application, providing essential modules and utilities for developing server-side React components in full-stack applications. It exports a few built-in components, a memory store, various types, utility functions, and the core React Server functionality.

## Server

```tsx
import {Server} from '@state-less/react-server
```
This is the entrypoint component for your react server. You need to render it using the provided `render` function.

## render

The render function allows you to render React Server components on the server (in the context of a client request).
```tsx
import {render} from '@state-less/react-server'

const server = <Server>

</Server>

render(server);
```

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