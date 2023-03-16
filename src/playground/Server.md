# Server
What you see above is a very simple server-side component. It provides information about the running server. (version, os, etc.) and should be the entrypoint of your server.   

*/backend/src/index.tsx*
```
const server = <Server>
    // your components here
</Server>;
```

*/frontend/src/server-components/Server.tsx*
```
export const Server = () => {
  const [props] = useComponent('server', {});
  
  return (
    <div>
      <Alert severity="success">Server running react-server v{props.version}</Alert>
    </div>
  );
};
  ```
  
  You can access the server properties in your app, by using the `useComponent` hook.

  ## useComponent
  ```
  const [props] = useComponent('server');
  ```

  ## GraphQl
  ```
  query GetServerInfo($key: ID!, $props: JSON) {
    renderComponent(key: $key, props: $props) {
      rendered {
        ... on Server {		
          platform
          version
          uptime
        }
      }
    }
  }
  ```