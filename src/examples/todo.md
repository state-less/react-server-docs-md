# Todo List

Let's create an example Todo App, which demonstrates how to build a functional and interactive application using React Server, complete with real-time updates and seamless state management.

You need to install material ui if you want to build this yourself. 

```
yarn add @mui/material @mui/icons-material
yarn add @emotion/react @emotion/styled
```

*backend/src/components/Todos.tsx*
```tsx
import { Scopes, useState } from '@state-less/react-server';
import { v4 } from 'uuid';
import { ServerSideProps } from './ServerSideProps';

type TodoObject = {
    id: string | null;
    title: string;
    completed: boolean;
};

export const Todo = ({ id, completed, title }: TodoObject) => {
    const [todo, setTodo] = useState<TodoObject>(
        {
            id,
            completed,
            title,
        },
        {
            key: `page${id}`,
            scope: Scopes.Client,
        }
    );

    const toggle = () => {
        setTodo({ ...todo, completed: !todo.completed });
    };

    return <ServerSideProps key={`${id}-todo`} {...todo} toggle={toggle} />;
};

export const Todos = () => {
    const [todos, setTodos] = useState<TodoObject[]>([], {
        key: 'todos',
        scope: Scopes.Client,
    });

    const addTodo = (todo: TodoObject) => {
        const id = v4();
        const newTodo = { ...todo, id };

        if (!isValidTodo(newTodo)) {
            throw new Error('Invalid todo');
        }
        setTodos([...todos, newTodo]);
    };

    const removeTodo = (id: string) => {
        setTodos(todos.filter((todo) => todo.id !== id));
    };
    return (
        <ServerSideProps key="todos-props" add={addTodo} remove={removeTodo}>
            {todos.map((todo) => (
                <Todo {...todo} />
            ))}
        </ServerSideProps>
    );
};

const isValidTodo = (todo): todo is TodoObject => {
    return todo.id && todo.title && 'completed' in todo;
};
```

In this example, both Todos and Todo are React Server components, which means they manage and render their state on the server-side. The Todos component is responsible for maintaining a list of Todo items, with each item having a unique ID, title, and completion status. The state of the Todos component is managed using React Server's useState with a Scopes.Client scope, ensuring that each connected client has their own separate list of todos and individual todo items' states. This allows users to interact with the app independently without affecting others' data.

The Todo component represents an individual todo item and manages its own state, such as the completion status. Similar to the Todos component, the Todo component also uses React Server's useState with Scopes.Client scope to manage its state. By using server-side state management, these components can efficiently update and synchronize their state with clients, ensuring that the UI is always up-to-date and reactive to changes. This approach simplifies state management and reduces the need for manual refetching or polling mechanisms.

Let's head to the frontend implementation.

*frontend/src/components/TodoApp.tsx*
```tsx
import {
  Box,
  Button,
  Card,
  CardHeader,
  Checkbox,
  IconButton,
  List,
  ListItem,
  ListItemIcon,
  ListItemSecondaryAction,
  ListItemText,
  TextField,
} from '@mui/material';
import EditIcon from '@mui/icons-material/Edit';
import RemoveCircleIcon from '@mui/icons-material/RemoveCircle';
import { useComponent } from '@state-less/react-client';
import { useEffect, useState } from 'react';

export const TodoApp = (props) => {
  const [component, { loading }] = useComponent('todos', {});
  const [title, setTitle] = useState('');
  const [edit, setEdit] = useState(false);

  if (loading) {
    return null;
  }
  
  return (
    <Card>
      <CardHeader
        title={
          <Box sx={{ display: 'flex', alignItems: 'center' }}>
            <TextField
              value={title}
              label="Title"
              onChange={(e) => setTitle(e.target.value)}
            />
            <Button
              onClick={() => {
                setTitle('');
                component.props.add({ title, completed: false });
              }}
            >
              Add
            </Button>
          </Box>
        }
        action={
          <IconButton onClick={() => setEdit(!edit)}>
            <EditIcon />
          </IconButton>
        }
      ></CardHeader>
      <List>
        {component?.children.map((todo, i) => (
          <TodoItem
            key={i}
            todo={todo.key}
            edit={edit}
            remove={component?.props?.remove}
          />
        ))}
      </List>
    </Card>
  );
};

const TodoItem = (props) => {
  const { todo, edit, remove } = props;
  const [component, { loading }] = useComponent(todo, {});

  if (loading) return null;

  return (
    <ListItem>
      {edit && (
        <ListItemIcon>
          <IconButton onClick={() => remove(component.props.id)}>
            <RemoveCircleIcon />
          </IconButton>
        </ListItemIcon>
      )}
      <ListItemText primary={component.props.title} />
      <ListItemSecondaryAction>
        <Checkbox
          checked={component?.props.completed}
          onClick={() => {
            console.log('TODO', todo);
            component?.props.toggle();
          }}
        />
      </ListItemSecondaryAction>
    </ListItem>
  );
};
```
The TodoApp component in the frontend code utilizes React Server components to display and interact with a list of todos. It renders a form for adding new todos and a list of existing todos. The TodoItem component is responsible for rendering individual todo items, allowing users to toggle their completion status and, when in edit mode, remove the todo items from the list. Both components use the useComponent hook to access and interact with the state and functionality provided by the corresponding backend components.

Don't be intimidated by the length of the frontend code. It's a bit verbose because we're using material ui to make it look nice.

