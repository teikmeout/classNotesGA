# Express

[Express](http://expressjs.com/) is a minimal and **_flexible Node.js web application framework_** that provides a robust set of
features for web and mobile applications.

#### Table of Contents
* [Setting up express](#setting-up-express)
* [MVC](#mvc)
* [Setting up server.js](#setting-up-server)
* [Setting up routes](#setting-up-routes)

Since this is an Express server with Node.js dependency we need to run node.
Everytime we make changes the server has to be refreshed. We use **`nodemon`** for that.
```
npm install -g nodemon
```
> g = global, not in a local directory.


## Setting up express  

You need Node Package Manager.
In terminal you need to setup npm.
```
npm init
```
> This part will take you through a process of preference selection. It's important to point at our correct server file by it's name, be it `server.js` or `app.js`.

Then install the correct dependencies for Express. The `--save` option makes sure you save this preference in the `package.json` created by `npm`.
```
npm install --save express
```

We need morgan to view the server logger with a specific display.
```
npm install --save morgan
```

For the backend view rendering we choose [EJS](http://www.embeddedjs.com/).
**EJS** is short for Embeded Javascript and helps render JS info on HTML via backend.
```
npm install --save ejs
```

So we need public linking to work when deploying, so we use PATH
```
npm install --save path
```

**All of the install process can be done in one single line of code**.
```
npm install --save express morgan ejs path
```


> At this point it's important to create a `.gitignore` and include `node_modules`. This is also a good place to add [**ESLINT**](http://eslint.org/) for code linting

Adding ESLINT
```
eslint --init
```
> This will run a process to chose a styleguide and what preferences you want, we're using *AirBnB* JS styleguide and I like to see my info in a JS format.



## MVC

Directory boilerplate
```
Project/
  |_ public/
  |     |_ css/
  |     |   |_ style.css
  |     |_ images/
  |_ routes/
  |     |_ Index.js
  |_ services/
  |     |_ weather.js
  |     |_ quotes.js
  |_ views/
  |     |_ Index.ejs
  |     |_ Search.ejs
  |_ server.js
```

## Setting up server
Opening up our `server.js` or `app.js` we start with this code

```javascript
// "importing" express package
const express = require('express'); 
// "importing" morgan logger package
const logger = require('morgan');
// creating a new instance of Express App
const app = express();
// selecting port options
const port = process.argv[2] || process.env.PORT || 3000;
// using dev mode for logger
app.use(logger('dev'));

// opening listening port of our server
app.listen(port, () => console.log(`The server is running on port ${port}!!`));
```

### Setting up EJS
Inside of the `server.js` file add this code
```javascript
// setting view to ejs
app.set('view engine', 'ejs');
// pointing at the right directory to go look for views
app.set('views', './view');
```


### Setting up Path
Path is used to set fixed paths toward our public and static files
This will allow us to call up on our `css` directory even when it's not in the same place (eg. Heroku)
Inside our `server.js` file we add 
```javascript
// "importing" path package
const path = require('path');

// telling express that all static files come from this folder
app.use(express.static(path.join(__dirname, 'public')));
```
This is the file structure we are allowing to work with
```
Project/
  |_ public/
       |_ css/
       |   |_ style.css
       |_ images/
```

### Setting up routes
In your routes folder we will start up an `Index.js` file to determine how it will route it's information
```javascript
// using express routes in this file
const router = require('express').Router();

// HOME route
// this method uses GET
router.get('/', (req, res) => {
  res.render('index');
}); // end of router

// sending that module our of our JS file to the world
module.exports = router;
```

In our `server.js` file we will go and add our route
```javascript
// setting up the homeRoute
const homeRoute = require('./routes.index');

// defining route for '/'
app.use('/', homeRoute);
```
> Great idea to have inside of our `views` directory our `index.ejs` file ready with some boilerplate
```
Project/
  |_views/
      |_ index.ejs
```

Now we want to create a new file in our `routes` directory named very similar to our route name
```
Project/
  |_routes/
      |_ home.js 
```

Inside `home.js` we're going to require express in it's router version
```javascript
// using express routes in this file
const router = require('express').Router();

// HOME route
router.get('/', (req, res) => {
  res.render('map');
}); // end of router

// allowing external access of this module
module.exports = router;
```
> `req` represents the request object coming from the user, `res` is the response object that we are going to send back.

### Setting up environmental variables
Sometimes we might be interested in saving a API key or some kind of variable that you don't want to upload into github but at the same time you need to access your information
We use `.env` to get this functionality
Adding dependencies to `npm`
```
npm install --save dotenv
```

Now we create a `.env` file inside our project root
```
Project/
  |_ views/
  |_ .env
```

Inside `.env` we will save the key that we are going to use
```
MAPS_KEY=fdasfdsafdsafdsafdsafdsafdsafdsafdsafdsafdsafdsafdsa
```
> All caps means a const variable and we don't need to export this module since it's only useful fo `.env`






NOTE: more on [Github Markdown](https://help.github.com/categories/writing-on-github/) for this post.
