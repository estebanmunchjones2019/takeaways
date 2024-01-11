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
    function handleSubmit(event: FormEvent<👉HTMLFormElement>){ // HTMLForElement added on tsconfig.json -> lib -> DOM
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
	severity?👈: 'low' | 'medium' | 'high' // when hovered 'hard' | 'soft' | undefined
    // or 
	severity: 'low' | 'medium' | 'high' | undefined 
}
````

### What if I want to props be mandatory only when other one is passed and has certain value?

#### Discriminated unions:

````ts
// e.g severity is mandatory when the mode is 'warning'
type InfoBoxProps = {
    mode: 'hint'
}

type WarninBoxProps = {
    mode: 'warning', 
    severity: 'low' | 'medium' | 'high' // I make this a required prop when mode is warning
}

type ComponentProps = InfoBoxProps | WarninBoxProps
````

````tsx
const Component = ({mode, severity ❌}: ComponentProps) // I CANT destructure an optional prop

const Component = (props: ComponentProps) => {
    const {mode} = props
	if (mode === 'hint'){
        return someJSX
    }
    const {severity} = props; // 💡 TS is smart enough to know that at this point, mode is 'warning' and severity is a passed prop
}
````



### Input wrapper component

This component has additional html around the input, that I don't want to repeat each time:

````tsx
import { ComponentPropsWithoutRef } from "react";

type InputProps = {
  label: string;
  id: string
} & ComponentPropsWithoutRef<'input'>

export default function Input({label, id, 👉...otherProps}: InputProps) { // 💡I grab all the rest of props with `...someName`
  return (
    <p>
      <label htmlFor={id}>{label}</label>
      <input id={id} 👉{...otherProps}></input> 
      {/* // I pass the remaining props to the input */}
    </p>
  )
}
````



### Button wrapper component

````tsx
import { type ComponentPropsWithoutRef, type ReactNode } from "react";

type ButtonProps = {
  el: 'button',
} & ComponentPropsWithoutRef<'button'>

type AnchorProps = {
  el: 'anchor',
} & ComponentPropsWithoutRef<'a'>

export default function Button({el, ...props ❌}: AnchorProps | ButtonProps) { // destructuring props here is ambiguos to TS
  if (el === 'button'){
    return (
      <button {...props}></button>
    )
  }
  return (
    <a {...props}></a>
  )
}
````



````tsx

export default function Button(props: AnchorProps | ButtonProps) { // I grab all the rest of props with ...someName
  if (props.el === 'button'){
    const { el, ...otherProps } = props //✅ destructure remaining props inside if checks, so TS knows which type of the union I'm working with
    return (
      <button {...otherProps}/>
    )
  }
  const { el, ...otherProps } = props

  return (
    <a {...otherProps}></a>
  )
}
````

### Infering anchor or button from html props (href)

````tsx
import { type ComponentPropsWithoutRef } from "react";

type ButtonProps =  ComponentPropsWithoutRef<'button'>

type AnchorProps =  ComponentPropsWithoutRef<'a'>

export default function Button(props: AnchorProps | ButtonProps) { // if href is present, then render an anchor element
  if (props.href ❌){
    return (
      <a {...props}></a>
    )
  }
  return (
    <button {...props}></button>
  )
}
````

### Type predicate to the rescue!

Partial solution:

````tsx
import { type ComponentPropsWithoutRef } from "react";

type ButtonProps =  ComponentPropsWithoutRef<'button'>

type AnchorProps =  ComponentPropsWithoutRef<'a'>

// TS inferes that the fn returns a boolean, but it doesn't know which type made that fn return true (or false)
// hey TS, if this function returns true, it means that the prop arg passed is of the type AnchorProps
const isAnchorProps = (props: ButtonProps | AnchorProps): 👉props is AnchorProps => {
  return 'href' in props
}

export default function Button(props: AnchorProps | ButtonProps) { // I grab all the rest of props with ...someName
  if (isAnchorProps(props)){
    return (
      <a {...props}></a>
    )
  }
  return (
    <button {...props}></button>
  )
}
````

````tsx
// ❌downside: I can mix props of anchor and button html elements when rendering Button component
// 🚨 mixing props of both object types is allowed by default.
import Button from "./components/Button";
import Input from "./components/Input";

function App() {
  return (
    <>
      <h1>Let's get started!</h1>
      <Input id="name" label="name"></Input>
      <Input id="age" label="age" type="number"></Input>
      <Button target="" 👈 an achor prop here, allowed>Submit</Button>
      <Button href="https://google.com" disabled👈 (a button prop here allowed (no TS error 😬))>A link</Button>
    </>
  )
 
}

export default App;

````



We can make the types a little bit more specific:

````tsx
import { type ComponentPropsWithoutRef } from "react";

type ButtonProps =  ComponentPropsWithoutRef<'button'> & { href?: never}

type AnchorProps =  ComponentPropsWithoutRef<'a'> & { href?: string} 

const isAnchorProps = (props: ButtonProps | AnchorProps): props is AnchorProps => {
  return 'href' in props
}

export default function Button(props: AnchorProps | ButtonProps) { // I grab all the rest of props with ...someName
  if (isAnchorProps(props)){
    return (
      <a {...props}></a>
    )
  }
  return (
    <button {...props}></button>
  )
}
````

````tsx
import Button from "./components/Button";
import Input from "./components/Input";

function App() {
  return (
    <>
      <h1>Let's get started!</h1>
      <Input id="name" label="name"></Input>
      <Input id="age" label="age" type="number"></Input>
      <Button target="" no error here, I can still mix things up>Submit</Button> 
      <Button href="https://google.com" disabled ❌ error here (nice!)>A link</Button>
    </>
  )
 
}

export default App;
````

### TS limitations:

Cons: if I don't pass an href prop, it could be any type (anchor or button)

Pro:I if pass an href prop, it is a anchor type for sure.

### Polymorphic component

A component that wraps a component but it doesn't know which one in advanced

why building a container:

- reuse styling
- reuse logic
- reuse JSX

a not very useful wrapper:

````tsx
import {  ElementType } from "react";

type ContainerProps = {
  as: ElementType // it could be input, CustomComponent, etc
}

export default function Container({as: Component}: ContainerProps) { // I grab all the rest of props with ...someName
  return (
    <Component></Component>
  )
}
````

````tsx
// parent component
<Container as="button"></Container> // an html element
<Container as={Button}></Container> // a React component
````

Using generic to pass props to the as Component or html tag

````tsx
import {  ComponentPropsWithoutRef, ElementType, ReactNode } from "react";

type ContainerProps<T extends 👉ElementType> = { 
  as?: ElementType; // it could be input, CustomComponent, etc (name of html tag or Component)
  children: ReactNode // JSX code or text
} & ComponentPropsWithoutRef<T> // T needs to be of type 👉ElementType

// I convert the Container component to a generic function, to understand the type from the arguments, and pass it to the generic type ContainerProps
export default function Container<C extends ElementType>({as: Component = 'div', children, ...props}: ContainerProps<C>) { // I grab all the rest of props with ...someName
  return (
    <Component {...props}>{children}</Component>
  )
}
````



### Examples

````tsx
// Example: A Button component that has an icon and text
// The icon is passed via a prop, which is a function that returns JSX code
import type { ComponentPropsWithoutRef, ElementType, ReactNode } from 'react';

type IconButtonProps = {
  icon: ElementType;
  onClick: () => void;
  children: ReactNode;
} & ComponentPropsWithoutRef<'button'>;

export function IconButton({
  // icon is aliased to Icon because it should be used like a custom component name
  icon: Icon,
  children,
  ...otherProps
}: IconButtonProps) {
  return (
    <button {...otherProps}>
      <span>
        <Icon />
      </span>
      <span>{children}</span>
    </button>
  );
}

// Example Usage:

function HeartIcon() {
  return <span>❤️</span>;
}

export function Demo() {
  return (
    <IconButton icon={HeartIcon} onClick={() => console.log('Button clicked!')}>
      Like
    </IconButton>
  );
}
````

````tsx

// Example: A Card component that has multiple "slots" for content
// Main slot => children prop
// Actions slot => actions prop

import { ReactNode } from 'react';

type CardProps = {
  title: string;
  children: ReactNode;
  // "actions" is like an extra "slot" of this component
  // It's the same type as the children prop, since we expect JSX code as a prop value
  actions: ReactNode;
};

export function Card({ title, children, actions }: CardProps) {
  return (
    <section>
      <h2>{title}</h2>
      {children}
      {actions}
    </section>
  );
}

// Example Usage:
export function Demo() {
  return (
    <Card
      title="My Card"
      actions={
        <button onClick={() => console.log('Button clicked!')}>
          Click Me!
        </button>
      }
    >
      <p>Some content</p>
    </Card>
  );
}
````

````tsx
// Example: A Generic List Component
// This reusable component can be used to render a list of items of any type. The type of the items is passed via a generic type parameter.

import { type ReactNode } from 'react';

type ListProps<T> = {
  items: T[];
  renderItem: (item: T) => ReactNode;
};

export function List<T>({ items, renderItem }: ListProps<T>) {
  return <ul>{items.map(renderItem)}</ul>;
}

// Example Usage:

export function Demo() {
  const users = [
    { id: 'u1', name: 'Max' },
    { id: 'u2', name: 'Manuel' },
  ];

  const hobbies = ['Sports', 'Reading', 'Cooking'];

  return (
    <main>
      <section>
        <h2>Users</h2>
        <List
          items={users}
          renderItem={(user) => <li key={user.id}>{user.name}</li>}
        />
      </section>
      <section>
        <h2>Hobbies</h2>
        <List
          items={hobbies}
          renderItem={(hobby) => <li key={hobby}>{hobby}</li>}
        />
      </section>
    </main>
  );
}
````



### Passing references to components

````tsx
import { ComponentPropsWithoutRef } from "react";

type InputProps = {
  label: string;
  id: string;
  ref: ❌ // doesn't work, TS complaints
} & ComponentPropsWithoutRef<'input'>
````

````tsx

import { ComponentPropsWithRef } from "react";

type InputProps = {
  label: string;
  id: string
} & ComponentPropsWithRef<'input'> // ❌ it doesn't work, TS doesn't complain but there's a JS error at runtime
````

````ts
import { type ComponentPropsWithoutRef, forwardRef } from "react"; // forwardRef is a real react fn, not a type

type InputProps = {
  label: string;
  id: string
} & ComponentPropsWithoutRef<'input'>

// once I pass one type to the forwardRef generic fn, I need to pass the second, and then I can omit if inside the inner fn
const Input = 👉forwardRef<HTMLInputElement, InputProps>(function ({label, id, ...props}, ref) { // I can ommit ({label, id, ...props}, ref)👉: InputProps
  return (
    <p>
      <label htmlFor={id}>{label}</label>
      <input id={id} {...props} ref={ref}></input> 
    </p>
  )
})

export default Input
````

````tsx
import { useRef } from "react";
import Button from "./components/Button";
import Container from "./components/Container";
import Input from "./components/Input";

function App() {

  const inputRef = useRef<HTMLInputElement>(null)
  return (
    <>
      <Input id="name" label="name" 👉ref={inputRef}></Input>
    </>
  )
}

export default App;
````



### A form component

Using `unknown` for an object which I don't know the shape off.

````tsx
import { ComponentPropsWithoutRef, FormEvent } from "react";

type FormProps = ComponentPropsWithoutRef<'form'> & {
  onSave: (formData: 👉unknown) => void; // formData is an object that could have different keys depending on which fields are passed as children
}


// once I pass one type to the forwardRef generic fn, I need to pass the second, and then I can omit if inside the inner fn
const Form = ({children, onSave, ...props}: FormProps) => {
  const submitHandler = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault()
    const formData = new FormData(event.currentTarget) // formData.get('fieldName') clunky ❌
    const simpleFormData = Object.fromEntries(formData) // simpleFormData.fieldName better ✅
    onSave(simpleFormData)
  }
  
  return (
    <form {...props} onSubmit={submitHandler}>
      {children}
    </form>
  )
}

export default Form
````



And we can cast the type when we DO know what the type will be for that `unknown` variable.

````tsx

import { FormEvent, useRef } from "react";
import Button from "./components/Button";
import Container from "./components/Container";
import Input from "./components/Input";
import Form from "./components/Form";

function App() {

  const inputRef = useRef<HTMLInputElement>(null)

  // I know the fields passed as children to <Form>, so I can type the argument unknown
  // all picked up input values are strings
  const onSave = (formData: unknown) => {
    debugger;
    // I assign it to new variable and cast the type (convince TS about it)
    const extractedData = formData 👉as { name: string; age: string}
    console.log(extractedData)
  }

  return (
    <>
      <h1>Let's get started!</h1>
      <Form onSave={onSave}>
        <Input id="name" label="name"></Input>
        <Input id="age" label="age" type="number"></Input>
        <Button type="submit" target="">Submit</Button>
      </Form>
      <Button href="https://google.com">A link</Button>
      <Container as="button">Click me first</Container>
      <Container as={Button} onClick={() => console.log('clicked')}>Click me</Container>
    </>
  )
 
}

export default App;
````

The App.tsx component knows which input fields are rendered, that's were we cast

### Exposing component APIs (functions) via useImperativeHandle hook

The easy way of clearing the form:

````tsx
import { ComponentPropsWithoutRef, FormEvent } from "react";

type FormProps = ComponentPropsWithoutRef<'form'> & {
  onSave: (formData: unknown) => void; // formData cis an object that could have different keys depending on which fields are passed as children
}


// once I pass one type to the forwardRef generic fn, I need to pass the second, and then I can omit if inside the inner fn
const Form = ({children, onSave, ...props}: FormProps) => { // I can ommit ({label, id, ...props}, ref)👉: InputProps
  const submitHandler = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault()
    const formData = new FormData(event.currentTarget) // formData.get('fieldName') clunky ❌
    const simpleFormData = Object.fromEntries(formData) // simpleFormData.fieldName better ✅
    onSave(simpleFormData)
    event.currentTarget?.👉reset()
  }
  
  return (
    <form {...props} onSubmit={submitHandler}>
      {children}
    </form>
  )
}

export default Form
````

The more difficult way to do it by exposing a fn (API):
summary: an object with methods is passed to the parent component via refs.

````tsx
import { FormEvent, useRef } from "react";
import Button from "./components/Button";
import Container from "./components/Container";
import Input from "./components/Input";
import Form, { type FormHandle } from "./components/Form";

function App() {

  // this is an object with the `clear` method
  const customForm = useRef<FormHandle>(null)

  const onSave = (formData: unknown) => {
    const extractedData = formData as { name: string; age: string}
      console.log(extractedData)
      customForm.current?.clear()
  }

  return (
    <>
      <h1>Let's get started!</h1>
      <Form onSave={onSave} ref={customForm}>
        <Input id="name" label="name"></Input>
        <Input id="age" label="age" type="number"></Input>
        <Button type="submit" target="">Submit</Button>
      </Form>
    </>
  )
 
}

export default App;
````

````tsx
import { forwardRef, ComponentPropsWithoutRef, FormEvent, useImperativeHandle, useRef } from "react";

export type FormHandle = {
  clear: () => void
}

type FormProps = ComponentPropsWithoutRef<'form'> & {
  onSave: (formData: unknown) => void; // formData cis an object that could have different keys depending on which fields are passed as children
}

// the first type is the type of the reference passed up to the parent component
const Form = forwardRef<👉FormHandle, FormProps>(({children, onSave,...props}, ref) => { // I can ommit ({label, id, ...props}, ref)👉: InputProps

  // I want to access the form HTML element to clear the form  
  const form = useRef<HTMLFormElement>(null)

  useImperativeHandle(ref, () => {
    // 👉this returned object becomes the ref that the parent component gets
    return 👉{
      // I access the DOM via another ref here attached to the <form> element
      clear() { form.current?.reset() }
    }
  })

  const submitHandler = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault()
    const formData = new FormData(event.currentTarget) // formData.get('fieldName') clunky ❌
    const simpleFormData = Object.fromEntries(formData) // simpleFormData.fieldName better ✅
    onSave(simpleFormData)
  }
  
  return (
    <form {...props} onSubmit={submitHandler} ref={form}>
      {children}
    </form>
  )
})

export default Form
````

###  

### Alternative: Avoiding Type Casting with "as"

In the previous lecture, we used *"Type Casting"* (also called *"Type Assertion"*) via TypeScript's `as` keyword to *"tell"* TypeScript that a value is of a specific type.

This is a technique that makes sense when working with data where TypeScript has no chance of inferring the type where you on the other hand know the exact type.

If you're not 100% sure about the type of value you'll be dealing with at runtime (i.e., if there are multiple possible value types) or if you want to be extra safe, you can also use a combination of *"Type Guards"* to narrow down the type until TypeScript is able to infer the final type.

Here's the code from the previous lecture, now adjusted to use *"Type Guards"* for *"Type Narrowing"*:

```tsx
function handleSave(data: unknown) {
  // const extractedData = data as { name: string; age: string };
    // type guard here 👇
  if (
    !data ||
    typeof data !== 'object' ||
    !('name' in data) ||
    !('age' in data)
  ) {
      return;
  }

  // at this point, TypeScript knows that data MUST BE an object 
  // with a name and age property
  // otherwise, the previous if statement would have returned
  console.log(data); // 👈 at this point, TS knows exactly that data is an object with name and age property
  customForm.current?.clear();
}
```

### Advanced type -safe state with Context and useReducer

first issue: convince TS that the already initialised context is not null

How NOT ❌ to fix it:

````tsx
// ConsumerComponent.tsx
const timersCtx = useContext(TimersContext)!👈; // I don't want to repeat this for every consumer component ❌
````

OR, use a type guard

````tsx
// ConsumerComponent.tsx

const timersCtx = useContext(TimersContext); 

if (timersCtx === null) throw Error('this should never happen') // I don't want to repeat this for every consumer component ❌
````

How to fix it ✅:

We can build a custom hook that returns the context already applying the type guard:

````tsx
import { ReactNode, createContext, useContext } from "react";

type TimersContextProviderProps = {
  children: ReactNode
}

type Timer = {
  name: string;
  duration: number
}

type TimersState = {
  isRunning: boolean;
  timers: Timer[]
}

export type TimersContextValue = TimersState & {
  // methods to  change the state 
  addTimer: (data: Timer) => void;
  startTimers: () => void;
  stopTimers: () => void;
}

// when creating the context, I initialise it with null (and add it in a union type)
const TimersContext = createContext<TimersContextValue | 👉null>(null)

// 👉I export a custom hook with a type guard (so the consumers are assured the context is never null)
👉 export const useTimersContext = () => {
  const timersContext = useContext(TimersContext)
  if (timersContext === null) throw Error('timersContext is null, this should never happpen')
  return timersContext;
}

const TimersContextProvider = ({children}: TimersContextProviderProps) => {
  // when providint the context, I give it some value different than null
  const ctx = {
    isRunning: false,
    timers: [],
    addTimer(data){},
    startTimers(){},
    stopTimers(){}
  }
  return <TimersContext.Provider value={ctx}>{children}</TimersContext.Provider>
}

// I export a wrapper component to provide the context👇
export default TimersContextProvider
````

````tsx

import AddTimer from './components/AddTimer.tsx';
import Header from './components/Header.tsx';
import Timers from './components/Timers.tsx';
import TimersContextProvider from './store/timers-context.tsx';

function App() {
  return (
   👉 <TimersContextProvider>
      <Header />
      <main>
        <AddTimer />
        <Timers />
      </main>
    </TimersContextProvider>
  );
}

export default App;
````

````tsx

import { useTimersContext } from '../store/timers-context.tsx';
import Button from './UI/Button.tsx';

export default function Header() {
  const {isRunning} = 👉useTimersContext() // neat! 😎

  return (
    <header>
      <h1>ReactTimer</h1>

      <Button>{isRunning ? 'Stop Timers' : 'Start Timers'}</Button>
    </header>
  );
}
````

### useReducer()

is an fn called automatically by React when an action is dispatched. React will pass the current state and the action that wants to change the state

How to not set the action type:
````tsx
type Action = {
  type: 'ADD_TIMER' | 'START_TIMERS' | 'STOP_TIMERS',
  // payload types could be different for future types, and I
  payload?: Timer
}

const timersReducer = (state: TimersState, action: Action): TimersState => {
  debugger;
  switch (action.type) {
    case "ADD_TIMER":
      const updatedTimers = [...state.timers]
      updatedTimers.push(action.payload!👈❌)
      return {
        ...state,
        timers: updatedTimers
      }
    case "START_TIMERS":
          // I can also access action.payload here
          // action.payload
      return {
        ...state,
        isRunning: true
      }
    case "STOP_TIMERS":
      return {
        ...state,
        isRunning: false
      }
  }
}
````



The fix ✅: use discriminated unions:
````tsx
type Action = {
  type: 'ADD_TIMER',
  payload: Timer
} | {
  type: | 'START_TIMERS' | 'STOP_TIMERS'
}

const timersReducer = (state: TimersState, action: Action): TimersState => {
  debugger;
  switch (action.type) {
    case "ADD_TIMER":
      const updatedTimers = [...state.timers]
      updatedTimers.push(action.payload✅)
      return {
        ...state,
        timers: updatedTimers
      }
    case "START_TIMERS":
          // I can also access action.payload here
          // action.payload ❌ I get a TS error
      return {
        ...state,
        isRunning: true
      }
    case "STOP_TIMERS":
      return {
        ...state,
        isRunning: false
      }
  }
}
````

Full code of context and useReducer:

````tsx
import { ReactNode, createContext, useContext, useReducer } from "react";

type TimersContextProviderProps = {
  children: ReactNode
}

export type Timer = {
  name: string;
  duration: number
}

type TimersState = {
  isRunning: boolean;
  timers: Timer[]
}

// discriminated union here 👇
type Action = {
  type: 'ADD_TIMER',
  payload: Timer
} | {
  type: | 'START_TIMERS' | 'STOP_TIMERS'
}

export type TimersContextValue = TimersState & {
  addTimer: (data: Timer) => void;
  startTimers: () => void;
  stopTimers: () => void;
}

// when creating the context, I initialise it with null
const TimersContext = createContext<TimersContextValue | null>(null)

export const useTimersContext = () => {
  const timersContext = useContext(TimersContext)
  // 💡 write the guard one, reuse this custom hook multiple times
  if (timersContext === null) throw Error('timersContext is null, this should never happpen')
  return timersContext;
}

const initialState: TimersState = {
  isRunning: false,
  timers: []
}

const timersReducer = (state: TimersState, action: Action): TimersState => {
  switch (action.type) {
    case "ADD_TIMER":
      return {
        ...state,
        timers: [
          ...state.timers,
          action.payload
        ]
      }
    case "START_TIMERS":
      return {
        ...state,
        isRunning: true
      }
    case "STOP_TIMERS":
      return {
        ...state,
        isRunning: false
      }
  }
}



const TimersContextProvider = ({children}: TimersContextProviderProps) => {
  // when providint the context, I give it some value different than null
  
  // 💡 I keep global state inside this wrapper component
  const [timersState, dispatch] = useReducer(timersReducer, initialState)

  console.log('timersState', timersState)

 // this is a brand new object everytime this wrapper component re-renders (state changes, or new children passed to it)
  const ctx = {
    ...timersState, // 👈 I pass the updated state via context
    addTimer(data: Timer){
      dispatch({
        type:'ADD_TIMER',
        payload: data
      })
    },
    startTimers(){
      dispatch({
        type:'START_TIMERS'
      })
    },
    stopTimers(){
      dispatch({
        type:'STOP_TIMERS'
      })
    }
  }
  return <TimersContext.Provider value={ctx}>{children}</TimersContext.Provider>
}

export default TimersContextProvider
````

````tsx
// read and mutate the global state
import { useTimersContext } from '../store/timers-context.tsx';
import Button from './UI/Button.tsx';

export default function Header() {
  const {isRunning, startTimers, stopTimers} = useTimersContext()

  return (
    <header>
      <h1>ReactTimer</h1>

      <Button onClick={isRunning ? stopTimers : startTimers}>{isRunning ? 'Stop Timers' : 'Start Timers'}</Button>
    </header>
  );
}
````



### sideEffects, useEffect and data fetching

problem: setInterval called outside useEffect with no dep array will yield and infinite loop

````tsx
import Container from './UI/Container.tsx';
import { Timer as TimerProps } from '../store/timers-context.tsx'
import { useState } from 'react';

const step = 50;

export default function Timer({name, duration}: TimerProps) {
  const [remainingTime, setRemainingTime] = useState(duration)

  console.log('Timer component runs')

  // infinte loop! ❌  
  setInterval(() => {
    setRemainingTime(remainingTime - step)
  }, step)

  return (
    <Container as="article">
      <h2>{name}</h2>
      <progress max={duration} value={remainingTime}></progress>
      <p>{duration}</p>
    </Container>
  );
}
````



problem: the interval is not cleared

if the component is unmounted, there will be fns running every x ammount of ms, even if the component doesnt need it

````tsx
import Container from './UI/Container.tsx';
import { Timer as TimerProps } from '../store/timers-context.tsx'
import { useEffect, useState } from 'react';

const step = 50;

export default function Timer({name, duration}: TimerProps) {
  const [remainingTime, setRemainingTime] = useState(duration * 1000)

  console.log('Timer component runs with remainingTime: ', remainingTime)

  // the setInterval will keep running after the component is unmounted
  // Memory leak! ❌ 
  useEffect(() => {
    setInterval(() => {
      console.log('setInterval callback runs')
      debugger
      setRemainingTime(remainingTime => remainingTime- step)
    }, step)
  }, [])

  return (
    <Container as="article">
      <h2>{name}</h2>
      <progress max={duration} value={remainingTime / 1000}></progress>
      <p>{(remainingTime / 1000).toFixed(2)}</p>
    </Container>
  );
}
````



solution 1: try to clearInterval inside the component: the interval is not defined

````tsx

import Container from './UI/Container.tsx';
import { Timer as TimerProps } from '../store/timers-context.tsx'
import { useEffect, useState } from 'react';

const step = 50;

export default function Timer({name, duration}: TimerProps) {
  const [remainingTime, setRemainingTime] = useState(duration * 1000)

  console.log('Timer component runs with remainingTime: ', remainingTime)

  if (remainingTime <= 0) {
    clearInterval(interval) // ❌interval variable not defined here 
  }

  useEffect(() => {
    const interval = setInterval(() => {
      console.log('setInterval callback runs')
      debugger
      setRemainingTime(remainingTime => {
        if (remainingTime <= 0) remainingTime
        return remainingTime - step
      })
    }, step)
  }, [])

  return (
    <Container as="article">
      <h2>{name}</h2>
      <progress max={duration} value={remainingTime / 1000}></progress>
      <p>{(remainingTime / 1000).toFixed(2)}</p>
    </Container>
  );
}
````

solution 2: create a let variable inside the component: the variable is re-created each time the component re renders

````tsx
import Container from './UI/Container.tsx';
import { Timer as TimerProps } from '../store/timers-context.tsx'
import { useEffect, useState } from 'react';

const step = 50;

export default function Timer({name, duration}: TimerProps) {
  const [remainingTime, setRemainingTime] = useState(duration * 1000)

  console.log('Timer component runs with remainingTime: ', remainingTime)

  let interval: number; // ❌ variable recreated each time the component re renders

  if (remainingTime <= 0) {
    clearInterval(interval)
  }

  useEffect(() => {
    interval = setInterval(() => {
      console.log('setInterval callback runs')
      debugger
      setRemainingTime(remainingTime => {
        if (remainingTime <= 0) remainingTime
        return remainingTime - step
      })
    }, step)
  }, [])

  return (
    <Container as="article">
      <h2>{name}</h2>
      <progress max={duration} value={remainingTime / 1000}></progress>
      <p>{(remainingTime / 1000).toFixed(2)}</p>
    </Container>
  );
}
````

solution 3: create a let variable outside the component: the variable is shared accross all Timer instances

````tsx
import Container from './UI/Container.tsx';
import { Timer as TimerProps } from '../store/timers-context.tsx'
import { useEffect, useState } from 'react';

const step = 50;

let interval: number; // ❌ variable shared for all Timer instances, only one timer will be cleared if many Timer components rendered on the page

export default function Timer({name, duration}: TimerProps) {
  const [remainingTime, setRemainingTime] = useState(duration * 1000)

  console.log('Timer component runs with remainingTime: ', remainingTime)

  if (remainingTime <= 0) {
    clearInterval(interval)
  }

  useEffect(() => {
    interval = setInterval(() => {
      console.log('setInterval callback runs')
      debugger
      setRemainingTime(remainingTime => {
        if (remainingTime <= 0) remainingTime
        return remainingTime - step
      })
    }, step)
  }, [])

  return (
    <Container as="article">
      <h2>{name}</h2>
      <progress max={duration} value={remainingTime / 1000}></progress>
      <p>{(remainingTime / 1000).toFixed(2)}</p>
    </Container>
  );
}
````



#### solution 4: using a ref (keep a variable that is not state that survives re renders)

With references, we can:

- access the DOM
- pass an object with methods to a parent component
- store a value that is kept on re-renders

````tsx
export default function Timer({name, duration}: TimerProps) {
  const [remainingTime, setRemainingTime] = useState(duration * 1000)
  const interval = useRef<number👈>(null)

  console.log('Timer component runs with remainingTime: ', remainingTime)

  // clear the interval when the timer reaches 0
  if (remainingTime <= 0) {
    clearInterval(interval)
  }

  useEffect(() => {
    interval.current👈 = setInterval(() => { // I get a TS error ❌ there: Cannot assign to 'current' because it is a read-only property.ts(2540)
````

The fix is to add useRef<number | null👈>(null)

💡I useRef<number | null>(null) I need the union here, but wasn't needed when accessing the DOM because the ref prop assures that is never null (inferred from there)

Problem: React in stirct mode re-renders twice (for debugging purposes), so two intervals are set, and when unmounted, the intervals keep running in the background

````tsx
  useEffect(() => {
    interval.current = setInterval(() => {
      console.log('setInterval callback runs')
      setRemainingTime(remainingTime => {
        if (remainingTime <= 0) remainingTime
        return remainingTime - step
      })
    }, step)
    return () => clearInterval(interval.current) // ❌ this  interval.current could have been changed by another hook  
  }, [])
````

The fix: use a variable inside the hook:

````tsx
import Container from './UI/Container.tsx';
import { Timer as TimerProps, useTimersContext } from '../store/timers-context.tsx'
import { useEffect, useRef, useState } from 'react';

const step = 50;

export default function Timer({name, duration}: TimerProps) {
  const [remainingTime, setRemainingTime] = useState(duration * 1000)
  const interval = useRef<number | null>(null)
  const { isRunning } =  useTimersContext()

  console.log('Timer component runs with remainingTime: ', remainingTime)

  // clear the interval when the timer reaches 0
  if (remainingTime <= 0 && interval.current) {
    clearInterval(interval.current)
  }

  useEffect(() => {
    let timer: number; // ✅ a variable was added there
    if (isRunning){
      timer = setInterval(() => {
        console.log('setInterval callback runs')
        setRemainingTime(remainingTime => {
          if (remainingTime <= 0) remainingTime
          return remainingTime - step
        })
      }, step)
      interval.current = timer;
    } else if (interval.current){
       // clean up the interval when stopped manually
      clearInterval(interval.current)
    }
    // clean up function when unmounting or when this hooks re-runs
    return () => clearInterval(timer) // 👈 So I'm 100% sure this variable wasn't changed by other hook
  }
  , [isRunning])

  return (
    <Container as="article">
      <h2>{name}</h2>
      <progress max={duration} value={remainingTime / 1000}></progress>
      <p>{(remainingTime / 1000).toFixed(2)}</p>
    </Container>
  );
}
````



### Data fetching

TODO: code the whole get helper fn, catch errors and add loading state

````ts
// async fns wrap the returned value into a promise
const get = async (url: string) => {
  const response = await fetch(url)

  // the fetch api doesn't throw an error when the response code is 400ish or 500ish unfortunately
  if (!response.ok) throw Error('Response with code 400ish or 500ish')

  const data = await response.json() as unknown // 👈 first casting here
  return data
}

export default get;
````

````tsx
import { ReactNode, useEffect, useState } from "react";
import BlogPosts, { BlogPost } from "./components/BlogPosts";
import get from "./utils/get";

type BlogPostResponse = {
  id: number;
  title: string;
  body: string;
  userId: number
}

function App() {
  const [blogPosts, setBlogPosts] = useState<BlogPost[]>()
  const [isLoading, setIsLoading] = useState(false)
  const [error, setError] = useState<string>()

  useEffect(() => {
    (async () => {
      setIsLoading(true)
      try {
        const blogPostsResponse= await get('https://jsonplaceholder.typicode.com/posts') as BlogPostResponse[] // 👈 second casting here
        const blogPosts = blogPostsResponse.map(({id, title, body}) => {
          return {
            id,
            title,
            text: body,
          }
        })

        setBlogPosts(blogPosts)
      } catch(error) {
		// option 1: casting
         // setError((error as Error).message)
		// option 2: type narrowing
         if (error instanceof Error) {
            setError(error.message)
         } 
      }
      setIsLoading(false)
    })()
  }, [])

  // as blogPosts can be undefined, I need to do this workaround ❌
  let content: ReactNode;
    
  if (blogPosts) {
    content = <BlogPosts posts={blogPosts}></BlogPosts>
  }

  if (isLoading) {
    content = <p>Loading...</p>
  }

  return (
    <>
      <h1>Data Fetching!</h1>
      {content}
      {error && <p>{error}</p>}
    </>
  );
}

export default App;
````



TODO: show casting

TODO: explain how to address the type of the error: casting or type narrowing with an if check

TODO: explain extra code added because data had an undefined initial state

#### Alternative: Using the "zod" Library for Response Data Validation

When fetching data, it can be a good idea to parse & validate the fetched data to check if it's in line with your data structure expectations.

A great library for doing that validation is the [Zod](https://zod.dev/) library because this library embraces TypeScript and is written such that TypeScript is able to infer the structure of the parsed / validated data.

I could create an entire course about Zod, but here's a very brief introduction.

When working with Zod (after installing it via `npm install zod`), your main task is to create a schema for the data you're trying to validate.

For example, when fetching blog posts, you would define the schema for a single blog post:

```tsx
import { z } from 'zod';

const rawDataBlogPostSchema = z.object({
  id: z.number(),
  userId: z.number(),
  title: z.string(),
  body: z.string(),
});
```

**Important:** This is JavaScript code! It will be compiled and will execute at runtime.

But, under the hood, it's written such that, during development, TypeScript is able to infer the type of the values that will be parsed / validated via that schema.

Speaking of that, here's how you would use this `rawDataBlogPostSchema` to validate a value:

```tsx
const parsedData = rawDataBlogPostSchema.parse(someData)
```

This will throw an error if `someData` is not in line with the defined schema (e.g., if a property is missing or of a different value type).

It will return the parsed data if validation succeeds.

The great thing is, that TypeScript now knows the type of `parsedData` => It will be the type you set up in your schema.

In this example, TypeScript would know that `parsedData` contains the properties `id` (number), `userId` (number), `title` (string) and `body` (string).

Therefore, even if `someData` was `any` or `unknown`, `parsedData` will be a known type.

When using Zod in the course demo app, you could therefore adjust the `App` component file like this:

```tsx
import { z } from 'zod';
// other imports ...

// outside of App component function (since this doesn't need to be re-created all the time)
const rawDataBlogPostSchema = z.object({
  id: z.number(),
  userId: z.number(),
  title: z.string(),
  body: z.string(),
});
// z.array() is a Zod method that creates a new schema based on another schema
// as the name suggests, it's simply an array containing the expected objects
const expectedResponseDataSchema = z.array(rawDataBlogPostSchema);

function App() {
  // other code like useState() etc ...

  useEffect(() => {
    async function fetchPosts() {
      setIsFetching(true);
      try {
        const data = await get(
          'https://jsonplaceholder.typicode.com/posts'
        );
        const parsedData = expectedResponseDataSchema.parse(data);
        // No more type casting via "as" needed!
        // Instead, here, TypeScript "knows" that parsedData will be an array
        // full with objects as defined by the above schema
        const blogPosts: BlogPost[] = parsedData.map((rawPost) => {
          return {
            id: rawPost.id,
            title: rawPost.title,
            text: rawPost.body,
          };
        });
        setFetchedPosts(blogPosts);
      } catch (error) {
        if (error instanceof Error) {
          setError(error.message);
        }
        // setError('Failed to fetch posts!');
      }

      setIsFetching(false);
    }

    fetchPosts();
  }, []);

  // other code ...
}
```

#### Alternative: A Generic "get" Function

As always, there are, of course, multiple ways of building the `get` function.

You could, for example, also build it as a generic function that accepts the expected return value type as a type argument:

```
export async function get<T>(url: string) {
  const response = await fetch(url);

  if (!response.ok) {
    throw new Error('Failed to fetch data.');
  }

  const data = await response.json() as unknown; 
  return data as T;
}
```

Now the *"Type Casting"* (*"Type Assertion"*) takes place right inside the get function to *"force"* TypeScript to treat data as type `T`.

`T` is then set when calling get:

```
const data = await get<RawDataBlogPost[]>(
  'https://jsonplaceholder.typicode.com/posts'
);
```

This allows you to use `get()` without having to cast the type to the expected value type.

It's of course up to you, whether you prefer this approach whether the approach shown in the videos.

##### **Level-up: Use with Zod**

You can also take this to the next level when using [Zod](https://zod.dev/) (see previous lecture).

You can adjust the `get` function to accept a second parameter that could be called `zodSchema` and should be a Zod schema object (of type `ZodType`).

This Zod schema can then be used inside the `get` function to parse the received response.

```
import { z } from 'zod';

export async function get<T>(url: string, zodSchema: z.ZodType<T>) {
  const response = await fetch(url);

  if (!response.ok) {
    throw new Error('Failed to fetch data.');
  }

  const data = (await response.json()) as unknown;

  try {
    return zodSchema.parse(data);
  } catch (error) {
    throw new Error('Invalid data received from server.');
  }
}
```

Since Zod would throw an error if parsing the data fails, TypeScript knows that if it succeeds, the data will be a value of the type defined by the Zod schema (i.e., TypeScript will narrow the type to be of that type).

Therefore, no more type casting is needed anywhere. Instead, in the place where `get()` should be called, you just need to define a Zod schema that describes the expected type and pass it to `get()`.

```
import { z } from 'zod';


const rawDataBlogPostSchema = z.object({
  id: z.number(),
  userId: z.number(),
  title: z.string(),
  body: z.string(),
});

const data = await get('https://jsonplaceholder.typicode.com/posts', z.array(rawDataBlogPostSchema));

data[0].userId; // works => TypeScript knows that userId will exist on the returned data
```



### Redux

in the reducers, I can mutate the state, as a new state object will be returned behind the scenes

when using useReducer, I need to return a new object manually



show usage of custom useReducer hook to add type safety when working with thunks
````
import { configureStore } from '@reduxjs/toolkit'

import { cartSlice } from '/cart' 
````



TODO: clarify getting typeof a js value and use in TS

