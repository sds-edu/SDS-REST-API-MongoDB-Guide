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