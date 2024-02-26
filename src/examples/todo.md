# Todo List

Let's embark on creating a Todo App example to illustrate the power of React Server in crafting functional and interactive applications.

For building this project yourself, ensure you have Material UI installed to leverage its components effectively.

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

In this example, both Todos and Todo are React Server components, responsible for managing and rendering their respective states on the server-side. The Todos component maintains a list of Todo items, each comprising a unique ID, title, and completion status.

The state of the Todos component is efficiently managed using React Server's useState with a Scopes.Client scope. This ensures that each connected client possesses its own isolated list of todos and individual todo items' states. Consequently, users can interact with the app independently, without any interference with others' data.

On the other hand, the Todo component represents an individual todo item and autonomously manages its state, including the completion status. Similarly, it employs React Server's useState with Scopes.Client scope to handle its state. Leveraging server-side state management allows these components to seamlessly update and synchronize their states with clients. As a result, the UI remains consistently up-to-date and responsive to changes without the need for manual data fetching or polling mechanisms.

Now, let's delve into the frontend implementation.

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
The TodoApp component in the frontend code leverages React Server components to seamlessly manage and present a list of todos. It orchestrates the rendering of a form for adding new todos alongside a display of existing todos. Within this structure, the TodoItem component assumes responsibility for rendering individual todo items. It empowers users to toggle their completion status and, when in edit mode, remove the todo items from the list. To facilitate this functionality, both components harness the useComponent hook, which grants access to and facilitates interaction with the state and features offered by the corresponding backend components.

Although the frontend code may appear lengthy, its verbosity is primarily attributed to the integration of material ui, which enhances the visual appeal of the application. Don't let its size deter you; the comprehensive styling enriches the user experience while maintaining the simplicity and effectiveness of the application's core functionality.

