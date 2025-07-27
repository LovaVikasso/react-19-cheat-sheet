# React 19 Cheat Sheet

- [1. Getting Started](#getting-started)
  - [1.1 Installation](#installation)
  - [1.2 New Project](#new-project)
  - [1.3 Launch Project](#launch-project)
- [2. Core Concepts](#core-concepts)
  - [2.1 Components](#components)
  - [2.2 TSX](#tsx)
  - [2.3 Props](#props)
- [3. Hooks](#hooks)
  - [3.1 State Hooks](#state-hooks)
  - [3.2 Effect Hooks](#effect-hooks)
  - [3.3 Context Hooks](#context-hooks)
  - [3.4 Ref Hooks](#ref-hooks)
  - [3.5 Performance Hooks](#performance-hooks)
  - [3.6 New React 19 Hooks](#new-react-19-hooks)
- [4. Forms](#forms)
  - [4.1 Actions](#actions)
  - [4.2 Form Hooks](#form-hooks)
  - [4.3 Form Validation](#form-validation)
- [5. Server Components](#server-components)
  - [5.1 Server vs Client Components](#server-vs-client-components)
  - [5.2 Data Fetching](#data-fetching)
- [6. Suspense](#suspense)
  - [6.1 Loading States](#loading-states)
  - [6.2 Error Boundaries](#error-boundaries)
- [7. Context](#context)
  - [7.1 Context API](#context-api)
  - [7.2 use() Hook](#use-hook)
- [8. Performance](#performance)
  - [8.1 Memoization](#memoization)
  - [8.2 Code Splitting](#code-splitting)
- [9. Metadata](#metadata)
- [10. Best Practices](#best-practices)

## Getting Started

### Installation

```bash
# Create new React 19 project with Vite
npm create vite@latest my-react-app -- --template react

# Or with Next.js (recommended for React 19 features)
npx create-next-app@latest my-next-app

# Install React 19 in existing project
npm install react@latest react-dom@latest
```

### New Project

```bash
# Vite + React
npm create vite@latest my-app -- --template react

# Next.js (recommended)
npx create-next-app@latest my-app

# Vite + TypeScript + React
npm create vite@latest my-app -- --template react-ts
```

### Launch Project

```bash
# Development
npm run dev
# or
yarn dev
# or
pnpm dev

# Production build
npm run build
# or
yarn build
# or
pnpm build

# Preview production build
npm run preview
# or
yarn preview
# or
pnpm preview
```

## Core Concepts

### Components

```tsx
// Function Component (recommended)
type WelcomeProps = { name: string };

const Welcome = ({ name }: WelcomeProps) => <h1>Hello, {name}!</h1>;

// Arrow Function Component (эквивалентно выше)
const WelcomeArrow: React.FC<WelcomeProps> = ({ name }) => (
  <h1>Hello, {name}!</h1>
);

// Class Component (legacy)
interface WelcomeState {}

class Welcome extends React.Component<WelcomeProps, WelcomeState> {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

### TSX

```tsx
// Basic TSX
const element: JSX.Element = <h1>Hello, world!</h1>;

// TSX with expressions
const name: string = 'John';
const element: JSX.Element = <h1>Hello, {name}!</h1>;

// TSX with attributes
type User = {
  avatarUrl: string;
  name: string;
};

const user: User = { avatarUrl: '/avatar.jpg', name: 'John' };
const element: JSX.Element = <img src={user.avatarUrl} alt={user.name} />;

// TSX with children
const element: JSX.Element = (
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>
);

// TSX with event handlers
const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
  console.log('Button clicked!');
};

const buttonElement: JSX.Element = (
  <button onClick={handleClick} type="button">
    Click me
  </button>
);

// TSX with conditional rendering
const isLoggedIn: boolean = true;
const conditionalElement: JSX.Element = (
  <div>{isLoggedIn ? <p>Welcome back!</p> : <p>Please log in</p>}</div>
);

// TSX with list rendering
const items: string[] = ['Apple', 'Banana', 'Orange'];
const listElement: JSX.Element = (
  <ul>
    {items.map((item, index) => (
      <li key={index}>{item}</li>
    ))}
  </ul>
);

// TSX with styled components (if using styled-components)
type StyledButtonProps = {
  primary?: boolean;
  size?: 'small' | 'medium' | 'large';
};

const StyledButton = styled.button<StyledButtonProps>`
  background: ${(props) => (props.primary ? 'blue' : 'white')};
  font-size: ${(props) => {
    switch (props.size) {
      case 'small':
        return '12px';
      case 'large':
        return '20px';
      default:
        return '16px';
    }
  }};
`;
```

### Props

```tsx
// Passing props
type WelcomeProps = {
  name: string;
  age: number;
  children: React.ReactNode;
};

const Welcome = ({ name, age, children }: WelcomeProps) => (
  <div>
    <h1>Hello, {name}!</h1>
    <p>Age: {age}</p>
    {children}
  </div>
);

// Using component with props
<Welcome name="John" age={25}>
  <p>This is children content</p>
</Welcome>;

// Props spreading
type ButtonProps = React.ButtonHTMLAttributes<HTMLButtonElement> & {
  variant?: 'primary' | 'secondary';
};

const Button = ({ variant = 'primary', ...props }: ButtonProps) => (
  <button className={variant} {...props} />
);
```

## Hooks

### State Hooks

#### useState

```tsx
import { useState } from 'react';

type User = { name: string; email: string };

const Counter = () => {
  const [count, setCount] = useState(0);
  const [user, setUser] = useState<User>({ name: '', email: '' });

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount((prev) => prev + 1)}>
        Increment (functional)
      </button>
    </div>
  );
};
```

#### useReducer

```tsx
import { useReducer } from 'react';

type CounterState = { count: number };
type CounterAction = { type: 'increment' } | { type: 'decrement' };

const initialState: CounterState = { count: 0 };

const reducer = (state: CounterState, action: CounterAction): CounterState => {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
};
```

### Effect Hooks

#### useEffect

```tsx
import { useEffect, useState } from 'react';

type User = { id: string; name: string; email: string };
type UserProfileProps = { userId: string };

const UserProfile = ({ userId }: UserProfileProps) => {
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]);

  useEffect(() => {
    console.log('Component mounted');
  }, []);

  useEffect(() => {
    console.log('Component rendered');
  });

  useEffect(() => {
    const timer = setInterval(() => {
      console.log('Timer tick');
    }, 1000);
    return () => clearInterval(timer);
  }, []);

  return <div>{user?.name}</div>;
};
```

#### useLayoutEffect

```tsx
import { useLayoutEffect, useState } from 'react';

type TooltipProps = {
  children: React.ReactNode;
  position: { x: number; y: number };
};

type TooltipStyle = { top: number; left: number };

const Tooltip = ({ children, position }: TooltipProps) => {
  const [tooltipStyle, setTooltipStyle] = useState<TooltipStyle>({
    top: 0,
    left: 0,
  });

  useLayoutEffect(() => {
    const element = document.getElementById('tooltip-target');
    if (element) {
      const rect = element.getBoundingClientRect();
      setTooltipStyle({
        top: rect.top + rect.height,
        left: rect.left,
      });
    }
  }, [position]);

  return <div style={tooltipStyle}>{children}</div>;
};
```

### Context Hooks

#### useContext

```tsx
import { createContext, useContext } from 'react';

type Theme = 'light' | 'dark';
const ThemeContext = createContext<Theme>('light');

const ThemedButton = () => {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Themed Button</button>;
};

const App = () => (
  <ThemeContext.Provider value="dark">
    <ThemedButton />
  </ThemeContext.Provider>
);
```

#### use() - New React 19 Hook

```tsx
import { use } from 'react';

type User = { name: string; email: string };
type UserProfileProps = { userPromise: Promise<User> };

const UserProfile = ({ userPromise }: UserProfileProps) => {
  const user = use(userPromise);
  const theme = use(ThemeContext);
  return (
    <div className={theme}>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
};

// With async components
type UserDataProps = { userId: string };
export const UserData = async ({ userId }: UserDataProps) => {
  const user = await fetchUser(userId);
  return <UserProfile user={user} />;
};
```

### Ref Hooks

#### useRef

```tsx
import { useRef, useEffect } from 'react';

const TextInputWithFocusButton = () => {
  const inputRef = useRef<HTMLInputElement>(null);
  const focusInput = () => inputRef.current?.focus();
  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus the input</button>
    </>
  );
};

const Timer = () => {
  const intervalRef = useRef<NodeJS.Timeout | undefined>(undefined);
  useEffect(() => {
    intervalRef.current = setInterval(() => {
      console.log('Timer tick');
    }, 1000);
    return () => intervalRef.current && clearInterval(intervalRef.current);
  }, []);
};
```

#### useImperativeHandle

```tsx
import { forwardRef, useImperativeHandle, useRef } from 'react';

type FancyInputRef = {
  focus: () => void;
  scrollIntoView: () => void;
};

const FancyInput = forwardRef<
  FancyInputRef,
  React.InputHTMLAttributes<HTMLInputElement>
>((props, ref) => {
  const inputRef = useRef<HTMLInputElement>(null);
  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current?.focus(),
    scrollIntoView: () => inputRef.current?.scrollIntoView(),
  }));
  return <input ref={inputRef} {...props} />;
});

const App = () => {
  const inputRef = useRef<FancyInputRef>(null);
  return (
    <>
      <FancyInput ref={inputRef} />
      <button onClick={() => inputRef.current?.focus()}>Focus</button>
    </>
  );
};
```

### Performance Hooks

#### useMemo

```tsx
import { useMemo } from 'react';

type ExpensiveComponentProps = { items: string[]; filter: string };

const ExpensiveComponent = ({ items, filter }: ExpensiveComponentProps) => {
  const filteredItems = useMemo(
    () => items.filter((item) => item.includes(filter)),
    [items, filter]
  );
  return (
    <div>
      {filteredItems.map((item) => (
        <div key={item}>{item}</div>
      ))}
    </div>
  );
};
```

#### useCallback

```tsx
import { useCallback, useState } from 'react';

type ChildComponentProps = { onClick: () => void };

const ChildComponent = ({ onClick }: ChildComponentProps) => (
  <button onClick={onClick}>Click me</button>
);

const ParentComponent = () => {
  const [count, setCount] = useState(0);
  const handleClick = useCallback(() => {
    console.log('Button clicked');
  }, []);
  return (
    <div>
      <p>Count: {count}</p>
      <ChildComponent onClick={handleClick} />
    </div>
  );
};
```

### New React 19 Hooks

#### useOptimistic

```tsx
import { useOptimistic, useState } from 'react';

type Todo = { id: number; text: string; pending?: boolean };

const TodoList = () => {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [optimisticTodos, addOptimisticTodo] = useOptimistic<Todo[], Todo>(
    todos,
    (state, newTodo) => [...state, { ...newTodo, pending: true }]
  );
  const addTodo = async (text: string) => {
    addOptimisticTodo({ id: Date.now(), text });
    const newTodo = await saveTodoToServer(text);
    setTodos((prev) => [...prev, newTodo]);
  };
  return (
    <ul>
      {optimisticTodos.map((todo) => (
        <li key={todo.id} style={{ opacity: todo.pending ? 0.5 : 1 }}>
          {todo.text}
        </li>
      ))}
    </ul>
  );
};
```

#### useActionState

```tsx
import { useActionState } from 'react';

type State = { error: string | null; success: boolean; name?: string };

const updateName = async (
  prevState: State,
  formData: FormData
): Promise<State> => {
  const name = formData.get('name');
  if (!name || typeof name !== 'string') {
    return { error: 'Name is required', success: false };
  }
  try {
    await updateUser(name);
    return { success: true, error: null, name };
  } catch {
    return { error: 'Failed to update name', success: false };
  }
};

const NameForm = () => {
  const [state, formAction, isPending] = useActionState<State, FormData>(
    updateName,
    {
      error: null,
      success: false,
    }
  );
  return (
    <form action={formAction}>
      <input name="name" placeholder="Enter name" />
      <button type="submit" disabled={isPending}>
        {isPending ? 'Updating...' : 'Update'}
      </button>
      {state.error && <p style={{ color: 'red' }}>{state.error}</p>}
      {state.success && <p style={{ color: 'green' }}>Updated!</p>}
    </form>
  );
};
```

## Forms

### Actions

```jsx
// Server Action
async function submitForm(prevState, formData) {
  'use server';

  const name = formData.get('name');
  const email = formData.get('email');

  // Validate and save data
  await saveToDatabase({ name, email });

  return { success: true };
}

// Client Component
function ContactForm() {
  return (
    <form action={submitForm}>
      <input name="name" required />
      <input name="email" type="email" required />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Form Hooks

#### useFormStatus

```jsx
import { useFormStatus } from 'react-dom';

function SubmitButton() {
  const { pending } = useFormStatus();

  return (
    <button type="submit" disabled={pending}>
      {pending ? 'Submitting...' : 'Submit'}
    </button>
  );
}

function Form() {
  return (
    <form action={submitAction}>
      <input name="name" />
      <SubmitButton />
    </form>
  );
}
```

#### useFormState

```jsx
import { useFormState } from 'react-dom';

async function updateUser(prevState, formData) {
  const name = formData.get('name');

  if (!name) {
    return { error: 'Name is required' };
  }

  await saveUser(name);
  return { success: true };
}

function UserForm() {
  const [state, formAction] = useFormState(updateUser, {
    error: null,
    success: false,
  });

  return (
    <form action={formAction}>
      <input name="name" />
      <button type="submit">Update</button>
      {state.error && <p>{state.error}</p>}
      {state.success && <p>Updated!</p>}
    </form>
  );
}
```

### Form Validation

```jsx
// Client-side validation
function ValidatedForm() {
  const [errors, setErrors] = useState({});

  function validateForm(formData) {
    const newErrors = {};

    if (!formData.get('email').includes('@')) {
      newErrors.email = 'Invalid email';
    }

    if (formData.get('password').length < 8) {
      newErrors.password = 'Password too short';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  }

  async function handleSubmit(formData) {
    if (!validateForm(formData)) {
      return;
    }

    // Submit form
  }

  return (
    <form action={handleSubmit}>
      <input name="email" />
      {errors.email && <span>{errors.email}</span>}

      <input name="password" type="password" />
      {errors.password && <span>{errors.password}</span>}

      <button type="submit">Submit</button>
    </form>
  );
}
```

## Server Components

### Server vs Client Components

```jsx
// Server Component (default in Next.js App Router)
async function UserList() {
  const users = await fetchUsers(); // Runs on server

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

// Client Component
('use client');

import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}
```

### Data Fetching

```jsx
// Server Component with data fetching
async function BlogPost({ id }) {
  const post = await fetch(`/api/posts/${id}`, {
    cache: 'no-store', // Disable caching
  }).then((res) => res.json());

  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </article>
  );
}

// With revalidation
async function BlogList() {
  const posts = await fetch('/api/posts', {
    next: { revalidate: 3600 }, // Revalidate every hour
  }).then((res) => res.json());

  return (
    <div>
      {posts.map((post) => (
        <BlogPost key={post.id} id={post.id} />
      ))}
    </div>
  );
}
```

## Suspense

### Loading States

```jsx
import { Suspense } from 'react';

function Loading() {
  return <div>Loading...</div>;
}

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <UserProfile />
    </Suspense>
  );
}

// Nested Suspense
function App() {
  return (
    <Suspense fallback={<div>Loading app...</div>}>
      <Header />
      <Suspense fallback={<div>Loading content...</div>}>
        <MainContent />
      </Suspense>
      <Footer />
    </Suspense>
  );
}
```

### Error Boundaries

```jsx
import { ErrorBoundary } from 'react';

function ErrorFallback({ error, resetErrorBoundary }) {
  return (
    <div role="alert">
      <p>Something went wrong:</p>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  );
}

function App() {
  return (
    <ErrorBoundary FallbackComponent={ErrorFallback}>
      <Suspense fallback={<div>Loading...</div>}>
        <UserProfile />
      </Suspense>
    </ErrorBoundary>
  );
}
```

## Context

### Context API

```jsx
import { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

function App() {
  return (
    <ThemeProvider>
      <Header />
      <Main />
    </ThemeProvider>
  );
}
```

### use() Hook

```jsx
import { use } from 'react';

// With promises
function UserProfile({ userPromise }) {
  const user = use(userPromise);
  return <div>{user.name}</div>;
}

// With context
function ThemedButton() {
  const theme = use(ThemeContext);
  return <button className={theme}>Button</button>;
}

// With async components
async function UserData({ userId }) {
  const user = await fetchUser(userId);
  return <UserProfile user={user} />;
}
```

## Performance

### Memoization

```jsx
import { memo, useMemo, useCallback } from 'react';

// Memoized component
const ExpensiveComponent = memo(function ExpensiveComponent({ data }) {
  return <div>{/* Expensive rendering */}</div>;
});

// Memoized value
function ParentComponent({ items }) {
  const sortedItems = useMemo(() => {
    return items.sort((a, b) => a.name.localeCompare(b.name));
  }, [items]);

  const handleClick = useCallback((id) => {
    console.log('Clicked:', id);
  }, []);

  return (
    <div>
      {sortedItems.map((item) => (
        <ExpensiveComponent key={item.id} data={item} onClick={handleClick} />
      ))}
    </div>
  );
}
```

### Code Splitting

```jsx
import { lazy, Suspense } from 'react';

// Lazy loading components
const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}

// Dynamic imports
function DynamicComponent({ componentName }) {
  const Component = lazy(() => import(`./components/${componentName}`));

  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Component />
    </Suspense>
  );
}
```

## Metadata

```jsx
// In Next.js App Router
import { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'My App',
  description: 'My app description',
  keywords: ['react', 'next.js'],
  openGraph: {
    title: 'My App',
    description: 'My app description',
  },
};

// Dynamic metadata
export async function generateMetadata({ params }) {
  const post = await fetchPost(params.id);

  return {
    title: post.title,
    description: post.excerpt,
  };
}
```

## Best Practices

### Component Structure

```jsx
// Good component structure
function UserCard({ user, onEdit, onDelete }) {
  // 1. Hooks at the top
  const [isEditing, setIsEditing] = useState(false);

  // 2. Event handlers
  const handleEdit = () => {
    setIsEditing(true);
    onEdit(user);
  };

  // 3. Computed values
  const fullName = `${user.firstName} ${user.lastName}`;

  // 4. Render
  return (
    <div className="user-card">
      <h3>{fullName}</h3>
      <p>{user.email}</p>
      <div className="actions">
        <button onClick={handleEdit}>Edit</button>
        <button onClick={() => onDelete(user.id)}>Delete</button>
      </div>
    </div>
  );
}
```

### State Management

```jsx
// Local state for UI
function TodoList() {
  const [todos, setTodos] = useState([]);
  const [filter, setFilter] = useState('all');

  const filteredTodos = useMemo(() => {
    switch (filter) {
      case 'active':
        return todos.filter((todo) => !todo.completed);
      case 'completed':
        return todos.filter((todo) => todo.completed);
      default:
        return todos;
    }
  }, [todos, filter]);

  return (
    <div>
      <select value={filter} onChange={(e) => setFilter(e.target.value)}>
        <option value="all">All</option>
        <option value="active">Active</option>
        <option value="completed">Completed</option>
      </select>
      {filteredTodos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
    </div>
  );
}
```

### Error Handling

```jsx
// Error boundaries
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    console.error('Error caught:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}

// Async error handling
function AsyncComponent() {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetchData().then(setData).catch(setError);
  }, []);

  if (error) return <div>Error: {error.message}</div>;
  if (!data) return <div>Loading...</div>;

  return <div>{data}</div>;
}
```

### Performance Tips

```jsx
// 1. Use React.memo for expensive components
const ExpensiveComponent = React.memo(function ExpensiveComponent({ data }) {
  return <div>{/* Expensive rendering */}</div>;
});

// 2. Use useCallback for event handlers passed to children
function Parent() {
  const handleClick = useCallback((id) => {
    console.log('Clicked:', id);
  }, []);

  return <Child onClick={handleClick} />;
}

// 3. Use useMemo for expensive calculations
function Component({ items }) {
  const sortedItems = useMemo(() => {
    return items.sort((a, b) => a.name.localeCompare(b.name));
  }, [items]);

  return (
    <div>
      {sortedItems.map((item) => (
        <Item key={item.id} item={item} />
      ))}
    </div>
  );
}

// 4. Avoid inline objects and functions
// Bad
function BadComponent() {
  return (
    <Child style={{ color: 'red' }} onClick={() => console.log('click')} />
  );
}

// Good
function GoodComponent() {
  const style = useMemo(() => ({ color: 'red' }), []);
  const handleClick = useCallback(() => console.log('click'), []);

  return <Child style={style} onClick={handleClick} />;
}
```
