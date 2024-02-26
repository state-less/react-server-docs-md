## Let's build something

To offer you a comprehensive understanding of React Server's capabilities, let's explore rendering dynamic content by constructing a basic scaffold for a Content Management System (CMS).

### The Navigation

First, let's initiate the construction of a headless navigation system, which we can readily utilize.

Initially, we'll forgo the incorporation of authentication. For the time being, we'll assign admin privileges to all users. Implementing authentication can be seamlessly integrated later.

Establishing a server-side navigation system doesn't entail complex logic. Primarily, we require CRUD (Create, Read, Update, Delete) functionality to manage navigation entries.

Keeping the approach straightforward, let's commence by crafting a server-side component.

_backend/src/components/Navigation_

```tsx
import { Scopes, useState } from "@state-less/react-server";
import { ServerSideProps } from "./ServerSideProps";
import { v4 } from "uuid";

type NavigationEntry = {
  id: string;
  path: string;
  title: string;
};

export const isEntry = (entry: any): entry is NavigationEntry => {
  return entry.id && entry.path && entry.title;
};

/** This should check if the path contains a / and also that it doesn't contain any special characters */
const isValidPath = (path: string) => {
  return /^\/([0-9A-Za-z_\-][\/]?)*$/.test(path);
};

export const Navigation = () => {
  const [entries, setEntries] = useState<NavigationEntry[]>([], {
    key: "entries",
    scope: Scopes.Client,
  });

  const addEntry = (entry) => {
    const id = v4();
    const newEntry = { ...entry, id };

    if (!isEntry(newEntry)) {
      throw new Error("Invalid entry");
    }

    if (!isValidPath(newEntry.path)) {
      throw new Error("Invalid path");
    }

    if (entries.find((e) => e.path === entry.path)) {
      throw new Error("Entry already exists");
    }
    setEntries([...entries, newEntry]);
  };

  const removeEntry = (id) => {
    setEntries(entries.filter((entry) => entry.id !== id));
  };

  return (
    <ServerSideProps
      entries={entries}
      addEntry={addEntry}
      removeEntry={removeEntry}
    />
  );
};
```

Head over to your client side code and create a `ServerNavigation.tsx` file under `src/server-components/`.

The frontend code looks a bit more verbose, but don't get intimidated by it. It's actually very simple.

_frontend/src/server-components/ServerNavigation_

```tsx
import {
  IconButton,
  List,
  ListItem,
  ListItemSecondaryAction,
  ListItemText,
  TextField,
  Alert,
  Button,
  LinearProgress,
} from "@mui/material";
import { useComponent } from "@state-less/react-client/dist/index";
import { useState } from "react";
import { localClient } from "../lib/client";

import CloseIcon from "@mui/icons-material/Close";

/** This should check if the path contains a / and also that it doesn't contain any special characters */
const isValidPath = (path: string) => {
  return /^\/([0-9A-Za-z_\-][\/]?)*$/.test(path);
};

const errors = (messages) => {
  return messages
    .filter(([, isErr]) => isErr)
    .map(([msg]) => msg)
    .join(", ");
};
export const ServerNavigation = () => {
  const [component, { error, loading }] = useComponent("navigation", {});
  const [title, setTitle] = useState("");
  const [path, setPath] = useState("");

  const errs = [
    ["Invalid path", !isValidPath(path)],
    ["Path must start with /", path[0] !== "/"],
    ["Title and path are required", !title || !path],
  ];
  const titleMessage = errors(errs.slice(2));
  const pathMessage = errors(errs);
  return (
    <div>
      {loading && <LinearProgress />}
      {error && <Alert severity="error">{error.message}</Alert>}
      <TextField
        label="Title"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        error={!!titleMessage}
        helperText={titleMessage}
      ></TextField>
      <TextField
        label="Path"
        value={path}
        onChange={(e) => setPath(e.target.value)}
        error={!!pathMessage}
        helperText={pathMessage}
      ></TextField>
      <List>
        {component?.props?.entries.map((entry) => {
          return (
            <ListItem>
              <ListItemText primary={entry.title} secondary={entry.path} />
              <ListItemSecondaryAction>
                <IconButton
                  onClick={() => component.props.removeEntry(entry.id)}
                >
                  <CloseIcon />
                </IconButton>
              </ListItemSecondaryAction>
            </ListItem>
          );
        })}
      </List>
      <Button onClick={() => component.props.addEntry({ title, path })}>
        Add Entry
      </Button>
    </div>
  );
};
```
