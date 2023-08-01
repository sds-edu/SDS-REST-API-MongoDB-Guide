## What are we building?

- Address book
- blah blah blah

## Plan

### Intro and setting up

- Part 1: A brief overview of REST API
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
  \_ By following clear conventions, make it easier for other developers to integrate with your API

### `REST` conventions

- The endpoint refers directly to the resource and the HTTP verbs (e.g. `GET`, `POST`, `PUT`, `DELETE`) specify the actions.
- Typically takes 2 URI per resource: one for the whole collection and one for a single object in that collection
- We can also nest collections

## Part 2: Setting up Node and Express

### 2.1 Setting up the development environment

Prerequisite:

- You should have Node and a package manager on your operating system. To install, please visit the guide [here](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/development_environment#installing_node)

#### 2.2.1 Create a `package.json` file for your application

Use in the `backend` directory run `npm init` to create a package.json file for your application. This command prompts you for a number of things, including the name and version of your application and the name of the initial entry point file (by default this is index.js). For now, just accept the defaults:

```
// npm
npm init

// yarn
yarn init

// pnpm
pnpm init
```

#### 2.2.2 Install Express

```js
// npm
npm install express

// yarn
yarn add express

// pnpm
pnpm add express
```

After running the command, `express` should appear under `dependencies` in your `package.json`

```js
"dependencies": {
    ...
    "express": "^4.18.2",
    ...
}
```

#### 2.2.3 Install `dotenv`

```js
// npm
npm install dotenv --save

// yarn
yarn add dotenv --save

// pnpm
pnpm add dotenv --save
```

#### 2.2.4 [Optional] Install `nodemon`

```js
// npm
npm install nodemon

// yarn
yarn add nodemon

// pnpm
pnpm add nodemon
```

#### 2.2.5 Add to the scripts in `package.json`

```js
"start": "nodemon index.js"
```

### 2.2.6 Minimal setup of an Express server

In the `index.js` add the following lines of code:

```js
const express = require('express') // // import modules/libraries with require()

const app = express() // intialize express app

const PORT = 8080

// optional
app.get('/', (req, res) => {
  // a HTTP request consists of path (i.e. /), HTTP action (i.e. GET)
  res.json({ message: 'Hello World' }) // the server send back the respond  in the json format through res.json()
})

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}...`)
})
```

To start the server, run the following command:

```
npm start
```

You should be able to see `Server is running on port 8080...` in the console.

If you visit `localhost:8080` you should be able to see "Hello World".

## Part 3: Set up MongoDB

### 3.1 Setup MongoDB (Atlas or Local)

Follow these guide to set up MongoDB:

- MongoDB Atlas: [here](https://www.mongodb.com/docs/atlas/tutorial/create-atlas-account/)

- MongoDB Local: [here](https://www.prisma.io/dataguide/mongodb/setting-up-a-local-mongodb-database)

### 3.2 Connect the DB to the backend server

Note: MongoDB Atlas is used in the example shown

#### 3.2.1 Install `mongoose`

In the `backend` directory, install mongoose:

```js
npm install mongoose
```

#### 3.2.2 Initialze the database

In the config directory, create `db.js`. This is where we will initialize the mongodb database.

```js
// db.js

const mongoose = require('mongoose')

const connectDB = async () => {
  try {
    const con = await mongoose.connect(process.env.MONGO_URI) // read from the .env file
    console.log(`MongoDB Connected: ${con.connection.host}`)
  } catch (error) {
    console.log(error)
    process.exit(1)
  }
}

module.exports = connectDB
```

The `MONGO_URI` is the connection string of your mongodb. It should be defined in the `.env` file:

```js
// .env

// example only
MONGO_URI = mongodb+srv://<username>:<password>@....mongodb.net/?retryWrites=true&w=majority
```

#### 3.2.3 Connect to the server to the database

Import and configure `dotenv` in `index.js`

```js
// index.js

const express = require('express')

const app = express()
const dotenv = require('dotenv').config() // add here

const port = process.env.PORT || 8080

app.listen(port, () => {
  console.log(`Server is running on port ${port}...`)
})
```

We can now use connectDB() in `index.js`

```js
const express = require('express')

const dotenv = require('dotenv').config()
const connectDB = require('./config/db')

const port = process.env.PORT || 8080

connectDB() // add here

const app = express()

// optional
app.get('/', (req, res) => {
  res.json({ message: 'Hello World' })
})

app.listen(port, () => {
  console.log(`Server is running on port ${port}...`)
})
```

Run the server and you should be able to see:

```js
Server is running on port 8080...
MongoDB Connected: example-example.blahblah.mongodb.net
```

## Part 4: Cross-origin resource sharing (CORS)

### Same-origin policy

- The same-origin policy is a security mechanism enforced by the browser that restricts how a document or script loaded by one origin can interact with a resource from another origin.

- Basically, if the origin of the resource is different from the origin of the document or script that is trying to access it, the browser will block the interaction.

- This helps isolate potentially malicious documents, reducing possible attack vectors. Can help protect users from malicious websites.

Problem 🤔 : but we need to access our API from a different origin! This is where CORS comes in.

### CORS

- CORS is a mechanism that allows web servers to specify which cross-origin requests are allowed.

- This allows developers to relax the same-origin policy in a controlled way, while still maintaining a high level of security.

Note: CORS can be set up with Express relatively easily through a middleware. The official docs can be found [here](https://expressjs.com/en/resources/middleware/cors.html#enabling-cors-pre-flight).

### 4.1 Setting up CORS

Ideally, in the production environment, we probably want to specifically only allow access to our backend resources from our frontend website. However, for ease of development purposes, we will allow access from all origin for now.

To set up CORS, in the `backend` directory, run the command to install cors package:

```
npm install cors
```

Update `index.js` to add the CORS middleware:

```js
// index.js

const express = require('express')

const dotenv = require('dotenv').config()
const connectDB = require('./config/db')

const cors = require('cors') // add

const port = process.env.PORT || 8080

connectDB()

const app = express()

app.use(cors()) // add

// optional
app.get('/', (req, res) => {
  res.json({ message: 'Hello World' })
})

app.listen(port, () => {
  console.log(`Server is running on port ${port}...`)
})
```

## Part 5: REST API

### 5.1 Initial directory structure

Here, we will briefly go over our backend structure.

```
├── backend
│   ├── config
│   ├── controllers
│   ├── middlewares
│   ├── models
│   └── routes
```

- `config`: this is where we intialize our mongoDB
- `controllers`: define all the controllers needed for the application
- `middlewares`: contains our defined middlewares
- `models`: data models required for the application
- `routes`: a folder for each logical set of routes

This visualization summarizes our backend project structure:

![backend-structure](https://github.com/Punpun1643/CS3219-labs/assets/60144099/7abd018e-9807-4a96-91ee-613d18b3c602)

### 5.3 `GET` - get all addresses

#### 5.3.1 Define the address model

An address will have 2 attributes: address title, and address description. We need to define this address model.

In the `models` directory, create `addressModel.js` with the following:

```js
// addressModel.js

const mongoose = require('mongoose')

const addressSchema = mongoose.Schema({
  title: {
    type: String,
    required: [true, 'Please enter address title'],
  },
  description: {
    type: String,
    required: [true, 'Please enter address description'],
  },
})

module.exports = mongoose.model('Address', addressSchema)
```

#### 5.3.2 Define API routes

Our api endpoints for addresses will look something like this: `/api/addresses/...` e.g. `/api/addresses/:id`

Before definding the specific endpoints in the `routes` directory, we can add the following to `index.js`:

```js
// index.js

const express = require('express')

const dotenv = require('dotenv').config()
const connectDB = require('./config/db')

const cors = require('cors')

const port = process.env.PORT || 8080

connectDB()

const app = express()

app.use(cors())
app.use(express.json()) // parse JSON data available in request body
app.use(express.urlencoded({ extended: false })) // parse URL-encoded data available in request body

app.use('/api/addresses', require('./routes/addressRoutes')) // add

// optional
app.get('/', (req, res) => {
  res.json({ message: 'Hello World' })
})

app.listen(port, () => {
  console.log(`Server is running on port ${port}...`)
})
```

In the `routes` directory, create `addressRoutes.js`, this is where we will define the specific enpoints.

```js
// addressRoutes.js

const express = require('express')
const router = express.Router()

router.route('/')

module.exports = router
```

#### 5.3.3 `getAddresses` controller - get all addresses

In the `controllers` directory, create `addressController.js`. Here, we will define a controller that deals with getting all the addresses from our database:

```js
// addressController.js

const Address = require('../models/addressModel')

// @desc    Get all addresses
// @route   GET /api/addresses
// @access  Public
const getAddresses = async (req, res) => {
  const addresses = await Address.find({})

  res.status(200).json(addresses)
}

module.exports = { getAddresses }
```

In `addressRoutes.js`, we can use `getAddresses` as follow:

```js
// addressRoutes.js

const express = require('express')
const router = express.Router()

const {
    getAddresses
  } = require('../controllers/addressController')

router.route('/').get(getAddresses) // here

module.exports = router
```

This means that the endpoint to get all addresses is `/api/addresses/`

To test whether the endpoint work correctly, you can use postman and make a `GET` request to `localhost:8080/api/addresses/`

The output should be as follow if there is no address object in the DB:

![2C62B77F-267D-4EEC-9D84-ABC86CD7C314](https://github.com/Punpun1643/CS3219-labs/assets/60144099/a6ada0a4-e38f-48cf-8b0a-11f8ceb3b200)

However, if you were to manually add an address object into the DB, it could look as follow:

![8F778A9A-2511-4588-BC36-844F25992B1C](https://github.com/Punpun1643/CS3219-labs/assets/60144099/fee6ff86-f10d-4ce8-b019-9715de4d44dc)

### 5.4 `POST` - create an address

Similar to `GET` request, we can implement the creation of an address as follow:

```js
// addressRoutes.js

const express = require('express')
const router = express.Router()

const { getAddresses, addAddress } = require('../controllers/addressController')

router.route('/').get(getAddresses).post(addAddress)

module.exports = router
```

```js
// addressController.js

...

// @desc    Add an address
// @route   POST /api/addresses
// @access  Public
const addAddress = async (req, res) => {
  const { title, description } = req.body

  if (!title || !description) {
    return res.status(400).json({ message: 'Please enter all fields.' })
  }

  // catch exception when fields are missing
  try {
    const address = await Address.create({
      title,
      description,
    })

    res.status(201).json({
      _id: address._id,
      title: address.title,
      description: address.description,
    })
  } catch (error) {
    res.status(400).json({ message: 'Invalid address data.' })
  }
}

module.exports = { getAddresses, addAddress }
```

You could also add more checks e.g. prevent adding addresses with the same name etc.

To test the endpoint, you can try making a `POST` request to the endpoint as follow:



### 5.5 `DELETE` - delete an address

```js
// addressRoutes.js

const express = require('express')
const router = express.Router()

const {
  getAddresses,
  addAddress,
  deleteAddress,
} = require('../controllers/addressController')

router.route('/').get(getAddresses).post(addAddress)
router.route('/:id').delete(deleteAddress) // add 

module.exports = router
```

```js
// addressController.js
...

// @desc Delete goal
// @route DELETE /api/goals/:id
// @access Private
const deleteGoal = asyncHandler(async (req, res) => {
  const goal = await Goal.findById(req.params.id)

  if (!goal) {
    res.status(400)
    throw new Error('Goal not found!')
  }

  if (!req.user) {
    res.status(401)
    throw new Error('User not authorized!')
  }

  await goal.deleteOne()

  res.status(200).json({ id: req.params.id })
})
```

To test the endpoint, you can try making a `DELETE` request as follow:



### 5.6 `PUT` - update an address

```js
// addressRoute.js

const express = require('express')
const router = express.Router()

const {
  getAddresses,
  addAddress,
  deleteAddress,
  editAddress,
} = require('../controllers/addressController')

router.route('/').get(getAddresses).post(addAddress)
router.route('/:id').delete(deleteAddress).put(editAddress)

module.exports = router

```

```js
// addressController.js

// @desc Edit an address
// @route PUT /api/addresses/:id
// @access Public

const editAddress = async (req, res) => {
  const { title, description } = req.body

  if (!title || !description) {
    return res.status(400).json({ message: 'Please enter all fields.' })
  }

  try {
    const address = await Address.findById(req.params.id)

    address.title = title
    address.description = description

    await address.save()

    res.status(201).json({
      _id: address._id,
      title: address.title,
      description: address.description,
    })
  } catch (error) {
    res.status(400).json({ message: 'Invalid address data.' })
  }
}
```

To test the endpoint, you can try making a `PUT` request as follow:




## Part 6: Integration with the frontend

### 6.1 

### 6.2

### 6.3