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

⚠️ IMPORTANT: self closing `<script />` tag is not supported.

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

the `+` operator here doesn't SUM numbers but it joins strings into a single one. Downside? messy syntax. It's better to do **string interpolation**.

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

is the solution to string concatenation, which is ugly.

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

#### IMPORTANT: usually no `;` is added after the the funcion `}` closing curly brace, in function declarations, but it's added to function expressions (functions saved in constants)

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

variables and constants should be declared (not neccessarly defined, with a value, or, in other words, initialized) before using them.

```js
let greeting;

console.log(greeting); // doesn't break the code, prints undefined
```

on the other hand, having functions declared with the keyword `function` can be placed anywhere in the code, because the browser takes them to the top after parsing the JS code. This is called **hoisting**.

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

// even calling addNumber(1,2) here doesn't make the `result` variable available outside the function

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
greetUser('Max'); // will still greet Max
```

the local `userName` variable inside the funtion is `shadowed`, so it doesn't overwrite the global variable, it's a different variable than the global one, named the same, inside the function block.

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

### Input values from forms are always strings

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
// it has currentResult.toString() under the hood 😮
```



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

Funny facts: the order of the keys an an object may change, to be **sorted alphabetically** in the dev tools:

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

⚠️ Never assign `undefined` manually to a variable!!

#### Null

is **never** assumed by default, and is assigned manually to reset a variable

```js
userName = 'Esteban'

// the form was submitted
// instead of re-assigning an empty string, null is assigned
userName = null
```

#### 💡 `undefined & null` are great for managing empty data, either because it hasn't been set, or it has been reseted.

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

### Async and defer: never blocks HTML parsing 😉

#### ⚠️ make sure you start downloading your scripts in the head!

Keep this word in mind: **HTML parsing**

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

⚠️ It throws an error because the `JS` code targets DOM elements not yet parsed.

```bash
Uncaught ReferenceError: addBtn is not defined
    at app.js:59
```

The download of the `JS` files blocks (or stops) the HTML parsing.

### Defer attribute: when the JS targets the DOM

Download JS -> execute it when the HTML parsing finished.

it tells the browser to download the `JS` files straight away, no blocking the **HTML parsing**, and execute the `JS` after the HTML has been parsed.

```html
<head>
  <script src="assets/scripts/vendor.js" defer></script>
  <script src="assets/scripts/app.js" defer></script>   
</head>
<body>
</body>
```

💡The time difference between the end of the HTML parse and the `Script execution` is much shorter than when importing the scripts at the end of the `<body>`.

With `defer` the scripts execution follows the order they're listed on the HTML.

### Async attribute: when the JS doesn't target the DOM

Download JS -> execute when finished downloading

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

⚠️`async & defer` doesn't have any effect when used here:

```html
//No effect!
<script defer or async>
 alert('hello world')
</script>
```

because there's no such `JS` file to download.😅

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

- down arrow: Step into next function call (chain of function calls)

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
    if (condition I don't want) {
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

### Putting re-used strings constants into variables

```js
const ATTACK = "ATTACK";

function attackHandler(){
   attack(ATTACK);
}

function attack(mode) {
    let attackMonsterValue;
    if (mode === ATTACK) {...
```

write the string once, and use the variable instead, smart!

### Declaring a constant with 2 possible values

It's a great scenario for a `ternary operator`:

```js
const userName = isLoggedIn ? 'Max' : null;
```

use for simple stuff, not nesting it, because gets unreadable.

### Expression vs Statement

```js
// expression - used on the right hand side of assignment operators
// it returns something
isLoggedIn ? 'Max' : null;

//statement. It CAN'T be used on the right hand side of assignment operators
const username = const somethingelse ❌

// an if STATEMENT is an example
```

### Boolean tricks with logical operators

**Double bang** `!!` to convert truthy/falsy values to true/false:

```js
!!"" // return false
```

**Default value** assigment via OR || operator:

```js
const userName = someInput || 'Max'; // the || operator will return the 
// first truthy value
```

**double &&**:

```js
const userName = isLoggedIn && 'Max' 
// will assign 'Max if isLoggedIn is truish
// and will assign the isLoggedIn value if it's falsis
```

the double && is used a lot in JSX in React to conditionally render views, is a minimalistic version of the ternary expression, that just have a return value for a truthy condition, but none for falsy.

### Switch statement

```js
if (location === 'Argentina'){
    console.log('hola')
} else if (location === 'UK'){
    console.log('hello');
} else if //
```

the above code repeats the equality expression, so `switch` statement is a more compact alternative:

```js
switch(location) { // location or and expresion that returns a value
    case 'argentina':
        console.log('hola');
    break;
    case 'UK': {
       console.log('hello');
    }
    break;
}
```

`switch` uses `===` under the hood.

add `break` keyword, because `switch` statements use `fall through` mechanism, and it doesn't stop till the end.

### For loop

```js
for (let i = 0; i < 10; i++){
    //do something
}
```

`let i = 10` is code that runs at the beginning, and `let` has been chosen in order to reassign it.

`i < 10` is a condition that gets evaluated on every iteration;

`i++` is a code that runs after every iteration.

### For of (arrays)

```js
 for (const el of log) {
    console.log(el);
 }

 // const is used because el is recreated on every iteration
```

The above syntax simplifies this loop:

```js
  for (let i = 0; i < log.length; i++) {
        console.log(log[i]);
    }
```

variables defined inside the `for` parenthesis, are scoped to the loop.

Do you want to keep track of the index as well? no problem:

```js
let i = 0;
 for (const el of log) {
    console.log(el);
    console.log(i);
    i++;
 }
```

### For in (objects)

```js
const offer = {
        price: 10,
        tag: 'perfume'
    }
    for (const key in offer) {
        console.log(offer[key]);
    }
```

`offer.key` won't work because JS will look for a property in the object called `key`.

A string must be passed to inside the brackets [];

### While loop

```js
 let i = 0;
    while (i < 10) {
        console.log(i);
        i++
    }
 // the above example should be done with a classic for loop instead

 let finished = false;
 const randomNumbers = [];
 while (!finished) {
     const rndNumber = Math.random();
     randomNumbers.push(rndNumber);
     if (rndNumber < 0.5) {
         // to exit the loop  
         finished = true;
 }
 }
```

a while loop start not knowing how many iterations it will take to complete.

### Do while loop

```js
let i = 10
do {
        console.log(i);
        i++
    }
    while (i < 10);

// will print 10, because the do block runs first
```

how does it work? first do, then check the condition

### Loop + break: good for liping iterations execution

```js
for (let i = 0; i < 5; i++) {
        //lets stops the execution if i === 3
        if (i === 3) {
            break;
        }
        console.log(i);
    }

//prints 0,1,2
```

useful for stoping the loop executions under certain conditions.

### Loop + continue:  good for skipping one iteration

```js
for (let i = 0; i < 5; i++) {
       //lets avoid printing 3
       if (i === 3) {
           continue;
       }
       console.log(i);
   }

// prints 0,1,2,4
```

### Labels: useful for breaking outer loop from inside inner loop

```js
outerLoop: for (let i = 0; i < 5; i++) {
    console.log('outer', i);
    for (let i = 0; i < 5; i++) {
        if (i === 3) {
            break outerLoop;
        }
        console.log('inner', i);
    }
}
```

### Try and catch

We can throw many things that will stop execution, like objects, strings, Error, etc

```js
throw {message: 'code below wont run'};

//it prints: app.js:155 Uncaught {message: 'code below wont run'}
//, (in red colour, indicating it's a system error);
```

maybe a `user input` or `network error` causes the code to stop exectution, not ideal, right?

```js
try {
    throw {message: 'code below wont run'};
} catch {
    console.log('there was an error, but code below will be executed :)')
}
// code below keeps being executed!
```

It's common to throw our own errors in the app, so we save up adding a lot of if statements for different types of errors.

```js
let chosenMaxLife;

function getValue() {
    const enteredValue = prompt('enter the max life value', '100');
    userInput = +enteredValue;
    if(isNaN(userInput) || userInput <= 0) {
        throw {message: 'invalid user input'}
    } else {
        return userInput;
    }
}

try {
    chosenMaxLife = getValue();

} catch (error) {
    console.log(error);
    chosenMaxLife = 100;
}
```

### Finally block

is rarely used.

```js
try {
    // some code that might fail
} catch (error) {
    // maybe store the error in a database
    // re-throw the errror
    throw(error); // not very common, very advanced
} finally {
    // runs after the try {} block;
    // do something, because the rest of the code
    // wont run if the error was thrown
}
// rest of the code
```

### ES5 &  ES6

Var vs Let & Const

1) **Better scoping**

`var` respects `global` and `function` scopes, while `let` and `const`  respects `curly braces` (present in for loops, if blocks, functions):

```js
const name = 'Max';

if (name === 'Max') {
    var hobbies = ['coding', 'cooking'];
}


console.log(hobbies); // prints the hobbies, it was defined
// in the global scope
```

```js
const name = 'Max';

if (name === 'Max') {
    let hobbies = ['coding', 'cooking'];
}


console.log(hobbies); // throws an error (not defined)
// because let saw the curly braces!
```

So, using `var` causes variable polution, and using `const` and `let` keeps them better scoped.



2. **Better variables and usage order declaration**
   
   variables with `var` can be declared at the bottom of the files, after they have been used.

```js
console.log(userName);
var userName = "Max"; // returns `undefined` but not an error
```

what happens is that the JS engine does this under the hood, with `hosting`:


```js
var userName;

console.log(userName);

userName = "Max";
```

but with `let` or `const`, an error is thrown:



```js
console.log(userName);
let userName = 'Max'; // throws this error:
// Uncaught ReferenceError: userName is not defined
```

we're forced into declaring the variable first and then use it.



3. Avoids redeclaring variables:

```js
var userName = "Max";
var userName = "Manuel" // works!

let userName = "Max";
let userName = "Manuel" // Throws an error!
// Uncaught SyntaxError: Identifier 'userName' has already been declared
```



4. Strict-mode can be enabled
   
   use `use strict` mode at the file or function level, to avoid unexpected behaviours, that browser vendors do when executing the JS code. Generally, it's not neccessary.
   
   ```js
   // behaviour no 1: assigning a non declared variable
   
   userName = 'Max' // assigning value to a non declared variable
   console.log(userName); // prints Max, wow!
   
   'use strict'
   userName = 'Max'
   console.log(userName); // throws an error
   // ncaught ReferenceError: userName is not defined
   
   ```

```js
   // behaviour no 2: assigning a reserved name
   var undefined = 'foo' // doesnt throw an error
   
   'use strict'
   var undefined = 'foo' // Uncaught TypeError: Cannot assign 
   // to read only property 'undefined' of object '#<Window>'
   
   //using let or const, also disables this behaviour
   let undefined = 'foo';
   const undefined = 'foo'; // Uncaught SyntaxError: Identifier 
   //'undefined' has already been declared
```



## JS engines and what they do

#### Parsing and compilation

code is interpreted to `bytecode` (faster) and starts execution, but still low performance, so it's then compiled to `machine code` (OS code) that runs on the chip.

What are browser APIs? bridges between the JS code and C++ code built in the browser, like `window.document`, etc



#### Execution: inside the JS engine

1) Managing memory: `heap` is long term memory. The `stack` is the short term memory and manages which function is executing and if there's a return, to which function is returning a value to.
   
   functions code gets stored in the `heap`.
   
   The browser reaches the `heap` and the `stack`.

2) Managing execution steps



### The `stack`


the first thing to be pushed to the `stack` is all the code present in the file, and it's kind of wrapped in an `anonymous` function. Then, the function `greet()` is added to the top of the stack, so the thing on top of th stack is the thing that is currently running.
Then, `getName()` is added to the top of the stack; Then `prompt()` is added to the top of the stack.

The stack manages execution contenxt, e.g which functions are being executed, and the order of the nested calls and primitite values (cheap to re-create). The heap stores reference values, like objects, because are expensive the re-create.

One the `prompt()` function is done, by returning a value to `getName()` is being removed from the stack;

Even if a function doesn't have a `return` statement, there's an implicit return after the last line, which makes the stack to remove when done.

Once the `greet()` function is done, the `anonymous` function is also removed from the stack because there's no more things to run on the file.

```js
function getUserName() {
  return prompt('enter userName', '');
}


function greet() {
  const userName = getUserName();
  console.log('Hello' + userName);
}

greet();
```

The stack is a shortlive data structure to keep track of which functions are being executed.



#### Debugger & the stack

go to `Sources`, place a breakpoint, and the check the stack, you'll see the functions there stacked.

So to sum it up JS engine = heap + stack.

If event listeners have been set up, the `Event loop` knows them and it will reach the JS engine and push callback functions to the stack.



### Primitive vs Reference values



#### Primitives

 are shared by copy



```js
let name = "Max";
let anotherName = name; // "Max" was copied, stored in the stack,
// and passed here
console.log(anotherName) // prints "Max";
name = "Manu";
console.log(anotherName) // still prints "Max";
```

name.length(), tells us that a string can be temparely be transformed to an object to access some properties.



#### Reference values:


when assigning an object to a variable, the pointer is stored in that variable, not the object itself.



```js
let userData = {
    name: 'tebi',
    age: 32
}; // this object is stored in the heap, and userData holds the 
// pointer (or reference) to it.

let newUserData  = userData; // the same pointer is passed here

userData.name = 'Sarah';
console.log(newUserData.name) = 'Sarah';
```

How to avoid mutating the object? cloning objects and arrays


```js
let newUserData = {...userData};
userData.name = 'Sarah'; // 
console.log(newUserData.name) = 'tebi'; // different pointer
// it's pointing to another object, a clone one
```



### Garbage collection

JS engines have garbage collectors that delete stored things in the heap.



```js
let person = {name: 'Max'};
person = null; 
// garbage collector deletes {name: 'Max'} from the heap;
// because the pointer is no longer used in the rest of the code
```

Nowadays, browsers are intelligent enough to check if `person` is being used or not, and also delete the object from the heap, without the need of having to reassign the variable to null, for example.

#### Memory leaks

when you keep having references to objects in the code, but you don't use them, so it's means keeping space in memory for nothing.

adding `click` event listeners to a button will always result on event listener replacement `if the same function pointer is passed as callback`, so a button will woun't trigger 2 callback functions.



```js
function print() {
    console.log('button clicked');
}

someHtmlElement.addEventListener('click', print);
someHtmlElement.addEventListener('click', print);

// upon click, just one console.log() will be printed


someHtmlElement.addEventListener('click', function() {console.log('button clicked')};
someHtmlElement.addEventListener('click', function() {console.log('button clicked')};

//upon click, 2 console.log() will be printed!!
// 2 functions that do the same are stored in the heap!
// that's a memory leak;
```



## More on functions

### Functions are objects

````javascript
function hello () {
    console.log('hello');
} 👈 // no semicolon to function declarations, that's the convention

console.log(typeof hello); // prints `function`

console.dir(hello) // it prints the function object!
````

The method **`console.dir()`** displays an interactive list of the properties of the specified JavaScript object.

### Function expression vs function declaration

Expression: right hand side of an `=` operator.

````javascript
// right hand side, it's an expression
const start = function 👉 ❌startGame() { // the name of the function is not needed, could be anonymous
  console.log('Game is starting...');
}; 👈 // semicolon is usually added to expressions.
startGameBtn.addEventListener('click', start);

//it's a declaration
function startGame() {}
````

```javascript
// right hand side breaks hoisting! `start` is hoisted as undefined 😕
startGameBtn.addEventListener('click', start);

const start = function() {
  console.log('Game is starting...');
};

❌ Uncaught ReferenceError: Cannot access 'start' before initialization
    at app.js:16:40
```

### Which approach to choose?

the expression approach is prefered since it forces devs to declare the function before calling it:

```javascript
✅ // prefered approach
const start = function() {
  console.log('Game is starting...');
};
```

### To be anonymous or not, that's the question 🤔

You might want to name them to get better logs in the console when that function breaks.

```javascript
// anonymous function log
startGameBtn.addEventListener('click', function() {
  console.log('Game is starting...', age);
});

app.js:19 Uncaught ReferenceError: age is not defined
    at HTMLButtonElement.<anonymous>👈 (app.js:19:38) 👈 // line number is not enough when having bundled code in prod


// named function log
    startGameBtn.addEventListener('click', function startGame() {
  console.log('Game is starting...', age);
});

app.js:4 Uncaught ReferenceError: age is not defined
    at HTMLButtonElement.startGame👈 (app.js:4:38) 
```



### Arrow functions

`=>` is a keyword made up of math symbols, it's not an operator like the `=` operator!

Predence: && have precedence over || operator, so && are executed before.

````javascript
// empty return, `undefined` is returned
const getWinner = (computerSelection, playerSelection) => {
	....some code
	return;
}

const winner = getWinner(); // winner will be `undefined`
````

````javascript
// calling functions with less arguments than expected
// there's no JS error 😮

 const winner = getWinner(computerSelection); // doesn't throw error!
````

````js
// fallback params!

const getWinner = (computerSelection, playerSelection👉 = DEFAULT_SELECTION) => {
  // only kicks in when `playerSelection` is `undefined`.
  // it doesn't work for falsy values, like null, 0, etc
  
// we can add a value depending on other argument's values!
  const getWinner = (computerSelection, playerSelection👉 = computerSelection === ROCK ? PAPER : DEFAULT_SELECTION) => {
````

### How to add breakpoints when the code stopped in one?

just go to `Source` in the dev tools and add a new breakpoint with the cursos, and press the play button! Easy!

I can also disable the selected breakpoints by going to `Source->Breakpoints` window, and uncheck them!



### Rest parameters (...someNumers)

variable amount of arguments! https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters

this function it can handle any number of args:

```js
const addNumbers = (...numbers) => {
  let sum = 0;
  numbers.forEach(number => {
    sum = sum + number;
  });
  return sum;
}

console.log(addNumbers(1,2));
```

The arguments get added to an array, so we can iterate over it!

```js
// OLD way ❌

const addNumbers = function () {
  let sum = 0;
  for (number of 👉 arguments) { // arguments is magically in the scope of the fn,
    // it's not an array, an array-like object, so it doesn't have the .forEach method 😮
    sum = sum + number;
  }
  return sum;
}

console.log(addNumbers(1,2));
```



### Declaring functions inside functions

````js
const addNumbers = (...numbers) => {
  👉 const validateNumber = (number) => {
		return isNaN(number) ? 0 : number;
	}

  let sum = 0;
  numbers.forEach(number => {
    sum = sum + validateNumber(number);
  });
  return sum;
}
````

functions are objects, so we can can objects stored inside objects, right?

The `validateNumber` function is scoped to the curly braces of the parent function.

### Callbacks

````
const addNumbers = (👉 callback, ...numbers) => {
  const validateNumber = (number) => {
		return isNaN(number) ? 0 : number;
	}

  let sum = 0;
  numbers.forEach(number => {
    sum = sum + validateNumber(number);
  });
  👉callback(sum);
}

const myCallBack = (sum) => {
  alert(sum);
}

addNumbers(👉myCallBack, 1,2, 'banana', 5);
````



### Adding extra parameters on the fly: the .bind() method:

Using .bind() method returns a new function reference with some new configuration (give values to some params), so that function can be called at some point (upon an event, etc) with the pre-configured param values!

````js
const sayHello2 = (👉 greeting, name) =>{
  console.log(greeting + name);
}

// silly example calling the function right away, not done in real life
sayHello2.bind(this, 👉'Special greetings ')('tebi!'); // Special greetings, tebi!
````

💡 usually the binded function is passed as a callback fn an then exectuted at some point by the parent function

````js
someFn(sayHello2.bind(this, 'Special greetings'), someOtherParams);

const someFn = (callback, someOtherParams) => {
	callback('tebi');
}

// there could be some logic inside the someFn to pass the appropiate greeting to callback, but that could lead
// to a lot of if checks on the someOtherParams, or ternary expressions
````

````js
// bind usage in the calculator
// the callbacks are preconfigured for each button 🔥
const operators = {
  ADD: '+',
  SUBSTRACT: '-',
  MULTIPLY: '*',
  DIVIDE: '/'
}

function calculate(operation) {
  const enteredNumber = getUserNumberInput();
  const initialResult = currentResult;
  
  if (operation === 'ADD') {
    currentResult += enteredNumber;
  } else if (operation === 'SUBTRACT') {
    currentResult -= enteredNumber;
  } else if (operation === 'MULTIPLY') {
    currentResult *= enteredNumber;
  } else {
    currentResult /= enteredNumber;
  }
  createAndWriteOutput(operator.operation, initialResult, enteredNumber);
  writeToLog(operation, initialResult, enteredNumber, currentResult);
}

addBtn.addEventListener('click', calculate.bind(this, 'ADD'));
subtractBtn.addEventListener('click', calculate.bind(this, 'SUBTRACT'));
multiplyBtn.addEventListener('click', calculate.bind(this, 'MULTIPLY'));
divideBtn.addEventListener('click', calculate.bind(this, 'DIVIDE'));
````



### Interacting with the DOM

````
// console
document
// we see kind of HTML, and we can select html elements, and they get hovered on the page
// document is just a JS object (with all the methods we need), represented by Chrome in this HTML way in the console.
````

````bash
console.dir(document) // to see the full object!
````

### Why can we use `alert()` without the window.?

because the browser adds the window. to functions and variables it doesn't find in our code! 😮

### The DOM

There are 2 types of Nodes (JS objects):

- Element Nodes divs, paragraphs, etc
- Text Nodes (white spaces coming from the indentation of the HTML file)

whitespaces are Text Nodes, and not rendered in the screen 😮

#### The `$0` shortcut:

the last selected element in the `Elements` tab in the dev tools gets stored in the `$0` variable, so it can be used in the console, neat!

We can't see the text nodes in the `Elements` tab.

### Targeting elements

there are 2 camps of methods:

- return the fisrt match (getElementById, querySelector)
- return an array-like array: NodeList (it might not have .forEach and other methods) (e.g querySelectorAll)

💡`getElementBySomething` methods return an object that is updated if the Node changes later in time ⏺

the other ones just return a snapshot of the Nodes 📸

### Applying methods on Element Nodes

we can do `someElement.querySelector('some-selector');` that's great! we can have a more scoped search

`getElementById` is not inside Element Nodes, only in the document.

```js
const h1 = document.querySelector('h1');
h1.textContent = 'Lalala'; // deletes and creates a new Text Node
```

Use mdn to look for the Element Node documentation, e.g h1. Under `DOM interface`, we have, eg. https://developer.mozilla.org/en-US/docs/Web/API/HTMLHeadingElement.

Some props are read only ⚠️

### Attributes vs Properties

- Attributes: what's inside the HTML tag, e.g `id`
- Properties: the browsers configures some props, like the `id` based what is was passed in the HTML as attributes

Some attributes, like <input value="some text here">, can't be changes with input.value method.

````
<input value="some text here">
input.value = 'other random text'

// we check Elements tab
<input value="some text here"> // still the same, one way data binding


input.id = 'some-id';
<input value="some text here" 👉 id="some-id"> // updates the HTML! 2 way data binding
````

So, when the user types, the value of `value` is updated in the JS object but not on the HTML attribute read on Elements tab, so 

````
input.setAttribute('value', 'some other default text');
<input 👉 value="some other default text">

input.value = 'some previous text the user entered' // decoupled state!
````

💡value attribute and property are decouple on purpose, so we can always backtrack to the original value if we need to, after the user entered some text:

````js
input.value = input.getAttribute('value'); // reseting the value propr with the original attribute!
````

Tip: the JS object propr and the rendered UI are in sync

`input.getAttribute()` helps to read the value written in the HTML tag attribute



### Traversing the DOM: children, descendents, Parent and Ancestors

use `.children` to navigate the Node Elements (skiping the Text Nodes);

if you use `.childNodes` you get the list of all nodes (Elements and Text).

Remember: even the indentation of the HTML gets converted into text nodes. e.g: `someTextNode.data: "\n      "`

How to see the text nodes being rendered on the screen:
```css
// add this to the ol/ul or other element
element.style {
    white-space: pre;
}
```

Then, if we have the nodelist of text and element nodes in the console.dir, and we hover over the text nodes, we'll see them highlighted in the rendered page!

#### Perfomance issues:

````
document.querySelector('li:last-of-type'); // 🐌 running a query like that at the document level is expensive!

document.querySelector('ul').lastElementChild() 🚀
````

`````
someElement.closest('some-selector'); // looks for the closes parent that matches that selector
// it's kind of querySelector but for looking upstream
`````

💡 Run as fewer .querySelector queries as possible, and use parent and children navigation instead (always taking into account code readability, and future changes in the HTML order)

Deep traversing is also bad for perfomance

### Styling DOM elements

````
// inline
someElement.style.backgroundColor = 'red' // inline style -> highest specificity

// classes
someElement.className = 'banana' // add or remove classes, I can re-use styles
// cumbersome, be careful not to override previous classes added to the element ❌

someElement.classList.add('banana'); ✅
someElement.classList.remove('banana');

// ids
someElement.id = 'banana'

// some attributes (that are already targetted in the CSS file)
someElement.setAttribute('banana', 'true');
````



### Creating and inserting Elements

#### #1 HTML string

##### `innerHTML`:

````js
// innerHtml

const ol = document.querySelector('ol');

ol.innerHTML = '<li>the only element</>'; // replaces ALL original HTML content
````

Performance and lossing state issues:

````js
ol.innerHTML = ol.innerHTML + '<li>the only element</>'; // re-renders the original content, purple flashing on those elements
// on Chrome dev tools ⚠️

<div id="someId">
  <input>
</div>

const div = document.getElementById('someId')
 div.innerHTML = div.innerHTML + '<p>Something went wrong</p>'; // I loose the input value of the `value` prop
````



##### `insertAdjacentHTML`

```js
div.insertAdjacentHTML('beforeend', '<p>Something went wrong</p>'); // doesn't re-render the input field!
// more performant ✅
// but I only set the attributes ⚠️
```

 what if I want to attach and event listener to a button I've just added? 🤔
 I'd have to target it with querySelector and then add the prop, so annoying and poor perfomance ❌



#### Creating Node Element + append it

The methods to add the nodes are `append`, appendChild `prepend`, `before`, `after`, `replaceWith`.

```js
// createElement + appendChild

const div = document.getElementById('input-wrapper');

const error = document.createElement('p');

error.textContent = 'Something broke!';

div.appendChild(error);
```

These are better APIs, since I can append a list of Node Elements separated by a coma:

```js
div.append(error, otherNodeHere);
```



### Nodes are objects!

```js
div.append(error);

div.prepend(error); // moves the element up, it's the same object reference
```



### Cloning elements = cloning objects

Remember to deep clone nodes to, e.g get the textContent cloned as well:

````js
div.append(error);

const errorClone = error.cloneNode(true);

div.prepend(errorClone);
````



#### NodeList vs HTMLCollection

NodeList (querySelectorAll) is more perfomant because it's not updated when inserting **new** Elements there with JS, or deleting them.

HTMLCollection (getElementByTag) reflects new/removed items in the array when targeted with JS

````
const ol = document.querySelector('ol');

const nodeList = document.querySelectorAll('li');

const HTMLElementList = document.getElementsByTagName('li');

const newLi = document.createElement('li');

newLi.textContent = 'added item';

ol.append(newLi);


console.log('nodelist', nodeList); 👈 not updated in terms of new/removed elements

console.log('HTMLElementList', HTMLElementList); 👈 updated! ✅
````

Having a non live array it's maybe not an issue.



### Removing elements from the DOM

```js
someElement.remove()
```



### Favourite movies app

`getElementById` is faster than `querySelector(#someId)`

Don't use `.classList.add('someClass')` and  `.classList.remove('someClass') 

#### checking input values

````js
const addButtonHandler = () => {

    const titleValue = titleInput.value;
    const imageUrlValue = imageUrlInput.value;
    const ratingValue = ratingInput.value

    //conditions I don't want + early return
    if (
      // 👇 get rid of white spaces
        titleValue.trim() === '' ||
        imageUrlValue === '' ||
        ratingValue === '' ||
      👇// strings are coherced to numbers when checking with tese operators
        ratingValue < 1 ||
        ratingValue > 5
    ) {
        alert('invalid input');
        // early return
        return;
    }

}
````

If I don't have the `.trim` method, adding white spaces in the title field makes it valid.





(read from While loop 📚)

