# Voting

Let's integrate the voting functionality, showcased on this page, into our project. This feature is incredibly versatile and can be adapted to various scenarios. We'll utilize the Wilson score interval to accurately calculate the score, ensuring reliability and precision.

## Wilson score interval

The Wilson score interval, devised by Edwin Bidwell Wilson in 1927, offers a robust means of determining confidence in a Bernoulli parameter, taking into account the sample size. This scoring method is highly regarded and endorsed by the creators of the Stack Overflow platform for effectively rating questions and answers.

## Use cases

**Example 1**

Imagine you have a website with a user base of 10,000 individuals. Among them, a product has garnered feedback from 100 users, with 40 upvotes and 60 downvotes. To gauge the broader appeal of this product across your entire user population, you employ the Wilson score interval calculation: `wilson-score-interval(40, 100)`. The result, `{ left: 0.3093997461136029, right: 0.4979992153815976 }`, indicates with 95% confidence that "between 30.9% and 49.7% of total users would upvote this product."

Typically, the lower bound of this interval (30.9) is utilized as the outcome, offering a conservative estimate of the product's popularity.

**Example 2**

Beyond mere sorting by rating, the Wilson score interval boasts diverse applications. It serves as a reliable tool whenever a confident estimate is needed regarding the percentage of individuals who have taken or would take a specific action.

Furthermore, its versatility extends to scenarios where data doesn't neatly categorize into two distinct outcomes, such as 1-5 star ratings. With a bit of creative abstraction, one can adapt the Wilson score interval to situations where outcomes are more nuanced, enabling insights into user behavior across various contexts.

## Backend

Create the following new files under their respective paths and paste the contents.

_backend/src/components/Votings.tsx_

```tsx
import { Scopes, useState } from '@state-less/react-server';
import { ServerSideProps } from './ServerSideProps';

type VotingObject = {
    title: string;
    upvotes: number;
    downvotes: number;
    key?: string;
};

type ScoreObject = {
    upvote: number;
    downvote: number;
};

export const Votings = ({ scope = Scopes.Global }: { scope?: Scopes }) => {
    const [voting, setVoting] = useState<VotingObject>(
        {
            title: 'Voting',
            upvotes: 0,
            downvotes: 0,
        },
        {
            key: 'votings',
            scope,
        }
    );
    const [score, setScore] = useState<ScoreObject>(
        {
            upvote: 0,
            downvote: 0,
        },
        {
            key: 'score',
            scope,
        }
    );

    /* *
     * We use the wilson score to compute two bounds. One for the upvote proportion and one for the downvote proportion.
     * */
    const wilsonScoreInterval = (n, votes) => {
        if (n === 0) return 0; // no votes yet

        const z = 1.96; // 95% probability
        const phat = (1 * votes) / n;
        const left = phat + (z * z) / (2 * n);
        const right =
            z * Math.sqrt((phat * (1 - phat) + (z * z) / (4 * n)) / n);
        const leftBoundary = (left - right) / (1 + (z * z) / n);

        // We don't need the upper boundary of the score.
        // const rightBoundary = (left + right) / (1 + (z * z) / n);

        return leftBoundary;
    };

    const storeWilsonScore = (newVoting) => {
        // We need to pass newVoting because variable in the scope will have an outdated value.
        const { upvotes, downvotes } = newVoting;
        const upvoteScore = wilsonScoreInterval(upvotes + downvotes, upvotes);
        const downvoteScore = wilsonScoreInterval(
            upvotes + downvotes,
            downvotes
        );
        setScore({ upvote: upvoteScore, downvote: downvoteScore });
    };

    const upvote = () => {
        const newVoting = { ...voting, upvotes: voting.upvotes + 1 };
        setVoting(newVoting);
        storeWilsonScore(newVoting);
    };

    const downvote = () => {
        const newVoting = { ...voting, downvotes: voting.downvotes + 1 };
        setVoting(newVoting);
        storeWilsonScore(newVoting);
    };

    return (
        <ServerSideProps
            key="votings-props"
            {...voting}
            upvote={upvote}
            downvote={downvote}
            score={score}
        />
    );
};
```

Now you need to reference the component in your server.

_backend/src/index.ts_

```tsx
// ...
<Server>
	<Votings key='votings' />
</Server>
// ...
```

## Client

Setting up the client side is simple and intuitive, especially if you're familiar with Material UI. Ensure that you have all the necessary packages installed to avoid encountering errors during the process.

Create a new file under the following path and paste the contents.

_frontend/src/server-components/VotingApp.tsx_

```tsx
import ThumbDownIcon from '@mui/icons-material/ThumbDown';
import ThumbUpIcon from '@mui/icons-material/ThumbUp';
import { Alert, Box, Button, Typography } from '@mui/material';
import { useComponent } from '@state-less/react-client';

export const VotingApp = () => {
  const [component, { loading, error }] = useComponent('votings', {});

  if (loading) return <div>Loading...</div>;

  return (
    <>
      {error && <Alert severity="error">{error.message}</Alert>}
      <Typography variant="h4" align="center" sx={{ my: 2 }} gutterBottom>
        Voting App
      </Typography>
      <Box
        display="flex"
        alignItems="center"
        justifyContent={'center'}
        flexDirection={'row'}
        sx={{ my: 2 }}
      >
        <Button
          variant="contained"
          color="success"
          onClick={() => component?.props.upvote()}
          startIcon={<ThumbUpIcon />}
        >
          {component?.props?.upvotes || 0}
        </Button>
        <Typography variant="h5" align="center" sx={{ mx: 2 }}>
          {Math.round(
            (component?.props?.upvotes || 0) *
              (component?.props?.score?.upvote || 0)
          ) -
            Math.round(
              (component?.props?.downvotes || 0) *
                (component?.props?.score?.downvote || 0)
            )}
        </Typography>
        <Button
          variant="contained"
          color="error"
          onClick={() => component?.props.downvote()}
          endIcon={<ThumbDownIcon />}
        >
          {component?.props?.downvotes || 0}
        </Button>
      </Box>

      <Box display="block" alignItems="center" columnGap={4}>
        <Typography variant="h6" align="center" sx={{ mx: 2 }}>
          Upvote Bound: {component?.props?.score?.upvote.toFixed(2)}
        </Typography>

        <Typography variant="h6" align="center" sx={{ mx: 2 }}>
          Downvote Bound: {component?.props?.score?.downvote.toFixed(2)}
        </Typography>
      </Box>
    </>
  );
};
```

Once you created the file you can render the Voting component anywhere in your app

```tsx
<VotingApp />
```
