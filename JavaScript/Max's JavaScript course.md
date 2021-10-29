# Max's JavaScript course

## Getting started

### Prettier

Add Prettier VScode extension first and use a shortcut to make code look better:

To check the shortcut, go to `Code -> Preferences -> Shortcuts` and then type `format document`. Then check whis is the shortcut. It should be:

`option + shift + F`

### VSCode settings

under `code -> preferences -> settings`, there are `User` (applied to all projects) and `Workspace` applied to the a spesific project.

The `.editorconfig` file in the project is the one managing the workspace settings

### Using incognito mode on VSCode

it helps preventing extensions running extra code on our apps

## Basics: Variables, Data Types, Operators & Functions

how to add JS code to an `html` file?

1) ```html
   <head>
    <script>
        alert('hey, I'm an alert!)
    </script>
   </head>
   ```

the downside: the html gets long an messy with inline JS.

2) ```html
   <head>
       <script src="path-to-the-JS-file"> </script >
   </head>
   ```

IMPORTANT: self closing `<script />` tag is not supported.

IMPORTANT: the above ways, are **BLOCKING** the loading of the HTML. In our case, the html doesnt load till we click on the alert 😯

3) Add the script to the end of the `<body>`:
   
   ```html
   <body>
       <script src="path-to-the-JS-file"> </script >
   </body>
   ```

Contact Max, on Chrome, this 3rd option still blocks the html, but it doesnt block it on Firefox.

### The order if JS imports matter

```html
<body>

// this line imports JS that define variables that are DOM elements
<script src="assets/scripts/vendor.js"></script>

// this code will use those DOM elements
<script src="assets/scripts/app.js"></script>


</body>
```

### Declaring variables

A variable can be declared, but not defined, so it becomes a **uninitialized variable**:

```js
let currentResult;
```

and the value is `undefined`.

### Naming variables:

allowed special characters:

1) number, but not at the begining. `let sprint1`, 

2) underscore: `user_name`. Snake case not recommended. Names can still start with `_`

3) `$` like, `let $posts`, like when using observables in Angular;

Don't do:

`let let`, becaus how will JS know which `let` you wanna use in the following piece of code XD.

### 

### Semicolons

are optional in JS, but required in this use case (two expressions in one line):

```js
let variableA; let variableB;
```

### Operators

styntax features that let us manipulate values

#### Mathematical operators

the not known by me are:

1) `%` that returns the remainder of dividing two numbers. 

2) `**`Exponentiation, `2**3=8`

The operators return things, e.g `+` like the SUM result of two number

#### Assign operator

it's just `=`, we use in the variable definitions, e.g.

### Data types

numbers with decimals are called `floating numbers` like this one: 3.1416

### String concatenation

adding dinamyc values to strings:

```js
let fullName = 'Esteban ' + surname + ' some extra random surname'
```

the `+` operator here doesn't SUM numbers but it joins strings into a single one. Downside? messy syntax. It's better to do **strint interpolation**.

### From top to bottom

the JS code is **Parsed, compiled and executed** from **TOP to BOTTOM**

### Challenge: print string quoutes on html

target: print `'('`to print a string, **using just single quotes** at the beginning of the string inside **won't work**, i

```js
let calculationDescription = ''('' 
```

using consecutive single quites is the same as having `empty string` `(` and `empty string` , and JS doesn't know what `(` does.

The solution? combining 2 types of quotes:

```js
let calculationDescription = "'('"
```

The above does print the single quote string on the html, yes!

### Template literals or string interpolation

is the solution to string concatenation, wich is ugly.

```js
let greeting = `Welcome onboard ${userName}`
```

### Writting strings into multiple lines:

#### with string interpolation:

```js
let greeting = `Hello


this is multiline string`
```

in the browser, it looks like this:

`Hello this is a multiline string`

The browser decides, for an `h2` for example, to omit the white spaces, but if we add:

```css
white-space: pre;
```

that will show the string with the white spaces on the rendered html!😯

White spaces and tab spaces ARE part of the string (and those spaces are taken out by CSS in browser by default, but they are there if CSS is not applied.)

Conclusion: be careful when splitting lines, the breakes and spaces are part of the string

#### With single or double quotes

```js
let greeting = 'Hello ' +
                'this is multiline string'
```

White spaces and tab spaces are not taking into account, because there's a calculation`+` in between. 

So, how to add line breakes and spaces then?

```js
let greeting = 'Hello \n
                this is a multiline string'
```

this `/n` can also be used for backticks, but it's not used too much because the backticks allow line splitting in the code already

The `\` character escapes the next one, that means that then next one is not treated ,e.g, as a normal `n`, but is combined with with the `\` to form a `line break`

The `print string challenge` could have also be solved by escaping:

```js
let greeting = '\'Hello\''
```

the `\'` prints a single quote :0

More escape sequences here: [String - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Escape_notation)

How to add a tab space? `\t`.

### Functions

functions are code on demand.

```js
function addNumbers (num1, num2) {
    //do something here
    return something
}
```

we have the function keyword, the function name, the parameters, and the body of the function inside the {}.

#### IMPORTANT: usually no `;` is added after the the funcion `}` closing curly brace.

`num1` variable is defined inside the function body of the function, which is a block, but can't be used outside the function.

There are built in functions when the JS runs on the browser, thanks to the `window` objetc:

```js
alert('Hey');
//or
window.alert('Hey')
```

Adding a return statement, makes the function more usable, because we can store the returned value on a variable:

```js
const result = addNumbers(1,2);
```

`1-2` the `-` operator returns the value which is the deduction of them. Well, that's the effect of the `return` inside the function, we can then use the returned value! wohoo!

### The order in code matters

variables and constants should be declared (not neccessarly defined, with a value, or, in other words, initialize it) before using them.

```js
let greeting;

console.log(greeting); // doesn't break the code, prints undefined
```

on the other hand, having functions declared with the keyword `function` can be place anywhere in the code, because the browser takes them to the top after parsing the JS code. This is called hoisting.

```js
addNumbers (1,2); //works thanks to hoisting


function addNumber(num1, num2) {
    return num1 + num2;
}
```

IMPORTANT: functions not using the above syntax are not hoisted 😬

### Pure function

```js
// a pure function
function addNumber(num1, num2) {
    return num1 + num2;
}
```

it gets inputs, returns outputs. With the same intputs, it returns the same outputs.And doesn't change global scope variables:

```js
let globalvariable;

// not a pure function, CRAP CODE
function addNumber(num1, num2) {
    globalVariable = num1 + num3
}
```

Have as fewer global variables as possible.

### Local or block scope variables

```js
function addNumber(num1, num2) {
    const result = num1 + num2;
    return result;
}

// even calling addNumber(1,2) here doesn't make the result variable available outside the function

console.log(result); // will throw an error "result is not defined"
```

### Shadow variables

it's a local variable, declared in a function, that has the same name as an already declared global variable.

```js
let userName = 'Max';
function greetUser(name) {
  let userName = name;
  alert(userName);
}
userName = 'Manu';
greetUser('Max');
```

the local `userName` variable inside the funtion is `shadowed`, so it doesn't overwrite the global variable, it's a different variable than the global one, named the same.

### The return statement:

the `return` statement finish the function execution.

```js
function getGreeting() {
    return 'Hello there!'
    // code below doesn't execute
    alert("I'll never be executed :(");
}
```

and empty `return` statement can be used to quit a function conditionally (early return);

the above code doesn't throw an error

### Functions without return statament

They're used to manipulate global variables:

```js
function add() {
    currentResult = currentResult + userInput.value;
}
```

How to escape the VSCode autocompletion? by pressing ESC

### Adding a name of a function

doesn't do anything to the code:

```js
function add() {
    ...
}

add

//the above add is ignored by JS, it's just the name
// of a function, without instructions
```

### What are parameters in functions?

are inputs for the function, that are available just inside the function, like local variables.

### Input values are always strings

if you get a number from a form, the value is a string, so it's needs to be converted to a number:

```js
// many ways to do it!
let userInput = +someInput.value
let userInput = parseFloat(someInput.value);
// this parseInt only takes the integer, not the decimals of the stringified number
let userInput = parseInt(someInput.value);
```

`parseFloat` and `parseInt` are built in JS methods.

### Mixing Numbers & Strings

If we give JS a string and number in a sum, it will treat the number as a string, and create a long string:

```js
let sum = 1 + someInput.value // it will return a string 120 e.g,
//if the input is 20

3 + '3' // prints 33 as a string

3 - '3' // prints 0 as number
3 * '3' // prints 9 as number
3 / '3' // prints 1 as number
```

You saw the example with a number and a "text number" being added

`3 + '3'` => `'33'`

in JavaScript.

That happens because the `+` operator also supports strings (for string concatenation).

It's the only arithmetic operator that supports strings though. For example, this will not work:

`'hi' - 'i'` => `NaN`

`NaN` is covered a little later, the core takeaway is that you can't generate a string of 'h' with the above code. Only `+` supports both strings and numbers.

Thankfully, JavaScript is pretty smart and therefore is actually able to handle this code:

`3 * '3'` => `9`

Please note: It yields the number (!) `9`, NOT a string `'9'`!

Similarly, these operations also all work:

`3 - '3'` => `0`

`3 / '3'` => `1`

Just `3 + '3'` yields `'33'` because here JavaScript uses the *"I can combine text"* mode of the `+` operator and generates a string instead of a number.

### How to change number to string?

```js
someNumber.toString();
```

Using numbers inside a literal string, has `.toString()` applied to those numbers under the hood.

```js
const calcDescription = `${currentResult} + ${userInput.value}`
// it has currentResult.toString() under the hood
```

## 

### Comments

```js
// this is a comment

/* this is a block comment
that can use multiple lines!*/
```

### More operators

```js
currentResult = currentResult + enteredNumber;

// SAME AS

currentResult += enteredNumber
// it also works for -, * and / operators
```

```js
currentResult = currentResult + 1

// SAME AS

currentResult++

// it also works for -
currentResult--

// the ++ is Increment operator
// the -- is Decrement operator

//they return the value of the value before change
```

```js
++currentResult // returns the modified value
```

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

The  `function parameter types` are NOT infered from usage (e.g when calling them), but when we define the function.

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

As seen above, there's no specific types we need to add when calling `useEffect`. The definition doesn't have useState<T> associated with it.

Type inferance is done thank to <T>.

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
