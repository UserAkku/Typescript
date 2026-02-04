# **PART 11: TypeScript with React ⚛️**

## 11.1 React + TypeScript Setup 🚀

### **Why TypeScript with React?**

**JavaScript React mein problems:**

jsx

```jsx
// JavaScript
function UserCard(props) {
    return (
        <div>
            <h1>{props.name}</h1>
            <p>Age: {props.age}</p>
            <p>Email: {props.email}</p>
        </div>
    );
}

// Usage - koi error nahi!
<UserCard name="Rahul" />  
// ❌ age aur email missing, runtime pe undefined

<UserCard name="Rahul" age="twenty five" email="rahul@example.com" />
// ❌ age string hai, number hona chahiye

<UserCard naam="Rahul" age={25} email="rahul@example.com" />
// ❌ Typo: 'naam' instead of 'name'
```

**TypeScript React mein benefits:**

tsx

```tsx
// TypeScript
interface UserCardProps {
    name: string;
    age: number;
    email: string;
}

function UserCard(props: UserCardProps) {
    return (
        <div>
            <h1>{props.name}</h1>
            <p>Age: {props.age}</p>
            <p>Email: {props.email}</p>
        </div>
    );
}

// Usage
<UserCard name="Rahul" />  
// ❌ Error: Property 'age' is missing

<UserCard name="Rahul" age="twenty five" email="rahul@example.com" />
// ❌ Error: Type 'string' is not assignable to type 'number'

<UserCard naam="Rahul" age={25} email="rahul@example.com" />
// ❌ Error: Property 'naam' does not exist

<UserCard name="Rahul" age={25} email="rahul@example.com" />
// ✅ Perfect!
```

---

### **Project Setup**

#### **Method 1: Create React App with TypeScript**

bash

```bash
# New project with TypeScript template
npx create-react-app my-app --template typescript

cd my-app
npm start
```

**Generated structure:**
```
my-app/
├── src/
│   ├── App.tsx           # .tsx extension (not .jsx)
│   ├── index.tsx
│   └── react-app-env.d.ts  # React type definitions
├── tsconfig.json         # TypeScript config
└── package.json
```

---

#### **Method 2: Add TypeScript to Existing Project**

bash

```bash
# Install TypeScript + React types
npm install --save typescript @types/react @types/react-dom @types/node

# Generate tsconfig.json
npx tsc --init
```

**Rename files:**

- `.js` → `.ts`
- `.jsx` → `.tsx`

---

### **tsconfig.json for React**

json

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "jsx": "react-jsx",              // React 17+ (new JSX transform)
    // "jsx": "react",               // React 16 (old)
    "module": "ESNext",
    "moduleResolution": "node",
    
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    
    "allowSyntheticDefaultImports": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true                   // Webpack/Vite handles compilation
  },
  "include": ["src"]
}
```

---

## 11.2 Function Components - Basic Types 🎯

### **Basic Function Component**

tsx

```tsx
// Method 1: Normal function
function Greeting() {
    return <h1>Hello, World!</h1>;
}

// Method 2: Arrow function
const Greeting = () => {
    return <h1>Hello, World!</h1>;
};

// Method 3: With explicit type
const Greeting: React.FC = () => {
    return <h1>Hello, World!</h1>;
};
```

**Note:** `React.FC` (FunctionComponent) ab generally avoid karte hain. Direct function prefer karo.

---

### **Component with Props**

tsx

```tsx
// Define props interface
interface GreetingProps {
    name: string;
    age: number;
}

// Method 1: Preferred way
function Greeting(props: GreetingProps) {
    return (
        <div>
            <h1>Hello, {props.name}!</h1>
            <p>You are {props.age} years old.</p>
        </div>
    );
}

// Method 2: With destructuring (most common)
function Greeting({ name, age }: GreetingProps) {
    return (
        <div>
            <h1>Hello, {name}!</h1>
            <p>You are {age} years old.</p>
        </div>
    );
}

// Usage
<Greeting name="Rahul" age={25} />
```

---

### **Optional Props**

tsx

```tsx
interface UserCardProps {
    name: string;
    age: number;
    email?: string;        // Optional
    phone?: string;        // Optional
}

function UserCard({ name, age, email, phone }: UserCardProps) {
    return (
        <div>
            <h2>{name}</h2>
            <p>Age: {age}</p>
            {email && <p>Email: {email}</p>}
            {phone && <p>Phone: {phone}</p>}
        </div>
    );
}

// Valid usage
<UserCard name="Rahul" age={25} />
<UserCard name="Rahul" age={25} email="rahul@example.com" />
<UserCard name="Rahul" age={25} email="rahul@example.com" phone="9876543210" />
```

---

### **Default Props**

tsx

```tsx
interface ButtonProps {
    text: string;
    color?: string;
    size?: 'small' | 'medium' | 'large';
}

function Button({ 
    text, 
    color = 'blue',      // Default value
    size = 'medium'      // Default value
}: ButtonProps) {
    return (
        <button 
            style={{ 
                backgroundColor: color,
                fontSize: size === 'small' ? '12px' : size === 'large' ? '20px' : '16px'
            }}
        >
            {text}
        </button>
    );
}

// Usage
<Button text="Click me" />  // Uses defaults
<Button text="Click me" color="red" size="large" />
```

---

### **Union Type Props**

tsx

```tsx
interface AlertProps {
    message: string;
    type: 'success' | 'warning' | 'error' | 'info';  // Limited options
}

function Alert({ message, type }: AlertProps) {
    const colors = {
        success: '#4caf50',
        warning: '#ff9800',
        error: '#f44336',
        info: '#2196f3'
    };
    
    return (
        <div style={{ 
            backgroundColor: colors[type],
            padding: '10px',
            color: 'white'
        }}>
            {message}
        </div>
    );
}

// Usage
<Alert message="Success!" type="success" />
<Alert message="Warning!" type="warning" />
// <Alert message="Info" type="danger" />  // ❌ Error: 'danger' not allowed
```

---

### **Object Props**

tsx

```tsx
interface User {
    id: number;
    name: string;
    email: string;
}

interface UserProfileProps {
    user: User;
}

function UserProfile({ user }: UserProfileProps) {
    return (
        <div>
            <h2>{user.name}</h2>
            <p>ID: {user.id}</p>
            <p>Email: {user.email}</p>
        </div>
    );
}

// Usage
const userData: User = {
    id: 1,
    name: "Rahul",
    email: "rahul@example.com"
};

<UserProfile user={userData} />
```

---

### **Array Props**

tsx

```tsx
interface Product {
    id: number;
    name: string;
    price: number;
}

interface ProductListProps {
    products: Product[];
}

function ProductList({ products }: ProductListProps) {
    return (
        <div>
            {products.map(product => (
                <div key={product.id}>
                    <h3>{product.name}</h3>
                    <p>₹{product.price}</p>
                </div>
            ))}
        </div>
    );
}

// Usage
const products: Product[] = [
    { id: 1, name: "Laptop", price: 45000 },
    { id: 2, name: "Mouse", price: 500 }
];

<ProductList products={products} />
```

---

### **Function Props (Callbacks)**

tsx

```tsx
interface ButtonProps {
    text: string;
    onClick: () => void;                    // No parameters, no return
}

interface InputProps {
    value: string;
    onChange: (value: string) => void;      // Takes string, returns nothing
}

interface FormProps {
    onSubmit: (data: FormData) => Promise<void>;  // Async function
}

// Example 1: Button with callback
function Button({ text, onClick }: ButtonProps) {
    return <button onClick={onClick}>{text}</button>;
}

<Button text="Click me" onClick={() => console.log('Clicked!')} />

// Example 2: Input with callback
function Input({ value, onChange }: InputProps) {
    return (
        <input 
            value={value} 
            onChange={(e) => onChange(e.target.value)} 
        />
    );
}

<Input value={name} onChange={(newValue) => setName(newValue)} />
```

---

## 11.3 Children Props - Nested Components 👶

### **Basic Children**

tsx

```tsx
interface CardProps {
    children: React.ReactNode;  // Accepts anything React can render
}

function Card({ children }: CardProps) {
    return (
        <div style={{ border: '1px solid #ccc', padding: '20px' }}>
            {children}
        </div>
    );
}

// Usage
<Card>
    <h1>Title</h1>
    <p>Some content here</p>
</Card>
```

`React.ReactNode` includes:

- JSX elements
- Strings
- Numbers
- Arrays
- Fragments
- null/undefined

---

### **Specific Children Types**

tsx

```tsx
// Only string
interface TitleProps {
    children: string;
}

function Title({ children }: TitleProps) {
    return <h1>{children.toUpperCase()}</h1>;
}

<Title>Hello World</Title>  // ✅ OK
// <Title><span>Hello</span></Title>  // ❌ Error: not string

// Only single React element
interface WrapperProps {
    children: React.ReactElement;
}

function Wrapper({ children }: WrapperProps) {
    return <div className="wrapper">{children}</div>;
}

<Wrapper><p>Content</p></Wrapper>  // ✅ OK
// <Wrapper>Text</Wrapper>  // ❌ Error: not element

// Array of elements
interface ListProps {
    children: React.ReactElement[];
}
```

---

### **Children with Props Validation**

tsx

```tsx
interface TabsProps {
    children: React.ReactElement<TabProps>[];
}

interface TabProps {
    label: string;
    children: React.ReactNode;
}

function Tab({ label, children }: TabProps) {
    return <div>{children}</div>;
}

function Tabs({ children }: TabsProps) {
    return (
        <div>
            {children.map((child, index) => (
                <div key={index}>
                    <button>{child.props.label}</button>
                    {child}
                </div>
            ))}
        </div>
    );
}

// Usage
<Tabs>
    <Tab label="Tab 1">Content 1</Tab>
    <Tab label="Tab 2">Content 2</Tab>
</Tabs>
```

---

## 11.4 State Management with useState 🔄

### **Basic useState**

tsx

```tsx
import { useState } from 'react';

function Counter() {
    // Type inference works!
    const [count, setCount] = useState(0);  // count: number
    
    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
            <button onClick={() => setCount(count - 1)}>Decrement</button>
        </div>
    );
}
```

**TypeScript automatically infers:**

- `count` is `number`
- `setCount` accepts `number` or `(prev: number) => number`

---

### **Explicit Type Annotation**

tsx

```tsx
// When initial value is null/undefined
const [user, setUser] = useState<User | null>(null);

interface User {
    id: number;
    name: string;
    email: string;
}

function UserComponent() {
    const [user, setUser] = useState<User | null>(null);
    
    const fetchUser = async () => {
        const response = await fetch('/api/user');
        const data: User = await response.json();
        setUser(data);
    };
    
    return (
        <div>
            {user ? (
                <div>
                    <h2>{user.name}</h2>
                    <p>{user.email}</p>
                </div>
            ) : (
                <button onClick={fetchUser}>Load User</button>
            )}
        </div>
    );
}
```

---

### **Complex State Objects**

tsx

```tsx
interface FormState {
    username: string;
    email: string;
    password: string;
    agreeToTerms: boolean;
}

function SignupForm() {
    const [formData, setFormData] = useState<FormState>({
        username: '',
        email: '',
        password: '',
        agreeToTerms: false
    });
    
    const handleChange = (field: keyof FormState, value: string | boolean) => {
        setFormData(prev => ({
            ...prev,
            [field]: value
        }));
    };
    
    return (
        <form>
            <input 
                value={formData.username}
                onChange={(e) => handleChange('username', e.target.value)}
            />
            <input 
                value={formData.email}
                onChange={(e) => handleChange('email', e.target.value)}
            />
            <input 
                type="password"
                value={formData.password}
                onChange={(e) => handleChange('password', e.target.value)}
            />
            <input 
                type="checkbox"
                checked={formData.agreeToTerms}
                onChange={(e) => handleChange('agreeToTerms', e.target.checked)}
            />
        </form>
    );
}
```

---

### **Array State**

tsx

```tsx
interface Todo {
    id: number;
    text: string;
    completed: boolean;
}

function TodoList() {
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
                todo.id === id 
                    ? { ...todo, completed: !todo.completed }
                    : todo
            )
        );
    };
    
    const deleteTodo = (id: number) => {
        setTodos(prev => prev.filter(todo => todo.id !== id));
    };
    
    return (
        <div>
            {todos.map(todo => (
                <div key={todo.id}>
                    <input 
                        type="checkbox" 
                        checked={todo.completed}
                        onChange={() => toggleTodo(todo.id)}
                    />
                    <span>{todo.text}</span>
                    <button onClick={() => deleteTodo(todo.id)}>Delete</button>
                </div>
            ))}
        </div>
    );
}
```

---

## 11.5 Event Handling - Typed Events 🖱️

### **Common Event Types**

tsx

```tsx
function EventsExample() {
    // Mouse events
    const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
        console.log('Button clicked at', e.clientX, e.clientY);
    };
    
    const handleDivClick = (e: React.MouseEvent<HTMLDivElement>) => {
        console.log('Div clicked');
    };
    
    // Keyboard events
    const handleKeyPress = (e: React.KeyboardEvent<HTMLInputElement>) => {
        if (e.key === 'Enter') {
            console.log('Enter pressed');
        }
    };
    
    // Form events
    const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault();
        console.log('Form submitted');
    };
    
    // Input change events
    const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
        console.log('Input value:', e.target.value);
    };
    
    // Select change
    const handleSelectChange = (e: React.ChangeEvent<HTMLSelectElement>) => {
        console.log('Selected:', e.target.value);
    };
    
    // Textarea change
    const handleTextareaChange = (e: React.ChangeEvent<HTMLTextAreaElement>) => {
        console.log('Textarea value:', e.target.value);
    };
    
    return (
        <div onClick={handleDivClick}>
            <button onClick={handleClick}>Click me</button>
            <input onKeyPress={handleKeyPress} onChange={handleChange} />
            <select onChange={handleSelectChange}>
                <option value="1">Option 1</option>
                <option value="2">Option 2</option>
            </select>
            <textarea onChange={handleTextareaChange} />
            <form onSubmit={handleSubmit}>
                <button type="submit">Submit</button>
            </form>
        </div>
    );
}
```

---

### **Generic Event Handler**

tsx

```tsx
function Form() {
    const [formData, setFormData] = useState({
        name: '',
        email: '',
        message: ''
    });
    
    // Generic handler for all inputs
    const handleInputChange = (
        e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
    ) => {
        const { name, value } = e.target;
        setFormData(prev => ({
            ...prev,
            [name]: value
        }));
    };
    
    return (
        <form>
            <input 
                name="name"
                value={formData.name}
                onChange={handleInputChange}
            />
            <input 
                name="email"
                value={formData.email}
                onChange={handleInputChange}
            />
            <textarea 
                name="message"
                value={formData.message}
                onChange={handleInputChange}
            />
        </form>
    );
}
```

---

### **Custom Event Handlers**

tsx

```tsx
interface ButtonProps {
    onClick: (id: number, name: string) => void;
}

function Button({ onClick }: ButtonProps) {
    return (
        <button onClick={() => onClick(1, 'Rahul')}>
            Click me
        </button>
    );
}

// Usage
<Button onClick={(id, name) => {
    console.log(`User ${id}: ${name}`);
}} />
```

---

## 11.6 useEffect - Side Effects with Types ⚡

### **Basic useEffect**

tsx

```tsx
import { useState, useEffect } from 'react';

function DataFetcher() {
    const [data, setData] = useState<string | null>(null);
    
    useEffect(() => {
        // Effect function
        console.log('Component mounted');
        
        // Cleanup function (optional)
        return () => {
            console.log('Component unmounted');
        };
    }, []); // Dependency array
    
    return <div>{data}</div>;
}
```

---

### **Async useEffect (Data Fetching)**

tsx

```tsx
interface User {
    id: number;
    name: string;
    email: string;
}

function UserProfile({ userId }: { userId: number }) {
    const [user, setUser] = useState<User | null>(null);
    const [loading, setLoading] = useState<boolean>(true);
    const [error, setError] = useState<string | null>(null);
    
    useEffect(() => {
        let cancelled = false;
        
        const fetchUser = async () => {
            try {
                setLoading(true);
                const response = await fetch(`/api/users/${userId}`);
                const data: User = await response.json();
                
                if (!cancelled) {
                    setUser(data);
                    setError(null);
                }
            } catch (err) {
                if (!cancelled) {
                    setError('Failed to fetch user');
                }
            } finally {
                if (!cancelled) {
                    setLoading(false);
                }
            }
        };
        
        fetchUser();
        
        // Cleanup
        return () => {
            cancelled = true;
        };
    }, [userId]);  // Re-run when userId changes
    
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

### **Cleanup with Timers**

tsx

```tsx
function Timer() {
    const [seconds, setSeconds] = useState(0);
    
    useEffect(() => {
        const intervalId = setInterval(() => {
            setSeconds(prev => prev + 1);
        }, 1000);
        
        // Cleanup: clear interval when component unmounts
        return () => {
            clearInterval(intervalId);
        };
    }, []);
    
    return <div>Seconds: {seconds}</div>;
}
```

---

## 11.7 useRef - References with Types 🎯

### **DOM Element Refs**

tsx

```tsx
import { useRef, useEffect } from 'react';

function InputFocus() {
    const inputRef = useRef<HTMLInputElement>(null);
    
    useEffect(() => {
        // Focus input on mount
        inputRef.current?.focus();
    }, []);
    
    const handleClick = () => {
        if (inputRef.current) {
            inputRef.current.value = '';
            inputRef.current.focus();
        }
    };
    
    return (
        <div>
            <input ref={inputRef} />
            <button onClick={handleClick}>Clear & Focus</button>
        </div>
    );
}
```

---

### **Storing Mutable Values**

tsx

```tsx
function PreviousValue() {
    const [count, setCount] = useState(0);
    const prevCountRef = useRef<number>();
    
    useEffect(() => {
        prevCountRef.current = count;
    }, [count]);
    
    return (
        <div>
            <p>Current: {count}</p>
            <p>Previous: {prevCountRef.current}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
        </div>
    );
}
```

---

### **Different Element Types**

tsx

```tsx
function RefExamples() {
    const buttonRef = useRef<HTMLButtonElement>(null);
    const divRef = useRef<HTMLDivElement>(null);
    const videoRef = useRef<HTMLVideoElement>(null);
    const canvasRef = useRef<HTMLCanvasElement>(null);
    
    return (
        <div ref={divRef}>
            <button ref={buttonRef}>Button</button>
            <video ref={videoRef} />
            <canvas ref={canvasRef} />
        </div>
    );
}
```

---


**Kya seekha?**

- ✅ React + TypeScript setup
- ✅ Function components
- ✅ Props typing (sabhi types)
- ✅ Children props
- ✅ useState with types
- ✅ Event handling
- ✅ useEffect
- ✅ useRef