# Additional Topics and Best Practices

## Error Handling

React Server makes it simple and intuitive to handle errors. To throw an error within a component or function, simply use the throw statement. The error will be transmitted to the client via GraphQL and can be gracefully managed there. To access the error from the `useComponent` call, use the following syntax: `const [value, { error }] = useComponent('hello-world')`.

It's worth noting that React Server doesn't automatically catch errors. Instead, errors bubble up to the main render call. This approach ensures that you have complete control over error handling in your application.

If you have any valuable ideas, refer to this [issue](https://github.com/state-less/react-server/issues/18).

## Hydrating

To enable reactivity in child components, you can hydrate `useComponent` calls with data obtained from a Backend-for-Frontend (BFF) like Next.js, or from a parent component that renders its children.

If you don't hydrate the `useComponent` the child component will need to wait for its props to load from the server before displaying any data. By hydrating the nested `useComponent` call, you can ensure that the hook returns the hydration data synchronously. As a result, any calls to the hook during Server-Side Rendering (SSR) or child rendering will have immediate access to their data.


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

To ensure secure authentication in React Server, two methods are used: bearer tokens and OAuth. Here's how they work and some security considerations to keep in mind:

1. Bearer tokens: To authenticate users making requests to the React Server, bearer tokens are used. These tokens are cryptographically signed strings that are included in the HTTP Authorization header in the format "Bearer <token>". They contain information about the user and their permissions, allowing the server to verify the user's identity and grant access to resources based on their permissions.

2. OAuth: OAuth is an authorization standard that allows third-party applications access to user information on other services without sharing credentials. Access tokens, issued by an authorization server, are used to access user information. In React Server, OAuth can be used to authenticate users with external services such as Google, Facebook, or other identity providers.

When a request arrives at React Server, the server checks the Authorization header for a valid bearer token. The token is validated to ensure it is authentic and not expired. If it is valid, the server extracts user information and permissions from the token and processes the request.

For OAuth, the client-side library handles the OAuth flow with the external service. Once the client obtains an access token and an ID token from the external service, it sends them to React Server for authentication. The server then verifies the tokens with the external service and creates a new session for the user.

* To ensure secure authentication, HTTPS should always be used to prevent eavesdropping or man-in-the-middle attacks when transmitting bearer tokens and OAuth tokens between the client and the server.
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
