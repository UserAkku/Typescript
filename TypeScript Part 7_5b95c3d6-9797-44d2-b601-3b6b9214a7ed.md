# **PART 7: Classes - Object-Oriented Programming 🏗️**

## 7.1 Classes Kya Hote Hain? (Concept Clear Karo)

### **Real Life Analogy**

Socho **car manufacturing factory**:

- **Blueprint/Design** → Ye batata hai car mein kya hoga
  - 4 wheels
  - Engine
  - Steering
  - Color
  - Methods: start(), stop(), accelerate()
- **Actual Cars** → Blueprint se banaye gaye real cars
  - Red Ferrari (blueprint se)
  - Blue BMW (same blueprint se)
  - Black Audi (same blueprint se)

**Programming mein:**

- **Class** = Blueprint/Template
- **Object/Instance** = Real entity jo class se banti hai

---

### **JavaScript Mein Classes (ES6)**

javascript

```javascript
// JavaScript class
class Car {
    constructor(brand, color) {
        this.brand = brand;
        this.color = color;
    }
    
    start() {
        console.log(`${this.brand} car is starting...`);
    }
}

let myCar = new Car("Toyota", "Red");
myCar.start();  // "Toyota car is starting..."
```

**JavaScript ki problem:**

- Properties ka type pata nahi
- Private/public ka concept nahi
- Mistakes easily ho sakti hain

---

### **TypeScript Class - Type Safety Ke Saath**

typescript

```typescript
class Car {
    // Properties with types
    brand: string;
    color: string;
    year: number;
    
    // Constructor
    constructor(brand: string, color: string, year: number) {
        this.brand = brand;
        this.color = color;
        this.year = year;
    }
    
    // Method
    start(): void {
        console.log(`${this.brand} car is starting...`);
    }
    
    getAge(): number {
        return new Date().getFullYear() - this.year;
    }
}

// Instance banao
let myCar = new Car("Toyota", "Red", 2020);
console.log(myCar.brand);    // "Toyota"
console.log(myCar.getAge()); // 5 (2025 - 2020)
myCar.start();               // "Toyota car is starting..."
```

**Benefits:**

- ✅ Properties ka type defined
- ✅ Methods ka return type clear
- ✅ Constructor parameters typed
- ✅ Autocomplete milega

---

## 7.2 Class Properties & Methods - Detail Mein

### **Properties Declaration**

typescript

```typescript
class Person {
    // Properties declare karo (types ke saath)
    name: string;
    age: number;
    email: string;
    isActive: boolean;
    
    constructor(name: string, age: number, email: string) {
        this.name = name;
        this.age = age;
        this.email = email;
        this.isActive = true;  // Default value
    }
}

let person = new Person("Rahul", 25, "rahul@example.com");
console.log(person.name);      // "Rahul"
console.log(person.isActive);  // true
```

**Important:**

- Properties **pehle declare** karne padte hain (class body mein)
- Constructor mein assign karte hain
- Types zaroori hain

---

### **Property Initialization - 3 Ways**

#### **Way 1: Constructor Mein Initialize**

typescript

```typescript
class User {
    name: string;
    age: number;
    
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}
```

#### **Way 2: Direct Assignment**

typescript

```typescript
class User {
    name: string = "Unknown";
    age: number = 0;
    role: string = "user";
    
    // Constructor optional ho gaya
}

let user = new User();
console.log(user.name);  // "Unknown"
```

#### **Way 3: Parameter Properties (Shorthand)**

typescript

```typescript
class User {
    // Constructor parameters mein hi declare + initialize
    constructor(
        public name: string,
        public age: number,
        public email: string
    ) {
        // Automatic assignment!
        // this.name = name (automatically)
        // this.age = age (automatically)
        // this.email = email (automatically)
    }
}

let user = new User("Rahul", 25, "rahul@example.com");
console.log(user.name);  // "Rahul"
```

**Shorthand ka magic:**

- `public name: string` likha constructor mein
- Automatically property ban gayi
- Automatically assign ho gayi
- Code concise aur clean

---

### **Methods - Functions Inside Class**

typescript

```typescript
class Calculator {
    add(a: number, b: number): number {
        return a + b;
    }
    
    subtract(a: number, b: number): number {
        return a - b;
    }
    
    multiply(a: number, b: number): number {
        return a * b;
    }
    
    divide(a: number, b: number): number {
        if (b === 0) {
            throw new Error("Cannot divide by zero");
        }
        return a / b;
    }
}

let calc = new Calculator();
console.log(calc.add(5, 3));       // 8
console.log(calc.multiply(4, 5));  // 20
```

---

### **Methods Using Class Properties**

typescript

```typescript
class BankAccount {
    accountNumber: string;
    balance: number;
    
    constructor(accountNumber: string, initialBalance: number) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }
    
    deposit(amount: number): void {
        this.balance += amount;
        console.log(`Deposited ₹${amount}. New balance: ₹${this.balance}`);
    }
    
    withdraw(amount: number): void {
        if (amount > this.balance) {
            console.log("Insufficient balance!");
            return;
        }
        this.balance -= amount;
        console.log(`Withdrawn ₹${amount}. New balance: ₹${this.balance}`);
    }
    
    getBalance(): number {
        return this.balance;
    }
}

let account = new BankAccount("ACC001", 1000);
account.deposit(500);    // "Deposited ₹500. New balance: ₹1500"
account.withdraw(300);   // "Withdrawn ₹300. New balance: ₹1200"
console.log(account.getBalance());  // 1200
```

---

## 7.3 Access Modifiers - Control Karo Kis Ko Kya Access Hai 🔐

### **Access Modifiers Kya Hain?**

**Real life example:**

- **Your room**
  - **Public**: Drawing room (koi bhi aa sakta hai)
  - **Private**: Bedroom (sirf tum access kar sakte ho)
  - **Protected**: Family room (tum aur tumhare relatives)

**TypeScript mein 3 access modifiers:**

1. **public** - Kahin se bhi access (default)
2. **private** - Sirf class ke andar access
3. **protected** - Class aur uski child classes mein access

---

### **Public - Default Access**

typescript

```typescript
class Person {
    public name: string;  // Public keyword optional hai
    age: number;          // By default public
    
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    
    public greet(): void {  // Public method
        console.log(`Hi, I'm ${this.name}`);
    }
}

let person = new Person("Rahul", 25);
console.log(person.name);  // ✅ Access from outside
person.greet();            // ✅ Access from outside
```

**Public = Sab jagah se access ho sakta hai**

---

### **Private - Sirf Class Ke Andar**

typescript

```typescript
class BankAccount {
    public accountNumber: string;
    private balance: number;        // Private!
    private accountPin: string;     // Private!
    
    constructor(accountNumber: string, initialBalance: number, pin: string) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
        this.accountPin = pin;
    }
    
    // Private method
    private validatePin(pin: string): boolean {
        return this.accountPin === pin;
    }
    
    // Public method jo private data access karta hai
    public withdraw(amount: number, pin: string): void {
        if (!this.validatePin(pin)) {
            console.log("Wrong PIN!");
            return;
        }
        
        if (amount > this.balance) {
            console.log("Insufficient balance!");
            return;
        }
        
        this.balance -= amount;
        console.log(`Withdrawn ₹${amount}`);
    }
    
    public getBalance(pin: string): number | null {
        if (!this.validatePin(pin)) {
            console.log("Wrong PIN!");
            return null;
        }
        return this.balance;
    }
}

let account = new BankAccount("ACC001", 5000, "1234");

// ✅ Public properties/methods - accessible
console.log(account.accountNumber);  // "ACC001"
account.withdraw(1000, "1234");

// ❌ Private properties/methods - NOT accessible
console.log(account.balance);        // Error: Property 'balance' is private
console.log(account.accountPin);     // Error: Property 'accountPin' is private
account.validatePin("1234");         // Error: Method 'validatePin' is private
```

**Private ka use kyun?**

- Sensitive data protect karo (password, balance)
- Internal implementation hide karo
- Direct access se mistakes avoid karo

---

### **Protected - Class + Child Classes**

typescript

```typescript
class Animal {
    protected name: string;  // Protected
    
    constructor(name: string) {
        this.name = name;
    }
    
    protected makeSound(): void {  // Protected method
        console.log("Some sound...");
    }
}

class Dog extends Animal {
    constructor(name: string) {
        super(name);
    }
    
    bark(): void {
        // ✅ Access protected properties/methods from parent
        console.log(`${this.name} says: Woof!`);
        this.makeSound();
    }
}

let dog = new Dog("Tommy");
dog.bark();  // "Tommy says: Woof!"

// ❌ Protected members NOT accessible from outside
console.log(dog.name);      // Error: Property 'name' is protected
dog.makeSound();            // Error: Method 'makeSound' is protected
```

**Protected ka use kyun?**

- Parent class ke members ko child classes mein accessible banana
- Bahar se access prevent karna

---

### **Comparison Table**

<table>
<tbody><tr><th colspan="1" rowspan="1"><p>Modifier</p></th><th colspan="1" rowspan="1"><p>Class Ke Andar</p></th><th colspan="1" rowspan="1"><p>Child Class</p></th><th colspan="1" rowspan="1"><p>Bahar Se</p></th></tr><tr><td colspan="1" rowspan="1"><p><strong>public</strong></p></td><td colspan="1" rowspan="1"><p>✅ Yes</p></td><td colspan="1" rowspan="1"><p>✅ Yes</p></td><td colspan="1" rowspan="1"><p>✅ Yes</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>protected</strong></p></td><td colspan="1" rowspan="1"><p>✅ Yes</p></td><td colspan="1" rowspan="1"><p>✅ Yes</p></td><td colspan="1" rowspan="1"><p>❌ No</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>private</strong></p></td><td colspan="1" rowspan="1"><p>✅ Yes</p></td><td colspan="1" rowspan="1"><p>❌ No</p></td><td colspan="1" rowspan="1"><p>❌ No</p></td></tr></tbody>
</table>

---

### **Real Example: User System**

typescript

```typescript
class User {
    public username: string;
    private password: string;
    protected role: string;
    
    constructor(username: string, password: string, role: string = "user") {
        this.username = username;
        this.password = this.hashPassword(password);
        this.role = role;
    }
    
    // Private helper method
    private hashPassword(password: string): string {
        // Simulate hashing
        return `hashed_${password}`;
    }
    
    // Public method to verify password
    public verifyPassword(password: string): boolean {
        return this.hashPassword(password) === this.password;
    }
    
    // Protected method for child classes
    protected getRoleInfo(): string {
        return `Role: ${this.role}`;
    }
}

class Admin extends User {
    constructor(username: string, password: string) {
        super(username, password, "admin");
    }
    
    public showInfo(): void {
        console.log(`Admin: ${this.username}`);
        console.log(this.getRoleInfo());  // ✅ Protected accessible
    }
}

let user = new User("rahul", "secret123");
console.log(user.username);              // ✅ "rahul"
console.log(user.verifyPassword("secret123"));  // ✅ true

// console.log(user.password);           // ❌ Error: private
// console.log(user.role);               // ❌ Error: protected

let admin = new Admin("admin", "admin123");
admin.showInfo();
// "Admin: admin"
// "Role: admin"
```

---

## 7.4 Readonly Properties - Sirf Ek Baar Set Karo 🔒

### **Readonly Kya Hai?**

**Real life:**

- **Aadhaar number** - Ek baar mila, badal nahi sakta
- **Date of birth** - Ek baar fixed, change nahi hota
- **Product serial number** - Manufacture ke time fixed

typescript

```typescript
class Person {
    readonly id: number;           // Readonly
    readonly dateOfBirth: Date;    // Readonly
    name: string;                  // Normal (changeable)
    
    constructor(id: number, dob: Date, name: string) {
        this.id = id;
        this.dateOfBirth = dob;
        this.name = name;
    }
    
    updateName(newName: string): void {
        this.name = newName;  // ✅ OK - not readonly
    }
    
    updateId(newId: number): void {
        this.id = newId;  // ❌ Error: Cannot assign to 'id' because it is readonly
    }
}

let person = new Person(1, new Date("2000-01-15"), "Rahul");

// ✅ Read kar sakte ho
console.log(person.id);           // 1
console.log(person.dateOfBirth);  // Date object

// ✅ Normal property change kar sakte ho
person.name = "Rahul Kumar";

// ❌ Readonly property change nahi kar sakte
person.id = 2;              // Error!
person.dateOfBirth = new Date();  // Error!
```

**Rules:**

- Readonly properties ko **sirf constructor mein** assign kar sakte ho
- Constructor ke baad change nahi kar sakte

---

### **Readonly vs Const**

typescript

```typescript
// const - variables ke liye
const MAX_SIZE = 100;

// readonly - class properties ke liye
class Config {
    readonly MAX_SIZE = 100;
}
```

---

### **Real Example: Database Record**

typescript

```typescript
class DatabaseRecord {
    readonly id: string;
    readonly createdAt: Date;
    readonly createdBy: string;
    
    // Changeable fields
    public title: string;
    public content: string;
    public updatedAt: Date;
    
    constructor(id: string, createdBy: string, title: string, content: string) {
        this.id = id;
        this.createdAt = new Date();
        this.createdBy = createdBy;
        this.title = title;
        this.content = content;
        this.updatedAt = new Date();
    }
    
    update(title: string, content: string): void {
        this.title = title;
        this.content = content;
        this.updatedAt = new Date();
    }
}

let record = new DatabaseRecord("REC001", "admin", "Post Title", "Content here");

// ✅ Update allowed
record.update("New Title", "New Content");

// ❌ Readonly fields cannot be changed
record.id = "REC002";              // Error!
record.createdAt = new Date();     // Error!
record.createdBy = "user";         // Error!
```

---

## 7.5 Getters and Setters - Controlled Access 🎛️

### **Getter/Setter Kya Hai?**

**Real life example:**

- **Temperature thermostat**
  - Getter: Current temperature padho
  - Setter: Temperature set karo (validation ke saath - 16°C se 30°C)

**Without getter/setter:**

typescript

```typescript
class Temperature {
    celsius: number = 0;
}

let temp = new Temperature();
temp.celsius = -300;  // ❌ Invalid! But no validation
```

**With getter/setter:**

typescript

```typescript
class Temperature {
    private _celsius: number = 0;  // Private backing field
    
    // Getter
    get celsius(): number {
        return this._celsius;
    }
    
    // Setter with validation
    set celsius(value: number) {
        if (value < -273.15) {
            throw new Error("Temperature cannot be below absolute zero!");
        }
        this._celsius = value;
    }
}

let temp = new Temperature();

// Setter call (looks like property assignment)
temp.celsius = 25;  // ✅ Valid
console.log(temp.celsius);  // Getter call: 25

temp.celsius = -300;  // ❌ Error: Temperature cannot be below absolute zero!
```

**Syntax:**

typescript

```typescript
get propertyName(): type {
    return value;
}

set propertyName(value: type) {
    // validation
    // assignment
}
```

---

### **Getter/Setter Examples**

#### **Example 1: Age Validation**

typescript

```typescript
class Person {
    private _age: number = 0;
    
    get age(): number {
        return this._age;
    }
    
    set age(value: number) {
        if (value < 0) {
            throw new Error("Age cannot be negative!");
        }
        if (value > 150) {
            throw new Error("Age seems invalid!");
        }
        this._age = value;
    }
}

let person = new Person();
person.age = 25;           // ✅ Valid
console.log(person.age);   // 25

person.age = -5;           // ❌ Error: Age cannot be negative!
person.age = 200;          // ❌ Error: Age seems invalid!
```

---

#### **Example 2: Computed Properties**

typescript

```typescript
class Rectangle {
    constructor(
        private _width: number,
        private _height: number
    ) {}
    
    get width(): number {
        return this._width;
    }
    
    set width(value: number) {
        if (value <= 0) {
            throw new Error("Width must be positive");
        }
        this._width = value;
    }
    
    get height(): number {
        return this._height;
    }
    
    set height(value: number) {
        if (value <= 0) {
            throw new Error("Height must be positive");
        }
        this._height = value;
    }
    
    // Computed property (no setter - read-only)
    get area(): number {
        return this._width * this._height;
    }
    
    get perimeter(): number {
        return 2 * (this._width + this._height);
    }
}

let rect = new Rectangle(10, 20);
console.log(rect.area);       // 200 (computed)
console.log(rect.perimeter);  // 60 (computed)

rect.width = 15;
console.log(rect.area);       // 300 (automatically updated)

// rect.area = 500;  // ❌ Error: Cannot set read-only property
```

---

#### **Example 3: Format Conversion**

typescript

```typescript
class User {
    private _email: string = "";
    
    get email(): string {
        return this._email;
    }
    
    set email(value: string) {
        // Validation
        if (!value.includes("@")) {
            throw new Error("Invalid email format");
        }
        // Normalize (lowercase)
        this._email = value.toLowerCase();
    }
}

let user = new User();
user.email = "Rahul@Example.COM";  // Set with uppercase
console.log(user.email);            // "rahul@example.com" (stored lowercase)

user.email = "invalidemail";        // ❌ Error: Invalid email format
```

---

### **Read-only Getter (No Setter)**

typescript

```typescript
class Circle {
    constructor(private _radius: number) {}
    
    get radius(): number {
        return this._radius;
    }
    
    // No setter - radius can't be changed after creation
    
    get area(): number {
        return Math.PI * this._radius ** 2;
    }
    
    get circumference(): number {
        return 2 * Math.PI * this._radius;
    }
}

let circle = new Circle(5);
console.log(circle.radius);         // 5
console.log(circle.area);           // 78.54
console.log(circle.circumference);  // 31.42

// circle.radius = 10;  // ❌ Error: Cannot set read-only property
// circle.area = 100;   // ❌ Error: Cannot set read-only property
```

---

## 7.6 Static Members - Class-Level Properties/Methods ⚡

### **Static Kya Hai?**

**Real life example:**

- **Math class**
  - `Math.PI` - Har jagah same value
  - `Math.max()` - Har jagah same function
  - Instance banane ki zarurat nahi

**Normal vs Static:**

typescript

```typescript
// Normal - Instance ke liye
class Counter {
    count: number = 0;
    
    increment(): void {
        this.count++;
    }
}

let c1 = new Counter();
let c2 = new Counter();
c1.increment();  // c1.count = 1
c2.increment();  // c2.count = 1 (separate)

// Static - Class ke liye
class GlobalCounter {
    static count: number = 0;
    
    static increment(): void {
        GlobalCounter.count++;
    }
}

GlobalCounter.increment();  // GlobalCounter.count = 1
GlobalCounter.increment();  // GlobalCounter.count = 2
console.log(GlobalCounter.count);  // 2

// Instance nahi banana pada!
```

---

### **Static Properties**

typescript

```typescript
class MathUtils {
    static PI: number = 3.14159;
    static E: number = 2.71828;
    
    static calculateCircleArea(radius: number): number {
        return this.PI * radius * radius;
    }
}

// Class name se access karo
console.log(MathUtils.PI);                    // 3.14159
console.log(MathUtils.calculateCircleArea(5)); // 78.53975

// Instance nahi bana sakte useful functions ke liye
// let utils = new MathUtils();  // Zarurat nahi
```

---

### **Static Methods**

typescript

```typescript
class StringUtils {
    static reverse(str: string): string {
        return str.split('').reverse().join('');
    }
    
    static capitalize(str: string): string {
        return str.charAt(0).toUpperCase() + str.slice(1);
    }
    
    static truncate(str: string, maxLength: number): string {
        if (str.length <= maxLength) {
            return str;
        }
        return str.substring(0, maxLength) + '...';
    }
}

// Direct class se call karo
console.log(StringUtils.reverse("hello"));           // "olleh"
console.log(StringUtils.capitalize("typescript"));   // "Typescript"
console.log(StringUtils.truncate("Long text here", 8));  // "Long tex..."
```

---

### **Real Example: Database Connection (Singleton Pattern)**

typescript

```typescript
class Database {
    private static instance: Database;
    private connectionString: string;
    
    // Private constructor - bahar se new nahi kar sakte
    private constructor() {
        this.connectionString = "mongodb://localhost:27017";
        console.log("Database connection created");
    }
    
    // Static method to get instance
    static getInstance(): Database {
        if (!Database.instance) {
            Database.instance = new Database();
        }
        return Database.instance;
    }
    
    query(sql: string): void {
        console.log(`Executing: ${sql}`);
    }
}

// let db = new Database();  // ❌ Error: Constructor is private

// Correct way
let db1 = Database.getInstance();  // "Database connection created"
let db2 = Database.getInstance();  // No new message (same instance)

console.log(db1 === db2);  // true (same instance)

db1.query("SELECT * FROM users");
```

---

### **Static vs Instance - Kab Kya?**

<table>
<tbody><tr><th colspan="1" rowspan="1"><p>Use Case</p></th><th colspan="1" rowspan="1"><p>Use This</p></th><th colspan="1" rowspan="1"><p>Example</p></th></tr><tr><td colspan="1" rowspan="1"><p>Utility functions</p></td><td colspan="1" rowspan="1"><p>Static</p></td><td colspan="1" rowspan="1"><p><code class="hljs" spellcheck="false">Math.max()</code>, <code class="hljs" spellcheck="false">Array.isArray()</code></p></td></tr><tr><td colspan="1" rowspan="1"><p>Constants</p></td><td colspan="1" rowspan="1"><p>Static</p></td><td colspan="1" rowspan="1"><p><code class="hljs" spellcheck="false">Math.PI</code>, <code class="hljs" spellcheck="false">Number.MAX_VALUE</code></p></td></tr><tr><td colspan="1" rowspan="1"><p>Factory methods</p></td><td colspan="1" rowspan="1"><p>Static</p></td><td colspan="1" rowspan="1"><p><code class="hljs" spellcheck="false">Date.now()</code>, <code class="hljs" spellcheck="false">Object.create()</code></p></td></tr><tr><td colspan="1" rowspan="1"><p>Singleton pattern</p></td><td colspan="1" rowspan="1"><p>Static</p></td><td colspan="1" rowspan="1"><p>Database connection</p></td></tr><tr><td colspan="1" rowspan="1"><p>Instance-specific data</p></td><td colspan="1" rowspan="1"><p>Instance</p></td><td colspan="1" rowspan="1"><p><code class="hljs" spellcheck="false">user.name</code>, <code class="hljs" spellcheck="false">car.speed</code></p></td></tr><tr><td colspan="1" rowspan="1"><p>Instance behavior</p></td><td colspan="1" rowspan="1"><p>Instance</p></td><td colspan="1" rowspan="1"><p><code class="hljs" spellcheck="false">user.login()</code>, <code class="hljs" spellcheck="false">car.drive()</code></p></td></tr></tbody>
</table>

---

## 7.7 Inheritance - Class Se Class Banao 🧬

### **Inheritance Kya Hai?**

**Real life:**

- **Animal** (parent)
  - Name
  - Age
  - makeSound()
- **Dog** (child) extends Animal
  - Inherits: name, age, makeSound()
  - Adds: breed, bark()
- **Cat** (child) extends Animal
  - Inherits: name, age, makeSound()
  - Adds: color, meow()

**Code mein same concept:**

typescript

```typescript
// Parent class (Base class / Superclass)
class Animal {
    name: string;
    age: number;
    
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    
    makeSound(): void {
        console.log("Some generic animal sound");
    }
    
    move(distance: number): void {
        console.log(`${this.name} moved ${distance} meters`);
    }
}

// Child class (Derived class / Subclass)
class Dog extends Animal {
    breed: string;
    
    constructor(name: string, age: number, breed: string) {
        super(name, age);  // Parent constructor call (ZAROORI!)
        this.breed = breed;
    }
    
    // Method override
    makeSound(): void {
        console.log("Woof! Woof!");
    }
    
    // New method
    fetch(): void {
        console.log(`${this.name} is fetching the ball`);
    }
}

// Another child
class Cat extends Animal {
    color: string;
    
    constructor(name: string, age: number, color: string) {
        super(name, age);
        this.color = color;
    }
    
    makeSound(): void {
        console.log("Meow!");
    }
    
    climb(): void {
        console.log(`${this.name} is climbing`);
    }
}

// Usage
let dog = new Dog("Tommy", 3, "Labrador");
console.log(dog.name);     // "Tommy" (inherited)
console.log(dog.breed);    // "Labrador" (own)
dog.makeSound();           // "Woof! Woof!" (overridden)
dog.move(10);              // "Tommy moved 10 meters" (inherited)
dog.fetch();               // "Tommy is fetching the ball" (own)

let cat = new Cat("Whiskers", 2, "White");
cat.makeSound();           // "Meow!" (overridden)
cat.climb();               // "Whiskers is climbing"
```

---

### **super Keyword - Parent Ko Access Karo**

#### **Use 1: Parent Constructor Call**

typescript

```typescript
class Person {
    constructor(public name: string, public age: number) {
        console.log("Person constructor called");
    }
}

class Student extends Person {
    constructor(name: string, age: number, public grade: string) {
        super(name, age);  // Parent constructor PEHLE call karo
        console.log("Student constructor called");
    }
}

let student = new Student("Rahul", 20, "A");
// Output:
// "Person constructor called"
// "Student constructor called"
```

**Important:** Child constructor mein `super()` call karna **zaroori** hai (agar parent ka constructor hai)

---

#### **Use 2: Parent Method Call**

typescript

```typescript
class Animal {
    makeSound(): void {
        console.log("Generic animal sound");
    }
}

class Dog extends Animal {
    makeSound(): void {
        super.makeSound();  // Parent ka method call
        console.log("Plus: Woof!");
    }
}

let dog = new Dog();
dog.makeSound();
// Output:
// "Generic animal sound"
// "Plus: Woof!"
```

---

### **Method Overriding - Same Name, Different Implementation**

typescript

```typescript
class Shape {
    getArea(): number {
        return 0;  // Default
    }
    
    getInfo(): string {
        return `Area: ${this.getArea()}`;
    }
}

class Circle extends Shape {
    constructor(private radius: number) {
        super();
    }
    
    // Override
    getArea(): number {
        return Math.PI * this.radius ** 2;
    }
}

class Rectangle extends Shape {
    constructor(private width: number, private height: number) {
        super();
    }
    
    // Override
    getArea(): number {
        return this.width * this.height;
    }
}

let circle = new Circle(5);
console.log(circle.getArea());    // 78.54 (Circle implementation)
console.log(circle.getInfo());    // "Area: 78.54" (uses overridden getArea)

let rect = new Rectangle(10, 20);
console.log(rect.getArea());      // 200 (Rectangle implementation)
```

---

### **Protected Members in Inheritance**

typescript

```typescript
class BankAccount {
    protected balance: number;  // Child classes can access
    
    constructor(initialBalance: number) {
        this.balance = initialBalance;
    }
    
    public getBalance(): number {
        return this.balance;
    }
}

class SavingsAccount extends BankAccount {
    private interestRate: number;
    
    constructor(initialBalance: number, interestRate: number) {
        super(initialBalance);
        this.interestRate = interestRate;
    }
    
    addInterest(): void {
        // ✅ Can access protected balance
        const interest = this.balance * this.interestRate;
        this.balance += interest;
        console.log(`Interest added: ₹${interest}`);
    }
}

let savings = new SavingsAccount(10000, 0.05);
savings.addInterest();  // "Interest added: ₹500"
console.log(savings.getBalance());  // 10500

// console.log(savings.balance);  // ❌ Error: protected
```

---

**Kya seekha?**

- ✅ Classes basics
- ✅ Properties & methods
- ✅ Access modifiers (public, private, protected)
- ✅ Readonly
- ✅ Getters/Setters
- ✅ Static members
- ✅ Inheritance
- ✅ super keyword
- ✅ Method overriding