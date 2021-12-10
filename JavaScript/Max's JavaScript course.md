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

### Arrays

accessing inexisting elements in an array returns `undefined`. Example:

```js
const logEntries = ['hola'];

console.log(logEntries[1]) // returns undefined
```

### Console.log(myArray)

Funny facts: the order of the keys an an object may change, to be ****sorted alphabetically****:

```js
0:
 currentResult: 2
 enteredNumber: 2
 initialResult: 0
 operator: "ADD"
 [[Prototype]]: Object
length: 1
[[Prototype]]: Array(0)
```

### Accessing keys or properties in objects

what are objects? gruped data, structured in key-value pairs. eg 

```js
{
    user: "esteban", 
    surname: "munch jones"
}
```

```js
logEntry.operator // prints ADD
```

### Null / undefined / NaN

`null` and `undefined` are `data types` (like objects, and strings) and `values` at the same time.

#### Undefined

`undefined` is the default value `uninitialized` variables have.

```js
let someVariable; 
console.log(someVariable) // prints undefined


let someArray = [1];
console.log(someArray[3]); // prints undefined
```

Never assign `undefined` manually to a variable!!

#### Null

is **never** assumed by default, and is assigned manually to reset a variable

```js
userName = 'Esteban'

// the form was submitted
// instead of assigning an empty string, null is assigned
userName = null
```

`undefined & null` are great for managing empty data, either because it hasn't been set, or it has been reseted.

#### NaN

is NOT a `type` (is of type `number`) and can be used in calculations, is kind of an error code we get from a calculation (e.g multiplication of strings);

```js
console.log(1/'a'); // prints NaN
```

### Checking types: `typeof`

 wanna check the `type` of a variable at runtime? it returns a string with the type.

```js
typeof 'hola' // prints "string"
typeof 1 // prints "number"
```

playing with `typeof`:

```js
typeof undefined; // prints "undefined""
typeof null; // prints "object"
typeof NaN // print "number"


typeof {name: "tebi"} // prints "object"
typeof [1,2] // prints "object"
```

### Async and defer

If `JS` code depends on `HTML` (like accessing DOM elements), the `JS` files can be imported at the end of the `<body>`:

```html
   </body>
   SOME HTML HERE

    <script src="assets/scripts/vendor.js"></script>
    <script src="assets/scripts/app.js"></script>
  </body>
```

BUT, there's a performace issue, that can be checked going to `Incognito chrome tab` -> `inspect` -> `performance` -> `start recording` -> `check the timeline` -> `expand Network part in the zoom in the middle window`.

Then look at `Main` in the lower window, to see events:
The HTML is finished parsed, and then after some miliseconds, the `Scripts` are Evaluated. We could do better.

The key:

###### I'd like to `start downloading the scripts` at the same time as `Parsing the HTML`, but executing the `Scripts` after the `HTML Parsing` has been finished.

Option 1: (not recommended)

```html
<head>
  <script src="assets/scripts/vendor.js"></script>
  <script src="assets/scripts/app.js"></script>   
</head>
<body>
</body>
```

It throws an error because the `JS` code targets DOM elements not yet parsed.

```bash
Uncaught ReferenceError: addBtn is not defined
    at app.js:59
```

The download of the `JS` files blocks (or stops) the HTML parsing.

### Defer attribute: when the JS targets the DOM

it tells the browser to download the `JS` files straight away, no blocking the HTML parsing, and execute the `JS` after the HTML has been parsed.

```html
<head>
  <script src="assets/scripts/vendor.js" defer></script>
  <script src="assets/scripts/app.js" defer></script>   
</head>
<body>
</body>
```

The time difference between the end of the HTML parse and the `Script execution` is much shorter than when importing the scripts at the end of the `<body>`.

With `defer` the scripts execution follows the order they're listed on the HTML.

### Async attribute: when the JS doesn't target the DOM

when the  `JS` doesnt target the DOM, why bothering waiting for the HTML parse to finish? it just executes the `JS` when the download finished.

```html
<head>
  <script src="assets/scripts/vendor.js" async></script>
  <script src="assets/scripts/app.js" async></script>   
</head>
<body>
</body>
```

when using `async` the order of the scripts executions depends on which one gets downloaded first.

`async & defer` doesn't have any effect when used here:

```html
//No effect!
<script defer or async>
 alert('hello world')
</script>
```

because there's no such `JS` file to download.

```html
// DONT combine inline JS and src
// the inline script will be ignored!
<script src="...">
 alert('hello world')
</script>
```

## Debugging and efficient development

### VSCode shorcuts.

Go to `Code` -> `Preferences` -> `Keyboard Shortcuts` and look for command names (Add selection to next bind match) and keybindings (cmd D)

### Autocompletion

if the function name autocompletion menu has been exited by pressing `ESC`, it can be re-opened with `ctrl + space`.

if the parameters hint has been escaped, it can be reopened with `Trigger Parameter Hints`, `shift + cmd + space`.

### VSCode config

If we go to `Settings` -> `Workspace`, and then change the `Tab width` to 4, we get a new hidden file created:

```json
// .vscode/settings.json

{
    "prettier.tabWidth": 4
}
```

The `User` config keeps the settings on a global file in the machine.

### Chrome debugger tools

#### Breakpoints

After hitting a `debugger` keyword in the code or `breakpoint` applied on the Chrome dev tools, there are buttons:

- Play button: Resume script execution

- Step over next function call. (kind of go the next line):
  
  ```js
   debugger;
   const enteredNumber = getUserInput();
   // will jump to this line
   currentResult += enteredNumber;
  ```

- down arrow: Step into next function call:

Will get you inside this function:

```js
function getUserInput() {
  return parseInt(userInput.value); //on this line
}
```

How to get out of that function? press the up arrow

- up arrow: set out of current function

- The right arrow with the point: Step. It's a combination of `Step Over next function call` and `down arrow`.

#### Conditional breakpoints

You can right click `Edit breakpoint` and add a condition for it's triggered.

```js
getUserInput() > 100
```

#### Brekpoints on events

What if I wanna stop JS execution and debbug whenever a registered event for clicks happens in the app? e.g when clicking add, deduct, didive and multiply?
would be cumbersome to add 4 breakpoint manually.

The most efficient way is to use `Event listeners breakpoint` and select `click`.

### The callStack

Is a list of functions that are being called. You can click on the functions and jump back and forth.

The bottom funtion on the list is the most outern function on the code.

### Scope

On `Scope` we have variables values, pretty neat!

Variable values can be changed!

### Watch

You can also `Watch` variables or expression using variables, like:

`enteredNumber` + `currentResult` at all times.

### Editing source

Code can be edited in source, but it needs to be saved with `cmd + S` to be tested. Faster than opening VSCode.

## Debugging inside VSCode

1) You click next to a line of code, and the red dot is active

2) Run -> Start debugging

3) ```json
   `launch.json` is created automatically:
   ```
   
   {
   
       // Use IntelliSense to learn about possible attributes.
       // Hover to view descriptions of existing attributes.
       // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
       "version": "0.2.0",
       "configurations": [
           {
               "type": "pwa-chrome",
               "request": "launch",
               "name": "Launch Chrome against localhost",
               "url": "http://localhost:8080",
               "webRoot": "${workspaceFolder}"
           }
       ]
   
   }

```
4) Edit the file by replacing the `url`:

```json
 "url": "file:///Users/estebanmunchjones/Documents/Coding/courses/JavaScript/calculator/basics-01-starting-project/index.html"
```

   Just copy and paste from the browser url bar where the app was running

5) On the left, there will be the same info as inside the browser (variables, etc), and next to the `Terminal` there's a `Debug Console` where variables and expressions can be run, excelent! (e.g: print the value of `currentResult`).

6) Press red square to stop debugging, that will close the Chrome window opened.

## Control structures

### if blocks

```js
if (condition1) {
    //do something
} else if (condition2) {
    // do something
} else {
    // do something else
}
```

conditions evaluate to true or false.

In order to get this booleans values, `boolean operators` are used:

1) `==` and `!=` equal and un equal operators, they check value. e.g: `3 == '3'` return `true`

2) `===` and `!===` strict equan and un equal operators. They check value and type. e.g `3 === '3'` returns `false`.

3) `> >= ` and `< <=` for numbers and strings

4) `!` not True operator

### Danger!: Comparing objects and Arrays

```javascript
{name: "Esteban"} == {name: "Esteban"} // returns false
[1,2] == [1,2] // returns false
```

event the content is the same, they're different pointers (not copies, like `strings` and `numbers`).

### Early return

it's better to have early return, so the main logic is not nested in an `if` block:

```javascript
// early return
const someFunction() {
    if (conditio I don't want) {
        return
    }

//rest of the logic
}
```

```javascript
// nested main code here, bad practice
const someFunction() {
    if (condition I want) {
        //rest of the logic
    }
}
```

### Operators precedence

Which operators are executed first by JS in the same line?

```javascript
3 + 2 < 7 + 20 //returns true

// + operator has precedence over < operator        
```

check out the list of operators precedence here: [Operator precedence - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

```javascript
true && false || true // returns true.
// && has precence (executed first) over ||
```

### Falsy an truthy values

if non-bolean values are used as a condition, JS coerces ("convert without really converting") them to booleans.

```javascript
if(""){// doesn't get executed} // empty string evaluates to false
if("Hola"){// gets executed} // non-empty strings evaluate to true


if(0){//doesn't get executed}
if(-1){//gets executed} // numbers != 0 evaluate to true

if({}){// all objects are truthy}
if([]){// all arrays are truthy}

if(null) if(undefined) if(Nan){//all are falsy}
```

### Hardcoded values convention

```javascript
const ATTACK_VALUE = 10;
```

capital letters and underscores.

### Event listeners naming conventions

```javascript
function attackHandler() {}
// OR
function onAttack(){}

attackBtn.addEventListener('click', attackHandler);

attackBtn.addEventListener('click', onAttack);
```
