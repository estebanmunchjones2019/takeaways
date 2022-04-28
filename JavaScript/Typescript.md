## Typescript

installation:

```bash
sudo npm i -g typescript   
```

Then write a `.ts` file and run the compiler

```bash
tsc app.ts
```

and an `app.js` file will be create with the compiled code.

VSCode has built in `TS` support, so when we're writting code in `TS`, it gets parsed and error like: `Argument of type 'string' is not assignable to parameter of type 'number'.` are thrown while we type, neat!

`tsc` compiles to old `JS` by default, (e.g uses `var`), but that can be changed.

If there's a `TS` error, when running `tsc <file-name>` will throw an error but will still generate the `JS` file (with the bug there).

If when working with the `TS` file we see an error like: `Duplicate function implementation.` it means that the same function is present in the created `JS` file by the compiler, and we need to close that `JS` file to fix the error.

### Assigning types

```js
function add(num1: number, num2: number ){
    return a + b
}

// `add: number` wasn't necessary, was infered via the return statement
// hovering over add on VSCode, we see 
// function add(a: number, b:number): number
```

when declaring the funcion, we assign types to the parameters.

```js
const result = add(3,5);
// no need to assign types to `result`, it's infered
```

The above is possible thanks to `TS` built in `type infereance` feature.

**Bad Practice**: assign types when not required, like:

```js
const result: number = add(3,5); //BAD PRACTICE
```

### Constant and variable types

```js
// on constants
const isDone = true; // hovering over isDone, const isDone = true

// but on variables
let isDone = true; // hovering over isDone, let isDone: boolean
// because isDone come become false later on.
```

### Custom types

besides `number` `boolean` etc, we can have custom types, like `function types`;

A `Function type` is made up of these 2 parts: `parameters` and `return` types.

```js
function add(num1: number, num2: number):number
```

If there are no `return` statement:

```js
function printResult(result) {
    console.log(result);
}

// hovering over printResult, function printResult(result: any): void
```

Si, `void` & `any` are types only present in `TS`.

In `JS` a funtion that doesn't have a returned value returns `undefined`, but we can't add `undefined` when assigning return types in a function:

BAD PRACTICE + GETTING an ERROR 👇:

```js
function printResult(result): undefined {
  console.log(result);
}
// I get `undefined` underlined because:
// A function whose declared type is neither 'void' nor 'any' 
// must return a value.
```

The `function parameter types` are NOT infered from usage (e.g when calling them), but when we define the function.

### Type any: the last resort

is having a seat belt and not using it.

### Overriding type inferance with Type Casting

```js
cont num1 = num1Input.value // I get `value` underlined, but why!!?
```

that error makes `TS` to assign the type `any` to `num1`, but I want `TS` to tell me that the `value` is a string, and then yell at me that the `add` function's param `num1` should be a number instead.

The problem lays down on:

```js
const num1Input = document.getElementById("num1");
// hover over num1Input
// const num1Input: HTMLElement
```

So not all `HTMLElement` s have a value `property`.

let's cast now to fix it!

HOW NOT to do it:

```js
// DOESNT WORK
const num1Input: HTMLInputElement = document.getElementById("num1");
// the above doesn't change the default type `HTMLElement` 
// returned by getElementById



// the good way, `as` syntax
const num1Input = document.getElementById("num1") as HTMLInputElement;


// alternative syntax
const num2Input = <HTMLInputElement>document.getElementById("num2");
```

We could think that having `<input type="number">` makes the `.value` a number, but it doesn't work, and casting is needed.

### Objects

```js
// WRONG WAY
  const resultContainer: object = {
    res: result
  }
  console.log(resultContainer.res); // .res is outlined because
// Property 'res' does not exist on type 'object'.
```

```js
// RIGHT WAY (altough not needed because of inferance)
  const resultContainer: { res : number} = {
    res: result
  }
  console.log(resultContainer.res);
```

### Arrays

```js
// if an array is initialized as an empty array (or no array at all)
// so I could push a number or a string to it. Not very safe
let results = [];

// how to fix it?
let results: number[] = [];

results.push('foo'); // will throw a TS error: 
// `Argument of type 'string' is not assignable to parameter
// of type 'number'.`
```

### Methods inside objects?

```js
const resultContainerObject = {
  res: 1,
  print() {
    console.log(this.res)
  }
}

// the resultContainerObject type is
// const resultContainerObject: {
//    res: number;
 //   print(): void;    or print: () => void;
// }
```

```js
let results: {res: number}[] = [];
results.push({
    res: 1,
    print() {
       console.log(this.res)
    }
})

// doesn't throw an error because TS is happy that 
// the object pushed has AT LEAST that res: number property

// COULDNT VERIFY this when re-reading these notes
```

```js
// re-defining results type, example
let results: {res: number, print: (a: number) => void}[] = [];

// then I can .print without TS errors
results[0].print();
```

### Type alias: makes typing easier

Are your types long, or you wanna re-use them?

```js
type CalculationResult = { res : number, print: () => void};

let results: CalculationResult[] = [];
```

just use the `TS` keyword `type`.

### Literal types: being more specific

what if we expect a parameter `string` to have only 2 possible values? like `console` or `alert`?

```js
function printResult(result: number, printMode: 'console' | 'alert') {
  if( printMode === 'console') {
    console.log(result);
  } else if (printMode === 'alert') {
    alert(result)
  }
```

the strings `console` and `alert` are `literal types`.

### Union types

a `union type` is `'console' | 'alert'` , so it's a union of 2 `types (literal or not)` with a `|` pipe operator. Another example is `string | number`

Benefit: it's more secure, and provides better autocompletion when calling `printResult()`.

spicy trick: `union types` can also be defined as `alias types`:

```js
type PrintMode = 'console' | 'alert';
```

### Enum: an object with the option inside

```js
enum OutputMode {CONSOLE, ALERT};

function printResult(result: number, printMode: OutputMode) {
  if( printMode === OutputMode.CONSOLE) {
    console.log(result);
  } else if (printMode === OutputMode.ALERT) {
    alert(result)
  }
}


printResult(result, OutputMode.ALERT);
```

behind the scenes, the enum options are stored as number: `0`, `1`, and so on.

### Classes

we can write `classes` in`TS` and then it gets compiled to constructor functions in `JS`.

```js
// JS way of writing classes
class User{
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}


// TS way of writing classes
// it needs properties as FIELDS
class User{
  name: string;
  age: number
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}


// It gets compiled to this Constructor Function
var User = /** @class */ (function () {
    function User(name, age) {
        this.name = name;
        this.age = age;
    }
    return User;
}());
```

### Adding some privacy

```ts
// TS way of writing classes
// it needs properties as FIELDS
class User{
  name: string;
  private age: number
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}


const user1 = new User('tebi', 32);

user1.age; // age is underlined with this error:
// Property 'age' is private and only accessible within class 'User'.
```

`public` is default, so you don't need to add it.

### Avoiding FIELDS shortcut: the constructor Angular syntax

```ts
class User {
  constructor(public name: string, private age: number) {}
}
```

This way, parameters get converted into properties automatically! 🚀

Remember the good old Angular days when injecting services:

`constructor(private wordpressService: WordPressService)`;

### Class inheritance syntax

```ts
class Admin extends User {
  constructor(name: string, age: number, public peopleInCharge: number){
    super(name, age);
  }
}
```

Note that on the constructor, `name` and `age` are just forwarded to super, and don't use `private` or `public` keywords.

### Interfaces

They can be used for adding types to `objects created manually` (not using classes):

They do a similar job as `type aliases`:

```ts
// TYPE ALIAS
type CalculationResult = { res : number, print: () => void};

//INTERFACE
interface CalculationResult { res : number, print: () => void};


const result: CalculationResult = [{res: 3}];
// (using the interface up there throws no error,
// since it's not a contract, e.g, print can be missing)
```

Interfaces have the advantage that can be used as CONTRACTS in classes. If there's a missing property or method in the object, `TS` will complain. wow!

```ts
class Calculation implements CalculationResultsInt {
  constructor(public res: number){}
}
// here, the interface is implemented as a contract
// Calculation will be underlined with the following error
// Class 'Calculation' incorrectly implements interface 'CalculationResultsInt'.
//  Property 'print' is missing in type 'Calculation' but required in type 'CalculationResultsInt'.
```

A class can have multiple contracts with `Interfaces`:

```ts
class Calculation implements CalculationResultsInt, SomeOtherInt {
  constructor(public res: number){}
}
```

What's the main value of `Interfaces`? to re-use them in different classes definitions!

### Generic Types

```ts
// syntatic suggar here:
let results: CalculationResultsInt[] = [];

// can also be written like this, with the Array generic
let results: Array<CalculationResultsInt> = [];
```

```ts
let results: Array = []; // Array is underlined with the following error
// `Generic type 'Array<T>' requires 1 type argument(s).`
```

Generic types don't work on their own, they expect and argument, inside the `<>` brackets.

`Promise` is another generic, because the promise resolves to something.

### Custom generics

```ts
// this has no autocompletion for .split()
function logAndEcho(val: any) {
  console.log(val);
  return val;
}

logAndEcho('Hello there').split(' '); // because the return value of

// the function is any
```

But, we can be more specific, and create a `custom generic`, so we can add a type when calling the function, and get autocompletion.

```ts
// T or whatever name, is just a reference
function logAndEcho<T>(val: T) {
  console.log(val);
  return val;
}

const splitteDString = logAndEcho('Hello there').split(' ');
// now split has been autocompleted!
```

### Configuring the TS compiler

```bash
tsc app.ts -w   // for watch, to it recompiles upon changes
```

but, how to compile multiple files at the same time?

```bash
tsc app.ts
tsc otherFile.ts
// cumbersome
```

```bash
tsc --init   
```

that adds a `tsconfig.js` to the root level of the project.

now, I can just run this to compile all `.ts` files:

```bash
tsc
// or
tsc -w
```

We might get new errors, because the `tsconfig`

```js
 /* Type Checking */
    "strict": true,  
```

for example, it checks for possible `null` values.

```ts
const button = document.querySelector("button");
// button is of type
// const button: HTMLButtonElement | null
```

Solution?

```ts
//  optional chaining operator (?.) 
button?.addEventListener()

// or add an if check 
if (button) {button?.addEventListener()}

// or add an !, to tell TS: this will never be null
const button = document.querySelector("button")!;
```

Note: The `?.` operator is like the `.` chaining operator, except that instead of causing an error if a reference is [nullish](https://developer.mozilla.org/en-US/docs/Glossary/Nullish) ([`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null) or [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)), the expression short-circuits with a return value of `undefined`.

## TS with Hooks

[Mastering React Hooks with Typescript - YouTube](https://www.youtube.com/watch?v=zM_ZiSl2n2E&ab_channel=JackHerrington)

Without types, TS infers a `never` type:

### UseState

#### Avoiding the `Never` type:

```ts
const [arr, setArr] = useState([]); // TS says: never[]


setArr([...arr, arr.length + 1])

// The error: const arr: never[]. 
// Type 'number' is not assignable to type 'never'.
```

And then, I get this TS error when trying to modify the array:

How to fix it?

```ts
const [arr, setArr] = useState<number[]>([]);
```

#### Starting state with `null`:

```ts
const [name, setName] = useState(null);
setName('Jack');

// the error: Argument of type '"Jack"' is 
// not assignable to parameter of type 'SetStateAction<null>'.
```

how to fix it?

```ts
const [name, setName] = useState<string | null>(null);
```

### useEffect

1st bug: we have a closure here, `val` gets stuck at 1 inside the `setInterval` callback function:

```ts
useEffect(() => {
        window.setInterval(() => {
            setVal(val + 1);
        }, 1000)
    }, []);

<div>{val}</div> // it prints 1, 2, 2, 2....
```

how to fix it?

```ts
 useEffect(() => {
        window.setInterval(() => {
            setVal((v) => v + 1);
        }, 1000)
    }, []);
```

passing a function to `setVal` to grab the current value, and avoid the fix value from the closure.

2nd bug: the setInterval never gets cleared, so after unmouting, it keeps firing.

HOT TIP: hover over `useEffect` and right click and `Go to definition` to the see how the function is defined, with its types.

As seen above, there's no specific types we need to add when calling `useEffect`. The definition doesn't have useState associated with it.

Type inferance is done thank to .

```ts
// definition inside node_modules
function useEffect(effect: EffectCallback, deps?: DependencyList): void;
```

On the other hand, `useState`:

```ts
//definition inside node_modules
function useState<S>(initialState: S | (() => S)): [S, Dispatch<SetStateAction<S>>];
```

#### createContext

type inferance works on `createContext`

```ts
const intialState = {
    name: "Esteban",
    surname: "Munch Jones"
}

const context = createContext(intialState);
```

but we can even pass a `type`:

```ts
const context = createContext<typeof intialState>(intialState);
```

### useContext

the type is also inferred:

```ts
const user = useContext(UserContext);
// user has type: const user: {name: string; surname: string;}
```

but can also be overwritten:

```ts
const user = useContext<UserState>(UserContext);
```

### useRef

```ts
const inputRef = useRef<HTMLInputElement | null>(null);
```

It's usefull to be specific about the HTML type.
