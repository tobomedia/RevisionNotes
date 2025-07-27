# TypeScript Cheatsheet

*A comprehensive reference for TypeScript's most commonly used features*

📚 **Official Documentation:** [TypeScript Handbook](https://www.typescriptlang.org/docs/)

## Table of Contents

### 🚀 Getting Started
- [Basic Types](#basic-types)
- [Interfaces](#interfaces)
- [Type Aliases](#type-aliases)

### 🎯 Core Language Features
- [Functions](#functions)
- [Generics](#generics)
- [Classes](#classes)

### 🔧 Advanced Type System
- [Advanced Types](#advanced-types)
- [Type Guards](#type-guards)

### 📦 Module System
- [Modules](#modules)
- [Declaration Files](#declaration-files)

### 🛠️ Advanced Features
- [Decorators](#decorators)
- [Common Patterns](#common-patterns)

### ⚙️ Configuration & Setup
- [Configuration (tsconfig.json)](#configuration-tsconfigjson)

### 📚 Resources
- [Quick Reference Links](#quick-reference-links)

---

## Basic Types

📖 [Basic Types Documentation](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

```typescript
// Primitives
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;
let data: any = { foo: "bar" };
let nothing: void = undefined;
let empty: null = null;
let notDefined: undefined = undefined;

// Arrays
let numbers: number[] = [1, 2, 3];
let strings: Array<string> = ["a", "b", "c"];

// Tuples
let tuple: [string, number] = ["hello", 42];

// Enums
enum Color {
  Red,
  Green,
  Blue
}
let color: Color = Color.Red;

// String literal types
type Direction = "up" | "down" | "left" | "right";
let move: Direction = "up";
```

## Interfaces

📖 [Interfaces Documentation](https://www.typescriptlang.org/docs/handbook/2/objects.html)

```typescript
// Basic interface
interface User {
  id: number;
  name: string;
  email?: string; // Optional property
  readonly createdAt: Date; // Read-only property
}

// Extending interfaces
interface Admin extends User {
  permissions: string[];
}

// Function in interface
interface Calculator {
  add(a: number, b: number): number;
  subtract: (a: number, b: number) => number;
}

// Index signatures
interface StringDictionary {
  [key: string]: string;
}

// Generic interfaces
interface Repository<T> {
  find(id: number): T | null;
  save(entity: T): void;
}
```

## Type Aliases

📖 [Type Aliases Documentation](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases)

```typescript
// Basic type alias
type ID = number | string;

// Object type
type Point = {
  x: number;
  y: number;
};

// Function type
type Handler = (event: Event) => void;

// Union types
type Status = "loading" | "success" | "error";

// Intersection types
type Employee = Person & {
  employeeId: number;
  department: string;
};
```

## Functions

📖 [Functions Documentation](https://www.typescriptlang.org/docs/handbook/2/functions.html)

```typescript
// Function declarations
function greet(name: string): string {
  return `Hello, ${name}!`;
}

// Arrow functions
const add = (a: number, b: number): number => a + b;

// Optional parameters
function log(message: string, level?: string): void {
  console.log(`[${level || "INFO"}] ${message}`);
}

// Default parameters
function createUser(name: string, age: number = 18): User {
  return { name, age };
}

// Rest parameters
function sum(...numbers: number[]): number {
  return numbers.reduce((total, num) => total + num, 0);
}

// Function overloads
function process(data: string): string;
function process(data: number): number;
function process(data: string | number): string | number {
  return typeof data === "string" ? data.toUpperCase() : data * 2;
}
```

## Generics

📖 [Generics Documentation](https://www.typescriptlang.org/docs/handbook/2/generics.html)

```typescript
// Generic functions
function identity<T>(arg: T): T {
  return arg;
}

// Generic with constraints
interface Lengthwise {
  length: number;
}

function logLength<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}

// Generic classes
class Container<T> {
  private value: T;
  
  constructor(value: T) {
    this.value = value;
  }
  
  getValue(): T {
    return this.value;
  }
}

// Multiple type parameters
function merge<T, U>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}

// Generic type aliases
type ApiResponse<T> = {
  data: T;
  status: number;
  message: string;
};
```

## Classes

📖 [Classes Documentation](https://www.typescriptlang.org/docs/handbook/2/classes.html)

```typescript
// Basic class
class Animal {
  private name: string;
  protected species: string;
  public age: number;
  
  constructor(name: string, species: string, age: number) {
    this.name = name;
    this.species = species;
    this.age = age;
  }
  
  // Method
  speak(): void {
    console.log(`${this.name} makes a sound`);
  }
  
  // Getter
  get getName(): string {
    return this.name;
  }
  
  // Setter
  set setAge(age: number) {
    if (age > 0) this.age = age;
  }
  
  // Static method
  static createPet(name: string): Animal {
    return new Animal(name, "domestic", 1);
  }
}

// Inheritance
class Dog extends Animal {
  constructor(name: string, age: number) {
    super(name, "canine", age);
  }
  
  speak(): void {
    console.log(`${this.getName} barks`);
  }
}

// Abstract class
abstract class Shape {
  abstract area(): number;
  
  displayArea(): void {
    console.log(`Area: ${this.area()}`);
  }
}

// Implementing interfaces
class Rectangle implements Shape {
  constructor(private width: number, private height: number) {}
  
  area(): number {
    return this.width * this.height;
  }
}
```

## Advanced Types

📖 [Advanced Types Documentation](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)  
📖 [Utility Types Documentation](https://www.typescriptlang.org/docs/handbook/utility-types.html)  
📖 [Template Literal Types](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html)

```typescript
// Conditional types
type NonNullable<T> = T extends null | undefined ? never : T;

// Mapped types
type Partial<T> = {
  [P in keyof T]?: T[P];
};

type Required<T> = {
  [P in keyof T]-?: T[P];
};

// Utility types
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}

type UserUpdate = Partial<User>; // All properties optional
type PublicUser = Omit<User, 'password'>; // Exclude password
type UserCredentials = Pick<User, 'email' | 'password'>; // Only email and password
type UserRecord = Record<string, User>; // Dictionary of users

// Template literal types
type EventName<T extends string> = `on${Capitalize<T>}`;
type ClickEvent = EventName<"click">; // "onClick"

// keyof operator
type UserKeys = keyof User; // "id" | "name" | "email" | "password"

// typeof operator
const config = {
  apiUrl: "https://api.example.com",
  timeout: 5000
};
type Config = typeof config;
```

## Type Guards

📖 [Narrowing Documentation](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)  
📖 [Discriminated Unions](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#discriminated-unions)

```typescript
// typeof type guard
function padLeft(value: string, padding: string | number) {
  if (typeof padding === "number") {
    return Array(padding + 1).join(" ") + value;
  }
  return padding + value;
}

// instanceof type guard
class Bird {
  fly() { console.log("Flying"); }
}

class Fish {
  swim() { console.log("Swimming"); }
}

function move(animal: Bird | Fish) {
  if (animal instanceof Bird) {
    animal.fly(); // TypeScript knows it's a Bird
  } else {
    animal.swim(); // TypeScript knows it's a Fish
  }
}

// Custom type guard
interface Cat {
  meow(): void;
}

interface Dog {
  bark(): void;
}

function isCat(animal: Cat | Dog): animal is Cat {
  return 'meow' in animal;
}

function makeSound(animal: Cat | Dog) {
  if (isCat(animal)) {
    animal.meow(); // TypeScript knows it's a Cat
  } else {
    animal.bark(); // TypeScript knows it's a Dog
  }
}

// Discriminated unions
interface Square {
  kind: "square";
  size: number;
}

interface Rectangle {
  kind: "rectangle";
  width: number;
  height: number;
}

type Shape = Square | Rectangle;

function area(shape: Shape): number {
  switch (shape.kind) {
    case "square":
      return shape.size * shape.size;
    case "rectangle":
      return shape.width * shape.height;
  }
}
```

## Modules

📖 [Modules Documentation](https://www.typescriptlang.org/docs/handbook/2/modules.html)

```typescript
// Named exports
export const PI = 3.14159;
export function calculateArea(radius: number): number {
  return PI * radius * radius;
}
export class Circle {
  constructor(public radius: number) {}
}

// Default export
export default class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }
}

// Import
import Calculator, { PI, calculateArea } from './math';
import * as MathUtils from './math';

// Re-exports
export { PI, calculateArea } from './math';
export * from './shapes';
```

## Decorators

📖 [Decorators Documentation](https://www.typescriptlang.org/docs/handbook/decorators.html)

> **Note:** Decorators are still experimental. Enable with `"experimentalDecorators": true` in tsconfig.json

```typescript
// Class decorator
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
}

// Method decorator
function enumerable(value: boolean) {
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    descriptor.enumerable = value;
  };
}

class Person {
  @enumerable(false)
  getName() {
    return "John";
  }
}

// Property decorator
function format(formatString: string) {
  return function (target: any, propertyKey: string): any {
    let value = target[propertyKey];
    
    const getter = () => `${formatString} ${value}`;
    const setter = (newVal: string) => { value = newVal; };
    
    return {
      get: getter,
      set: setter,
      enumerable: true,
      configurable: true
    };
  };
}

class Greeter {
  @format("Hello")
  greeting: string = "world";
}
```

## Declaration Files

📖 [Declaration Files Documentation](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html)  
📖 [Global Augmentation](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#global-augmentation)

```typescript
// Ambient declarations
declare var myGlobalVar: string;
declare function myGlobalFunction(param: string): void;

// Module declarations
declare module "my-library" {
  export function doSomething(): void;
  export default class MyClass {
    constructor(param: string);
  }
}

// Global augmentation
declare global {
  interface Window {
    myCustomProperty: string;
  }
}

// Namespace
declare namespace MyNamespace {
  interface Config {
    apiUrl: string;
  }
  
  function initialize(config: Config): void;
}
```

## Common Patterns

💡 These are practical TypeScript implementations of common design patterns

```typescript
// Singleton pattern
class Database {
  private static instance: Database;
  private constructor() {}
  
  static getInstance(): Database {
    if (!Database.instance) {
      Database.instance = new Database();
    }
    return Database.instance;
  }
}

// Factory pattern
interface Product {
  name: string;
  price: number;
}

class ProductFactory {
  static create(type: string): Product {
    switch (type) {
      case "book":
        return { name: "Book", price: 10 };
      case "pen":
        return { name: "Pen", price: 2 };
      default:
        throw new Error("Unknown product type");
    }
  }
}

// Observer pattern
interface Observer<T> {
  update(data: T): void;
}

class Subject<T> {
  private observers: Observer<T>[] = [];
  
  subscribe(observer: Observer<T>): void {
    this.observers.push(observer);
  }
  
  notify(data: T): void {
    this.observers.forEach(observer => observer.update(data));
  }
}

// Builder pattern
class UserBuilder {
  private user: Partial<User> = {};
  
  setName(name: string): UserBuilder {
    this.user.name = name;
    return this;
  }
  
  setEmail(email: string): UserBuilder {
    this.user.email = email;
    return this;
  }
  
  build(): User {
    return this.user as User;
  }
}

const user = new UserBuilder()
  .setName("John")
  .setEmail("john@example.com")
  .build();
```

## Configuration (tsconfig.json)

📖 [TSConfig Reference](https://www.typescriptlang.org/tsconfig)  
📖 [Compiler Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "lib": ["ES2020", "DOM"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "removeComments": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "moduleResolution": "node",
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"]
    },
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

## Quick Reference Links

### Essential Resources
- 📚 [TypeScript Handbook](https://www.typescriptlang.org/docs/) - Complete documentation
- 🎮 [TypeScript Playground](https://www.typescriptlang.org/play) - Try TypeScript online
- 📝 [TSConfig Reference](https://www.typescriptlang.org/tsconfig) - Configuration options
- 🔧 [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) - Community type definitions

### Learning Resources
- 🎓 [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/) - Free online book
- 📖 [TypeScript Exercises](https://typescript-exercises.github.io/) - Practice problems
- 🧪 [Type Challenges](https://github.com/type-challenges/type-challenges) - Advanced type system challenges

### Tools & IDE Support
- 🔧 [TypeScript ESLint](https://typescript-eslint.io/) - Linting rules
- 🎨 [Prettier](https://prettier.io/) - Code formatting
- 💻 [VS Code TypeScript](https://code.visualstudio.com/docs/languages/typescript) - IDE support

---

*This cheatsheet covers TypeScript 4.9+ features. For the latest updates, always refer to the [official documentation](https://www.typescriptlang.org/docs/).*