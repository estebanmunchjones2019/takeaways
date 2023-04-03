# Max's NodeJS course

### Improved dev workflow and debuging

````json
// package.json

{
  "name": "barebones-graphql",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodemon app.js", // 😮runs with npm start OR npm run start
    "start-app": "nodemon app.js" // runs with npm run start-app
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2",
    "express-graphql": "^0.12.0",
    "graphql": "^15.8.0"
  }
}
````

make sure `nodemon` is installed as part of the app, not just globally, as the app needs to contain all the necessary packages to run.

### Dev and prod dependecies

```
npm i nodemon --save-dev // dev dependency
npm i express--save OR npm i // prod dependency
```

No needed to separate the dependancies, it's just nice to see what's used in prod and dev

e.g Nodemon won't be used in prod, as we don't want the server to start and we won't be making changes to the code

````bash
// npm i nodemon OR npm i nodemon --save-dev + NOT having nodemon installed:

nodemon app.js // throws error, because nodemon is not installed globally, hence, not a command we can run from the terminal🚨
````

but, we can run it with an npm script, e.g `npm start`

````js
npm start // will look for nodemon LOCALLY
````



### Node debugger VSCode

the next fn button won't stop on some code lines, because fns are registered as middlewares, callbacks, and they'r gonna be executed in the future.

Install nodemon globally for the next step (debugger will use nodemon to restart the debugger after a change in a file, same as the server).

How to restart the debugger? VSCode -> Debug icon -> Create a launch.json file:

````json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    // Please run npm i -g nodemon
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "skipFiles": [
                "<node_internals>/**"
            ],
            "program": "${workspaceFolder}/index.js",
            "outFiles": [
                "${workspaceFolder}/**/*.js"
            ],
            "restart": true,
            "runtimeExecutable": "nodemon", 
            // "console": "integratedTerminal" // not needed, as the red square button kills nodemon
        }
    ]
}
````

💡When using the debugger, we don't need to have the server running using `npm start` or ` nodemon app.js`, the debugger will start the server for us with nodemon!

💡Don't forget to click green arrow `Launch` in order to get the debugger running

💡use the Debug console to access variables and test some methods on them. To change the value of variables, just use the Variables section of the debugger and double click and edit the values





### GraphQL

### REST API limitations

what if the FE needs different fields of a post in different pages?

#### Solution 1: create more endpoints /posts-slim

Disadvantage: a lot of work in the BE! unflexible endpoints

#### Solution 2: use query params `?slim=true`. 

Needs a lot of documentation for the FE engineers to figure out which query params they can use. Messy code

#### Solution 3: GraphQl

query what you want, no need to update the BE. Easy to know what you can query from the BE

It's like SQL in the FE!

How does GraphQL works?

it's a single endpoint POST request to `backendUrl/qraphql` endpoint (/graphql is just a convention, can be named anything, or just be the root of the server), because the **query expression** (String converted to JSON) is added to the body of the request.

Parts of the query expression:

````
{
	query {
		users {
			name,
			phone
		}
	}
}
````

#### Operation types:

- `query`: like when doing GET requests in REST endpoints
- `mutation`: like when doing POST, PUT, PATCH, DELETE requests in REST endpoints
- `subscription`:  set up real time connections via web sockets!

#### Operation "endpoints":

- users/ posts/ user/post, etc

#### Requested Fields:

- name/phone



### Backend structure

- Type Definition (Posts, Comments)
- Query Definitions
- Mutation Definitions
- Subscription Definitions
- Resolvers (e.g logic to look for the a post in the db)

**Query + Mutation + Subs** Definitions are like **routes** in REST endpoints, they'r called **Schemas**, they define what's possible in our server in terms of operations

**Resolvers** are like **controllers**  in REST endpoints



### Convert REST server into GraphQL server

1) get rid of express routes. From now on, there will be a single endpoint /graphql

2) Install graphql packages:

   ```bash
   npm i graphql express-graphql
   ```

   Note: express-graphql has been deprecated, please use this instead:
   https://github.com/graphql/graphql-http#migrating-express-grpahql

Express-graphql is more vanilla. **Apollo server** is a more robust solution that hides the details of this module away

3. Create a this structure

````
/graphql (or any name)
	resolvers.js (logic)
	schema.js (endpoints, what can be queried, mutated, or subscribed to)
````

4. Schema.js

   ````js
   const {buildSchema} = require('graphql');
   
   // backticks are used to write multiline a multiline string
   module.exports = buildSchema(`
   	// type name can be anything (this is like TS)
   	type RootQuery {
   		greetingMessage: String!
   	}
   	schema { //👈
   		query:👈 RootQuery
   	}
   `);
   
   // OR
   
   module.exports = buildSchema(`
     type News {
       title: String!,
       body: String
   
     }
     type Query👈 { // Query is a special type understood by graphql
       hello: String,
       news: News
     }
   `);
   ````

   Bang operator makes a field of the the type required (not null). e.g String!

5. Resolvers.js

   ````js
   module.exports = {
     	// the fn name should match the field
       greetingMessage() {
           return 'Hello world!';
       }
   }
   ````

6. Let's expose the Graphql server to the world!

   Make sure imports are not named imports! (I spent hours debugging this)

   ````js
   const express = require('express');
   const { graphqlHTTP } = require('express-graphql');
   const schema = require('./graphql/schema');
   const root = require('./graphql/resolvers')
   
   
   
   const app = express();
   app.use('/graphql', graphqlHTTP({
     schema: schema,
     rootValue: root,
     graphiql: true, // enables interactive FE
   }));
   app.listen(4000);
   console.log('Running a GraphQL API server at http://localhost:4000/graphql');
   
   
   
   // OR using the new package
   var express = require('express');
   // var { graphqlHTTP } = require('express-graphql');
   const { createHandler } = require('graphql-http/lib/use/express')
   const schema = require('./graphql/schema');
   const root = require('./graphql/resolvers')
   
   
   
   var app = express();
   app.all('/graphql', createHandler({
     schema,
     rootValue: root, // 😢 no graphiql FE available
   }));
   app.listen(4000);
   console.log('Running a GraphQL API server at http://localhost:4000/graphql');
   ````

7. Then query `news` like this:

   Using Postman:

   ```json
   // POST request to /graphql
   // body as raw JSON
   {
   	"query" {
   		news {
   			title
   		}
   	}
   }
   
   // OR
   {
   	"query" {
   		hello
   	}
   }
   ```

   OR just use the graphiql FE!

   ```json
   { news 
   	{ title }
   }
   
   // OR
   { hello }
   ```

   

#### 💡Pro tip: using await inside a fn, makes that fn return a promise under the hood!

### Mutations

Let's do any of the PATCH, PUT, DELETE  REST api classic things:

```js
const { buildSchema } = require('graphql');

// Construct a schema, using GraphQL schema language
module.exports = buildSchema(`
  type News {
    title: String!,
    body: String
  }
  type Query {
    hello: String,
    news: News
  }

  type Post {
    id: ID!,
    title: String,
    body: String,
    tags: [String]
    }

    input PostInput {
        title: String,
        body: String,
        tags: [String]
    }
    
  type Mutation {
    addPost(post👈: PostInput): Post
  }
`);
```

````js
module.exports = {
    hello: () => {
      return 'Hello world!';
    },
    news: () => {
        return {
            title: 'title here',
            body: null
        }
    },
    addPost: ({post👈}, req) => { // the destructured variable needs to match `post`
        post.id = (Math.random()*10000).toFixed(0);
        posts.push(post);
        return post;
    }
 		// OR
  	addPost: (args, req) => {
      	const post = args.post;
      	// OR const {post} = args
        post.id = (Math.random()*10000).toFixed(0);
        posts.push(post);
        return post;
    }
  };
````



````json
// graphiql
mutation {
  addPost(post👈: { // input here
    title: "hello",
    body: "world",
    tags: ["sports", "fishing"]
  })
  { // expected output here
    id,
    title
  } 
}
````

in the Network tab, I've got:

````json
Request Payload:

query:
  // all the helper text from graphiql
  # Welcome to GraphiQL
  #
  # GraphiQL is an in-browser tool for writing, validating, and
  # testing GraphQL queries.
  #
  # Type queries into this side of the screen, and you will see intelligent
  # typeaheads aware of the current GraphQL type schema and live syntax and
  # validation errors highlighted within the text.
  #
  # GraphQL queries typically start with a "{" character. Lines that start
  # with a # are ignored.
  #
  # An example GraphQL query might look like:
  #
  #     {
  #       field(arg: "value") {
  #         subField
  #       }
  #     }
  #
  # Keyboard shortcuts:
  #
  #  Prettify Query:  Shift-Ctrl-P (or press the prettify button above)
  #
  #     Merge Query:  Shift-Ctrl-M (or press the merge button above)
  #
  #       Run Query:  Ctrl-Enter (or press the play button above)
  #
  #   Auto Complete:  Ctrl-Space (or just start typing)
  #

  // the mutation!👈
  mutation {
    addPost(post: {
      title: "hello",
      body: "world",
      tags: ["sports", "fishing"]
    })
    {
      id,
      title
    } 
  }

variables:null
````

```json
// Request reponse

{"data":{"addPost":{"id":"6003","title":"hello"}}}
```



### Validation

with a rest server we used express-validator npm package. This package has `validator` package as a dependency.

But I don't want to have the same validation for all requests that come via my /graphql route, I want more control.

the resolvers get the mutation input values,  so we can check values there:

````bash
npm i validator
````

​	

💡 I can safely throw Errors as there's a try and catch block provided by graphqlHTTP middleware 

````js
addPost: ({post}, req) => {
    const errors = [];
    if (!validator.isLength(post.title, {min: 4})){
      	// I could have pushed just strings to the array
        errors.push({message: 'title too short'})
    }
    if (!validator.isLength(post.body, {max: 10})){
        errors.push({message: 'body too long'});
    }
    if (errors.length > 0){
        let errorString = '';
        errors.forEach(error => errorString = errorString + `${error.message} || `);
        const error = new Error(errorString);
        throw error;
    }
    post.id = (Math.random()*10000).toFixed(0);
    posts.push(post);
    return post;
}
````

Simpler code here:

```js
addPost: ({post}, req) => {
    const errors = [];
    if (!validator.isLength(post.title, {min: 4})){
        errors.push('title too short')
    }
    if (!validator.isLength(post.body, {max: 10})){
        errors.push('body too long');
    }
    if (errors.length > 0){
        const error = new Error(errors.join(' || '));
        throw error;
    }
    post.id = (Math.random()*10000).toFixed(0);
    posts.push(post);
    return post;
}
```

### what the response look like?

````json
// res body:
{
  "errors": [
    {
      "message": "title too short || body too long",
      "locations": [
        {
          "line": 33,
          "column": 12
        }
      ],
      "path": [
        "addPost"
      ]
    }
  ],
  "data": {
    "addPost": null
  }
}
````

The status code is 200, and this is always the case for a graphql response.

````js
// resolvers.js
addPost: ({post}, req) => {
    const errors = [];
    if (!validator.isLength(post.title, {min: 4})){
        errors.push('title too short')
    }
    if (!validator.isLength(post.body, {max: 10})){
        errors.push('body too long');
    }
    if (errors.length > 0){
        const error = new Error('Oops, invalid input'); // just pass a string when creating the error
        error.code = 422; // then add all the fields you want to the error object
        error.data = errors; // let's add the array of error messages!
        throw error;

    }
    post.id = (Math.random()*10000).toFixed(0);
    posts.push(post);
    return post;
}

// version using errors as objects
    addPost: ({post}, req) => {
        const errors = [];
        if (!validator.isLength(post.title, {min: 4})){
            errors.push(👉{message: 'title too short'})
        }
        if (!validator.isLength(post.body, {max: 10})){
            errors.push(👉{message: 'body too long'});
        }
        if (errors.length > 0){
            const error = new Error('Oops, invalid input');
            error.code = 422;
            // error.data = errors.join(', ');
            error.data = errors;
            throw error;

        }
        post.id = (Math.random()*10000).toFixed(0);
        posts.push(post);
        return post;
    }
````

````js
// index.js
app.all('/graphql', graphqlHTTP({
  schema,
  rootValue: root,
  graphiql: true,
  👉customFormatErrorFn(error){
    // e.g if error was syntax in code
    if (!error.originalError) error;

    // if it was my own thrown error
    return { 
      message: error.message || 'Oops, something went wrong', 
      status: error.originalError.code, 
      data: error.originalError.data
    }
  }
}));
````



```json
// response, WOW!
{
  "errors": [
    {
      "message": "Oops, invalid input",
      "status": 422,
      "data": [
        "title too short",
        "body too long"
      ]
    }
  ],
  "data": {
    "addPost": null
  }
}

// response using the errors as objects version
{
  "errors": [
    {
      "message": "Oops, invalid input",
      "status": 422,
      "data": [
        {
          "message": "title too short"
        },
        {
          "message": "body too long"
        }
      ]
    }
  ],
  "data": {
    "addPost": null
  }
}
```



### Connecting the FE to BE

we can use fetch to make queries to the backend, no need for a special library so far.

Response codes from a graphql server are 200 or 500

````js
// FE
(async()=> {
    try {
        const res = await fetch('http://localhost:4000/graphql', {
            method: 'POST',
            headers: {
                'Content-type': 'application/json'
            },
          	// 💡 body is just and object { query: `some query here`}
            body: JSON.stringify({
                query: `
                    mutation { addPost(post: 
                        {
                        title: "title here",
                        body: "body here "
                        }
                    )
                    {
                        title
                    }
                    }
                `
            })
        });
        const data = await res.json();
      	// responses are always 200, so we need to throw our errors
      	// fetch doesn't even throw error on its own when codes are not 200
      	// that's why axios is handier
        if (data.errors && data.errors[0].status === 422){
            throw new Error(data.errors[0].data[0].message);
        }
        if (data.errors){
            throw new Error('Post creation failed');
        }
        console.log(data);
    } catch(error){
        console.log(error.message);
    }
})()
````







This didn't happen on my project:

Error: method not allowed. How to fix it?

`graphqlHTTP` middleware rejects other methods than GET and POST.

let's make OPTIONS request to bypass the `graphqlHTTP` middlaware

````js
// Max's fix
app.use('*', (req, res, next) => {
	// sends the response with a 200 status code to the FE
	if (req.method === 'OPTIONS') res.status(200);
});

// graphqlHTTP code here
````



