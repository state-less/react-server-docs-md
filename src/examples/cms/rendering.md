# Rendering Dynamic Content

The DynamicPageExample component aims to demonstrate the capabilities of rendering dynamic content based on user navigation. By using the useComponent hook, it connects to the backend navigation component and renders a list of available navigation entries. Users can click on the navigation items, which in turn sets the path and updates the displayed content. To showcase the dynamic content, the ServerPageContainer component is used, fetching and rendering the page content associated with the selected path which is passed as client-side props to the server-side component in order to fetch the appropriate content

Note that we don't need any backend code in this example as we already have our navigation and the pages.

```
import {
  Box,
  List,
  ListItem,
  ListItemText,
  Paper,
  TextField,
} from '@mui/material';
import { useComponent } from '@state-less/react-client';
import { useState } from 'react';
import { ServerPageContainer } from '../../server-components/ServerPages';

export const DynamicPageExample = () => {
  const [navigation, { loading, error }] = useComponent('navigation', {});
  const [path, setPath] = useState('/');

  return (
    <Box sx={{ display: 'flex' }}>
      <Paper square>
        <List sx={{ minWidth: '250px', marginTop: '50px' }}>
          {navigation?.props?.entries.map((child) => {
            return (
              <ListItem
                selected={path === child.path}
                button
                onClick={(e) => setPath(child.path)}
              >
                <ListItemText
                  primary={child.title}
                  secondary={child.path}
                ></ListItemText>
              </ListItem>
            );
          })}
        </List>
      </Paper>
      <Box sx={{ width: '100%' }}>
        <TextField
          fullWidth
          value={path}
          onChange={(e) => setPath(e.target.value)}
        ></TextField>
        <ServerPageContainer path={path} />
      </Box>
    </Box>
  );
};
```

As you can see, it's straightforward to render dynamic content with React Server Components by passing props between server and client.
