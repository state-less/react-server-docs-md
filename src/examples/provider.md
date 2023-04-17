# Apollo Provider

In order to implement the component examples from the [examples](/examples) pages, you need to wrap your application in an in an `<ApolloProvider client={client} />`. React Server will use the provided client.

In *my-frontend/src/App.tsx* wrap your entire application in the provider.
```tsx
import { localClient } from './lib/client';
function App() {
  return (
    <div className="App">
      <ApolloProvider
        client={process.env.NODE_ENV === 'production' ? client : localClient}
      >
        {/* Your code here */}
      </ApolloProvider>
    </div>
  );
}
```
_Note: You can still override the provided client if you pass one in the options_
