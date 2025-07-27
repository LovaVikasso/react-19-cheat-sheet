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
- [10. Advanced Patterns](#advanced-patterns)
  - [10.1 Higher-Order Components (HOC)](#higher-order-components-hoc)
  - [10.2 Render Props](#render-props)
  - [10.3 Compound Components](#compound-components)
  - [10.4 Custom Hooks](#custom-hooks)
  - [10.5 Render Functions](#render-functions)
- [11. State Management](#state-management)
  - [11.1 Zustand](#zustand)
  - [11.2 Redux Toolkit](#redux-toolkit)
  - [11.3 Jotai](#jotai)
  - [11.4 Valtio](#valtio)
- [12. Styling](#styling)
  - [12.1 CSS Modules](#css-modules)
  - [12.2 Styled Components](#styled-components)
  - [12.3 Tailwind CSS](#tailwind-css)
  - [12.4 CSS-in-JS](#css-in-js)
- [13. Testing](#testing)
  - [13.1 Jest](#jest)
  - [13.2 React Testing Library](#react-testing-library)
  - [13.3 Vitest](#vitest)
- [14. Animation](#animation)
  - [14.1 Framer Motion](#framer-motion)
  - [14.2 React Spring](#react-spring)
  - [14.3 CSS Transitions](#css-transitions)
- [15. Internationalization](#internationalization)
  - [15.1 React Intl](#react-intl)
  - [15.2 i18next](#i18next)
- [16. Best Practices](#best-practices)

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

## Advanced Patterns

### Higher-Order Components (HOC)

```tsx
// Basic HOC
type WithLoadingProps = { loading: boolean };
type WithLoadingComponentProps<T> = T & WithLoadingProps;

const withLoading = <T extends object>(Component: React.ComponentType<T>) => {
  const WithLoading = (props: WithLoadingComponentProps<T>) => {
    if (props.loading) {
      return <div>Loading...</div>;
    }
    return <Component {...(props as T)} />;
  };

  WithLoading.displayName = `withLoading(${
    Component.displayName || Component.name
  })`;
  return WithLoading;
};

// Usage
type UserProps = { user: { name: string; email: string } };
const UserProfile = ({ user }: UserProps) => (
  <div>
    <h2>{user.name}</h2>
    <p>{user.email}</p>
  </div>
);

const UserProfileWithLoading = withLoading(UserProfile);

// HOC with additional props
const withErrorBoundary = <T extends object>(
  Component: React.ComponentType<T>,
  fallback: React.ComponentType<{ error: Error }>
) => {
  return class ErrorBoundary extends React.Component<T & { error?: Error }> {
    state = { hasError: false, error: null as Error | null };

    static getDerivedStateFromError(error: Error) {
      return { hasError: true, error };
    }

    render() {
      if (this.state.hasError) {
        return <fallback error={this.state.error!} />;
      }
      return <Component {...this.props} />;
    }
  };
};
```

### Render Props

```tsx
// Mouse position with render props
type MousePosition = { x: number; y: number };
type MouseProps = { children: (position: MousePosition) => React.ReactNode };

const Mouse = ({ children }: MouseProps) => {
  const [position, setPosition] = useState<MousePosition>({ x: 0, y: 0 });

  useEffect(() => {
    const handleMouseMove = (event: MouseEvent) => {
      setPosition({ x: event.clientX, y: event.clientY });
    };

    window.addEventListener('mousemove', handleMouseMove);
    return () => window.removeEventListener('mousemove', handleMouseMove);
  }, []);

  return <>{children(position)}</>;
};

// Usage
const App = () => (
  <Mouse>
    {({ x, y }) => (
      <div>
        Mouse position: {x}, {y}
      </div>
    )}
  </Mouse>
);

// Data fetching with render props
type DataFetcherProps<T> = {
  url: string;
  children: (
    data: T | null,
    loading: boolean,
    error: Error | null
  ) => React.ReactNode;
};

const DataFetcher = <T,>({ url, children }: DataFetcherProps<T>) => {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [url]);

  return <>{children(data, loading, error)}</>;
};
```

### Compound Components

```tsx
// Toggle component
type ToggleContextType = {
  on: boolean;
  toggle: () => void;
};

const ToggleContext = createContext<ToggleContextType | undefined>(undefined);

const useToggle = () => {
  const context = useContext(ToggleContext);
  if (!context) {
    throw new Error('useToggle must be used within Toggle');
  }
  return context;
};

type ToggleProps = {
  children: React.ReactNode;
  initialOn?: boolean;
};

const Toggle = ({ children, initialOn = false }: ToggleProps) => {
  const [on, setOn] = useState(initialOn);
  const toggle = useCallback(() => setOn((prev) => !prev), []);

  return (
    <ToggleContext.Provider value={{ on, toggle }}>
      {children}
    </ToggleContext.Provider>
  );
};

// Compound components
Toggle.On = ({ children }: { children: React.ReactNode }) => {
  const { on } = useToggle();
  return on ? <>{children}</> : null;
};

Toggle.Off = ({ children }: { children: React.ReactNode }) => {
  const { on } = useToggle();
  return !on ? <>{children}</> : null;
};

Toggle.Button = ({
  children,
  ...props
}: React.ButtonHTMLAttributes<HTMLButtonElement>) => {
  const { on, toggle } = useToggle();
  return (
    <button onClick={toggle} aria-pressed={on} {...props}>
      {children}
    </button>
  );
};

// Usage
const App = () => (
  <Toggle>
    <Toggle.On>The button is on!</Toggle.On>
    <Toggle.Off>The button is off!</Toggle.Off>
    <Toggle.Button>Toggle me</Toggle.Button>
  </Toggle>
);
```

### Custom Hooks

```tsx
// useLocalStorage hook
const useLocalStorage = <T,>(key: string, initialValue: T) => {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch {
      return initialValue;
    }
  });

  const setValue = useCallback(
    (value: T | ((val: T) => T)) => {
      try {
        const valueToStore =
          value instanceof Function ? value(storedValue) : value;
        setStoredValue(valueToStore);
        window.localStorage.setItem(key, JSON.stringify(valueToStore));
      } catch {
        // Handle error
      }
    },
    [key, storedValue]
  );

  return [storedValue, setValue] as const;
};

// useDebounce hook
const useDebounce = <T,>(value: T, delay: number): T => {
  const [debouncedValue, setDebouncedValue] = useState<T>(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debouncedValue;
};

// useIntersectionObserver hook
const useIntersectionObserver = (
  ref: RefObject<Element>,
  options: IntersectionObserverInit = {}
) => {
  const [isIntersecting, setIsIntersecting] = useState(false);

  useEffect(() => {
    const element = ref.current;
    if (!element) return;

    const observer = new IntersectionObserver(([entry]) => {
      setIsIntersecting(entry.isIntersecting);
    }, options);

    observer.observe(element);
    return () => observer.disconnect();
  }, [ref, options]);

  return isIntersecting;
};

// useAsync hook
type AsyncState<T> = {
  data: T | null;
  loading: boolean;
  error: Error | null;
};

const useAsync = <T,>(asyncFn: () => Promise<T>, deps: DependencyList = []) => {
  const [state, setState] = useState<AsyncState<T>>({
    data: null,
    loading: true,
    error: null,
  });

  useEffect(() => {
    let mounted = true;

    setState({ data: null, loading: true, error: null });

    asyncFn()
      .then((data) => {
        if (mounted) {
          setState({ data, loading: false, error: null });
        }
      })
      .catch((error) => {
        if (mounted) {
          setState({ data: null, loading: false, error });
        }
      });

    return () => {
      mounted = false;
    };
  }, deps);

  return state;
};
```

### Render Functions

```tsx
// Component that accepts render function
type RenderFunctionProps<T> = {
  data: T;
  render: (item: T) => React.ReactNode;
};

const RenderFunction = <T,>({ data, render }: RenderFunctionProps<T>) => {
  return <div className="render-container">{render(data)}</div>;
};

// Usage with different render functions
const UserList = () => {
  const users = [
    { id: 1, name: 'John', email: 'john@example.com' },
    { id: 2, name: 'Jane', email: 'jane@example.com' },
  ];

  return (
    <div>
      <RenderFunction
        data={users}
        render={(users) => (
          <ul>
            {users.map((user) => (
              <li key={user.id}>{user.name}</li>
            ))}
          </ul>
        )}
      />

      <RenderFunction
        data={users}
        render={(users) => (
          <div>
            {users.map((user) => (
              <div key={user.id}>
                <strong>{user.name}</strong>: {user.email}
              </div>
            ))}
          </div>
        )}
      />
    </div>
  );
};
```

## State Management

### Zustand

```tsx
import { create } from 'zustand';
import { devtools, persist } from 'zustand/middleware';

type User = { id: string; name: string; email: string };

type UserStore = {
  users: User[];
  currentUser: User | null;
  addUser: (user: User) => void;
  removeUser: (id: string) => void;
  setCurrentUser: (user: User | null) => void;
};

const useUserStore = create<UserStore>()(
  devtools(
    persist(
      (set) => ({
        users: [],
        currentUser: null,
        addUser: (user) => set((state) => ({ users: [...state.users, user] })),
        removeUser: (id) =>
          set((state) => ({ users: state.users.filter((u) => u.id !== id) })),
        setCurrentUser: (user) => set({ currentUser: user }),
      }),
      { name: 'user-storage' }
    )
  )
);

// Usage
const UserList = () => {
  const { users, addUser, removeUser } = useUserStore();

  return (
    <div>
      {users.map((user) => (
        <div key={user.id}>
          {user.name}
          <button onClick={() => removeUser(user.id)}>Remove</button>
        </div>
      ))}
    </div>
  );
};
```

### Redux Toolkit

```tsx
import { createSlice, configureStore } from '@reduxjs/toolkit';
import { Provider, useSelector, useDispatch } from 'react-redux';

type User = { id: string; name: string; email: string };

type UserState = {
  users: User[];
  loading: boolean;
  error: string | null;
};

const userSlice = createSlice({
  name: 'users',
  initialState: { users: [], loading: false, error: null } as UserState,
  reducers: {
    addUser: (state, action) => {
      state.users.push(action.payload);
    },
    removeUser: (state, action) => {
      state.users = state.users.filter((u) => u.id !== action.payload);
    },
    setLoading: (state, action) => {
      state.loading = action.payload;
    },
    setError: (state, action) => {
      state.error = action.payload;
    },
  },
});

const store = configureStore({
  reducer: {
    users: userSlice.reducer,
  },
});

const { addUser, removeUser, setLoading, setError } = userSlice.actions;

// Usage
const UserList = () => {
  const users = useSelector((state: RootState) => state.users.users);
  const dispatch = useDispatch();

  return (
    <div>
      {users.map((user) => (
        <div key={user.id}>
          {user.name}
          <button onClick={() => dispatch(removeUser(user.id))}>Remove</button>
        </div>
      ))}
    </div>
  );
};
```

### Jotai

```tsx
import { atom, useAtom } from 'jotai';
import { atomWithStorage } from 'jotai/utils';

type User = { id: string; name: string; email: string };

// Basic atoms
const usersAtom = atom<User[]>([]);
const currentUserAtom = atom<User | null>(null);

// Derived atoms
const userCountAtom = atom((get) => get(usersAtom).length);
const activeUsersAtom = atom((get) =>
  get(usersAtom).filter((user) => user.id !== get(currentUserAtom)?.id)
);

// Persistent atoms
const themeAtom = atomWithStorage<'light' | 'dark'>('theme', 'light');

// Actions
const addUserAtom = atom(null, (get, set, user: User) => {
  const users = get(usersAtom);
  set(usersAtom, [...users, user]);
});

const removeUserAtom = atom(null, (get, set, id: string) => {
  const users = get(usersAtom);
  set(
    usersAtom,
    users.filter((u) => u.id !== id)
  );
});

// Usage
const UserList = () => {
  const [users] = useAtom(usersAtom);
  const [userCount] = useAtom(userCountAtom);
  const [, addUser] = useAtom(addUserAtom);
  const [, removeUser] = useAtom(removeUserAtom);

  return (
    <div>
      <p>Total users: {userCount}</p>
      {users.map((user) => (
        <div key={user.id}>
          {user.name}
          <button onClick={() => removeUser(user.id)}>Remove</button>
        </div>
      ))}
    </div>
  );
};
```

### Valtio

```tsx
import { proxy, useSnapshot } from 'valtio';

type User = { id: string; name: string; email: string };

type AppState = {
  users: User[];
  currentUser: User | null;
  addUser: (user: User) => void;
  removeUser: (id: string) => void;
  setCurrentUser: (user: User | null) => void;
};

const state = proxy<AppState>({
  users: [],
  currentUser: null,
  addUser: (user) => {
    state.users.push(user);
  },
  removeUser: (id) => {
    state.users = state.users.filter((u) => u.id !== id);
  },
  setCurrentUser: (user) => {
    state.currentUser = user;
  },
});

// Usage
const UserList = () => {
  const snap = useSnapshot(state);

  return (
    <div>
      {snap.users.map((user) => (
        <div key={user.id}>
          {user.name}
          <button onClick={() => state.removeUser(user.id)}>Remove</button>
        </div>
      ))}
    </div>
  );
};
```

## Styling

### CSS Modules

```tsx
// styles.module.css
.container {
  padding: 20px;
  background: #f5f5f5;
}

.title {
  color: #333;
  font-size: 24px;
}

.button {
  background: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
}

.button:hover {
  background: #0056b3;
}

// Component
import styles from './styles.module.css';

const StyledComponent = () => (
  <div className={styles.container}>
    <h1 className={styles.title}>Hello World</h1>
    <button className={styles.button}>Click me</button>
  </div>
);
```

### Styled Components

```tsx
import styled, { css, keyframes } from 'styled-components';

type ButtonProps = {
  primary?: boolean;
  size?: 'small' | 'medium' | 'large';
  disabled?: boolean;
};

const spin = keyframes`
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
`;

const StyledButton = styled.button<ButtonProps>`
  background: ${(props) => (props.primary ? '#007bff' : '#6c757d')};
  color: white;
  border: none;
  padding: ${(props) => {
    switch (props.size) {
      case 'small':
        return '8px 16px';
      case 'large':
        return '16px 32px';
      default:
        return '12px 24px';
    }
  }};
  border-radius: 4px;
  cursor: ${(props) => (props.disabled ? 'not-allowed' : 'pointer')};
  opacity: ${(props) => (props.disabled ? 0.6 : 1)};

  &:hover {
    background: ${(props) => (props.primary ? '#0056b3' : '#545b62')};
  }

  ${(props) =>
    props.disabled &&
    css`
      pointer-events: none;
    `}
`;

const Spinner = styled.div`
  animation: ${spin} 1s linear infinite;
  width: 20px;
  height: 20px;
  border: 2px solid #f3f3f3;
  border-top: 2px solid #007bff;
  border-radius: 50%;
`;

// Usage
const App = () => (
  <div>
    <StyledButton primary size="large">
      Primary Button
    </StyledButton>
    <StyledButton size="small">Secondary Button</StyledButton>
    <StyledButton disabled>Disabled Button</StyledButton>
    <Spinner />
  </div>
);
```

### Tailwind CSS

```tsx
// Component with Tailwind classes
const TailwindComponent = () => (
  <div className="min-h-screen bg-gray-100 py-6 flex flex-col justify-center sm:py-12">
    <div className="relative py-3 sm:max-w-xl sm:mx-auto">
      <div className="relative px-4 py-10 bg-white shadow-lg sm:rounded-3xl sm:p-20">
        <div className="max-w-md mx-auto">
          <div className="divide-y divide-gray-200">
            <div className="py-8 text-base leading-6 space-y-4 text-gray-700 sm:text-lg sm:leading-7">
              <h1 className="text-3xl font-bold text-gray-900 mb-8">
                Hello World
              </h1>
              <button className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded transition-colors duration-200">
                Click me
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
);

// Responsive design
const ResponsiveComponent = () => (
  <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 p-4">
    <div className="bg-white p-6 rounded-lg shadow-md">
      <h2 className="text-xl font-semibold mb-4">Card 1</h2>
      <p className="text-gray-600">Content here</p>
    </div>
    <div className="bg-white p-6 rounded-lg shadow-md">
      <h2 className="text-xl font-semibold mb-4">Card 2</h2>
      <p className="text-gray-600">Content here</p>
    </div>
    <div className="bg-white p-6 rounded-lg shadow-md">
      <h2 className="text-xl font-semibold mb-4">Card 3</h2>
      <p className="text-gray-600">Content here</p>
    </div>
  </div>
);
```

### CSS-in-JS

```tsx
// Emotion
import { css } from '@emotion/react';

const buttonStyles = css`
  background: #007bff;
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 4px;
  cursor: pointer;

  &:hover {
    background: #0056b3;
  }

  &:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
`;

const Button = ({ children, disabled, ...props }) => (
  <button css={buttonStyles} disabled={disabled} {...props}>
    {children}
  </button>
);

// Stitches
import { styled } from '@stitches/react';

const StyledButton = styled('button', {
  backgroundColor: '#007bff',
  color: 'white',
  border: 'none',
  padding: '12px 24px',
  borderRadius: '4px',
  cursor: 'pointer',

  '&:hover': {
    backgroundColor: '#0056b3',
  },

  variants: {
    size: {
      small: { padding: '8px 16px' },
      large: { padding: '16px 32px' },
    },
    variant: {
      primary: { backgroundColor: '#007bff' },
      secondary: { backgroundColor: '#6c757d' },
    },
  },
});
```

## Testing

### Jest

```tsx
// Component test
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Counter } from './Counter';

describe('Counter', () => {
  test('renders counter with initial value', () => {
    render(<Counter />);
    expect(screen.getByText('Count: 0')).toBeInTheDocument();
  });

  test('increments counter when button is clicked', async () => {
    const user = userEvent.setup();
    render(<Counter />);

    const button = screen.getByRole('button', { name: /increment/i });
    await user.click(button);

    expect(screen.getByText('Count: 1')).toBeInTheDocument();
  });

  test('decrements counter when decrement button is clicked', async () => {
    const user = userEvent.setup();
    render(<Counter />);

    const decrementButton = screen.getByRole('button', { name: /decrement/i });
    await user.click(decrementButton);

    expect(screen.getByText('Count: -1')).toBeInTheDocument();
  });
});
```

### React Testing Library

```tsx
// Custom hook test
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

describe('useCounter', () => {
  test('should increment counter', () => {
    const { result } = renderHook(() => useCounter());

    act(() => {
      result.current.increment();
    });

    expect(result.current.count).toBe(1);
  });

  test('should decrement counter', () => {
    const { result } = renderHook(() => useCounter());

    act(() => {
      result.current.decrement();
    });

    expect(result.current.count).toBe(-1);
  });

  test('should reset counter', () => {
    const { result } = renderHook(() => useCounter());

    act(() => {
      result.current.increment();
      result.current.increment();
      result.current.reset();
    });

    expect(result.current.count).toBe(0);
  });
});

// Integration test
test('user can add a todo', async () => {
  const user = userEvent.setup();
  render(<TodoApp />);

  const input = screen.getByPlaceholderText(/add a todo/i);
  const addButton = screen.getByRole('button', { name: /add/i });

  await user.type(input, 'Buy groceries');
  await user.click(addButton);

  expect(screen.getByText('Buy groceries')).toBeInTheDocument();
  expect(input).toHaveValue('');
});
```

### Vitest

```tsx
// Vitest configuration
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    setupFiles: ['./src/test/setup.ts'],
    globals: true,
  },
});

// Component test with Vitest
import { describe, it, expect } from 'vitest';
import { render, screen } from '@testing-library/react';
import { UserProfile } from './UserProfile';

describe('UserProfile', () => {
  it('renders user information', () => {
    const user = { name: 'John Doe', email: 'john@example.com' };
    render(<UserProfile user={user} />);

    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
  });

  it('shows loading state', () => {
    render(<UserProfile loading />);
    expect(screen.getByText(/loading/i)).toBeInTheDocument();
  });
});
```

## Animation

### Framer Motion

```tsx
import { motion, AnimatePresence } from 'framer-motion';

const AnimatedComponent = () => (
  <motion.div
    initial={{ opacity: 0, y: 20 }}
    animate={{ opacity: 1, y: 0 }}
    exit={{ opacity: 0, y: -20 }}
    transition={{ duration: 0.5 }}
  >
    <h1>Animated Content</h1>
  </motion.div>
);

const ListWithAnimations = () => {
  const [items, setItems] = useState(['Item 1', 'Item 2', 'Item 3']);

  return (
    <ul>
      <AnimatePresence>
        {items.map((item, index) => (
          <motion.li
            key={item}
            initial={{ opacity: 0, x: -100 }}
            animate={{ opacity: 1, x: 0 }}
            exit={{ opacity: 0, x: 100 }}
            transition={{ delay: index * 0.1 }}
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
          >
            {item}
          </motion.li>
        ))}
      </AnimatePresence>
    </ul>
  );
};

const DragComponent = () => (
  <motion.div
    drag
    dragConstraints={{ left: 0, right: 0, top: 0, bottom: 0 }}
    dragElastic={0.1}
    whileDrag={{ scale: 1.1 }}
  >
    <div className="w-20 h-20 bg-blue-500 rounded-full" />
  </motion.div>
);
```

### React Spring

```tsx
import { useSpring, animated, useTransition } from '@react-spring/web';

const SpringComponent = () => {
  const [isVisible, setIsVisible] = useState(false);

  const springProps = useSpring({
    opacity: isVisible ? 1 : 0,
    transform: isVisible ? 'translateY(0px)' : 'translateY(20px)',
    config: { tension: 300, friction: 20 },
  });

  return (
    <animated.div style={springProps}>
      <h1>Spring Animated Content</h1>
    </animated.div>
  );
};

const NumberCounter = () => {
  const [count, setCount] = useState(0);

  const { number } = useSpring({
    number: count,
    from: { number: 0 },
    config: { mass: 1, tension: 20, friction: 10 },
  });

  return (
    <div>
      <animated.span>{number.to((n) => n.toFixed(0))}</animated.span>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};
```

### CSS Transitions

```tsx
import { useState } from 'react';

const CSSTransitionComponent = () => {
  const [isVisible, setIsVisible] = useState(false);

  return (
    <div>
      <button onClick={() => setIsVisible(!isVisible)}>Toggle</button>
      <div
        className={`transition-all duration-300 ease-in-out ${
          isVisible
            ? 'opacity-100 transform translate-y-0'
            : 'opacity-0 transform -translate-y-4'
        }`}
      >
        <h1>CSS Transitioned Content</h1>
      </div>
    </div>
  );
};

// CSS classes
const styles = `
  .fade-enter {
    opacity: 0;
    transform: translateY(20px);
  }
  
  .fade-enter-active {
    opacity: 1;
    transform: translateY(0);
    transition: opacity 300ms, transform 300ms;
  }
  
  .fade-exit {
    opacity: 1;
    transform: translateY(0);
  }
  
  .fade-exit-active {
    opacity: 0;
    transform: translateY(-20px);
    transition: opacity 300ms, transform 300ms;
  }
`;
```

## Internationalization

### React Intl

```tsx
import { IntlProvider, FormattedMessage, useIntl } from 'react-intl';

const messages = {
  en: {
    'app.greeting': 'Hello, {name}!',
    'app.welcome': 'Welcome to our app',
    'app.button.submit': 'Submit',
    'app.button.cancel': 'Cancel',
  },
  es: {
    'app.greeting': '¡Hola, {name}!',
    'app.welcome': 'Bienvenido a nuestra aplicación',
    'app.button.submit': 'Enviar',
    'app.button.cancel': 'Cancelar',
  },
};

const App = () => {
  const [locale, setLocale] = useState('en');

  return (
    <IntlProvider messages={messages[locale]} locale={locale}>
      <div>
        <select value={locale} onChange={(e) => setLocale(e.target.value)}>
          <option value="en">English</option>
          <option value="es">Español</option>
        </select>

        <FormattedMessage id="app.greeting" values={{ name: 'John' }} />

        <p>
          <FormattedMessage id="app.welcome" />
        </p>

        <button>
          <FormattedMessage id="app.button.submit" />
        </button>
      </div>
    </IntlProvider>
  );
};

const LocalizedComponent = () => {
  const intl = useIntl();

  return (
    <div>{intl.formatMessage({ id: 'app.greeting' }, { name: 'World' })}</div>
  );
};
```

### i18next

```tsx
import { useTranslation } from 'react-i18next';
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';

const resources = {
  en: {
    translation: {
      'app.greeting': 'Hello, {{name}}!',
      'app.welcome': 'Welcome to our app',
      'app.button.submit': 'Submit',
      'app.button.cancel': 'Cancel',
    },
  },
  es: {
    translation: {
      'app.greeting': '¡Hola, {{name}}!',
      'app.welcome': 'Bienvenido a nuestra aplicación',
      'app.button.submit': 'Enviar',
      'app.button.cancel': 'Cancelar',
    },
  },
};

i18n.use(initReactI18next).init({
  resources,
  lng: 'en',
  fallbackLng: 'en',
  interpolation: {
    escapeValue: false,
  },
});

const App = () => {
  const { t, i18n } = useTranslation();

  const changeLanguage = (lng: string) => {
    i18n.changeLanguage(lng);
  };

  return (
    <div>
      <select
        value={i18n.language}
        onChange={(e) => changeLanguage(e.target.value)}
      >
        <option value="en">English</option>
        <option value="es">Español</option>
      </select>

      <h1>{t('app.greeting', { name: 'John' })}</h1>
      <p>{t('app.welcome')}</p>

      <button>{t('app.button.submit')}</button>
      <button>{t('app.button.cancel')}</button>
    </div>
  );
};
```

## Best Practices

### Component Structure

```tsx
// Good component structure with modern patterns
type UserCardProps = {
  user: { id: string; name: string; email: string; avatar?: string };
  onEdit: (user: User) => void;
  onDelete: (id: string) => void;
  variant?: 'default' | 'compact';
};

const UserCard = ({
  user,
  onEdit,
  onDelete,
  variant = 'default',
}: UserCardProps) => {
  // 1. Hooks at the top
  const [isEditing, setIsEditing] = useState(false);
  const [isHovered, setIsHovered] = useState(false);

  // 2. Derived state
  const fullName = `${user.name}`;
  const isCompact = variant === 'compact';

  // 3. Event handlers
  const handleEdit = useCallback(() => {
    setIsEditing(true);
    onEdit(user);
  }, [onEdit, user]);

  const handleDelete = useCallback(() => {
    if (confirm('Are you sure you want to delete this user?')) {
      onDelete(user.id);
    }
  }, [onDelete, user.id]);

  // 4. Render
  return (
    <motion.div
      className={`user-card ${isCompact ? 'compact' : ''}`}
      onHoverStart={() => setIsHovered(true)}
      onHoverEnd={() => setIsHovered(false)}
      whileHover={{ scale: 1.02 }}
      whileTap={{ scale: 0.98 }}
    >
      <div className="user-avatar">
        {user.avatar ? (
          <img src={user.avatar} alt={fullName} />
        ) : (
          <div className="avatar-placeholder">
            {fullName.charAt(0).toUpperCase()}
          </div>
        )}
      </div>

      <div className="user-info">
        <h3 className="user-name">{fullName}</h3>
        <p className="user-email">{user.email}</p>
      </div>

      <AnimatePresence>
        {isHovered && (
          <motion.div
            className="user-actions"
            initial={{ opacity: 0, x: 20 }}
            animate={{ opacity: 1, x: 0 }}
            exit={{ opacity: 0, x: 20 }}
          >
            <button onClick={handleEdit} className="btn-edit">
              Edit
            </button>
            <button onClick={handleDelete} className="btn-delete">
              Delete
            </button>
          </motion.div>
        )}
      </AnimatePresence>
    </motion.div>
  );
};
```

### Error Handling

```tsx
// Modern error boundary with hooks
type ErrorBoundaryState = {
  hasError: boolean;
  error: Error | null;
};

class ErrorBoundary extends React.Component<
  {
    children: React.ReactNode;
    fallback?: React.ComponentType<{ error: Error }>;
  },
  ErrorBoundaryState
> {
  constructor(props: any) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error: Error): ErrorBoundaryState {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
    // Log to error reporting service
  }

  render() {
    if (this.state.hasError) {
      const FallbackComponent = this.props.fallback || DefaultErrorFallback;
      return <FallbackComponent error={this.state.error!} />;
    }

    return this.props.children;
  }
}

const DefaultErrorFallback = ({ error }: { error: Error }) => (
  <div className="error-boundary">
    <h2>Something went wrong</h2>
    <details>
      <summary>Error details</summary>
      <pre>{error.message}</pre>
    </details>
  </div>
);

// Async error handling with custom hook
const useAsyncError = () => {
  const [, setError] = useState();
  return useCallback((e: Error) => {
    setError(() => {
      throw e;
    });
  }, []);
};

const AsyncComponent = () => {
  const throwError = useAsyncError();
  const { data, loading, error } = useAsync(fetchData);

  if (error) {
    throwError(error);
  }

  if (loading) return <div>Loading...</div>;

  return <div>{data}</div>;
};
```

### Performance Optimization

```tsx
// Optimized component with modern patterns
type ExpensiveListProps = {
  items: Array<{ id: string; name: string; description: string }>;
  filter: string;
  sortBy: 'name' | 'date';
};

const ExpensiveList = memo(({ items, filter, sortBy }: ExpensiveListProps) => {
  // Memoized expensive calculations
  const filteredAndSortedItems = useMemo(() => {
    const filtered = items.filter(
      (item) =>
        item.name.toLowerCase().includes(filter.toLowerCase()) ||
        item.description.toLowerCase().includes(filter.toLowerCase())
    );

    return filtered.sort((a, b) => {
      if (sortBy === 'name') {
        return a.name.localeCompare(b.name);
      }
      return 0; // Add date comparison logic
    });
  }, [items, filter, sortBy]);

  // Memoized event handlers
  const handleItemClick = useCallback((id: string) => {
    console.log('Item clicked:', id);
  }, []);

  // Virtual scrolling for large lists
  if (filteredAndSortedItems.length > 1000) {
    return (
      <VirtualizedList
        items={filteredAndSortedItems}
        itemHeight={60}
        renderItem={({ item }) => (
          <ListItem key={item.id} item={item} onClick={handleItemClick} />
        )}
      />
    );
  }

  return (
    <div className="list-container">
      {filteredAndSortedItems.map((item) => (
        <ListItem key={item.id} item={item} onClick={handleItemClick} />
      ))}
    </div>
  );
});

// Optimized list item
const ListItem = memo(
  ({ item, onClick }: { item: any; onClick: (id: string) => void }) => {
    const handleClick = useCallback(() => {
      onClick(item.id);
    }, [onClick, item.id]);

    return (
      <motion.div
        className="list-item"
        onClick={handleClick}
        whileHover={{ backgroundColor: '#f5f5f5' }}
        whileTap={{ scale: 0.98 }}
        layout
      >
        <h3>{item.name}</h3>
        <p>{item.description}</p>
      </motion.div>
    );
  }
);
```
