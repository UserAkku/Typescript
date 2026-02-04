# **PART 9: Advanced Type Manipulation - TypeScript Ki Asli Taaqat 🔥**

## 9.1 Conditional Types - Types Mein If-Else 🔀

### **Conditional Type Kya Hai?**

**Real life analogy:**

Socho **ID card banwana** hai:

- **Agar age &gt;= 18** → Adult ID card (driving license, voter ID)
- **Agar age &lt; 18** → Student ID card (school ID)

**TypeScript mein bhi same logic types ke liye:**

typescript

```typescript
T extends U ? X : Y
```

**Padho aise:** "Agar T, U extend karta hai (compatible hai), to X type, warna Y type"

---

### **Basic Conditional Type**

typescript

```typescript
type IsString<T> = T extends string ? true : false;

// Usage
type Test1 = IsString<string>;   // true
type Test2 = IsString<number>;   // false
type Test3 = IsString<boolean>;  // false
```

**Kya hua?**

- `T extends string` check kiya
- Agar T string hai → `true` type return
- Warna → `false` type return

---

### **Practical Example: Function Return Type**

typescript

```typescript
type ReturnTypeBasedOnInput<T> = T extends string 
    ? string[] 
    : T extends number 
    ? number[] 
    : never;

// Usage
type StringResult = ReturnTypeBasedOnInput<string>;  // string[]
type NumberResult = ReturnTypeBasedOnInput<number>;  // number[]
type BoolResult = ReturnTypeBasedOnInput<boolean>;   // never

function processInput<T extends string | number>(input: T): ReturnTypeBasedOnInput<T> {
    if (typeof input === "string") {
        return input.split("") as any;
    }
    return [input] as any;
}
```

---

### **Nested Conditional Types**

typescript

```typescript
type TypeName<T> = 
    T extends string ? "string" :
    T extends number ? "number" :
    T extends boolean ? "boolean" :
    T extends undefined ? "undefined" :
    T extends Function ? "function" :
    "object";

// Usage
type T1 = TypeName<string>;        // "string"
type T2 = TypeName<42>;            // "number"
type T3 = TypeName<true>;          // "boolean"
type T4 = TypeName<() => void>;    // "function"
type T5 = TypeName<{}>;            // "object"
```

---

### **Real Example: API Response Handler**

typescript

```typescript
// Success response has data
interface SuccessResponse<T> {
    success: true;
    data: T;
}

// Error response has error message
interface ErrorResponse {
    success: false;
    error: string;
}

// Conditional type based on success flag
type ApiResponse<T, Success extends boolean> = Success extends true
    ? SuccessResponse<T>
    : ErrorResponse;

// Usage
function handleResponse<T, S extends boolean>(
    success: S,
    dataOrError: S extends true ? T : string
): ApiResponse<T, S> {
    if (success) {
        return { success: true, data: dataOrError } as ApiResponse<T, S>;
    }
    return { success: false, error: dataOrError as string } as ApiResponse<T, S>;
}

// Type-safe usage
let successRes = handleResponse(true, { id: 1, name: "Rahul" });
// Type: SuccessResponse<{ id: number; name: string; }>
console.log(successRes.data.name);  // ✅ OK

let errorRes = handleResponse(false, "Network error");
// Type: ErrorResponse
console.log(errorRes.error);  // ✅ OK
// console.log(errorRes.data);  // ❌ Error: Property 'data' doesn't exist
```

---

### **Distributive Conditional Types**

typescript

```typescript
type ToArray<T> = T extends any ? T[] : never;

// Single type
type SingleArray = ToArray<string>;  // string[]

// Union type - distributes!
type UnionArray = ToArray<string | number>;  
// string[] | number[] (NOT (string | number)[])
```

**Kya hua?**

- Union type mein har member ke liye separately apply hota hai
- `string | number` → `ToArray<string> | ToArray<number>`
- Result: `string[] | number[]`

**Real use case:**

typescript

```typescript
type ExtractArrayType<T> = T extends (infer U)[] ? U : T;

type T1 = ExtractArrayType<string[]>;     // string
type T2 = ExtractArrayType<number[]>;     // number
type T3 = ExtractArrayType<string>;       // string (not array)

// With union
type T4 = ExtractArrayType<string[] | number[]>;  // string | number
```

---

## 9.2 The `infer` Keyword - Type Ko Extract Karo 🔍

### `infer` Kya Hai?

**Real life example:**

Tumhare paas **wrapped gift** hai. Tum usko **unwrap** karke andar ki cheez nikalna chahte ho.

**TypeScript mein:**

- Wrapped type hai (Array, Promise, Function, etc.)
- `infer` se inner type extract karo

---

### **Basic** `infer` Example

typescript

```typescript
// Array se element type nikalo
type GetArrayType<T> = T extends (infer U)[] ? U : never;

type T1 = GetArrayType<string[]>;    // string
type T2 = GetArrayType<number[]>;    // number
type T3 = GetArrayType<boolean[]>;   // boolean
type T4 = GetArrayType<string>;      // never (not an array)
```

**Breakdown:**

- `T extends (infer U)[]` - Agar T array hai
- `infer U` - Array ke element ka type U mein store karo
- Return U

---

### **Promise Unwrapping**

typescript

```typescript
type UnwrapPromise<T> = T extends Promise<infer U> ? U : T;

type T1 = UnwrapPromise<Promise<string>>;     // string
type T2 = UnwrapPromise<Promise<number>>;     // number
type T3 = UnwrapPromise<string>;              // string (not a promise)

// Real usage
async function fetchUser(): Promise<{ id: number; name: string }> {
    return { id: 1, name: "Rahul" };
}

type User = UnwrapPromise<ReturnType<typeof fetchUser>>;
// { id: number; name: string }
```

---

### **Function Return Type Extraction**

typescript

```typescript
type GetReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

// Examples
function getString(): string {
    return "hello";
}

function getNumber(): number {
    return 42;
}

function getUser(): { name: string; age: number } {
    return { name: "Rahul", age: 25 };
}

type RT1 = GetReturnType<typeof getString>;  // string
type RT2 = GetReturnType<typeof getNumber>;  // number
type RT3 = GetReturnType<typeof getUser>;    // { name: string; age: number }
```

**Ye hai built-in** `ReturnType` utility ka implementation!

---

### **Function Parameters Extraction**

typescript

```typescript
type GetParameters<T> = T extends (...args: infer P) => any ? P : never;

function processUser(name: string, age: number, email: string): void {
    console.log(name, age, email);
}

type Params = GetParameters<typeof processUser>;
// [string, number, string]

// Usage
const params: Params = ["Rahul", 25, "rahul@example.com"];
processUser(...params);  // ✅ Type-safe
```

---

### **First Element of Tuple**

typescript

```typescript
type First<T extends any[]> = T extends [infer F, ...any[]] ? F : never;

type T1 = First<[string, number, boolean]>;  // string
type T2 = First<[number]>;                   // number
type T3 = First<[]>;                         // never
```

---

### **Last Element of Tuple**

typescript

```typescript
type Last<T extends any[]> = T extends [...any[], infer L] ? L : never;

type T1 = Last<[string, number, boolean]>;  // boolean
type T2 = Last<[number]>;                   // number
type T3 = Last<[]>;                         // never
```

---

### **Real Example: Event Emitter**

typescript

```typescript
// Event map
interface Events {
    click: [x: number, y: number];
    change: [value: string];
    submit: [data: FormData];
}

// Extract event name
type EventName = keyof Events;

// Extract handler type for specific event
type EventHandler<E extends EventName> = Events[E] extends infer P
    ? (...args: P) => void
    : never;

class EventEmitter {
    private listeners: {
        [E in EventName]?: EventHandler<E>[]
    } = {};
    
    on<E extends EventName>(event: E, handler: EventHandler<E>): void {
        if (!this.listeners[event]) {
            this.listeners[event] = [];
        }
        this.listeners[event]!.push(handler);
    }
    
    emit<E extends EventName>(event: E, ...args: Events[E]): void {
        const handlers = this.listeners[event];
        if (handlers) {
            handlers.forEach(handler => handler(...args));
        }
    }
}

// Usage
const emitter = new EventEmitter();

// Type-safe event handlers
emitter.on("click", (x, y) => {
    console.log(`Clicked at ${x}, ${y}`);
});

emitter.on("change", (value) => {
    console.log(`Changed to ${value}`);
});

// Type-safe emit
emitter.emit("click", 10, 20);     // ✅ OK
emitter.emit("change", "hello");   // ✅ OK
// emitter.emit("click", "wrong");  // ❌ Error
```

---

## 9.3 Mapped Types - Transform Karo Types Ko 🗺️

### **Mapped Type Kya Hai?**

**Real life example:**

Tumhare paas **students ki list** hai. Tum har student ko **transform** karna chahte ho:

- Sab ko uppercase mein
- Sab ko prefix add karo
- Sab ko number assign karo

**TypeScript mein:**

- Existing type ke har property ko transform karo
- New type banao

---

### **Basic Mapped Type Syntax**

typescript

```typescript
type MappedType<T> = {
    [P in keyof T]: TransformedType
}
```

**Breakdown:**

- `P in keyof T` - T ke har key ko iterate karo
- `P` current key hai
- `: TransformedType` - Naya type assign karo

---

### **Example 1: Make All Optional**

typescript

```typescript
type MyPartial<T> = {
    [P in keyof T]?: T[P];
}

interface User {
    id: number;
    name: string;
    email: string;
}

type PartialUser = MyPartial<User>;
// {
//     id?: number;
//     name?: string;
//     email?: string;
// }

// Built-in Partial<T> ka ye implementation hai!
```

---

### **Example 2: Make All Required**

typescript

```typescript
type MyRequired<T> = {
    [P in keyof T]-?: T[P];
}
//                ^ minus sign removes optionality

interface Config {
    apiUrl?: string;
    timeout?: number;
    debug?: boolean;
}

type RequiredConfig = MyRequired<Config>;
// {
//     apiUrl: string;
//     timeout: number;
//     debug: boolean;
// }
```

---

### **Example 3: Make All Readonly**

typescript

```typescript
type MyReadonly<T> = {
    readonly [P in keyof T]: T[P];
}

interface User {
    id: number;
    name: string;
}

type ReadonlyUser = MyReadonly<User>;
// {
//     readonly id: number;
//     readonly name: string;
// }

let user: ReadonlyUser = { id: 1, name: "Rahul" };
// user.name = "Raj";  // ❌ Error: Cannot assign to 'name'
```

---

### **Example 4: Change All Types**

typescript

```typescript
type Stringify<T> = {
    [P in keyof T]: string;
}

interface Numbers {
    a: number;
    b: number;
    c: number;
}

type StringNumbers = Stringify<Numbers>;
// {
//     a: string;
//     b: string;
//     c: string;
// }
```

---

### **Example 5: Add Prefix to Keys**

typescript

```typescript
type Prefix<T, P extends string> = {
    [K in keyof T as `${P}${string & K}`]: T[K];
}

interface User {
    id: number;
    name: string;
}

type PrefixedUser = Prefix<User, "user_">;
// {
//     user_id: number;
//     user_name: string;
// }
```

---

### **Example 6: Pick Specific Properties**

typescript

```typescript
type MyPick<T, K extends keyof T> = {
    [P in K]: T[P];
}

interface User {
    id: number;
    name: string;
    email: string;
    age: number;
    address: string;
}

type UserPreview = MyPick<User, "id" | "name">;
// {
//     id: number;
//     name: string;
// }
```

---

### **Example 7: Omit Specific Properties**

typescript

```typescript
type MyOmit<T, K extends keyof T> = {
    [P in keyof T as P extends K ? never : P]: T[P];
}

interface User {
    id: number;
    name: string;
    password: string;
    email: string;
}

type UserWithoutPassword = MyOmit<User, "password">;
// {
//     id: number;
//     name: string;
//     email: string;
// }
```

---

### **Real Example: API Response Wrapper**

typescript

```typescript
// Wrap all properties in loading/error states
type AsyncState<T> = {
    [P in keyof T]: {
        loading: boolean;
        data: T[P] | null;
        error: string | null;
    }
}

interface UserData {
    profile: { name: string; age: number };
    posts: Array<{ title: string; content: string }>;
    followers: number;
}

type AsyncUserData = AsyncState<UserData>;
// {
//     profile: {
//         loading: boolean;
//         data: { name: string; age: number } | null;
//         error: string | null;
//     };
//     posts: {
//         loading: boolean;
//         data: Array<{ title: string; content: string }> | null;
//         error: string | null;
//     };
//     followers: {
//         loading: boolean;
//         data: number | null;
//         error: string | null;
//     };
// }
```

---

## 9.4 Template Literal Types - String Manipulation 📝

### **Template Literal Type Kya Hai?**

JavaScript ke template literals yaad hain?

javascript

```javascript
let name = "Rahul";
let greeting = `Hello, ${name}!`;
```

**TypeScript mein bhi types ke liye same:**

typescript

```typescript
type Greeting = `Hello, ${string}!`;

let g1: Greeting = "Hello, Rahul!";    // ✅ OK
let g2: Greeting = "Hello, Simran!";   // ✅ OK
let g3: Greeting = "Hi, Rahul!";       // ❌ Error
```

---

### **Union with Template Literals**

typescript

```typescript
type Color = "red" | "blue" | "green";
type Size = "small" | "medium" | "large";

type Product = `${Size}-${Color}-shirt`;

// Result type:
// "small-red-shirt" | "small-blue-shirt" | "small-green-shirt" |
// "medium-red-shirt" | "medium-blue-shirt" | "medium-green-shirt" |
// "large-red-shirt" | "large-blue-shirt" | "large-green-shirt"

let p1: Product = "small-red-shirt";    // ✅ OK
let p2: Product = "large-blue-shirt";   // ✅ OK
let p3: Product = "tiny-red-shirt";     // ❌ Error
```

---

### **Event Names Generation**

typescript

```typescript
type EventName = "click" | "scroll" | "mousemove";
type Handler = `on${Capitalize<EventName>}`;

// Result:
// "onClick" | "onScroll" | "onMousemove"

interface Events {
    onClick: () => void;
    onScroll: () => void;
    onMousemove: () => void;
}
```

---

### **CSS Properties**

typescript

```typescript
type CSSProperty = "margin" | "padding";
type Direction = "top" | "right" | "bottom" | "left";

type CSSProperties = `${CSSProperty}-${Direction}`;

// Result:
// "margin-top" | "margin-right" | "margin-bottom" | "margin-left" |
// "padding-top" | "padding-right" | "padding-bottom" | "padding-left"

let css1: CSSProperties = "margin-top";     // ✅ OK
let css2: CSSProperties = "padding-left";   // ✅ OK
```

---

### **Built-in String Manipulation Types**

typescript

```typescript
// Uppercase
type Upper = Uppercase<"hello">;  // "HELLO"

// Lowercase  
type Lower = Lowercase<"HELLO">;  // "hello"

// Capitalize
type Cap = Capitalize<"hello">;   // "Hello"

// Uncapitalize
type Uncap = Uncapitalize<"Hello">;  // "hello"
```

---

### **Real Example: Getter/Setter Generation**

typescript

```typescript
type Getters<T> = {
    [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
}

type Setters<T> = {
    [K in keyof T as `set${Capitalize<string & K>}`]: (value: T[K]) => void;
}

interface User {
    name: string;
    age: number;
    email: string;
}

type UserGetters = Getters<User>;
// {
//     getName: () => string;
//     getAge: () => number;
//     getEmail: () => string;
// }

type UserSetters = Setters<User>;
// {
//     setName: (value: string) => void;
//     setAge: (value: number) => void;
//     setEmail: (value: string) => void;
// }

type UserWithGettersSetters = User & UserGetters & UserSetters;

class UserClass implements UserWithGettersSetters {
    name: string = "";
    age: number = 0;
    email: string = "";
    
    getName() { return this.name; }
    getAge() { return this.age; }
    getEmail() { return this.email; }
    
    setName(value: string) { this.name = value; }
    setAge(value: number) { this.age = value; }
    setEmail(value: string) { this.email = value; }
}
```

---

## 9.5 Advanced Utility Types - Power Tools 🛠️

### **1. Record&lt;K, T&gt; - Key-Value Mapping**

typescript

```typescript
// Simple record
type UserRoles = Record<string, string>;

let roles: UserRoles = {
    "admin": "Administrator",
    "user": "Regular User",
    "guest": "Guest User"
};

// With specific keys
type Permissions = Record<"read" | "write" | "delete", boolean>;

let userPermissions: Permissions = {
    read: true,
    write: true,
    delete: false
};
```

---

### **2. Exclude&lt;T, U&gt; - Remove Types from Union**

typescript

```typescript
type AllStatus = "pending" | "approved" | "rejected" | "cancelled";
type ActiveStatus = Exclude<AllStatus, "cancelled" | "rejected">;
// "pending" | "approved"

// Real example
type Primitive = string | number | boolean | null | undefined;
type NonNullablePrimitive = Exclude<Primitive, null | undefined>;
// string | number | boolean
```

---

### **3. Extract&lt;T, U&gt; - Keep Only Matching Types**

typescript

```typescript
type AllStatus = "pending" | "approved" | "rejected" | "cancelled";
type CompletedStatus = Extract<AllStatus, "approved" | "rejected">;
// "approved" | "rejected"

// Real example
type Shape = "circle" | "square" | "rectangle" | "triangle";
type FourSided = Extract<Shape, "square" | "rectangle">;
// "square" | "rectangle"
```

---

### **4. NonNullable&lt;T&gt; - Remove null and undefined**

typescript

```typescript
type MaybeString = string | null | undefined;
type DefiniteString = NonNullable<MaybeString>;
// string

// Real example
function processValue(value: string | null | undefined): NonNullable<typeof value> {
    if (value === null || value === undefined) {
        throw new Error("Value required");
    }
    return value;  // Type: string
}
```

---

### **5. ReturnType&lt;T&gt; - Extract Return Type**

typescript

```typescript
function createUser() {
    return {
        id: 1,
        name: "Rahul",
        email: "rahul@example.com"
    };
}

type User = ReturnType<typeof createUser>;
// {
//     id: number;
//     name: string;
//     email: string;
// }

let user: User = createUser();
```

---

### **6. Parameters&lt;T&gt; - Extract Function Parameters**

typescript

```typescript
function updateUser(id: number, name: string, email: string): void {
    // Update logic
}

type UpdateUserParams = Parameters<typeof updateUser>;
// [number, string, string]

// Usage
const params: UpdateUserParams = [1, "Rahul", "rahul@example.com"];
updateUser(...params);
```

---

### **7. Awaited&lt;T&gt; - Unwrap Promise**

typescript

```typescript
type A = Awaited<Promise<string>>;  // string
type B = Awaited<Promise<Promise<number>>>;  // number (recursive unwrap)

// Real example
async function fetchUser() {
    return { id: 1, name: "Rahul" };
}

type User = Awaited<ReturnType<typeof fetchUser>>;
// { id: number; name: string }
```

---

### **Real Example: Form Builder**

typescript

```typescript
// Form field configuration
interface FieldConfig {
    type: "text" | "number" | "email" | "password" | "checkbox";
    label: string;
    required: boolean;
    defaultValue?: string | number | boolean;
}

// Generate form type from config
type FormType<T extends Record<string, FieldConfig>> = {
    [K in keyof T]: T[K]["type"] extends "checkbox"
        ? boolean
        : T[K]["type"] extends "number"
        ? number
        : string;
}

// Define form
const loginForm = {
    username: {
        type: "text" as const,
        label: "Username",
        required: true
    },
    password: {
        type: "password" as const,
        label: "Password",
        required: true
    },
    rememberMe: {
        type: "checkbox" as const,
        label: "Remember Me",
        required: false
    }
};

type LoginFormData = FormType<typeof loginForm>;
// {
//     username: string;
//     password: string;
//     rememberMe: boolean;
// }

function submitForm(data: LoginFormData) {
    console.log(data.username);    // string
    console.log(data.password);    // string
    console.log(data.rememberMe);  // boolean
}
```

---


**Kya seekha?**

- ✅ Conditional Types (if-else for types)
- ✅ `infer` keyword (type extraction)
- ✅ Mapped Types (transform types)
- ✅ Template Literal Types (string manipulation)
- ✅ Advanced Utility Types (powerful helpers)

**Ye sabse powerful features hain TypeScript ke!**