# Voting

Let's implement the voting example as seen on this page. It's a very useful functionality that can be used in many different ways. We are using the Wilson score interval to calculate the score.

## Wilson score interval

The Wilson score interval is a method for calculating the confidence of a Bernoulli parameter based on the sample size. It was introduced by Edwin Bidwell Wilson in 1927. Use of the score is recommended by the creators of the Stack Overflow website for rating questions and answers.

## Use cases

**Example 1**

If you know what a sample population thinks, you can use this tool to estimate the preferences of the population at large.

Suppose your site has a population of `10,000` users. One product has ratings from `100` users (your sample size): `40 upvotes`, and `60 downvotes`. You want to understand how popular the product would be across the whole population. So you run `wilson-score-interval(40, 100)`, which returns the `result { left: 0.3093997461136029, right: 0.4979992153815976 }`. Now you can estimate with 95% confidence that `between 30.9% and 49.7% of total users would upvote this product`.

It is common to use the lower bound of this interval (here, 30.9) as the result, as it is the most conservative estimate of the "real" score.

**Example 2**

Apart from sorting by rating, the Wilson score interval has a lot of potential applications! You can use the Wilson score interval anywhere you need a confident estimate for what percentage of people took or would take a specific action.

You can even use it in cases where the data doesn't break cleanly into two specific outcomes (e.g. 1-5 star ratings), as long as you are able to creatively abstract the outcomes into two buckets (e.g. % of users who voted 4 stars and above vs % of users who didn't).

## Backend

Create the following new files under their respective paths and paste the contents.

_backend/src/components/Votings.tsx_

```tsx
import { Scopes, useState } from '@state-less/react-server'
import { ServerSideProps } from './ServerSideProps'

type VotingObject = {
	title: string
	upvotes: number
	downvotes: number
	key?: string
}

type ScoreObject = {
	leftBound: number
	rightBound: number
}

export const Votings = () => {
	const [voting, setVoting] = useState<VotingObject>(
		{
			title: 'Voting',
			upvotes: 0,
			downvotes: 0,
		},
		{
			key: 'votings',
			scope: Scopes.Global,
		}
	)
	const [score, setScore] = useState<ScoreObject>(
		{
			leftBound: 0,
			rightBound: 0,
		},
		{
			key: 'score',
			scope: Scopes.Global,
		}
	)

	const upvote = () => {
		setVoting({ ...voting, upvotes: voting.upvotes + 1 })
	}

	const downvote = () => {
		setVoting({ ...voting, downvotes: voting.downvotes + 1 })
	}

	const wilsonScoreInterval = () => {
		const { upvotes, downvotes } = voting
		const n = upvotes + downvotes
		if (n === 0) return 0 // no votes yet

		const z = 1.96 // 95% probability
		const phat = (1 * upvotes) / n
		const left = phat + (z * z) / (2 * n)
		const right = z * Math.sqrt((phat * (1 - phat) + (z * z) / (4 * n)) / n)
		const leftBoundary = (left - right) / (1 + (z * z) / n)
		const rightBoundary = (left + right) / (1 + (z * z) / n)
		setScore({ leftBound: leftBoundary, rightBound: rightBoundary })
	}

	return (
		<ServerSideProps key={`votings-props`} {...voting} upvote={upvote} downvote={downvote} score={score} wilsonScoreInterval={wilsonScoreInterval} />
	)
}
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

The client side is straightforward. We're using material ui, so make sure you have all the required packages installed, otherwise you will encounter errors.

Create a new file under the following path and paste the contents.

_frontend/src/server-components/VotingApp.tsx_

```tsx
import ThumbDownIcon from '@mui/icons-material/ThumbDown'
import ThumbUpIcon from '@mui/icons-material/ThumbUp'
import { Alert, Box, Button, Typography } from '@mui/material'
import { useComponent } from '@state-less/react-client'
import { useEffect } from 'react'

export const VotingApp = () => {
	const [component, { loading, error }] = useComponent('votings', {})

	// we need to call wilsonScoreInterval() every time the upvotes or downvotes change
	useEffect(() => {
		if ((component?.props?.upvotes || component?.props?.downvotes) > 0) {
			component?.props.wilsonScoreInterval()
		}
	}, [component?.props?.upvotes, component?.props?.downvotes])

	if (loading) return <div>Loading...</div>

	return (
		<>
			<Typography variant='h4' align='center' sx={{ my: 2 }} gutterBottom>
				Voting App
			</Typography>
			<Box display='flex' alignItems='center' justifyContent={'center'} flexDirection={'row'} sx={{ my: 2 }}>
				{error && <Alert severity='error'>{error.message}</Alert>}
				<Button variant='contained' color='primary' onClick={() => component?.props.upvote()} startIcon={<ThumbUpIcon />}>
					Upvotes ({component?.props?.upvotes})
				</Button>
				<Typography variant='h5' align='center' sx={{ mx: 2 }}>
					{component?.props?.title}
				</Typography>
				<Button variant='contained' color='secondary' onClick={() => component?.props.downvote()} endIcon={<ThumbDownIcon />}>
					Downvotes ({component?.props?.downvotes})
				</Button>
			</Box>

			<Box display='block' alignItems='center' columnGap={4}>
				<Typography variant='h6' align='center' sx={{ mx: 2 }}>
					Left Bound: {component?.props?.score?.leftBound.toFixed(2)}
				</Typography>

				<Typography variant='h6' align='center' sx={{ mx: 2 }}>
					Right Bound: {component?.props?.score?.rightBound.toFixed(2)}
				</Typography>
			</Box>
		</>
	)
}
```

Once you created the file you can render the Voting component anywhere in your app

```tsx
<VotingApp />
```
