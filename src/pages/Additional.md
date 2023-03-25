# Additional Topics and Best Practices

## Error Handling

Handling errors in React Server is easy and intuitive. Simply throw errors within components or functions, and they will be transmitted to the client via GraphQL, where they are managed gracefully. Access the error from the `useComponent` call like this: `const [value, { error }] = useComponent('hello-world')`.

React Server does not catch errors automatically. Instead, errors bubble up to the main render call. This design choice ensures that you have full control over error handling within your application.

If you have any valuable ideas, refer to this [issue](https://github.com/state-less/react-server/issues/18).

## Hydrating

You can hydrate `useComponent` calls from data you obtained from a BFF such as Next.js or from a parent component that renders it's children.

This is important if you need reactivity in child components. If you don't hydrate the `useComponent` call, the child component needs to wait for their props to load from the server before displaying any data. 

By hydrating the nested `useComponent` call, you can make sure that the hook returns the hydration data synchronously. This means that any calls to the hook during SSR or child rendering have their data available immediately. 


```tsx
const Todos = () => {
    const [component, {error, loading}] = useComponent('todos');

    return <div>
        {component?.children.map((child) => (<ChildComponent data={child} />))}
    </div>
}

const Todo = ({data}) => {
    const [component, {error, loading}] = useComponent(data.key, { data });

    return <div>
        {JSON.stringify(component)}
    </div>
}
```
## Security Considerations

React Server is publicly accessible via a GraphQL endpoint, which necessitates the careful handling of sensitive data in components.

To prevent unauthorized code execution, it is crucial to call `authenticate` as early as possible within your component.

### SQL Injections
Since React Server does not currently use an SQL backend, SQL injection attacks are not a concern.

### CSRF
At present, CSRF is not managed. However, `@state-less/react-client` will automatically handle CSRF protection in future releases.

### XSS
React Server is built for use with React, which inherently guards against cross-site scripting (XSS) attacks. React automatically escapes values in JSX and sanitizes data rendered to the DOM, thereby reducing the risk of XSS attacks.


### Authentication

In React Server, authentication is performed using bearer tokens and OAuth. Here's an explanation of how this works, with a focus on security considerations:

1. Bearer tokens: Bearer tokens are used to authenticate the user making a request to the React Server. A bearer token is a cryptographically signed string that is sent as part of the HTTP Authorization header in the format Bearer <token>. The token contains information about the user and their permissions, allowing the server to verify the user's identity and grant access to resources based on the user's permissions.

2. OAuth: OAuth is an open standard for authorization that allows users to grant third-party applications access to their information on other services, without sharing their credentials. OAuth relies on access tokens, which are issued by an authorization server and can be used by the application to access the user's information. In the context of React Server, OAuth can be used as a way to authenticate users with external services, such as Google, Facebook, or other identity providers.

When an incoming request arrives at React Server, the server checks the Authorization header for a valid bearer token. If a token is found, React Server validates the token to ensure it is authentic and not expired. If the token is valid, the server extracts the user's information and permissions from the token and proceeds with processing the request.

In the case of OAuth, the React Server relies on the client-side library to handle the OAuth flow with the external service. Once the client has obtained an access token and an ID token from the external service, it can send these tokens to the React Server for authentication. React Server then verifies the tokens with the external service and creates a new session for the user.

* Always use HTTPS to ensure that the bearer tokens and OAuth tokens are transmitted securely between the client and the server, preventing eavesdropping or man-in-the-middle attacks.
* ~~Implement a proper token expiration policy. Short-lived tokens can help minimize the potential impact of a token being compromised. You can use refresh tokens to obtain new access tokens without requiring the user to re-authenticate.~~ (NYI*)

*\*NYI - Not yet implemented*

## Performance and Scalability

React Server is built on well-established technologies, ensuring robustness and performance. While it has not yet been micro-optimized, no known performance limitations have been identified.

In the future, React Server can be fully run on AWS Lambda, enabling practically unlimited vertical scaling (NYI*).

React Server can also be configured to run in a mix of stateful and stateless servers. This approach allows API calls to be routed through AWS Lambda, optimizing render performance by using caching servers near the client and handling function calls through AWS Lambda for high availability and unlimited scalability (NYI*).

There are currently no benchmarks. If you'd like to contribute, please refer to this [issue](https://github.com/state-less/react-server/issues/19).

*\*NYI - Not yet implemented*

## Testing

Test coverage needs improvement. At present, only rudimentary tests are available. Any assistance in writing tests would be greatly appreciated. For more information, please refer to this [issue](https://github.com/state-less/react-server/issues/20).

We use `jest` for running unit tests. If you're familiar with jest, adding tests to support React Server is straightforward.

## Troubleshooting

Feel free to open issues if you encounter difficulties or errors. React Server is currently in a pre-alpha release, and the codebase may change frequently.

As of now, there are no known common errors or causes, but they will be documented here in the future. You can also refer to this [issue](https://github.com/state-less/react-server/issues/21).

## Community and Support

If you're interested in this project, welcome aboard! Be sure to review our contribution guidelines if you plan to contribute.

We aim to build a supportive and helpful community around React Server, but as it's a brand new project, activity is currently limited.

For questions, bug reports, or contributions, head over to [GitHub](https://github.com/state-less/react-server/issuesF) and raise an issue.

If you need help building something with React Server, consider checking out [Stackoverflow](stackoverflow.com) and [Reddit](https://www.reddit.com/r/learnprogramming). The community is very supportive—just ask your questions, and someone will help you.

You can also visit the ##javascript IRC Channel on Quakenet.

We have a [Discord Server](https://discord.gg/MJuVT4kE) available for any questions or chatting with like-minded individuals.

## More
### Get the documentation running yourself

Interested in the documentation page itself? You can run it your own or contribute here:

```bash
git clone https://github.com/C5H8NNaO4/react-server-docs
cd react-server-docs
yarn install
yarn dev
```

Now get a server running and start hacking. 

```bash
git clone https://github.com/state-less/clean-starter.git -b react-server my-server
cd my-server
git remote remove origin
yarn install
yarn start
```

We are happy for any contributions.