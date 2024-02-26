# Comments
Let's implement the comments example showcased on this page. This example provides a detailed illustration and can be used with or without authentication. While it's a bit more verbose compared to other examples, it offers valuable insights into implementing comment functionality.

## Backend
Create the following new files under their respective paths and copy-paste the contents provided below.

We require a permission system to enable the deletion of comments posted by other users. To keep it simple, we'll designate administrators based on their email addresses.

Be sure to replace it with your own email address.

*backend/src/lib/permissions.ts*
```ts
export const admins = ['moritz.roessler@gmail.com'];
```

After you have set up the permissions, you can create the comments component. 

*backend/src/components/Comments.tsx*
```tsx
import {
    authenticate,
    ClientContext,
    IComponent,
    isClientContext,
    Scopes,
    useState,
} from '@state-less/react-server';
import { JWT_SECRET } from '../config';
import { ServerSideProps } from './ServerSideProps';
import { admins } from '../lib/permissions';
import { Session } from '../lib/types';

export enum CommentPolicies {
    Authenticate,
    AuthenticateRead,
    Delete,
}

export const Comments: IComponent<any> = (
    { policies = [] }: { policies: CommentPolicies[] },
    { context }
) => {
    if (
        isClientContext(context) &&
        policies.includes(CommentPolicies.AuthenticateRead)
    )
        authenticate(context.headers, JWT_SECRET);

    let user: Session | null;
    try {
        user = authenticate((context as ClientContext).headers, JWT_SECRET);
    } catch (e) {
        user = null;
    }

    const [comments, setComments] = useState([], {
        key: `comments`,
        scope: Scopes.Component,
    });

    const comment = (message) => {
        let decoded: Session | null = null;
        try {
            decoded = authenticate(
                (context as ClientContext).headers,
                JWT_SECRET
            );
        } catch (e) {
            decoded = null;
        }

        if (policies.includes(CommentPolicies.Authenticate)) {
            if (!decoded) {
                throw new Error('Not authenticated');
            }
        }

        if (!message) throw new Error('Message is required');

        const { strategy } = decoded || { strategy: 'anonymous' };
        const {
            email,
            decoded: { name, picture },
        } = decoded?.strategies?.[strategy] || {
            email: null,
            decoded: { name: 'Anonymous', picture: null },
        };

        const commentObj = {
            message,
            identity: {
                id: (context as ClientContext).headers['x-unique-id'],
                email,
                strategy,
                name,
                picture,
            },
        };
        const newComments = [...comments, commentObj];
        setComments(newComments);
    };

    const del = (index) => {
        const commentToDelete = comments[index];
        if (!commentToDelete) throw new Error('Comment not found');
        const isOwnComment =
            commentToDelete.identity.email ===
                user?.strategies?.[user?.strategy]?.email ||
            (commentToDelete.identity.id ===
                (context as ClientContext).headers['x-unique-id'] &&
                commentToDelete.identity.strategy === 'anonymous');

        if (
            !isOwnComment &&
            !admins.includes(user?.strategies?.[user?.strategy]?.email)
        ) {
            throw new Error('Not an admin');
        }

        const newComments = [...comments];
        newComments.splice(index, 1);
        setComments(newComments);
    };

    return (
        <ServerSideProps
            permissions={{
                comment: policies.includes(CommentPolicies.Authenticate)
                    ? !!user?.id
                    : true,
                delete: admins.includes(
                    user?.strategies?.[user?.strategy]?.email
                ),
            }}
            key="comments-props"
            comments={comments}
            comment={comment}
            del={del}
        />
    );
};
```

Now you need to reference the component in your server.

*backend/src/index.ts*
```tsx
// ...
<Server>
    <Comments key="comments" policies={[CommentPolicies.Authenticate]} />
</Server>
// ...
```

## Client

The client-side setup is relatively simple. We're utilizing Material UI, so ensure you have all the necessary packages installed to avoid any errors.

Create a new file at the specified path and copy-paste the provided contents into it.

*frontend/src/server-components/Comments.tsx*
```tsx
import {
  Alert,
  Avatar,
  Button,
  Card,
  CardActions,
  CardContent,
  Chip,
  IconButton,
  TextField,
  Tooltip,
  Typography,
} from '@mui/material';
import { authContext, useComponent } from '@state-less/react-client';
import { useContext, useState } from 'react';
import GoogleIcon from '@mui/icons-material/Google';
import DeleteIcon from '@mui/icons-material/Delete';

export const Comments = ({ id = 'comments' }) => {
  const [component, { error, loading }] = useComponent(id, {});
  const [comment, setComment] = useState('');
  const comments = component?.props?.comments || [];

  const canComment = component?.props?.permissions.comment;
  const canDelete = component?.props?.permissions.delete;

  return (
    <Card>
      {loading && <Alert severity="info">Loading...</Alert>}
      {error && <Alert severity="error">{error.message}</Alert>}
      {!canComment && (
        <Alert severity="info">You need to be logged in to comment.</Alert>
      )}

      {comments.map((comment, index) => {
        return (
          <Comment
            comment={comment}
            del={() => component?.props?.del(index)}
            canDelete={canDelete}
          />
        );
      })}

      <CardContent>
        <TextField
          multiline
          rows={3}
          onChange={(e) => setComment(e.target.value)}
          fullWidth
          value={comment}
        />
      </CardContent>
      <CardActions>
        <Tooltip
          title={canComment ? '' : 'You need to be logged in to comment.'}
        >
          <span>
            <Button
              onClick={() => {
                component?.props?.comment(comment);
                setComment('');
              }}
              disabled={!canComment}
            >
              Add
            </Button>
          </span>
        </Tooltip>
      </CardActions>
    </Card>
  );
};

const StrategyIcons = {
  google: GoogleIcon,
};
const Comment = ({ comment, del, canDelete }) => {
  const { session } = useContext(authContext);
  const isOwnComment =
    comment.identity.email === session?.strategies?.[session.strategy]?.email ||
    comment.identity.id === JSON.parse(localStorage.id);
  const Icon = StrategyIcons[comment.strategy];
  return (
    <Card sx={{ m: 1 }}>
      <CardContent sx={{ display: 'flex' }}>
        <Typography variant="body1">{comment.message}</Typography>
      </CardContent>
      <CardActions>
        {canDelete ||
          (isOwnComment && (
            <IconButton onClick={del}>
              <DeleteIcon />
            </IconButton>
          ))}
        <Chip
          avatar={
            comment.identity.picture && (
              <Avatar src={comment.identity.picture}>
                <Icon />
              </Avatar>
            )
          }
          label={comment.identity.name}
          sx={{ ml: 'auto' }}
        ></Chip>
      </CardActions>
    </Card>
  );
};
```

After creating the file, you can seamlessly render the Comments component anywhere within your application.

```tsx
<Comments />
```