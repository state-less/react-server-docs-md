# Authentication
## Server Side
Authentication is straightforward with React Server. React Server provides an `authenticate` util to check the headers for a valid jwt token.

All you need to do to authenticate a user is calling `authenticate` in your serverside component and if the user doesn't have a valid Bearer token in the headers authorization, rendering will throw an error.

*backend/src/components/Session*
```tsx
import { authenticate, isClientContext } from '@state-less/react-server';
import { ServerSideProps } from './ServerSideProps';
import { JWT_SECRET } from '../config';

export const Session = (props, { context = { headers: {} } }) => {
    const { headers } = context;

    if (!isClientContext(context)) {
        return <ServerSideProps session={null} />;
    }

    const session = authenticate(headers, JWT_SECRET);
    return <ServerSideProps session={session} />;
};
```

## Client Side

React-client comes with an `AuthenticationProvider` which you can use to simplify the authentication process.

To get started, wrap your App in an `AuthenticationProvider`. This provider exposes an `authenticate` function that needs to be called with a strategy and a data option.

The data required depends on the strategy used to authenticate the current user.

For instance, with a Google OAuth login, you need to pass `AuthStrategy.Google` as the *strategy*, and *data* expects an object with `{accessToken, idToken}`.
Both can be obtained by performing a client-side OAuth login with Google.

To implement a Google OAuth login, you can use the library react-google-login, which provides a `<GoogleLogin />` component that you can use to set up your client-side OAuth flow.

Here's an example of how to log in and obtain a valid session from React Server using react-google-login and authenticate from `@state-less/react-client`:

```tsx
import { Avatar, Button } from '@mui/material';
import { authContext } from '@state-less/react-client';
import { useContext } from 'react';
import GoogleLogin from 'react-google-login';
import GoogleIcon from '@mui/icons-material/Google';

const logError = (response) => {
  console.log(response);
};

export const LoggedInGoogleButton = (props) => {
  const { session, authenticate } = useContext(authContext);

  if (session.strategy !== 'google') {
    return null;
  }

  const decoded = session.strategies.google.decoded;
  return (
    <Button color="secondary">
      <Avatar
        src={decoded.picture}
        sx={{ width: 24, height: 24, mr: 1 }}
      ></Avatar>
      {decoded.name}
    </Button>
  );
};

export const GoogleLoginButton = () => {
  const { session, authenticate } = useContext(authContext);

  return session?.strategy === 'google' ? (
    <LoggedInGoogleButton />
  ) : (
    <GoogleLogin
      clientId="534678949355-odq15l4236372p864f63ci14g794sfqf.apps.googleusercontent.com"
      buttonText="Login"
      onSuccess={({ accessToken, tokenId }) => {
        console.log('Authenticating with google');
        authenticate({
          strategy: 'google',
          data: { accessToken, idToken: tokenId },
        });
      }}
      render={(props) => {
        return (
          <Button color="secondary" {...props}>
            <GoogleIcon sx={{ mr: 1 }} />
            Login
          </Button>
        );
      }}
      onFailure={logError}
      cookiePolicy={'single_host_origin'}
    />
  );
};

```

As demonstrated, it only requires a single call to `authenticate` to log in to React Server, offering a versatile approach for logging in with various providers.