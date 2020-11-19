# Angular 8 Academind's course



## Section 2: The basics

main.ts file is the first ts file run on developement in the browser:

```typescript
//main.ts

//checks for the platform
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

//bootstraps the app from the AppModule
platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));
```

```typescript
//app.module.ts

 bootstrap: [AppComponent]
```

```typescript
app.component.ts

@Component({
    selector: 'app-root',
```

-the `bootstrap` array tells Angular what are the known components at the time the index.html is analized by Angular by the first time. Now, angular knows the `<app-root>` selector and can render the template inside.

-Angular changes the DOM at runtime

-A basic component:

```typescript
//server.component.ts

import { Component } from '@angular/core';

//decorator to enhance the class with metadata
@Component({
	//must be a unique selector, just not to override another selector, like <p>
	selector: 'app-server',
	//relative path
	templateUrl './server.component.html'
})
```

-To add a new component, just open a new terminal, don't kill the `ng serve` already running process!

-if a component is added to `app.module.ts` file in the `declarations` array, Typescript will throw an error because it doesn't know what the component is. It must be imported from the correspondent file.

```typescript
//app.module.ts

//the .ts extension is added by webpack when bundled
import { Header1Component } from './components/layouts/header1/header1.component';


@NgModule({
  declarations: [
    AppComponent,
    Header1Component,
```

-A component **must have a template**, but it can lack a `selector` and `styleUrls`.

-`styleUrls` in an array, so multiple `.css` files can be imported.

-How to do inline styling  and template in `.ts` component? replace `styleUrls` and `templateUlr` by `styles` and `template`:

```typescript
//app.component.ts

@Component({
    template: `
		<h1>Hello world</h1>
		<p>welcome</p>
	` ,
    styles: [`
		h3 {
			color: blue;
		}
	`]
})
```

a string of css rules is passed to `styles` array.

-Instead of using `selector: 'app-server'` that allows angular to recognize our component when it sees `<app-server>` in a template, a component can be selected by `attribute` or by `class` (bot rarely used):

```typescript
//server.component.ts

@Component({
    selector: '[app-server]'
})

@Component({
    selector: '.app-server'
})
```

```html
//server.component.html

//angular will recognize our server component here, by attribute
<div app-server>
</div>

//angular will recognize our server component here, by class
<div class="app-server">
</div>

```



-If a component is not declared in `app.module.ts` the error says that the `app-server` is not a known element.



#### Data binding

-what is `data-binding?`: the communication between the `.ts`  and the `.html` file: 

![](images/data-binding.jpg)

1) **String interpolation**:

```html
//point to variables
// or use expressions that resolves to a string or something that can be strigyfied, like numbers, booleans, etc

<p> {{ 'Hello' }} I am {{ name }}
//it's possible to call methods that returns a string
<p>{{ getName() }}</p>

//ternary expressions allowed
<p>My mood is {{ isHappy? 'happy' : 'not happy' }}</p>

```

No multiline expressions like a `for` loop or `if` blocks, but ternary expressions can be used.

Extra: difference between function syntax inside a class:

```typescript
function print () {
	//need to *bind(this)* if *this* is used inside.
}

print() => {
	//I can use *this* inside directly
}

```



2) **Property binding**:

```html
//allowClick is a boolean
//the type of variable depends on the property which is binded to

<button [disabled]="!allowClick">Do something</button>
```



use case where string interpolation can be replaced with property binding:

```html
//prints true or false:

<p>{{ allowClick }}</p> 
//or
<p [innerHtml]="allowClick"></p> (not the best practice, but possible)
```

When to use string interpolation? when wanting to output a string in the template. Don't use innerHtml for this case.



What to put inside the strings?

```typescript
[disabled]="write a typescript expression here, like, " username == '' " "
```

3) **Event binding**:

use `onSomething` to name methods that are triggered from the template.

```html
<button (click)="onButtonClicked()">Click me</button>
```

```html
//instead of calling a method, a ts expression can be placed inside the quotation marks.

<button (click)="wasClicked = true">Click me</button>
```



4) **Two way data binding**:

To use two way data binding, `FormsModule` must be imported in `app.module.ts`:

```typescript
import { FormsModule } from '@angular/forms';
```



**Lets see a use case WITHOUT two way data binding**, updating manually the `serverName` variable with every keystroke, passing the `$event` object to a method. Pretty annoying:

```html
//template

<input
(input)="onUpdatServerName($event)">
<p>{{ serverName }}</p>

```

```typescript
//.ts file
serverName = '';

onUpdateServer(event) {
	//explicit casting, because ts complaints aboute the 'value' property.
	serverName = (<HTMLInputElement>event.target).value
}

```



Now, lets see **TWO WAY DATA BINDING**:

```html
//template
//it's like the input event is listened to, and aupdates the serverName variable, and the value property is binded to the serverName variable, all aumatically done with the directive 'ngModel'.

//there's a combination of [] brakets for getting data from the .ts file, and () parenthesis, that bind an event in the template to an expression in the .ts file.

<input
[(ngModel)]="serverName">
<p>{{ serverName }}</p>
```

```typescript
//.ts file

serverName = '';
```



#### Directives

What are they? they're instructions in the DOM. When adding `<app-server>` tag in the template, that's a directive, and Angular places the component's template and business logic there.

There are directives **WITH a template**, like `<app-server>`  and **WITHOUT a template**, like `turnThisGreen`, that just applies some css.

Directives are generally added like attributes, like `turnThisGreen`: 

```html
<p turnThisGreen >
```

![](images/directives.jpg)



##### Built in directives: structural and attribute directives

###### Structural directives: *ngIf and *ngFor

***ngIf**:

1) *ngIf: it's a structural directive, because it changes the structure of the DOM.

```html
<p *ngIf="some expression that resolves to true or false">hello</>
```

this directive adds an HTML element to the DOM under certain conditions.

2) *ngIf="someVariable" and *ngIf="!somevariable" to address an `if else` logic.

```html
<p *ngIf="isLoggedIn; ">Hello random user</p>
<p *ngIf="!isLoggedIn ">Hello, logged in user</p>

```

3) *ngIf and else alternative approach, wit. A local reference `#` must be used, the tell the directive which part of the template does the else statement template must be added to the DOM, and a `ng-template` directive must be used.

```html
<p *ngIf="isLoggedIn; else personalGretting">Hello random user</p>
<ng-template #personalGreeting>
<p>Hello, logged in user</p>
</ng-template>
```



***ngFor**:

```html
<div *ngFor="let server of servers">Hello</div>
```

`server` is a dynamic variable, not defined in the `.ts` file, but `servers` does. Then, the server variable can be passed to components via `@input`. 

How to use the index while looping?

```html
<div *ngFor="let server of servers; let myIndexVariable = index">Hello</div>
```



###### Attribute directives: ngStyle & ngClass

Attribute directives don't add or remove things from the DOM, they just modify things inside the elements they're placed on.

**ngStyle**: changes the styles dynamically, at runtime, depending on variable values.

The name of the directive is `ngStyle`, and **we can also bind to a property of the directive called also ngStyle**, using  brackets []. So, in other words, property binding is used to configure the directive.

```html
<p [ngStyle]="{'backgroundColor': getColor()}">Hello dear user</p>
```

the property ngStyle expects to get an object with key value pairs. `getColor()` is executed in the ts file and return the appropriate color for each user. E.g: if the gender of the user changes at runtime, the getColor() method could return another color at runtime.

**ngClass**: adds a class at runtime:

```html
<p [ngClass]="{highlight: isAuth()}">Hello dear user</p>
```

for classes like `highlight-yellow` (with a hyphen), just use quotation marks to  enclose the name, like `"highlight-yellow"`.



## Section 3: Course project - The Basics-

How to add bootstrap??

```bash
npm i --save bootstrap
```

and then, change `angular.json` to add the minified version of the css file from `node_modules`:

```json
"styles": [
              "node_modules/bootstrap/dist/css/bootstrap.min.css",
```

How to avoid the `spec` file when creating components with the CLI?

```bash
ng g c --spec false
```

How to structure the folders of the project: by feature, like `recipes`, `shopping-list`

How to create a model? just create a class:

```typescript
//recipe.model.ts

import { Ingredient } from '../shared/ingredient.model';

export class Recipe {
    public name: string;
    public description: string;
    public imagePath: string;
    public ingredients: Ingredient[];

    constructor(name: string, desc: string, imagePath: string, ingredients: Ingredient[]) {
        this.name = name;
        this.description = desc;
        this.imagePath = imagePath;
        this.ingredients = ingredients;
    }
}

//ingredient.model.ts
//advanced typescript shorcut
export class Ingredient {
    constructor(public name: string, public amount: number) {}
}


```

the variables are `public` so when the object is created using the constructor, they are available (and not accessed just from a getter).

How to use the `Recipe` model for type checking?

```typescript
//recipe-list.component.ts

import { Recipe } from '../recipe.model';

recipes: Recipe[] = [];
```

so, the class `Recipe` is a type now.



How to bind add the source path to for an image? 2 options:

```html
src={{ recipe.imagePath}}

[src]="recipe.imagePath"
```



If an `<a>` element is used as a button, the `href` attribute must be deleted, to avoid unexpected behaviors. 

If a `<button>` is used to send a form, it must have `type="submit"`, otherwise it should be `type="button"`

When using `<forms>`, remove the `action` attribute, because we don't want to send a request to the server.

What is source map? it's an angular addition added by the CLI to the bundles, that allows the js files to be mapped to typescript in the browser, and place debuggers in the browser.

How to access the  `.ts` files in the browser? dev tools -> sources  -> webpack -> . (dot folder)





## Section 5: Components & data binding

#### Component communication: parent-child levels

If we don't want to use a Model to use it as a type, an alternative is:

```typescript
//we simply say that server is gonna be an object with name and id properties
server: {name: string, id: number};
```

###### Passing data from parent to child element:

By default, all variables or properties, are only accessible from within its component. To make them available to **the parent** , use `@Input` decorator.

```typescript
//a decorator is added to the variable declaration
@Input() element = "";

//in the parent component
<child-component [element]="someData"><child-component>
    
//or another name for the variable can be used in the parent
@Input('otherElement') element = "";

//in the parent component
<child-component [otherElement]="someData"><child-component>
```

The above is called **custom property binding**.



###### Passing data from child to parent component:

an EventEmiter is used in the child component, and the parent should listen to that event.

```typescript
//child component
//the constructor of the EventEmitter class is executed with `new` keyword.
@Output() serverAdded = new EventEmitter<{name: string, id: number}>
this.serverAdded.emit({name: 'server-1', id: '1'})
    
//parent component
<child-component (serverAdded)="onServerAdded($event)"></child-component>
```

`EventEmitter` is a generic type, that's why it uses `<>`.  The data emitted in the event can be picked up with `$event` argument. The event can be listened by a parent, but not a grand parent.

The css rules affect just the component they're placed on. Style encapsulation: Angular modifies the css rules and html elements, by adding attributes. If we inspect in the browsers:

```
//original css rule
p {
color: blue;
}

//compliled css rule:
p[_ngcontent-etc]{
	color: blue;
}

//compiled html element. All the <p> inside a component have this attribute added. <p> in other components have a different attribute.
<p _ngcontent-etc>Hello</p>
```

so, the specific attributes make this encapsulation work.

How to change the default encapsulation behavior?

```typescript
@Component({
	encapsulation: ViewEncapsulation.Emulate or .None or .Native
})
```

`Emulate` is the default,  `None` applies the style rules of the component to the whole app (Not a good idea), and `Native` uses the shadowDom, not supported by some browsers.



###### Local references:

Pass the actual `html element` (like `HTMLInputElement`) from the template to `.ts` file via method:

An html element, with all its properties, can be accessed in the `.ts` file, when passed via a method  from the `template` .

```typescript
//html
<input #nameInput>
<button (click)="nameEntered(nameInput)"

//ts
nameEntered(name: HTMLInputElement) {
    //do something name.value
}
```



Get the `ElementRef` from the `.ts` file using @ViewChild, without the need of a method:

```typescript
//ts
@ViewChild('nameInput', { static: true}) nameInput: ElementRef;

//do something with its value
nameInput.nativeElement.value
```

the `nativeElement` property must be accessed to get access to the `html element`.

Note: **don't change the DOM assigning a new value for the `html element` from the `.ts` file**!!

there are other tools to change the DOM.



###### Passing html elements to a child component:

```html
<component>
	<h1>Hello</world>
</component>
```

React can access the passed html via `props.children` from a parent to a child component, and that children content can be placed inside the child component. But in Angular, **everything passed inside the opening and closing tags of a component, is lost.**

We need to tell Angular where to place the projected content:

```html
//parent component
<child-component>
	<p>Some nested html elements</p>
</child-componen>

//child component
<h1>The child componen</p>
<ng-content></ng-content>
```

the `<ng-content>` tag is a hook for Angular, to know where to put the content passed between its opening and closing tags.



#### Lifecycle Hook methods:

![](images/lifecylce-hooks.png)

`ngOnChanges` is called when the component is created, and when any @Input properties or attributes, change. It receives and argument `changes: SimpleChanges`. The changes object contains the previous value of the property, and it' s useful to do something with the old value, like storing it.

`ngOnInit` the object was created, and the properties are available. (you can call `this.someProperty`). Runs after the contructor.

`ngDoCheck` runs every time a property has changed inside a component (model), and checks if the template (view) needs to be modified. I also runs after events are fired from the template, observables resolved, etc. It runs twice in development, because there are 2 cycles by default, not on production mode. It can be good to add some code to manually inform Angular to change something that it didn't pick it up.



Projecting content (html passed the opening and closing tags of the children):

`ngAfterContentInit` runs after the view of the projected content has been initialized (the view of the parent has been initialized, which contains the projected content).

`ngAfterContentChecked` runs right after the above method.

`ngAfterViewInit` runs after the content of the child element has been initialized.

 `ngAfterViewChecked` run after the above method, and after `ngAfterContentChecked` 



`ngOnDestroy` runs right before the object is destroyed, good for clean up work.



`ngOnChanges`  is fired if a new object is passed into a component via @Input. If it same object changed one of its properties or methods, it doesn' t fire the method, because objects are reference types, (are reference of the object stored in memory) . Passing a primitive like a string will trigger it.



###### Accessing the template in lifecycle hook methods:

-Inside a component, an `ElementRef` can be accessed in `ngAfterViewInit` and not on `ngOnInit` .

-Inside a child component, the `ElementRef` placed in the projected content (in the parent template) can be accessed in `ngAfterContentInit`, via `@ContentChild('localReferenceName', {static: true})`



## Section 6: Course project: Components & data binding:

