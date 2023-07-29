## What are we building?
- Address book 
- blah blah blah

## Plan

### Intro and setting up
- Part 1: blah blah
- Part 2: ...
- Part 3: ...

### Structuring the backend architecture
- Part 4: Controllers, middleware, models, routes

### Building our REST API endpoints
- Part 4: `GET`

- Part 5:

- Part 6:

- Part 7:

## Part 1: A brief overview of REST API
### What is `REST`
- Statelessness 
- Cachebility
- How we organize our endpoints URI
Note: most of these are already covered by default if we use framework like Express


### Purpose of `REST` API
- Make API more maintainable
_ By following clear conventions, make it easier for other developers to integrate with your API

### `REST` conventions
- The endpoint refers directly to the resource and the HTTP verbs (e.g. `GET`, `POST`, `PUT`, `DELETE`) specify the actions.
- Typically takes 2 URI per resource: one for the whole collection and one for a single object in that collection
- We can also nest collections

## Part 2: Setting up Node and Express

### 2.1 Setting up the development environment

Prerequisite: you should have Node and a package manager on your operating system. To install, please visit the guide [here](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/development_environment#installing_node)

#### 2.2.1 Create a `package.json` file for your application

Use the `npm init` command to create a package.json file for your application. This command prompts you for a number of things, including the name and version of your application and the name of the initial entry point file (by default this is index.js). For now, just accept the defaults:

```
// npm
npm init

// yarn
yarn init

// pnpm
pnpm init
```

#### 2.2.2 Install Express

```
// npm
npm install express

// yarn
yarn add express

// pnpm
pnpm add express
```

After running the command, `express` should appear under `dependencies` in your `package.json`

```
code example
```

#### [Optional] Install `nodemon`

```
// npm
npm install nodemon

// yarn
yarn add nodemon

// pnpm
pnpm add nodemon
```

Add to the scripts in `package.json`

```
"dev": "nodemon index.js"
```

### 2.2 Minimal setup of an Express server

#### 2.2.1 

```
const express = require('express') // // import modules/libraries with require()

const app = express() // intialize express app

const PORT = 8080

app.get('/', (req, res) => { // a HTTP request consists of path (i.e. /), HTTP action (i.e. GET)
  res.json({ message: 'Hello World' }) // the server send back the respond  in the json format through res.json()
})

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}...`)
})
```

#### 2.2.2 Start the server

To start the server, run the following command:

```
node index.js
```

You should be able to see `Listening on port 3000...` in the console.


## Part 3: Cross-origin resource sharing (CORS) 

### Same-origin policy 

- The same-origin policy is a security mechanism enforced by the browser that restricts how a document or script loaded by one origin can interact with a resource from another origin.

- Basically, if the origin of the resource is different from the origin of the document or script that is trying to access it, the browser will block the interaction.

- This helps isolate potentially malicious documents, reducing possible attack vectors. Can help protect users from malicious websites.

Problem 🤔 : but we need to access our API from a different origin! This is where CORS comes in.

### CORS
- CORS is a mechanism that allows web servers to specify which cross-origin requests are allowed.

- This allows developers to relax the same-origin policy in a controlled way, while still maintaining a high level of security.

Note: CORS can be set up with Express relatively easily through a middleware. The official docs can be found [here](https://expressjs.com/en/resources/middleware/cors.html#enabling-cors-pre-flight). 


### 3.1 Setting up CORS
Ideally, in the production environment, we probably want to specifically only allow access to our backend resources from our frontend website. However, for ease of development purposes, we will allow access from all origin for now. 


## Part 4: 

### Initial directory structure

#### Middlewares

#### Controllers
