
# Poll Component Example

In this example, we'll demonstrate how to create a simple poll component with React Server. The component will manage the voting process for a poll, allowing users to vote for their favorite option.

## Authentication

Don't be confused if the poll example shows an error "Not Authorized". This is because the component has code to authenticate rendering of the component. 

If you wish to remove the authentication, just remove the call to `authenticate` in the Poll component. 
If you want to experiment with authentication, please refer to the [authentication](/authentication) page for instructions on how to implement server and clientside authentication (it's straightforward, try it out).

## Backend Code

First, let's implement the backend part of the poll component. The backend code manages the poll's state, allowing users to vote and unvote (if permitted) for the options provided. The component exports the necessary data and functions for the frontend to display and interact with the poll.

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

Now that we have our backend code, let's create the frontend component that will interact with the backend and display the poll results. The frontend component will receive the necessary properties and methods from the backend to display the poll options and manage the user's voting actions.

Insert the frontend code here:

```tsx
import {
  Alert,
  Card,
  IconButton,
  List,
  ListItem,
  ListItemSecondaryAction,
  ListItemText,
} from '@mui/material';
import { useComponent } from '@state-less/react-client';
import HeartIcon from '@mui/icons-material/Favorite';

export const Poll = () => {
  const [component, { error, loading }] = useComponent('poll', {});

  return (
    <Card>
      {loading && <Alert severity="info">Loading...</Alert>}
      {error && <Alert severity="error">{error.message}</Alert>}
      <List>
        {component?.props?.values?.map((value, i) => (
          <ListItem>
            <ListItemText
              primary={value}
              secondary={component?.props?.votes[i]}
            />
            <ListItemSecondaryAction>
              <IconButton
                onClick={() => component?.props?.vote(i)}
                color={component.props.voted === i ? 'primary' : 'default'}
              >
                <HeartIcon />
              </IconButton>
            </ListItemSecondaryAction>
          </ListItem>
        ))}
      </List>
    </Card>
  );
};
```

With the backend and frontend components in place, you can now include the poll component in your application and allow users to vote on various options. The React Server manages the poll's state and seamlessly integrates the voting process into your application.