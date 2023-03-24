# Dynamic Pages

In this backend code, the objective is to demonstrate how to create and manage dynamic content using React Server Components. By implementing a simple content management system (CMS) that allows adding, editing, and displaying pages with different paths and content, this example showcases the versatility and usefulness of React Server Components in building web applications. This approach enables developers to efficiently manage content and serve it to users, adapting to their specific needs and preferences.

A headless approach simplifies content management and delivery by decoupling the front-end presentation layer from the back-end data management, enabling seamless integration with various platforms and frameworks.

Let's go ahead and define the serverside component.

_backend/src/components/Pages.tsx_

```tsx
import { Scopes, useState } from "@state-less/react-server";
import { v4 } from "uuid";
import { isValidPath } from "./Navigation";
import { ServerSideProps } from "./ServerSideProps";

type PageObject = {
  id: string | null;
  path: string;
  content: string[];
};

export const DynamicPage = (props, { clientProps }) => {
  const [pages] = useState([], {
    key: "pages",
    scope: Scopes.Client,
  });

  if (!clientProps?.path) {
    return <Page id={null} content={["404"]} path="/404" />;
  }
  const page = pages.find((p) => p.path === clientProps.path);
  if (!page) {
    return <Page id={null} content={["404"]} path={clientProps.path} />;
  }

  return <Page {...page} />;
};

export const Page = ({ id, content, path }: PageObject) => {
  const [page, setPage] = useState<PageObject>(
    {
      id,
      content,
      path,
    },
    {
      key: `page${id}`,
      scope: Scopes.Client,
    }
  );

  const setContent = (newContent) => {
    setPage({ ...page, content: newContent });
  };

  return <ServerSideProps {...page} setContent={setContent} />;
};

export const Pages = () => {
  const [pages, setPages] = useState([], {
    key: "pages",
    scope: Scopes.Client,
  });

  const addPage = (page) => {
    const id = v4();
    const newPage = { ...page, id };
    if (pages.find((p) => p.path === page.path)) {
      throw new Error("Page already exists");
    }
    if (!isValidPath(newPage.path)) {
      throw new Error("Invalid path");
    }
    if (!isValidPage(newPage)) {
      throw new Error("Invalid page");
    }
    setPages([...pages, newPage]);
  };

  return (
    <ServerSideProps addPage={addPage}>
      {pages.map((page) => (
        <Page {...page} />
      ))}
    </ServerSideProps>
  );
};

const isValidPage = (page): page is PageObject => {
  return page.id && page.path && page.content;
};
```

_backend/src/index.tsx_

```tsx
import { DynamicPage, Pages } from "./components/Pages";

<Server key="server">
  <Pages key="pages" />
  <DynamicPage key="page" />
</Server>;
```

In order to render a single dynamic page with a given path, such as '/test', you can create a ServerPageContainer component that takes a path prop. This component uses the useComponent hook to connect with the backend page component, passing the desired path as a prop. When the data is fetched, the component renders the ServerPage component with the received data.

To manage and display all available pages on the frontend, create a ServerPages component. This component also utilizes the useComponent hook to connect with the backend pages component. It provides input fields for adding new pages with their corresponding paths and content. Users can add pages, and existing pages are displayed in a list using the ServerPage component. If there are any errors, they will be displayed in an Alert component for better user feedback.

Let's create the frontend code.

_frontend/src/server-components/ServerPages.tsx_

```tsx
import { useComponent } from "@state-less/react-client";
import {
  TextField,
  Button,
  Accordion,
  AccordionSummary,
  AccordionDetails,
  CardActions,
  CardContent,
  Card,
  Alert,
} from "@mui/material";
import { useState } from "react";
import { errors, isValidPath } from "./ServerNavigation";
export const ServerPages = () => {
  const [component, { loading, error }] = useComponent("pages", {});

  const [path, setPath] = useState("");
  const [content, setContent] = useState("");

  const errs = [
    ["Invalid path", !isValidPath(path)],
    ["Path must start with /", path[0] !== "/"],
    ["Path and content are required", !content || !path],
  ];
  const contentMessage = errors(errs.slice(2));
  const pathMessage = errors(errs);

  return (
    <>
      {error && <Alert severity="error">{error.message}</Alert>}
      <Card>
        <CardContent>
          <TextField
            label="Path"
            onChange={(e) => setPath(e.target.value)}
            error={pathMessage}
            helperText={pathMessage}
          />
          <TextField
            fullWidth
            rows={3}
            multiline
            label="Content"
            onChange={(e) => setContent(e.target.value)}
            error={contentMessage}
            helperText={contentMessage}
          />
        </CardContent>
        <CardActions>
          <Button
            onClick={() =>
              component.props.addPage({
                path,
                content: [content],
              })
            }
          >
            ADD
          </Button>
        </CardActions>
      </Card>
      {component?.children?.map((page) => {
        return <ServerPage {...page.props} />;
      })}
    </>
  );
};

export const ServerPageContainer = ({ path }) => {
  const [component, { loading, error }] = useComponent("page", {
    props: { path },
  });

  if (loading) return <div>Loading...</div>;

  return <ServerPage {...component.props} />;
};

const ServerPage = ({ path, content }) => {
  return (
    <Accordion>
      <AccordionSummary>{path}</AccordionSummary>
      <AccordionDetails>{content}</AccordionDetails>
    </Accordion>
  );
};
```

_frontend/src/App.tsx_

```tsx
import {
  ServerPageContainer,
  ServerPages,
} from "../../server-components/ServerPages";

const App = () => {
  return <ServerPageContainer path={"/test"} />;
};
```

Go ahead and create a page with a given path. We'll connect the navigation and the pages in the next step.
