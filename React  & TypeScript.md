# React  & TypeScript

## TypeScript Basics & Core concepts

Installing the typescript compiler globally:

````bash
npm i -g typescript

// then run the compiler
tsc

// or run it on demand without installing it
npx tsc

````

VSCode has typescript support, so we'll see error even before running the compiler

First example:
````ts
let animal = 'dog' // a 'string' type was infered here.  Just hover over it and we'll see 'let animal: string'

animal = 1; // ❌ Type 'number' is not assignable to type 'string'
````

What if I don't want to initialise the variable with a value? TS fallbacks to 'any' as the type

```ts
let animal; // no inferrance here. Just hover over it and we'll see 'let animal: any'

animal = 1; // no error here
```

````ts
let animal  // no inferrance here. Just hover over it and we'll see 'let animal: any'

animal = 1; // no error
animal = 'dog' // no error
````

What if I want a type and no value assigned to the variable?

```ts
let animal: string // this is and 'explicit type assignment'

animal = 1; // ❌ Type 'number' is not assignable to type 'string'
```

Avoid `explicit type assignment` when inference works:

````ts
let animal: string = 'dog' //❌dont do that! TS is smart enough in this case
````



### Basic primitive types

string, number and boolean



### Union types

a variable can have more than one type, so we need to write a union to allow ts to compile

````ts
// just declaring a variable 
let user: string | number;

user = 'dog'

user = 123
````

```ts
// or assigning a value
let user string | number = 'cagghrt'
```

### Working with object types

````ts
// all the object fields are required
type User = {
  name: string; // a coma also works instead of semicolon
  age: number;
  isAdmin: boolean;
  id: string | number
}

let user: User;

user = {
  name: 'Esteban',
  age: 34,
  isAdmin: true,
  id: 123 
}
````

### Array types

````ts
type Hobbies = string[] // or Array<string>

let hobbies: Hobbies;

hobbies = ['fishing', 'coding']
````

### Types in  functions

if the function doesn't return anything, a special TS type `void` is inferred.

````
const add = (a: number, b:number)=> { // void is inferred as the returned type
  const number = a + b; // infered as number by TS, really smart
  console.log(number)
}
````

````ts
const add = (a: number, b:number)=> {  // number is inferred as the returned type
  const number = a + b; // infered as number by TS, really smart
  console.log(number)
  return number
}
````

````ts
const add = (a: number, b:number): 👉number=> {  // ❌ unnecessary code
  const number = a + b; // infered as number by TS, really smart
  console.log(number)
  return number
}
````

### Defining function types

````
const add = (a: number, b:number)=> {
  const number = a + b; // infered as number by TS, really smart
  console.log(number)
  return number
}

// we need to define the fn type like this (a:number, b: number) => number
const calculate = (a:number, b: number, fn: (a:number, b: number) => number) => {
  // fn is a value here (a pointer to a function)
  // so, this value needs a type ( a function type )
  fn(a,b)
}

calculate(1,2, add) // ✅
calculate(1,2, () => console.log('hello world')) // ❌Type 'void' is not assignable to type 'number
````

Problem: function types can become long when defined inline

It can be outsource as any type:
````ts
// we define types at the right hand side of the equal sign
👉type Addfn = (a:number, b: number) => number

const add = (a: number, b:number)=> {
  const number = a + b; // infered as number by TS, really smart
  console.log(number)
  return number
}

// we need to define the fn type like this (a:number, b: number) => number
const calculate = (a:number, b: number, fn: 👉Addfn) => {
  fn(a,b)
}

calculate(1,2, add) // ✅
calculate(1,2, () => console.log('hello world')) // ❌Type 'void' is not assignable to type 'number
````



### Interfaces

They have advantages when:

1. Using clases
2. Writing a library, so users can use the declaration merging to fit the library to their types

````ts
// this is an object type
type CredentialsAsTtpe = {
  username: string;
  password: string;
}

// is the same as this
interface 👉Credentials {
  username: string;
  password: string;
}

// interface allows declaration merging ✅	
interface 👉Credentials {
	email: string
}

// I can force my class to have those fields, I can't implement a type but I can do it with an interface ✅
class AuthCredentials 👉implements Credentials {
  username: string;
  password: string;
  age: number
  // constructor missing here, just demo purposes
}

const login = (credentials: Credentials) => {
  console.log(credentials)
}

// TS knows that the AuthCredentials class has the Credentials fields already.
login(new AuthCredentials())

````

### Merging types

````ts
type Admin = {
  permissions: string[]
}

type User = {
  userName: string
}

// I want a type that is a merge of both (e.g have permissions AND a userName as required fields)
// type AdminUser = Admin | User; // ❌ doesnt work because if any of the fields is present, it makes TS happy
type AdminUser = Admin 👉& User

````

Merging interfaces

````ts
interface Admin {
  permissions: string[]
}

interface User {
  userName: string
}

// I want a type that is a merge of both (e.g have permissions AND a userName as required fields)
// type AdminUser = Admin | User; // ❌ doesnt work because if any of the fields is present, it makes TS happy
interface AdminUser 👉extends Admin, User {} // I can add more fields inside {}

let adminUser: AdminUser;

adminUser = {
  permissions: [],
  userName: 'batman'
}
````



### Literal types

````js
type Role = 'user' | 'admin' // onyl these values are allowed (I could also have added a number to the union type)

let role: Role

role = 'customer' // ❌
````



### Type guards

````ts
// they are if checks that checks for types

const performAction = (action: string | number, role: Role) => {
  if(typeof action === 'string' && role === 'admin' ){ // ✅we get autocompletion for 'admin'
      // inside here, TS did alreay a 👉`type narrowing`, to it knows that the action and the role will have already satisfied the if check
  } 
}
````

### Generic types

````ts
type Role = 'user' | 'admin'

// Array is a built in generic type. Generic types need a type to be passed inside <>
type Roles = Array<Role> // or Role[]
````

We can also build our own generic types, like when building libraries, to fit the needs of the dev

````ts
// we have a variable T (could be named anything) which is a type
type DataStorage<T> = {
  // we can reference that T type inside the object
  storage: T[],
  add: (data: T) => void
}

// `let` example
// when uysing the type, we pass the type inside the angle brakets
let dataStorage: DataStorage<string>

dataStorage = {
  storage: ['banana'],
  add(data) {
    this.storage.push(data)
  }
}

// or using the `const` example
const dataStorage1: DataStorage<string> = {
  storage: ['banana'],
  add(data) {
    this.storage.push(data)
  }
}
````



### Generic functions

They are functions that can be passed arguments of any type, but the the returned value can be worked out from the types of the arguments.

````ts
function merge<T, U>(a: T, b: U){ // T and U can be objects of any shape
  return { // TS can infer the returned value! T & U 👌
    ...a,
    ...b
  }
}

const newUser = merge<{ name: string}, {age: number}>( // when hovering: const newUser: {name: string;} & {age: number;}
  { name: 'bob'},  
  {age: 30}
)

const newUser1 = merge( // 👉<> not needed, TS can infer T and U from the order of the args passed
  { name: 'bob'},
  {age: 30}
)

````

