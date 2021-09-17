# Next course by Filip



## Project 1

Command line tips:

````
node

var 7 = a

ctr + c to exit
````

After running `node`, then we can enter js code directly in the console!

We can also run the same code on the `Console` of the dev tools in the browser.



Functional components

````
export default Home = () => (
	some JSX
)
````

When not using {} after the function, and using () instead, the return is implicit

To add some logic, use the {}:

````
export default Home = () => {
	const a = 'lala';
	return some JSX
}
````

We can also write:

````
return React.createElement('div', null, 'Hello world');

the null is the value of props
````

The good old class components syntax

`````
export default class Home extends React.Component {
	render(){
		return(
			some JSX
		)
	}
}
`````

When using a react component and don't pass any `children`, one self closing tag is enough:

````
<SomeReactComponent />
````

`React.Fragment` can be replaced with just `<>` and `</>`

Global styles can be added like this:

````
<style jsx>
	.some-selector{
		padding-top: 100px;
	}
</style>
````

Class based component the syntax:

````
class MovieList extends React.Component {
	constructor(props){
		super(props);
		state = {
			count: 0
		}
	}
	
	or
	
	state = {
		count: 0
	}
}
````

`super` is called to execute the parent class constructor, passing the props the components gets passed.

Destructuring: assinging variables made easy:

````
const count = this.state.count
const { count } = this.state
````

`this.state.setState()` merges the object we pass it on.

When using hooks, the state is replaced 100%



#### Binding `this` and how to avoid it:

````
class MovieList extends React.Component {

	constructor(props) {
		super(props);
		//in order to call this.something inside methods
		this.buttonClicked.bind(this);
	}
	buttonClicked(){
		//as this function is called as a callback, it lost the `this` reference, it doesn't refer to the class anymore, if not binded in the constructor
		this.something
	}
}

buttonClicked = () => {
	//the arrow syntax arrow autmatically binds `this`!
	this.something
}

render(){
	//just passing buttonClicked reference, not calling it with ()
	return <button onClick={this.buttonClicked}>
}
````

How to avoid binding when using a method?

````
class MovieList extends React.Component {

	buttonClicked(){
		//as this function is called as a callback, it lost the `this` reference, it doesn't refer to the class anymore, if not binded in the constructor, or passed as a callback function to the event
		this.something
	}
}

render(){
	//anonymous function with a return that executes the function with ()
	return <button onClick={() => this.buttonClicked()}>
}
````



#### useState() hook

````
export default MovieList = () => {

	const [count, setCount] = useState(initialValue);
	
	//same as
	
	const countArray = useState(initialValue);
	const count = countArray[0];
	const setCount = countArray[1];
	(...)
	return JSX
	
}
````

`useState()` returns an array with 2 elements: a state variable and a function to update it safely.

To show that `setCount` re-renders the JSX, place a debugger before the `return` statement.



#### Re-rendering children

when a parent component re-renders, the children do. This didn't happened in Angular, because it worked with the DOM directly and with change detection. But in React, when data changes, many components re-render to update the DOM.

how to pass down props?

````
<ChildrenComponent count={count} appName={"The app name here"}>
````

in Angular, appName="The app name here", I don't need to add 

````
<component inputField="string"></component>
<component [inputField]="'string'"></component>
<component inputField="{{'string'}}"></component>
````



#### How web app work

the html document that reaches the browser is then transform to an object, the DOM

contatenating strings:

`````
'hola' + nombre + 'que tal'
`hola ${nombre} que tal`
`````



How to shorten a string?

````
shorten = (text, maxLength) => {
	if(text && text.length > maxLength)
		return text.substr(0, maxLength) + '...';
}
````

Exporting and importing syntax:

````
export default someFunction;
import someFunction from '...'

export someFunction;
import {someFunction} from '...'
````

Funny fact: if you export from a file called `index.js` inside a folder, the import can just point to the folder!:

````
import someFunction from 'someFolder';
````



#### useEffect() to prevent infinite loop

If we fetch data inside a component and set the state, then, we have an infinite loop:

````
//infinite loop here:
export default async MovieList = () => {
	const [movies, setMovies] = useState([])''
	resMovies = await getMovies();
	setState(resMovies);
	
	return JSX
}
````



Solution:

````
useEffect(() => {
	const fetchData = async () => {
		resMovies = await getMovies();
		setState(resMovies);
	}
}, [])


//this causes an error, because the callback funtion returns a promise when using async like this, and useEffect should only return a clean up function.
useEffect(async () => {
	resMovies = await getMovies();
	setState(resMovies);
}, [])
````

This the same as `componentDidMount()` life cycle hook. As we're not tracking any prop or state, then, it's never re-triggered when that changes.

If we pass `movies` to the array, the infinite loop doesn't happen because `useEffect` hook is smart enough to not re-run when the new value of `movie` is the same as the previous one.

`useEffect` always runs the first time the component is rendered.



Destructuring the state inside `render()`:

````
render(){
	const {movies} = this.state
	return (
		<div>{movies}</div>
	)
}
````

How to conditionally render stuff? like an `ngIf`?

````
return(
	{errorMessage &&
	<div>Opps! this error occured: {errorMessage}<div>}
)
````

The magical double && sign. Is a clean `ngIf` syntax in React.



#### getInitialProps() [deprecated] [only use it for pages components, not children]

instead of fetching the date inside `useEffect()` on the client side, it can be done on the server side with this function `getInitialProps()`:

````
//class based components
export default class Home extends React.Component {
	static async getInitialProps(ctx) {
		const movies = async getMovies();
		return {
			movies;
		}
	}
}


//functional components
const Home = () => {
	return JSX
}
Home.getInitialProps = async () => {
	const movies = async getMovies();
  return {
  movies;
  }
}

export default Home;
````



#### How to add links to pages?

````
import Link from 'next/link';

<Link href="/about">
	<a>About us</a>
</Link>
````



#### _app.js file, the App container + how to fetch data in the server side

`<Component>` is a reference to the pages components rendered inside the app container.

`````
import App from 'next/app'

class MyApp extends App {
	static async getInitialProps(ctx) {
		//this line calls the getInitialProps method or function of the page loaded
		const appProps = await App.getInitialProps(ctx);
		return {...appProps}
	}
}

render(){
	const {Component, pageProps} = this.props
	return (
		<Layout>
			<Component {...pageProps}>
		</Layout>
	)
}
`````

the destructuring {...appProps} because otherwise, the default object pageProps will be inside the appProps object, like this: `appProps.pageProps`. Making a destructuring, the nested object is avoided.



#### Accessing data from the URL

````
//using the router
//if I just wanna show the id on the UI
import Router from 'next/router'
const Movie = () => {
	const router = Router();
	const {id} = router.query
	return JSX including {id}
}

//OR using getInitialProps [deprecated]
export async getInitialProps(ctx) {
	const id = ctx.query.id;
	const movieData = await getMovie(id);
	return {movieData}
}


//OR using getStaticProps
export async function (params) {
	const id = params.id
	//optionally, get the full movie object
	const movieData = await getMovie(id);
	return {
		props: {
			movieData
		}
	}
}

more elegant with destructuring: export async function ({query}){...}
````

The api for `Link` changed from version 9.5.3.

````
//before
<Link href="/movies/[id]">

//now
<Link href=`/movies/${id}`>
````

When having `'s ` in string, use double marks or backticks:

````
"Tebi's blog"
`Tebi's blog`
````



#### References in the DOM

How to click a close button for a modal programatically (e.g, after clicking the save button)

````
let closeButton = null

const someFunction = () => {
	closeButton.click();
}

return (
	<Button ref={el => closeButton = el}>Save changes</Button>
)

````



#### Forms in React and Next

handling input changes

````
const changeHandler = (event) => {
	const target = event.target
	const name = target.name
	setForm({
		...form,
		[name]: target.value
	})
} 
````

How to pick up values from a `select` input form?

````
//genre is a select input
const genreChangeHandler = (event) => {

	const values = [];
	const options = event.target.options;
	
	options.forEach((option) => {
		if(option.selected){
			values.push(option.value);
		}
	})
	
	setForm({
		...form,
		genre: values (or values.toString(), to have a long string with genres separated by a coma, like "drama, action")
	})
}
````

#### What if I have a method in a parent component to close a button in a child component?

`````
export default const Sidebar = () => {
	let modalRef = null
	
	const someFunction = () => {
		//e.g save a move to a database
		....
		//close model below
		modal.closeModal()
	}
	
	return(
		<>
      <Modal ref={el => modalRef = el}>
      	<SomeChildren here>
      </Modal>
    </>
	)
}
`````

IMPORTANT: the children, in this case `<Modal>` needs to be a class based component.

Another solution could be pass a boolean variable as a prop, and when is true, it a function is called in the children.



#### Adding a custom server to Next

a folder `server/index.js` was created, and in `package.json` this was changed:

````
"scripts": {
	"dev": "node server/index.js"
} 

//before, it was
"dev": "next dev",
````

`getRequesthandler()` helps serving the next.js pages

using a custom server affects Static site generation and serverles functions. NOT a good idea.

How to access the body or a request?

use body-parser middleware:

````
server.use(bodyParser.json());
````



#### How to see objects in the logs?

````
console.log(JSON.stringify(someObject));
````

`data.json` was created, and has double quotes for the keys and values.



#### How to create buttons to navigate to other pages?

````
<Link href="/movie/[id]/edit" as=`/movie/${id}/edit`>
	<Button>Edit</Button>
</Link>

or programatically
Router.push('/movie/[id]/edit', /movie/${id}/edit`)
````



Handling http request methods on serverless:

````
//api/posts.js

export default async (req, res) => {
	if(req.method == 'POST') {
		(...save the post)
		return res.json(something)
	}
	
	//the default is GET
	return res.json(somethingElse)
}
````

To accept POST requests, if a custom server was set up, add this:

handle is a middleware that let next handle the requests and applies the folder structure for routing and so on.

````
server.post('*', (res, req) => {
	res.handle(req, res);
})

or a better way

server.use(handle); for ALL methods, all routes


````

