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

<p [ngStyle]="{'backgroundColor': isAuth ? 'green' : 'red'}">Hello dear user</p>
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

there are other tools to change the DOM: `renderer2`, and `@HostBinding()` for directives.



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



## Section 7: Directives: deep dive:

Structural directives change the DOM around the element they're placed on.

![](images/directives-2.png)

How to filter an array in an `*ngFor` directive, in the template?

````typescript
//this DOES NOT work. Only ONE structural directive is allowed
*ngFor="let event of events"
*ngIf="event.date > today"
````

a `custom pipe` is the right tool.

#### Custom attribute directive:

The `selector` is a must, in the `@Directive` decorator: in this case, the `selector` is `[appBasicHiglight]`, to target the `appBasicHighlight` attribute place on an `html element`.  If the selector would have the square brakets, it would target , `<appBasicHighlight>` tag.

The `html element` the directive sits on, can be injected into the directive in the constructor:

Basic directive, following BAD PRACTICES, changing the DOM directly with `elementRef`:

`````typescript
//basic-highlight.directive.ts----BAD PRACTICES
@Directive({
	selector: '[appBasicHighlight]'
})
export class BasicHighlight implements OnInit{
	constructor(private elementRef: ElementRef){}
	
	ngOninit(){
		this.elementRef.nativeElement.style.backgroundColor = 'red'
	}
}

//html
<h1 appBasicHighlight>Hello world</h1>

`````

It uses the `ElementRef` but doesn't use `ViewChild`, it just injects it in the constructor.

The directive must be imported and declared in `app.module.ts` file.



Directive using GOOD PRACTICES, changing the DOM with `renderer2`:

````typescript
//basic-highlight.directive.ts----GOOD PRACTICES
@Directive({
	selector: '[appBasicHighlight]'
})
export class BasicHighlight implements OnInit{
	constructor(private elementRef: ElementRef, renderer2: Renderer2){}
	
	ngOninit(){
		this.renderer2.setStyle(this.elementRef.nativeElement, 'backgroundColor','red', flags?)
	}
}

//html
<h1 appBasicHighlight>Hello world</h1>
````

**To change the DOM = use `renderer2` API**, this runs in serviceWorkers environment and in node(?).



###### Reactive directives with `@HostListener()` 

The `renderer2` approach works, but I can't react to events, like a mouse hovering on the element.

`@HostListener()`  is a decorator that declares a DOM event to listen for, and provides a handler method to run when that event occurs.

`````typescript
//basic-highlight.directive.ts----GOOD PRACTICES
@Directive({
	selector: '[appBasicHighlight]'
})
export class BasicHighlight {
	constructor(private elementRef: ElementRef, renderer2: Renderer2){}
	
    //changes the background color to red when the element is hovered
    @HostListener('mouseenter') mouseOver() {
        this.renderer2.setStyle(this.elementRef.nativeElement, 'backgroundColor','red');   
     }
    
    //changes the background color to transparent when the element is not hovered
    @HostListener('mouseleave') mouseNotOver() {
        this.renderer2.setStyle(this.elementRef.nativeElement, 					      'backgroundColor','transparent');
     }
    
}

//html
<h1 appBasicHighlight>Hello world</h1>
`````

`mouseenter` and `mouseleave` are native events on html elements. But  `mouseOver()` and `mouseNotOver`  are just the names of the methods executed when the mentioned events are triggered.



###### Reactive directives with `@HostBinding()`      -better than `Renderer2` -

The `@HostBinding()` decorator must be provided an argument pointing an attribute of the host element. Could be the `style.backgroundColor` , `disabled`, `class`, etc. 

`@HostBinding()` binds an attribute of  the `html host element` to a property of the directive. 

`````typescript
//Using @HostBinding----GOOD PRACTICES
@Directive({
	selector: '[appBasicHighlight]'
})
export class BasicHighlight {
	constructor(){}
    
    //bind an html attribute to a property of the directive. Pass the html attribute you want to bind to, inside quotation marks, and define the initial value of the directive property
    @HostBinding('style.backgroundColor') backgroundColor = 'transparent';
	
    //listen to events: changes the background color to red when the element is hovered
    @HostListener('mouseenter') mouseOver() {
        this.backgroundColor = 'red'   
     }
    
    //listen to events: changes the background color to transparent when the element is not hovered
    @HostListener('mouseleave') mouseNotOver() {
       this.backgroundColor = 'transparent'
     }
    
}

//html
<h1 appBasicHighlight>Hello world</h1>
`````



####  Binding to directive properties

It's nice to pass data to the directive from the template, to configure it, using `@Inputs` inside the directive:

``````typescript
// Adding Inputs ----GOOD PRACTICES
@Directive({
	selector: '[appBasicHighlight]'
})
export class BasicHighlight implements OnInit{
    @Input() defaultColor = 'transparent';
    @Input() highlightColor = 'red';
    
    @HostBinding('style.backgroundColor') backgroundColor:string;
	
    constructor(){}
    
    ngOnInit() {
        this.backgroundColor = this.defaultColor;
    }
	
    @HostListener('mouseenter') mouseOver() {
        this.backgroundColor = this.highlighColor;   
     }

    @HostListener('mouseleave') mouseNotOver() {
       this.backgroundColor = this.defaultColor;
     }
    
}

//html
<h1 appBasicHighlight [defaultColor]="'transparent'" [highlightColor]="'red'">Hello world</h1>

//or

//html
<h1 appBasicHighlight defaultColor="transparent" highlightColor="red">Hello world</h1>
``````

How does Angular know if the binded attributes are inputs if directives, or just normal attributes of the `html element` ? it just figures it out by itself, by looking first at the directives placed on the element, and looks for its inputs.

A neat way to add an `@Input` is to use an alias, so the name of the input is the same as the directive, like what happens with `ngClass` when it's input called `ngClass` is binded like: `[ngClass]` in the template. So, the directive is placed and the `ngClass` input is binded at the same time. Super!

`````typescript
//use Alias for inputs----GOOD PRACTICES
@Directive({
	selector: '[appBasicHighlight]'
})
export class BasicHighlight implements OnInit{
    @Input() defaultColor = 'transparent';
    @Input('appBasicHihlight') highlightColor = 'red';
    
    @HostBinding('style.backgroundColor') backgroundColor:string;
	
    constructor(){}
    
    ngOnInit() {
        this.backgroundColor = this.defaultColor;
    }
	
    @HostListener('mouseenter') mouseOver() {
        this.backgroundColor = this.highlighColor;   
     }

    @HostListener('mouseleave') mouseNotOver() {
       this.backgroundColor = this.defaultColor;
     }
    
}

//html
<h1 [appBasicHighlight]="'red'" [defaultColor]="'transparent'">Hello world</h1>

//or

//html
<h1 appBasicHighlight="red" defaultColor="transparent">Hello world</h1>
`````



#### Structural directives behind the scenes

The star `*` before the `ngFor` is sugar syntax, and Angular translates that into a more complex code, that has property binding, so we can pass data to the directive:

`ng-template` is a container.

`````html
<h1 *ngIf="showTitle">Hello World!</h1>

//translated by Angular to:

<ng-template [ngIf]="showTitle">
	<h1>Hellos World</h1>
</ng-template>
`````



#### Custom structural directive: unless

The directive needs access to the `TemplateRef` to know **what** to render, and the `ViewContainerRef` to know **where** to render it.

`ViewContainerRef` represents a container where one or more views can be attached to a component.

`createEmbeddedView()` instantiates an embedded view and inserts it into this container.

`TemplateRef` represents an embedded template, like `<div><h1>Hello World</h1></div>`, and it's basically the `html element` the directive sits on, and the nested `html elements`. 

``````typescript
//custom structural directive
@Directive({
	selector: '[appUnless]'
})
export class BasicHighlight implements OnInit{
    //I use a setter, which is a method where I can run an if else block. The value of the property 'unless' is not used in the end.
    @Input('appUnless') set unless(condition:boolean) {
        //if the condition is false, I wanna attach content to the DOM
        if (!condition) {
            this.vcRef.createEmbeddedView(this.templateRef);
        } else {
            this.vcRef.clear();
        }
    }
  
    constructor(private templateRef: TemplateRef, private vcRef: ViewContainerRef){} 
 
}

//html. When `showTitle` is false, the title will be displayed
<h1 *appUnless="showTitle">Hello world</h1>

``````

**The selector of the directive and the alias of the `@Input` must match, in order to be able to bind that property from the template passing the condition.**



#### Structural directives: *ngSwitch

More than 2 conditions? use `*ngSwitch` to react to different conditions

````
<div *ngSwitch="10">
	<p *ngSwitchCase="10" >The number is 10</p>
	<p *ngSwitchCase="20" >The number is 20</>
	<p *ngSwitchDefault >Another number</p>
</div>
````



Note: I tried removing `jQuery` and add a directive to the mobile toggle button of the recipe book app and it didn't work because there are 2 different `html elements` that need to be linked and be added/removed classes. So, I ditched directives and used a variable `isToggleOpen` in the `.ts` component, and used `[ngClass]` to add or remove classes in the the 2 `html elements`.



## Section 9: Services and dependency injection

When to use services:

1) Duplication of code

2) Data management

3) Comunicate between components (via `Subjects`)



Services are just classes, but they are not instantiated manually inside components, like:

````typescript
//DONT!! component.ts

const myLoggingService = new LoggingService();

myLoggingService.someMethod;
````

Angular has a better way ton instantiate the class, with dependency injection, following a singleton pattern, where just one single object is created by Angular for the whole app or some parts. So, the state of the app can be kept there, and is decoupled from the components. 

As object are reference types (not primitives, like strings and numbers), so the when an object is created, then references are passed to 

#### How to consume a service in a component?

`````typescript
//component.ts

@Component({
    //pass just the type of the service. The service object scope is just this component, not too useful.
    providers:[LoggingService]
})

export class someComponent {
    //use the typescript shortcut with `private` or `public`, to create a property that is assigned the value of the argument of the same name passed in the constructor.
    constructor(private loggingService: LoggingService)
}
`````



What is an injector: a middleman

What does it do?

**So now its the dependency injection’s responsibility to:**

1. Create the objects
2. Know which classes require those objects
3. And provide them all those objects



A provider tells an injector *how to create the service*. Each component can use it's own, or the parent's:

Injectors are inherited, which means that if a given injector can't resolve a dependency, it asks the parent injector to resolve it. A component can get services from its own injector, from the injectors of its component ancestors, from the injector of its parent NgModule, or from the `root` injector.

The instance of the class (object) is injected at runtime in the component.



#### Where to configure the injectors?

You can configure injectors with providers at different levels of your app, by setting a metadata value in one of three places:

- In the `@Injectable()` decorator for the service itself.
- In the `@NgModule()` decorator for an NgModule.
- In the `@Component()` decorator for a component.

The `providers` array tells the injector _which_ instance of the service we want.



#### What is the scope of the singleton service object create?

Services are singletons *within the scope of an injector*. That is, there is at most one instance of a service in a given injector. In the first example, the scope of the injector is the component, so **the scope of the object is the component, and all its children**. For example: `users-list.component.ts` has a providers array, then, `user.component.ts` will have the same service class instance.

If an object living in a service is passed to components, and it's not cloned, the component can mutate the object. Will the `ngOnChanges` run when changes happen inside the object, if the reference (pointer) is always the same?? No.

![](images/services.png)

the inheritance of a service instance to child components is overwritten if the child components have `providers` in the `@Component()` metadata.

 To make a service injectable into another one, two conditions must happen:

1) The injected service must be provided in the `AppModule` via `providers` there, or use `@Injectable({ providedIn: 'root' })` in the service. There are these 2 ways. 

2)  And , the receiving service must have `@Injectable()` decorator (without the need of any extra metadata).



Advantage of using `@Injectable({ providedIn: 'root' })`:

Services <strong>can be loaded lazily </strong>by Angular (behind the scenes) and redundant code can be removed automatically. This can lead to a better performance and loading speed - though this really only kicks in for bigger services and apps in general.



#### Cross component communication via `Subjects`

communication between to adjacent components can be done with an `@Output` and `@Input` in a rigid and cumbersome way. But with services, and event in a component can be listened into another one via `services` and `Subjects` !. 

Note: `EventEmmiter` can also be used, but it's not as efficient as `Subjects`.



## Section 10: Course project: Services and dependency injection

When keeping state inside services, 2 things can be done to avoid bugs:

1)  `private` can be added in front of them so, they're not accessible like this:

`````typescript
//component.ts
myServiceInstance.propery1 = some new value
`````

2) clone objects when returning them inside methods:

````typescript
getUsers() {
	return [...this.users]
	//or
	this.users.slice();(it returns a brand new array, not a reference to the original one)
}
````

Good practice: initialize (define) the variables in `ngOninit`, and previously declare them in the component:

````typescript
@Component({some metadata here})
export class Users implements OnInit {
	private users = [];
	constructor(private usersService: UsersService){}
		
	ngOnInit() {
		this.users = this.usersService.getUsers();
        this.usersChanged.subscribe(users => {
            this.users = users;
        });
	}
}
````

the `getUsers()` method can be avoided if a `BehaviorSubject` is used in the service instead.

When using `<a>` as buttons, get rid off the `href`, so it doesn't do something else, and just execute the method linked to it.

How to add a list of ingredients to an array, in one go?? ingredients = [{}, {}], and newIngredients = [{}, {}, {}]

````
this.ingredients.push(...newIngredients)
````

the spread operators allows us to take each object if the array, and add them to an array. If `push(newIngredients)` is done, the default result is [{}, {}, [{}, {}, {}]], nested arrays!!

Other option is the good and old for `loop`, or `.forEach` and push on every iteration.



## Section 11: Routing

One `index.html`, different content for each route, so the user sees multiple pages, but it's just one, changed at runtime by angular router.

The first step is to let angular know what are the routes, at the app level, in `app.module.ts`:

````
//app.module.ts

import { Routes, RouterModule } from '@angular/router';

//define the array
const routes: Routes  = [
	{ path: 'users', component: UsersComponent},
	{ path: '', component: HomeComponent}
]

//pass the array
@NgModule({
	imports: [
		RouterModule.forRoot(routes)
	]
})

````

So, for each route, a component is loaded (and it's children, of course), and it's placed thanks to the directive `<router-outlet>` placed on the template.

