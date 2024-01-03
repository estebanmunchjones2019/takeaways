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

They are functions that can be **passed arguments of any type**, but the the **returned type can be worked out** from the types of the arguments.

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

newUser1.age // ✅ I get autocompletion 😎

````

How NOT to define a generic function:
````ts
// when hovering: function merge(a: any, b: any): any ⚠
function merge(a, b){  // ⚠ there's a yellow underline on a and b saying: parameter a implicitly has an `any`type
  return { 
    ...a,
    ...b
  }
}


const newUser1 = merge( // newUser: any , booo ❌
  { name: 'bob'},
  {age: 30}
)
````



## Using TS with React: Essentials

### Create a vite TS React project

````bash
npm create vite@latest react-ts-essentials
````

the `tsconfig.josn`file configures the ts compiler (tsc):
````json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */ // vite reads this
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",

    /* Linting */
    "strict": true, // VSCode reads this to display errors visually
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}

````

The ts compiler is a dev dependency:

````json
{
  "name": "react-ts-essentials",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite", // compiles ts, bundles and serves the app to the browser
    "build": "tsc && vite build", // just compiles ts and bundles the app
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.43", // RectNode type lives here
    "@types/react-dom": "^18.2.17",
    "@typescript-eslint/eslint-plugin": "^6.14.0",
    "@typescript-eslint/parser": "^6.14.0",
    "@vitejs/plugin-react": "^4.2.1",
    "eslint": "^8.55.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "eslint-plugin-react-refresh": "^0.4.5",
    👉"typescript": "^5.2.2",
    "vite": "^5.0.8"
  }
}

````

### Props

````ts

type CourseGoalProps = {
  title: string;
  description: string;
}

const CourseGoal = ({title, description}: CourseGoalProps) => { // or (props: CourseGoalProps) and then use props.title and props.description
  return <article>
    <h1>{title}</h1>
    <h2>{description}</h2>
  </article>
} 

export default CourseGoal;
````

### Children (inside the props object)

````ts
import { ReactNode } from "react"; // from package.json  @types/react

type CourseGoalProps = {
  title: string;
  description: string;
  children: ReactNode
}

const CourseGoal = ({title, description, children}: CourseGoalProps) => { // or (props: CourseGoalProps) and then use props.title and props.description
  return <article>
    <h1>{title}</h1>
    <h2>{description}</h2>
    {children}
  </article>
} 

export default CourseGoal;
````

````ts
import { type ReactNode } from "react"; // good practice to add that keyword type, avoids the bundling tool from bundling it
````



### Alternative way: PropsWithChildren

````ts
import { type PropsWithChildren } from "react";

type CourseGoalProps = PropsWithChildren<{
  title: string;
  description: string;
}>
````

### Key prop: no need to type it

All React components (built-in components and also your custom components) do accept a special `key` prop which is used by React to track specific component instances.

For example, the `key` prop should always be set when outputting a list of components.

This `key` prop can be set on custom components even if you didn't specify it in your props type!

For example, the following component code will work:

````ts
type UserProps = {
  name: string; // no need to add the key prop.
};

function User({ name }: UserProps) {
  return <li>User: {name}</li>;
}

function App() {
  const users = [{ name: 'John' }, { name: 'Mary' }, { name: 'Luke' }];

  return (
    <>
      <ul>
        {users.map((user, index) => (
          <User key={user} name={user.name} /> // when adding the key prop, TS doesn't complain
        ))}
      </ul>
    </>
  );
}
````



### Alternative syntax for typing props in functional components

````ts
import { type PropsWithChildren, type FC } from "react"; // good practice to add that keyword type (decorator), avoids the bundling tool from bundling it

type CourseGoalProps = PropsWithChildren<{
  title: string;
  description: string;
}>

const CourseGoal: 👉FC<CourseGoalProps> = ({title, description, children}) => {
````

### React shortcut when passing props

````ts
const Header = ({img, children}: HeaderProps) => {
  return <header>
    {children}
    <img 👉{...img}/> // when the object keys match the names of the props I need here: eg src and alt
    // under the hood:
    <img alt={alt} src={src}/>
  </header>
}
````

Props are passed as al

````ts
import CourseGoal from "./components/CourseGoal";
import goalsImg from './assets/goals.jpg' // goalsImage is just a path
import Header from "./components/Header";
import { useState } from "react";

type Goal = {
  title: string;
  description: string
  id: number
}

export default function App() {

  const [goals, setGoals] = useState<Goal[]>([])

  const handleAddGoal = () => {
    const newGoal: Goal = {
      title: 'hello',
      description: 'world',
      id: Math.random()
    }

    setGoals(prevGoals => [...prevGoals, newGoal])
  }

  const Goals = goals.map(({title, description, id}) => (
    <CourseGoal title={title} description={description} key={id}/>
  ))
  return (
  <main>
    <Header img={{src: goalsImg, alt: 'My goals'}}>
        Welcome to my goals app
    </Header>
    <button onClick={handleAddGoal}>Add goal</button>
    {Goals}
  </main>
  )
}
````



### Exporting types: avoiding type duplication

````ts
export type SomeType {}

import { type SomeType } from './someFile'
````



### Handling and typing events

````tsx
// handleSubmit needs FormEvent to be manually typed
import { type 👉FormEvent } from 'react'

export default SomeComponent = () => {
    function handleSubmit(event: FormEvent){
        event.preventDefault()
        etc
	}
    
    return (
        <form onSubmit={handleSubmit}> 
    
        </form>
    ) 
}


// if defined inline onSubmit={(event) => etc} // TS can infer it, because the fn is only used as an event handler and not somewhere else in the code
<form onSubmit={(event) => event.preventDefault()}> // the type is infered in this case
    
<form>
    
````

### Working with generic event types (FormEvent):

````tsx
// a FormEvent can have multiple sources, but we can tell the generic type that is has been triggered by the HTML
// if we want to access propr of the event object, then we need to tell TS which thing generated that object
import { type 👉FormEvent } from 'react'

export default SomeComponent = () => {
    function handleSubmit(event: FormEvent<👉HTMLForElement>){ // HTMLForElement added on tsconfig.json -> lib -> DOM
        event.preventDefault()
        const formData = new FormData(event.currentTarget) // 👉without passing the type above, this throws an error as the FormEvent might not have that currentTarget prop
        // I use currentTarget instead of current because React will not always add that prop
        // extract the input values here
	}
    
    return (
        <form onSubmit={handleSubmit}> 
    
        </form>
    ) 
}
// HTMLFormElement is available thanks to the tsconfig.json lib part DOM
````

To get inputs from a form there 3 ways in React:

1. each input has a state and has two way data binding (update on every stroke)
2. Using the event and grabbing them after the submit button is clicked
3. using refs to access the DOM after the submit button is clicked

### Using useRef to get the input values to access the DOM

````tsx
export default SomeComponent = () => {
	const title = useRef<👉HTMLInputElement>(null); // HTMLInputElement allows for title.current!.👉value access
	const description = useRef<HTMLInputElement>(👉null); // null allows for <input ref={title} // ref can't be undefined, but it can be null
    function handleSubmit(event: FormEvent<HTMLForElement>){
        event.preventDefault()
        // hey TS title.current! means will NOT be null when this code runs, trust me
        const enteredTitle = title.current👉!.value // React will add this .current prop
        const enteredDesc = description.current!.value
	}
    
    return (
        <form onSubmit={handleSubmit}> 
    
        </form>
    ) 
}
````



🚀Pro tip: long TS error messages? the cause is the most nested error



### Storing a React component in a variable

````tsx
let warningBox: ReactNode (this can be undefined, which JSX accepts whitout complaining)

if (goals.length > 4) {
	warningBox = <InfoBox mode="warning">Hey, too many goals here!</InfoBox>
}

return (
	{warningBox}
	// some more JSX here
)
````

### React components with flexible props 

````tsx

type ComponentProps = {
	children: ReactNode;
	mode?👈: 'hard' | 'soft' // when hovered 'hard' | 'soft' | undefined
    // or 
	mode: 'hard' | 'soft' | undefined 
}
````

### What if I want to be passed only when other one is passed and has certain value?

````
// just use unions

type ComponentProps = {
	children: ReactNode;
	mode: 'hint' | 'warning'
	severity?: 'low' | 'medium' | 'high'
} | {
	children: ReactNode
}
````

