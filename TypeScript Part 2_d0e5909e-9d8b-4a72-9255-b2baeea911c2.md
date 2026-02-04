# **PART 2: Types - TypeScript Ka Dil**

## 2.1 Type Kya Hota Hai? (Concept Samjho)

Bhai, pehle samjho **type** ka matlab kya hai real life mein:

### **Real Life Example:**

Socho tumhare paas ek **dabba (container)** hai:

1. **Paani ka dabba** 💧
   - Isme sirf paani aa sakta hai
   - Agar tum tel daaloge, galat hai
2. **Chawal ka dabba** 🍚
   - Isme sirf chawal
   - Agar sugar daaloge, mess ho jayega
3. **Money box** 💰
   - Isme sirf paise (numbers)
   - Agar letters daaloge, kaam nahi karega

**TypeScript mein bhi same concept hai:**

- Har variable ek dabba hai
- Type batata hai ki **iss dabbe mein kya aa sakta hai**
- Galat cheez daali to **error**

---

## 2.2 JavaScript Mein Types Kaise Hote The?

JavaScript mein types **loose/flexible** the:

javascript

```javascript
// JavaScript
let box = 10;           // Ab box mein number hai
console.log(box);       // 10

box = "hello";          // Ab box mein string hai
console.log(box);       // "hello"

box = true;             // Ab box mein boolean hai
console.log(box);       // true

box = [1, 2, 3];       // Ab box mein array hai
console.log(box);       // [1, 2, 3]
```

**Dekho kya hua:**

- `box` variable ne **har baar apna type badal liya**
- JavaScript ne koi complaint nahi ki
- Ye **flexibility** kabhi kabhi **dangerous** ho jati hai

**Problem Example:**

javascript

```javascript
let userAge = 25;              // Number hai
// ... 100 lines of code ...
userAge = "twenty five";       // Galti se string ban gaya
// ... 100 lines of code ...
let nextYearAge = userAge + 1; // "twenty five1" ❌
```

---

## 2.3 TypeScript Mein Types Kaise Kaam Karte Hain?

TypeScript mein tum **pehle hi bata do** ki variable mein **kya type** ka data aayega:

typescript

```typescript
let userAge: number = 25;
```

**Isko padho aise:**

- `userAge` naam ka variable hai
- Ye **colon (**`:`) ke baad `number` likha hai
- Matlab: "userAge mein **sirf numbers** aa sakte hain"

**Ab agar tum galat type dalo:**

typescript

```typescript
let userAge: number = 25;
userAge = "twenty five";  // ❌ ERROR!
```

**TypeScript compiler bolega:**
```
Error: Type 'string' is not assignable to type 'number'
```

**Translation:** "Bhai, tumne kaha tha userAge mein number aayega, par tum string daal rahe ho!"

---

## 2.4 Basic Types - Ek Ek Karke Samjho

### **Type 1: Number** 🔢

**Kya hai?**

- Sab tarah ke numbers: integers, decimals, negative, etc.

**Kab use kare?**

- Age, price, quantity, distance, percentage - jahan bhi calculation ho

typescript

```typescript
let age: number = 25;              // Integer
let price: number = 99.99;         // Decimal
let temperature: number = -5;      // Negative
let distance: number = 3.14159;    // Pi value

// Alag-alag number formats
let decimal: number = 42;          // Normal
let hex: number = 0xFF;            // Hexadecimal (255)
let binary: number = 0b1010;       // Binary (10)
let octal: number = 0o744;         // Octal (484)
```

**Real Example:**

typescript

```typescript
// E-commerce product
let productPrice: number = 1500;
let discount: number = 10;         // 10% discount
let finalPrice: number = productPrice - (productPrice * discount / 100);
console.log(finalPrice);           // 1350
```

**Common Mistake:**

typescript

```typescript
let count: number = "10";  // ❌ Error!
// String hai, number nahi
```

---

### **Type 2: String** 📝

**Kya hai?**

- Text/words/sentences

**Kab use kare?**

- Names, messages, emails, descriptions - jahan bhi text ho

typescript

```typescript
let firstName: string = "Rahul";
let lastName: string = 'Kumar';              // Single quotes bhi chalenge
let email: string = `rahul@example.com`;     // Backticks bhi chalenge

// Template literals (backticks ka special use)
let fullName: string = `${firstName} ${lastName}`;
console.log(fullName);  // "Rahul Kumar"

let greeting: string = `Hello, my name is ${firstName} and I am ${age} years old`;
console.log(greeting);  // "Hello, my name is Rahul and I am 25 years old"
```

**Real Example:**

typescript

```typescript
// User registration
let username: string = "rahul_123";
let password: string = "mySecretPass";
let welcomeMessage: string = `Welcome ${username}! Your account is created.`;
```

**Common Mistake:**

typescript

```typescript
let name: string = 123;  // ❌ Error!
// Number hai, string nahi
```

---

### **Type 3: Boolean** ✅❌

**Kya hai?**

- Sirf **do values**: `true` ya `false`
- Yes/No questions ke liye

**Kab use kare?**

- Flags, conditions, status check - jahan sirf true/false ho

typescript

```typescript
let isLoggedIn: boolean = true;
let hasPermission: boolean = false;
let isAdult: boolean = age >= 18;   // Expression bhi ho sakta hai
```

**Real Example:**

typescript

```typescript
// E-commerce features
let isProductInStock: boolean = true;
let isUserPremium: boolean = false;
let canUserCheckout: boolean = isProductInStock && isLoggedIn;

if (canUserCheckout) {
    console.log("Proceed to payment");
} else {
    console.log("Cannot checkout");
}
```

**Common Mistake:**

typescript

```typescript
let isActive: boolean = "true";  // ❌ Error!
// String hai, boolean nahi

let isActive: boolean = 1;       // ❌ Error!
// Number hai (JavaScript mein 1 = true hota tha, par TypeScript mein nahi)
```

---

### **Type 4: Null aur Undefined**

**Kya hai?**

- **null**: Deliberately empty value (jaan bujh ke khali rakha)
- **undefined**: Value set hi nahi ki gayi

typescript

```typescript
let emptyValue: null = null;
let notSet: undefined = undefined;
```

**Difference samjho:**

typescript

```typescript
// Undefined - variable declare kiya par value nahi di
let userName: string;
console.log(userName);  // undefined (abhi value assign nahi hui)

// Null - deliberately empty
let userProfile: object | null = null;  // Abhi user logged in nahi hai
// Baad mein jab login ho to:
userProfile = { name: "Rahul", age: 25 };
```

**Real Example:**

typescript

```typescript
// Database se data fetch karna
function getUserById(id: number): User | null {
    // Agar user mile
    if (userFound) {
        return { id: 1, name: "Rahul" };
    }
    // Agar user na mile
    return null;  // Deliberately bata rahe hain ki kuch nahi mila
}

let user = getUserById(123);
if (user === null) {
    console.log("User not found");
} else {
    console.log(user.name);
}
```

---

### **Type 5: Any** ⚠️

**Kya hai?**

- **Kuch bhi** aa sakta hai
- TypeScript ki type checking **band** ho jati hai

**Kab use kare?**

- **Avoid karo jitna ho sake!**
- Sirf tab use karo jab:
  - Third-party library use kar rahe ho jiska type pata nahi
  - Legacy JavaScript code migrate kar rahe ho
  - Temporary fix chahiye

typescript

```typescript
let randomValue: any = 10;
randomValue = "hello";      // ✅ OK
randomValue = true;         // ✅ OK
randomValue = [1, 2, 3];   // ✅ OK
randomValue = { x: 10 };   // ✅ OK

// Any ke saath sab kuch kar sakte ho (dangerous!)
randomValue.toUpperCase();  // No error, par runtime pe crash ho sakta hai
randomValue.fly();          // No error, par method exist nahi karta
```

**Why Dangerous?**

typescript

```typescript
let data: any = "hello";
let length: number = data.length;  // OK - string.length
data = 123;
length = data.length;              // OK - but undefined at runtime!
```

**Better Alternative - Unknown:**

typescript

```typescript
let userInput: unknown = getUserInput();

// Direct use nahi kar sakte
// userInput.toUpperCase();  // ❌ Error!

// Pehle check karna padega
if (typeof userInput === "string") {
    userInput.toUpperCase();  // ✅ Ab safe hai
}
```

---

### **Type 6: Void**

**Kya hai?**

- **Kuch return nahi hota**
- Functions ke liye jo sirf kaam karte hain, value nahi dete

typescript

```typescript
function logMessage(message: string): void {
    console.log(message);
    // return kuch nahi
}

function saveToDatabase(data: any): void {
    // Database mein save kar do
    // Koi value return nahi karni
}
```

**Real Example:**

typescript

```typescript
// Button click handler
function handleClick(): void {
    console.log("Button clicked!");
    // Sirf action perform karo, return kuch nahi
}

// Form submit
function submitForm(formData: FormData): void {
    // API call karo
    // Success message dikha do
    // Return value ki zarurat nahi
}
```

**Difference: void vs undefined**

typescript

```typescript
// void - function return nahi kar raha
function doSomething(): void {
    console.log("Done");
}

// undefined - function explicitly undefined return kar raha
function doSomethingElse(): undefined {
    console.log("Done");
    return undefined;
}
```

---

### **Type 7: Never**

**Kya hai?**

- Function **kabhi return hi nahi hota**
- Ya to infinite loop hai
- Ya error throw karta hai

typescript

```typescript
// Error throw karne wala function
function throwError(message: string): never {
    throw new Error(message);
    // Yahan se aage code execute hi nahi hoga
}

// Infinite loop
function infiniteLoop(): never {
    while (true) {
        console.log("Running forever...");
    }
    // Kabhi end nahi hoga
}
```

**Real Example:**

typescript

```typescript
// API error handler
function handleFatalError(error: string): never {
    console.error("Fatal error:", error);
    throw new Error(error);
    // Application crash ho jayega
}

// Game loop
function gameLoop(): never {
    while (true) {
        updateGame();
        renderGame();
        // Infinite loop - tab tak chale jab tak game close na ho
    }
}
```

**Difference: void vs never**

typescript

```typescript
// void - Function execute hota hai aur khatam ho jata hai
function logger(): void {
    console.log("Log");
}  // Yahan function end

// never - Function kabhi end nahi hota
function crash(): never {
    throw new Error("Crash!");
}  // Yahan se code aage nahi jayega
```

---

## 2.5 Type Inference - TypeScript Smart Hai! 🧠

TypeScript **khud guess** kar sakta hai type:

typescript

```typescript
// Explicitly type declare kiya
let age: number = 25;

// Type declare nahi kiya, par TypeScript samajh gaya
let name = "Rahul";  // TypeScript: "Ye string hai"
let isActive = true; // TypeScript: "Ye boolean hai"
let price = 99.99;   // TypeScript: "Ye number hai"

// Hover karo VS Code mein, type automatically show hoga
```

**Kab automatic inference kaam karta hai?**

typescript

```typescript
// Variable initialization
let city = "Delhi";  // string inferred

// Function return
function getAge() {
    return 25;  // TypeScript: "Return type number hai"
}

// Arrays
let numbers = [1, 2, 3];  // number[] inferred
let names = ["Rahul", "Simran"];  // string[] inferred
```

**Best Practice:**

- Simple cases mein type likhna optional hai
- Complex cases mein **explicitly type likho** (clarity ke liye)

typescript

```typescript
// Simple - type mat likho
let count = 0;

// Complex - type likho
let userData: { name: string; age: number } = {
    name: "Rahul",
    age: 25
};
```

---

## 2.6 Type Annotation vs Type Inference

### **Type Annotation** (Tum batate ho)

typescript

```typescript
let age: number = 25;
//      ^^^^^^^ Tumne explicitly bataya
```

### **Type Inference** (TypeScript samajh jata hai)

typescript

```typescript
let age = 25;
//  TypeScript automatically samajh gaya: "number hai"
```

**Kab kya use kare?**

<table>
<tbody><tr><th colspan="1" rowspan="1"><p>Situation</p></th><th colspan="1" rowspan="1"><p>Use This</p></th></tr><tr><td colspan="1" rowspan="1"><p>Value assign kar rahe ho</p></td><td colspan="1" rowspan="1"><p>Type Inference (automatic)</p></td></tr><tr><td colspan="1" rowspan="1"><p>Value baad mein assign karoge</p></td><td colspan="1" rowspan="1"><p>Type Annotation (manual)</p></td></tr><tr><td colspan="1" rowspan="1"><p>Function parameters</p></td><td colspan="1" rowspan="1"><p>Type Annotation (zaroori hai)</p></td></tr><tr><td colspan="1" rowspan="1"><p>Complex types</p></td><td colspan="1" rowspan="1"><p>Type Annotation (clarity)</p></td></tr></tbody>
</table>

**Examples:**

typescript

```typescript
// ✅ Good - Inference
let name = "Rahul";
let age = 25;

// ✅ Good - Annotation (value baad mein)
let userId: number;
// ... 100 lines later ...
userId = 123;

// ✅ Good - Annotation (function parameter)
function greet(name: string) {
    console.log(`Hello ${name}`);
}

// ❌ Redundant - Dono jagah type
let price: number = 100;  // number already inferred hai
```

---


**Kya seekha?**

- ✅ Type kya hota hai (concept)
- ✅ Har basic type ka use
- ✅ Kab kaunsa type use kare
- ✅ Common mistakes
- ✅ Type Inference vs Annotation