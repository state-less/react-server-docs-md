# Rendering Dynamic Content

The DynamicPageExample component illustrates the dynamic rendering of content based on user navigation. Leveraging the useComponent hook, it interfaces with the backend navigation component to display a list of available navigation entries. Users can interact by clicking on navigation items, dynamically updating the displayed content. To exhibit this functionality, the ServerPageContainer component fetches and renders the page content linked to the selected path. Notably, client-side props are passed to the server-side component to retrieve the relevant content, eliminating the need for additional backend logic.

```tsx
import {
  Box,
  List,
  ListItem,
  ListItemText,
  Paper,
  TextField,
} from "@mui/material";
import { useComponent } from "@state-less/react-client";
import { useState } from "react";
import { ServerPageContainer } from "../../server-components/ServerPages";

export const DynamicPageExample = () => {
  const [navigation, { loading, error }] = useComponent("navigation", {});
  const [path, setPath] = useState("/");

  return (
    <Box sx={{ display: "flex" }}>
      <Paper square>
        <List sx={{ minWidth: "250px", marginTop: "50px" }}>
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
      <Box sx={{ width: "100%" }}>
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
