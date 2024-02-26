
# Poll Component Example

In this exemplary demonstration, we'll showcase the seamless construction of a poll component using React Server. Our component will facilitate the entire voting process, empowering users to cast their votes for their preferred option effortlessly. With React Server's dynamic capabilities, we'll craft a responsive and interactive poll interface, fostering user engagement and interaction. Let's embark on this enlightening journey to create a poll component that seamlessly integrates with React Server!

## Authentication

Don't be alarmed if you encounter an "Not Authorized" error while exploring the poll example. This occurs due to the component's authentication requirement, ensuring secure rendering.

If you prefer to disable authentication, simply remove the `authenticate` function call within the Poll component. Conversely, if you're keen to delve into authentication, visit the [authentication](/authentication) page for clear instructions on implementing server and client-side authentication. It's an intuitive process worth experimenting with!

## Backend Code

Let's begin by crafting the backend logic for our poll component. This segment of the code will oversee the state of our poll, enabling users to cast their votes or retract them, if allowed, for the provided options. Additionally, we'll export essential data and functions to the frontend, facilitating seamless display and interaction with the poll.

Insert the backend code here:

```tsx
import {
    authenticate,
    isClientContext,
    Scopes,
    useState,
} from '@state-less/react-server';
import { JWT_SECRET } from '../config';
import { ServerSideProps } from './ServerSideProps';

export enum PollActions {
    Revert,
}

export const Poll = (
    {
        values,
        key,
        allow = [],
    }: { values: string[]; key: string; allow: PollActions[] },
    { context }
) => {
    if (isClientContext(context)) authenticate(context.headers, JWT_SECRET);
    const [votes, setVotes] = useState(
        values.map(() => 0),
        { key: `votes-${key}`, scope: 'global' }
    );

    const [voted, setVoted] = useState(-1, {
        key: 'voted',
        scope: Scopes.Client,
    });

    const unvote = (index) => {
        if (voted !== index) {
            throw new Error('Already voted');
        }
        const newVotes = [...votes];
        newVotes[index] -= 1;
        setVotes(newVotes);
        setVoted(-1);
    };

    const vote = (index) => {
        if (voted === index && allow.includes(PollActions.Revert)) {
            unvote(index);
            return;
        }
        if (voted !== -1) {
            throw new Error('Already voted');
        }
        const newVotes = [...votes];
        newVotes[index] += 1;
        setVotes(newVotes);
        setVoted(index);
    };

    return (
        <ServerSideProps
            key="poll-props"
            votes={votes}
            values={values}
            voted={voted}
            vote={vote}
        />
    );
};
```

## Frontend Code

With our backend logic in place, let's proceed to develop the frontend component responsible for interfacing with the backend and presenting the poll results. This component will receive essential properties and methods from the backend to showcase the poll options and oversee the user's voting actions.

Insert the frontend code here:

```tsx
import {
  Alert,
  Box,
  Card,
  IconButton,
  List,
  ListItem,
  ListItemSecondaryAction,
  ListItemText,
} from '@mui/material';
import { useComponent } from '@state-less/react-client';
import HeartIcon from '@mui/icons-material/Favorite';
import { ReactNode } from 'react';

export const Poll = ({
  id = 'poll',
  message,
}: {
  id?: string;
  message?: (props: Record<string, any>) => ReactNode;
}) => {
  const [component, { error, loading }] = useComponent(id, {});
  const sum = component?.props?.votes.reduce((a, b) => a + b, 0);
  return (
    <Card>
      {loading && <Alert severity="info">Loading...</Alert>}
      {error && <Alert severity="error">{error.message}</Alert>}
      {message && message?.(component?.props || {})}
      <List>
        {component?.props?.values?.map((value, i) => {
          const percentage = (100 / sum) * component?.props?.votes[i];
          return (
            <ListItem dense>
              <Box
                sx={{
                  ml: -2,
                  zIndex: 0,
                  position: 'absolute',
                  width: `${percentage}%`,
                  height: `100%`,
                  backgroundColor: 'info.main',
                }}
              />
              <ListItemText
                sx={{ zIndex: 0 }}
                primary={value}
                secondary={component?.props?.votes[i]}
              />
              <ListItemSecondaryAction>
                <IconButton
                  onClick={() => component?.props?.vote(i)}
                  disabled={
                    component?.props?.voted > -1 &&
                    component?.props?.voted !== i
                  }
                  color={component.props.voted === i ? 'primary' : 'default'}
                >
                  <HeartIcon />
                </IconButton>
              </ListItemSecondaryAction>
            </ListItem>
          );
        })}
      </List>
    </Card>
  );
};
```

Now that both the backend and frontend components are set up, you're ready to integrate the poll component into your application. With React Server managing the poll's state, the voting process seamlessly integrates into your application, providing users with a smooth and interactive experience.