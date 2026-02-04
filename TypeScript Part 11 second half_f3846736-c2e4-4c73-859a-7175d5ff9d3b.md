# **PART 11 (Continued): Advanced React + TypeScript ⚛️**

## 11.8 Context API with TypeScript 🌐

### **Context Kya Hai?**

**Problem: Prop Drilling**

tsx

```tsx
// ❌ Prop drilling - har level par pass karna padta hai
function App() {
    const user = { name: "Rahul", id: 1 };
    return <Dashboard user={user} />;
}

function Dashboard({ user }) {
    return <Sidebar user={user} />;
}

function Sidebar({ user }) {
    return <UserMenu user={user} />;
}

function UserMenu({ user }) {
    return <div>{user.name}</div>;
}
```

**Solution: Context API**

tsx

```tsx
// ✅ Context - directly access karo jahan chahiye
const UserContext = createContext(user);

function App() {
    return (
        <UserContext.Provider value={user}>
            <Dashboard />
        </UserContext.Provider>
    );
}

function UserMenu() {
    const user = useContext(UserContext);
    return <div>{user.name}</div>;
}
```

---

### **Basic Context Setup**

tsx

```tsx
import { createContext, useContext, useState } from 'react';

// 1. Define context type
interface User {
    id: number;
    name: string;
    email: string;
}

interface UserContextType {
    user: User | null;
    setUser: (user: User | null) => void;
}

// 2. Create context with default value
const UserContext = createContext<UserContextType | undefined>(undefined);

// 3. Create Provider component
function UserProvider({ children }: { children: React.ReactNode }) {
    const [user, setUser] = useState<User | null>(null);
    
    return (
        <UserContext.Provider value={{ user, setUser }}>
            {children}
        </UserContext.Provider>
    );
}

// 4. Create custom hook for easy access
function useUser() {
    const context = useContext(UserContext);
    
    if (context === undefined) {
        throw new Error('useUser must be used within UserProvider');
    }
    
    return context;
}

// 5. Usage in components
function App() {
    return (
        <UserProvider>
            <Dashboard />
        </UserProvider>
    );
}

function Dashboard() {
    const { user, setUser } = useUser();
    
    const handleLogin = () => {
        setUser({ id: 1, name: "Rahul", email: "rahul@example.com" });
    };
    
    return (
        <div>
            {user ? (
                <div>
                    <p>Welcome, {user.name}!</p>
                    <UserProfile />
                </div>
            ) : (
                <button onClick={handleLogin}>Login</button>
            )}
        </div>
    );
}

function UserProfile() {
    const { user } = useUser();
    return <div>{user?.email}</div>;
}
```

---

### **Theme Context - Real Example**

tsx

```tsx
type Theme = 'light' | 'dark';

interface ThemeContextType {
    theme: Theme;
    toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

function ThemeProvider({ children }: { children: React.ReactNode }) {
    const [theme, setTheme] = useState<Theme>('light');
    
    const toggleTheme = () => {
        setTheme(prev => prev === 'light' ? 'dark' : 'light');
    };
    
    return (
        <ThemeContext.Provider value={{ theme, toggleTheme }}>
            <div style={{
                backgroundColor: theme === 'light' ? '#fff' : '#333',
                color: theme === 'light' ? '#000' : '#fff',
                minHeight: '100vh'
            }}>
                {children}
            </div>
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

// Usage
function App() {
    return (
        <ThemeProvider>
            <Header />
            <Content />
        </ThemeProvider>
    );
}

function Header() {
    const { theme, toggleTheme } = useTheme();
    
    return (
        <header>
            <h1>My App</h1>
            <button onClick={toggleTheme}>
                Switch to {theme === 'light' ? 'dark' : 'light'} mode
            </button>
        </header>
    );
}
```

---

### **Multiple Contexts - Composition**

tsx

```tsx
interface AuthContextType {
    isAuthenticated: boolean;
    login: () => void;
    logout: () => void;
}

interface SettingsContextType {
    language: string;
    setLanguage: (lang: string) => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);
const SettingsContext = createContext<SettingsContextType | undefined>(undefined);

// Compose providers
function App() {
    return (
        <AuthProvider>
            <SettingsProvider>
                <ThemeProvider>
                    <Dashboard />
                </ThemeProvider>
            </SettingsProvider>
        </AuthProvider>
    );
}

// Or create a combined provider
function AppProviders({ children }: { children: React.ReactNode }) {
    return (
        <AuthProvider>
            <SettingsProvider>
                <ThemeProvider>
                    {children}
                </ThemeProvider>
            </SettingsProvider>
        </AuthProvider>
    );
}

function App() {
    return (
        <AppProviders>
            <Dashboard />
        </AppProviders>
    );
}
```

---

### **Context with Complex State**

tsx

```tsx
interface Todo {
    id: number;
    text: string;
    completed: boolean;
}

interface TodoContextType {
    todos: Todo[];
    addTodo: (text: string) => void;
    toggleTodo: (id: number) => void;
    deleteTodo: (id: number) => void;
}

const TodoContext = createContext<TodoContextType | undefined>(undefined);

function TodoProvider({ children }: { children: React.ReactNode }) {
    const [todos, setTodos] = useState<Todo[]>([]);
    
    const addTodo = (text: string) => {
        const newTodo: Todo = {
            id: Date.now(),
            text,
            completed: false
        };
        setTodos(prev => [...prev, newTodo]);
    };
    
    const toggleTodo = (id: number) => {
        setTodos(prev =>
            prev.map(todo =>
                todo.id === id ? { ...todo, completed: !todo.completed } : todo
            )
        );
    };
    
    const deleteTodo = (id: number) => {
        setTodos(prev => prev.filter(todo => todo.id !== id));
    };
    
    return (
        <TodoContext.Provider value={{ todos, addTodo, toggleTodo, deleteTodo }}>
            {children}
        </TodoContext.Provider>
    );
}

function useTodos() {
    const context = useContext(TodoContext);
    if (!context) {
        throw new Error('useTodos must be used within TodoProvider');
    }
    return context;
}

// Usage
function TodoApp() {
    return (
        <TodoProvider>
            <TodoInput />
            <TodoList />
        </TodoProvider>
    );
}

function TodoInput() {
    const [text, setText] = useState('');
    const { addTodo } = useTodos();
    
    const handleSubmit = (e: React.FormEvent) => {
        e.preventDefault();
        if (text.trim()) {
            addTodo(text);
            setText('');
        }
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input 
                value={text} 
                onChange={(e) => setText(e.target.value)} 
            />
            <button type="submit">Add</button>
        </form>
    );
}

function TodoList() {
    const { todos, toggleTodo, deleteTodo } = useTodos();
    
    return (
        <div>
            {todos.map(todo => (
                <div key={todo.id}>
                    <input 
                        type="checkbox" 
                        checked={todo.completed}
                        onChange={() => toggleTodo(todo.id)}
                    />
                    <span style={{ 
                        textDecoration: todo.completed ? 'line-through' : 'none' 
                    }}>
                        {todo.text}
                    </span>
                    <button onClick={() => deleteTodo(todo.id)}>Delete</button>
                </div>
            ))}
        </div>
    );
}
```

---

## 11.9 Custom Hooks - Reusable Logic 🪝

### **Custom Hook Kya Hai?**

**Reusable logic ko extract karke ek hook banao**

tsx

```tsx
// ❌ Without custom hook - repeated code
function ComponentA() {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        fetch('/api/data')
            .then(res => res.json())
            .then(data => {
                setData(data);
                setLoading(false);
            });
    }, []);
    
    // ... render
}

function ComponentB() {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        fetch('/api/data')
            .then(res => res.json())
            .then(data => {
                setData(data);
                setLoading(false);
            });
    }, []);
    
    // ... render
}

// ✅ With custom hook - DRY
function useFetch(url: string) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        fetch(url)
            .then(res => res.json())
            .then(data => {
                setData(data);
                setLoading(false);
            });
    }, [url]);
    
    return { data, loading };
}

function ComponentA() {
    const { data, loading } = useFetch('/api/data');
    // ... render
}

function ComponentB() {
    const { data, loading } = useFetch('/api/data');
    // ... render
}
```

---

### **Example 1: useFetch - Generic Data Fetching**

tsx

```tsx
interface UseFetchResult<T> {
    data: T | null;
    loading: boolean;
    error: string | null;
}

function useFetch<T>(url: string): UseFetchResult<T> {
    const [data, setData] = useState<T | null>(null);
    const [loading, setLoading] = useState<boolean>(true);
    const [error, setError] = useState<string | null>(null);
    
    useEffect(() => {
        let cancelled = false;
        
        const fetchData = async () => {
            try {
                setLoading(true);
                const response = await fetch(url);
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                
                const json: T = await response.json();
                
                if (!cancelled) {
                    setData(json);
                    setError(null);
                }
            } catch (err) {
                if (!cancelled) {
                    setError(err instanceof Error ? err.message : 'An error occurred');
                }
            } finally {
                if (!cancelled) {
                    setLoading(false);
                }
            }
        };
        
        fetchData();
        
        return () => {
            cancelled = true;
        };
    }, [url]);
    
    return { data, loading, error };
}

// Usage
interface User {
    id: number;
    name: string;
    email: string;
}

function UserProfile({ userId }: { userId: number }) {
    const { data: user, loading, error } = useFetch<User>(`/api/users/${userId}`);
    
    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error}</div>;
    if (!user) return <div>No user found</div>;
    
    return (
        <div>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
        </div>
    );
}
```

---

### **Example 2: useLocalStorage - Persist State**

tsx

```tsx
function useLocalStorage<T>(key: string, initialValue: T): [T, (value: T) => void] {
    // Get initial value from localStorage or use default
    const [storedValue, setStoredValue] = useState<T>(() => {
        try {
            const item = window.localStorage.getItem(key);
            return item ? JSON.parse(item) : initialValue;
        } catch (error) {
            console.error('Error reading from localStorage:', error);
            return initialValue;
        }
    });
    
    // Update localStorage when value changes
    const setValue = (value: T) => {
        try {
            setStoredValue(value);
            window.localStorage.setItem(key, JSON.stringify(value));
        } catch (error) {
            console.error('Error writing to localStorage:', error);
        }
    };
    
    return [storedValue, setValue];
}

// Usage
function ThemeSelector() {
    const [theme, setTheme] = useLocalStorage<'light' | 'dark'>('theme', 'light');
    
    return (
        <div>
            <p>Current theme: {theme}</p>
            <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
                Toggle Theme
            </button>
        </div>
    );
}

function Counter() {
    const [count, setCount] = useLocalStorage<number>('count', 0);
    
    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
        </div>
    );
}
```

---

### **Example 3: useDebounce - Delay Updates**

tsx

```tsx
function useDebounce<T>(value: T, delay: number): T {
    const [debouncedValue, setDebouncedValue] = useState<T>(value);
    
    useEffect(() => {
        const handler = setTimeout(() => {
            setDebouncedValue(value);
        }, delay);
        
        // Cleanup: cancel timeout if value changes
        return () => {
            clearTimeout(handler);
        };
    }, [value, delay]);
    
    return debouncedValue;
}

// Usage - Search input
function SearchComponent() {
    const [searchTerm, setSearchTerm] = useState('');
    const debouncedSearchTerm = useDebounce(searchTerm, 500);
    
    useEffect(() => {
        if (debouncedSearchTerm) {
            // API call only after user stops typing for 500ms
            console.log('Searching for:', debouncedSearchTerm);
            fetch(`/api/search?q=${debouncedSearchTerm}`)
                .then(res => res.json())
                .then(data => console.log(data));
        }
    }, [debouncedSearchTerm]);
    
    return (
        <input 
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
            placeholder="Search..."
        />
    );
}
```

---

### **Example 4: useToggle - Boolean State**

tsx

```tsx
function useToggle(initialValue: boolean = false): [boolean, () => void] {
    const [value, setValue] = useState<boolean>(initialValue);
    
    const toggle = () => {
        setValue(prev => !prev);
    };
    
    return [value, toggle];
}

// Usage
function Modal() {
    const [isOpen, toggleModal] = useToggle(false);
    
    return (
        <div>
            <button onClick={toggleModal}>Open Modal</button>
            {isOpen && (
                <div className="modal">
                    <h2>Modal Content</h2>
                    <button onClick={toggleModal}>Close</button>
                </div>
            )}
        </div>
    );
}

function Sidebar() {
    const [isCollapsed, toggleCollapsed] = useToggle(false);
    
    return (
        <div style={{ width: isCollapsed ? '50px' : '200px' }}>
            <button onClick={toggleCollapsed}>
                {isCollapsed ? '→' : '←'}
            </button>
            {!isCollapsed && <div>Sidebar content</div>}
        </div>
    );
}
```

---

### **Example 5: useOnClickOutside - Click Outside Detection**

tsx

```tsx
function useOnClickOutside<T extends HTMLElement>(
    ref: React.RefObject<T>,
    handler: () => void
): void {
    useEffect(() => {
        const listener = (event: MouseEvent | TouchEvent) => {
            // Do nothing if clicking ref's element or descendent elements
            if (!ref.current || ref.current.contains(event.target as Node)) {
                return;
            }
            
            handler();
        };
        
        document.addEventListener('mousedown', listener);
        document.addEventListener('touchstart', listener);
        
        return () => {
            document.removeEventListener('mousedown', listener);
            document.removeEventListener('touchstart', listener);
        };
    }, [ref, handler]);
}

// Usage - Dropdown menu
function Dropdown() {
    const [isOpen, setIsOpen] = useState(false);
    const dropdownRef = useRef<HTMLDivElement>(null);
    
    useOnClickOutside(dropdownRef, () => setIsOpen(false));
    
    return (
        <div ref={dropdownRef}>
            <button onClick={() => setIsOpen(!isOpen)}>
                Toggle Menu
            </button>
            {isOpen && (
                <div className="menu">
                    <div>Option 1</div>
                    <div>Option 2</div>
                    <div>Option 3</div>
                </div>
            )}
        </div>
    );
}
```

---

## 11.10 useReducer - Complex State Management 🔄

### **useReducer Kya Hai?**

**useState ka advanced version - complex state logic ke liye**

tsx

```tsx
// useState - Simple state
const [count, setCount] = useState(0);

// useReducer - Complex state with actions
const [state, dispatch] = useReducer(reducer, initialState);
```

---

### **Basic Example - Counter**

tsx

```tsx
// 1. Define state type
interface CounterState {
    count: number;
}

// 2. Define action types
type CounterAction = 
    | { type: 'INCREMENT' }
    | { type: 'DECREMENT' }
    | { type: 'RESET' }
    | { type: 'SET'; payload: number };

// 3. Create reducer function
function counterReducer(state: CounterState, action: CounterAction): CounterState {
    switch (action.type) {
        case 'INCREMENT':
            return { count: state.count + 1 };
        case 'DECREMENT':
            return { count: state.count - 1 };
        case 'RESET':
            return { count: 0 };
        case 'SET':
            return { count: action.payload };
        default:
            return state;
    }
}

// 4. Use in component
function Counter() {
    const [state, dispatch] = useReducer(counterReducer, { count: 0 });
    
    return (
        <div>
            <p>Count: {state.count}</p>
            <button onClick={() => dispatch({ type: 'INCREMENT' })}>+</button>
            <button onClick={() => dispatch({ type: 'DECREMENT' })}>-</button>
            <button onClick={() => dispatch({ type: 'RESET' })}>Reset</button>
            <button onClick={() => dispatch({ type: 'SET', payload: 10 })}>Set to 10</button>
        </div>
    );
}
```

---

### **Complex Example - Todo List**

tsx

```tsx
interface Todo {
    id: number;
    text: string;
    completed: boolean;
}

interface TodoState {
    todos: Todo[];
    filter: 'all' | 'active' | 'completed';
}

type TodoAction =
    | { type: 'ADD_TODO'; payload: string }
    | { type: 'TOGGLE_TODO'; payload: number }
    | { type: 'DELETE_TODO'; payload: number }
    | { type: 'EDIT_TODO'; payload: { id: number; text: string } }
    | { type: 'SET_FILTER'; payload: 'all' | 'active' | 'completed' }
    | { type: 'CLEAR_COMPLETED' };

function todoReducer(state: TodoState, action: TodoAction): TodoState {
    switch (action.type) {
        case 'ADD_TODO':
            return {
                ...state,
                todos: [
                    ...state.todos,
                    {
                        id: Date.now(),
                        text: action.payload,
                        completed: false
                    }
                ]
            };
            
        case 'TOGGLE_TODO':
            return {
                ...state,
                todos: state.todos.map(todo =>
                    todo.id === action.payload
                        ? { ...todo, completed: !todo.completed }
                        : todo
                )
            };
            
        case 'DELETE_TODO':
            return {
                ...state,
                todos: state.todos.filter(todo => todo.id !== action.payload)
            };
            
        case 'EDIT_TODO':
            return {
                ...state,
                todos: state.todos.map(todo =>
                    todo.id === action.payload.id
                        ? { ...todo, text: action.payload.text }
                        : todo
                )
            };
            
        case 'SET_FILTER':
            return {
                ...state,
                filter: action.payload
            };
            
        case 'CLEAR_COMPLETED':
            return {
                ...state,
                todos: state.todos.filter(todo => !todo.completed)
            };
            
        default:
            return state;
    }
}

function TodoApp() {
    const [state, dispatch] = useReducer(todoReducer, {
        todos: [],
        filter: 'all'
    });
    
    const [inputText, setInputText] = useState('');
    
    const handleAddTodo = (e: React.FormEvent) => {
        e.preventDefault();
        if (inputText.trim()) {
            dispatch({ type: 'ADD_TODO', payload: inputText });
            setInputText('');
        }
    };
    
    const filteredTodos = state.todos.filter(todo => {
        if (state.filter === 'active') return !todo.completed;
        if (state.filter === 'completed') return todo.completed;
        return true;
    });
    
    return (
        <div>
            <form onSubmit={handleAddTodo}>
                <input 
                    value={inputText}
                    onChange={(e) => setInputText(e.target.value)}
                />
                <button type="submit">Add</button>
            </form>
            
            <div>
                <button onClick={() => dispatch({ type: 'SET_FILTER', payload: 'all' })}>
                    All
                </button>
                <button onClick={() => dispatch({ type: 'SET_FILTER', payload: 'active' })}>
                    Active
                </button>
                <button onClick={() => dispatch({ type: 'SET_FILTER', payload: 'completed' })}>
                    Completed
                </button>
                <button onClick={() => dispatch({ type: 'CLEAR_COMPLETED' })}>
                    Clear Completed
                </button>
            </div>
            
            <div>
                {filteredTodos.map(todo => (
                    <div key={todo.id}>
                        <input 
                            type="checkbox"
                            checked={todo.completed}
                            onChange={() => dispatch({ type: 'TOGGLE_TODO', payload: todo.id })}
                        />
                        <span style={{ 
                            textDecoration: todo.completed ? 'line-through' : 'none' 
                        }}>
                            {todo.text}
                        </span>
                        <button onClick={() => dispatch({ type: 'DELETE_TODO', payload: todo.id })}>
                            Delete
                        </button>
                    </div>
                ))}
            </div>
        </div>
    );
}
```

---

## 11.11 Generic Components - Reusable 🔧

### **Generic Component Kya Hai?**

**Component jo different types ke saath kaam kare**

tsx

```tsx
// Generic List component
interface ListProps<T> {
    items: T[];
    renderItem: (item: T) => React.ReactNode;
}

function List<T>({ items, renderItem }: ListProps<T>) {
    return (
        <div>
            {items.map((item, index) => (
                <div key={index}>{renderItem(item)}</div>
            ))}
        </div>
    );
}

// Usage with different types
interface User {
    id: number;
    name: string;
}

interface Product {
    id: number;
    title: string;
    price: number;
}

function App() {
    const users: User[] = [
        { id: 1, name: "Rahul" },
        { id: 2, name: "Simran" }
    ];
    
    const products: Product[] = [
        { id: 1, title: "Laptop", price: 45000 },
        { id: 2, title: "Mouse", price: 500 }
    ];
    
    return (
        <div>
            <h2>Users</h2>
            <List 
                items={users}
                renderItem={(user) => <div>{user.name}</div>}
            />
            
            <h2>Products</h2>
            <List 
                items={products}
                renderItem={(product) => (
                    <div>
                        {product.title} - ₹{product.price}
                    </div>
                )}
            />
        </div>
    );
}
```

---

### **Generic Table Component**

tsx

```tsx
interface Column<T> {
    header: string;
    accessor: keyof T;
    render?: (value: T[keyof T]) => React.ReactNode;
}

interface TableProps<T> {
    data: T[];
    columns: Column<T>[];
}

function Table<T>({ data, columns }: TableProps<T>) {
    return (
        <table>
            <thead>
                <tr>
                    {columns.map((column, index) => (
                        <th key={index}>{column.header}</th>
                    ))}
                </tr>
            </thead>
            <tbody>
                {data.map((row, rowIndex) => (
                    <tr key={rowIndex}>
                        {columns.map((column, colIndex) => {
                            const value = row[column.accessor];
                            return (
                                <td key={colIndex}>
                                    {column.render ? column.render(value) : String(value)}
                                </td>
                            );
                        })}
                    </tr>
                ))}
            </tbody>
        </table>
    );
}

// Usage
interface User {
    id: number;
    name: string;
    email: string;
    age: number;
}

function UserTable() {
    const users: User[] = [
        { id: 1, name: "Rahul", email: "rahul@example.com", age: 25 },
        { id: 2, name: "Simran", email: "simran@example.com", age: 23 }
    ];
    
    const columns: Column<User>[] = [
        { header: "ID", accessor: "id" },
        { header: "Name", accessor: "name" },
        { header: "Email", accessor: "email" },
        { 
            header: "Age", 
            accessor: "age",
            render: (age) => <strong>{age} years</strong>
        }
    ];
    
    return <Table data={users} columns={columns} />;
}
```

---

## 11.12 ForwardRef - Ref Forwarding 🔗

### **ForwardRef Kya Hai?**

**Parent component se child component ke DOM element ko access karna**

tsx

```tsx
import { forwardRef, useRef } from 'react';

// Without forwardRef - ❌ Doesn't work
function Input(props: { placeholder: string }) {
    return <input {...props} />;
}

// With forwardRef - ✅ Works
interface InputProps {
    placeholder: string;
}

const Input = forwardRef<HTMLInputElement, InputProps>((props, ref) => {
    return <input ref={ref} {...props} />;
});

// Usage
function App() {
    const inputRef = useRef<HTMLInputElement>(null);
    
    const focusInput = () => {
        inputRef.current?.focus();
    };
    
    return (
        <div>
            <Input ref={inputRef} placeholder="Enter text" />
            <button onClick={focusInput}>Focus Input</button>
        </div>
    );
}
```

---

### **ForwardRef with Custom Component**

tsx

```tsx
interface ButtonProps {
    variant: 'primary' | 'secondary';
    children: React.ReactNode;
    onClick?: () => void;
}

const Button = forwardRef<HTMLButtonElement, ButtonProps>(
    ({ variant, children, onClick }, ref) => {
        return (
            <button
                ref={ref}
                onClick={onClick}
                style={{
                    backgroundColor: variant === 'primary' ? 'blue' : 'gray',
                    color: 'white',
                    padding: '10px 20px'
                }}
            >
                {children}
            </button>
        );
    }
);

// Add display name for debugging
Button.displayName = 'Button';

// Usage
function App() {
    const buttonRef = useRef<HTMLButtonElement>(null);
    
    useEffect(() => {
        console.log('Button width:', buttonRef.current?.offsetWidth);
    }, []);
    
    return (
        <Button ref={buttonRef} variant="primary" onClick={() => console.log('Clicked')}>
            Click Me
        </Button>
    );
}
```

---


**Kya seekha?**

- ✅ Setup & Configuration
- ✅ Function components & Props
- ✅ useState, useEffect, useRef
- ✅ Context API
- ✅ Custom Hooks
- ✅ useReducer
- ✅ Generic Components
- ✅ ForwardRef