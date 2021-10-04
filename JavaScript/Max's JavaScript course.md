# Max's JavaScript course

## 

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

### How to revert commits on master?

```bash
$ git checkout <commit-id> .
$ git add .
$ git commit -m "Reverting to <commit-id>"
$ git push -u origin <branch-name>
```

Don't forget the period `.`when checking out

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

@Pavel 'Snakebite' Zakharov 
Your original getUrlParams function was created to get ['Montreal', 'A4', 'A6', 'Coupe'…etc] but what I need on useMetadata is quite different to this.

So, let’s use different functions, as what we need on Provider.tsx and useMetadata have different data shapes needs.

Trying to make getParamsFromUrl to be reusable is making writing more code on each of the code that consumes uses that function.

Deep explanation:
To make getParamsFromUrl to be reusable, it needs to return `{ ‘region’: [‘Montreal’], ‘carline-type’: '



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
function add(num1: number, num2: number ){}
```

when declaring the funcion, we assign types to the parameters.

```js
const result = add(3,5);
// no need to assign types to `result`, it's infered
```
