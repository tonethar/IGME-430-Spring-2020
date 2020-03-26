
# SimpleMVC Walkthrough

## I. Overview
- This demo gives you experiences with:
  - MongoDB and Mongoose
  - Handlebars
  - Express
- You should have already gone through these demos/HW:
  - [Intro to MongoDB Video & HW](./mongo-shell-intro.md)
  - [Intro to Handlebars Video & HW](./express-handlebars-demo.md)
- The videos for this demo are here:
  - coming soon
  - ...
  
<hr><hr>

## II. Files
- start - https://github.com/IGM-RichMedia-at-RIT/Simple-MVC-Example
- done - https://github.com/IGM-RichMedia-at-RIT/simple-mvc-example-done

<hr><hr>

## III. Get started
- Download the start files, `cd` into the folder, and do an `npm install`
- In GitBash/PowerShell - start up `mongod` (the MongoDB *server*)

<hr>

### III-A. Take a look at package.json
  - "mongoose" is a "dependencies" key
  - the mongoose package allows our app to use JS to talk to MongoDB
  - https://www.npmjs.com/package/mongoose
  
<hr>

### III-B. Look in app.js
- note `dbURL`
  - this is the path to `mongod`
  - The string after `mongodb://localhost` is the database name - it can be anything you want
- note `mongooseOptions`
- `mongoose.connect()`
  - add a log to see a 'Connected to MongoDB!' message
  - `npm run nodemon` to see the 'Connected to MongoDB!'  message

<hr>

### III-C. look in router.js
- good to go! All the **express** routes are here

<hr>

### III-D. Look in controllers/index.js
- all the functions that **express** calls (from router.js) are here

<hr>

### III-E. Look in models/Cats.js
- this is empty for now
- this is where we are going to put our `CatModel` and `CatSchema` data

<hr>

### III-F. Look in models/index.js 
- this is interface for entire models folder - see the comments in the file

<hr><hr>


## IV. Head to Cat.js

### IV-A. Import the `mongoose` library

```js
// require mongoose, a popular MongoDB library for nodejs
// Mongoose connects in the app.js file. Once mongoose is connected,
// it stays connected across all of the files in this project
// As long as you import this after you have connected,
// then mongoose will give you the active DB connection which is what we want
const mongoose = require('mongoose');
```

<hr>

### IV-B. Declare `CatModel`

```js
// variable to hold our Model
// A Model is our data structure to handle data. This can be an object, JSON, XML or anything else.
// A mongoDB model is a Mongo database structure with the API attached
// That is, a model has built-in functions for its data structure like find, findOne, etc.
// Usually you will retrieve data from the database through the Model object
let CatModel = {};
```

<hr>

### IV-C. Create `CatSchema`

```js
// A DB Schema to define our data structure
// The schema really just defines a DB data structure.
// In this case it also defines what functions/methods will be attached to objects that come
// back from the database.
// Schemas also add constraints to the fields so that you can enforce that
// objects have fields of the right type
// This should always be done since it ensures your variables will be the right type and
// it helps prevent injection of invalid data
// Schemas are made in JSON.
// The name of the field will be the variable name for each object
// The json of each field are the constraints around it
// There are many constraints available
// For example,
// type is the data type (String, Number, Date, Boolean, etc).
// required is whether or not the field is required to allow a document to be created
// trim is whether or not the field should strip spaces before and after value
// unique is whether or not the field must be a unique value
// (meaning no two Cat object can have the same value for that field)
// min is the minimum numeric value
// max is the maximum numeric value
// default is the default value if one is not provided
// match is the format to match done through regex
const CatSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    trim: true,
    unique: true,
  },

  bedsOwned: {
    type: Number,
    min: 0,
    required: true,
  },

  createdData: {
    type: Date,
    default: Date.now, // will call function at runtime
  },

});
```

<hr>

### IV-D. Add a static `findByName()` method to `CatSchema` 

```js
// Schema.statics are static methods attached to the Model or objects
// These DO NOT have their own instance. They are all the static function.
// They do not look at individual instance variables since there is
// no instance of them. Every static function
// only exists once and is called.
// In this case, findByName will be attached to the model and objects.
// They will be able to call this function, but they won't be able to
// reference any instance variables of that object (or at least accurately)
// These are used when you want a public function you can call to do a task,
// not a method that uses or returns instance variables
// That is, these are used when you don't need an object, just a function to call.
CatSchema.statics.findByName = (name, callback) => {
  const search = {
    name,
  };

  return CatModel.findOne(search, callback);
};
```

<hr>

### IV-E. Instantiate the `CatModel`

```js
// Create the cat model based on the schema. You provide it with a custom discriminator
// (the name of the object type. Can be anything)
// and the schema to make a model from.
// Look at the model variable definition above for more details.
CatModel = mongoose.model('Cat', CatSchema); // this ties `CatModel`  to `CatSchema` 
```

<hr>

### IV-F. Export `CatModel` & `CatSchema`

```js
// export our public properties
module.exports.CatModel = CatModel;
module.exports.CatSchema = CatSchema;
```

<hr><hr>

## V. Head to controllers/index.js

### V-A. Create a `Cat`

```js
// get an alias to the Cat model
const Cat = models.Cat.CatModel;
```

**and**

```js
// object for us to keep track of the last Cat we made and dynamically update it sometimes
let lastAdded = new Cat(defaultData);
console.dir(`lastAdded=${lastAdded}`);
```

<hr>

### V-B. Is everything working?

- check the console for the log of `lastAdded`
- fire up `mongo`
  - `show dbs` to see "simpleMVCExample"
  - `use simpleMVCExample`
  - show collections to see "cats"
  - `db.cats.find().pretty()`  - no documents in the collection yet

<hr>
  
### V-C. Complete the `hostIndex()` function

- see `res.render()` below:
  - index.handlebars is getting rendered
  - `currentName`, `title`, and `pageName` are getting passed in

```js
// function to handle requests to the main page
const hostIndex = (req, res) => {
  // res.render takes a name of a page to render.
  // These must be in the folder you specified as views in your main app.js file
  // Calling res.render('index') actually loads index.handlebars
  // A second parameter of JSON can be passed
  res.render('index', {
    currentName: lastAdded.name,
    title: 'Home',
    pageName: 'Home Page',
  });
};
```

<hr>

### V-D. Is it working?

- launch server to be sure that "/" page will load
  - and we can see the default cat name "unknown"

<hr>

### V-E. Complete the `readAllCats()` function (for page1.handlebars)

```js
// function to find all cats on request.
// Express functions always receive the request and the response.
const readAllCats = (req, res, callback) => {
  // Call the model's built in find function and provide it a
  // callback to run when the query is complete
  // Find has several versions
  // one parameter is just the callback
  // two parameters is JSON of search criteria and callback.
  // That limits your search to only things that match the criteria
  // The find function returns an array of matching objects
  // The lean function will force find to return data in the js
  // object format, rather than the Mongo document format.
  Cat.find(callback).lean();
};
```

<hr>

### V-F. Complete the `readCat()` function

- note the "error first" callback below

```js
// function to find a specific cat on request.
// Express functions always receive the request and the response.
const readCat = (req, res) => {
  const name1 = req.query.name;

  // function to call when we get objects back from the database.
  // With Mongoose's find functions, you will get an err and doc(s) back
  const callback = (err, doc) => {
    if (err) {
      return res.status(500).json({ err }); // if error, return it
    }

    // return success if everything is OK
    return res.json(doc);
  };

  // Call the static function attached to CatModels.
  // This was defined in the Schema in the Model file.
  // This is a custom static function added to the CatModel
  // Behind the scenes this runs the findOne method.
  // You can find the findByName function in the model file.
  Cat.findByName(name1, callback);
};
```

<hr>

### V-G. Complete the `hostPage1()` function

```js
// function to handle requests to the "/page1" route
// controller functions in Express receive the full HTTP request
// and a pre-filled out response object to send
const hostPage1 = (req, res) => {
  // function to call when we get objects back from the database.
  // With Mongoose's find functions, you will get an err and doc(s) back
  const callback = (err, docs) => {
    if (err) {
      return res.status(500).json({ err }); // if error, return it
    }

    // return success
    return res.render('page1', { title: "All Cats", cats: docs });
  };

  readAllCats(req, res, callback);
};
```

<hr>

### V-H. Is it working?

- Go look at **page1.handlebars**
- test the "/page1" route:
  - if there's data it will show
  - but we still haven't put anything in our MongoDB database, so we'll see nothing!

<hr>

### V-I. Add titles to `hostPage2()` and `hostPage3()`
- "Add Cat"
- "Page 3"
- now check **page2**  and **page3** to see the changes

<hr>

### V-J. Complete the `getName()` function

```js
// function to handle get request to send the name
const getName = (req, res) => {
  // res.json returns json to the page.
  // Since this sends back the data through HTTP
  // you can't send any more data to this user until the next response
  res.json({ name: lastAdded.name });
};
```

- now test the **/getName** endpoint in the browser
- head to **page1** to see it 

<hr><hr>

## VI. Let's start updating our database!


### VI-A. Complete the `setName()` function

- the **/setName** endpoint currently gives us a 404 page
- here's the code: 

```js
// function to handle a request to set the name
// controller functions in Express receive the full HTTP request
// ADDITIONALLY, with body-parser we will get the
// body/form/POST data in the request as req.body
const setName = (req, res) => {
  // check if the required fields exist
  // normally you would also perform validation
  // to know if the data they sent you was real
  if (!req.body.firstname || !req.body.lastname || !req.body.beds) {
    // if not respond with a 400 error
    // (either through json or a web page depending on the client dev)
    return res.status(400).json({ error: 'firstname,lastname and beds are all required' });
  }

  // if required fields are good, then set name
  const name = `${req.body.firstname} ${req.body.lastname}`;

  // dummy JSON to insert into database
  const catData = {
    name,
    bedsOwned: req.body.beds,
  };

  // create a new object of CatModel with the object to save
  const newCat = new Cat(catData);

  // create new save promise for the database
  const savePromise = newCat.save();

  savePromise.then(() => {
    // set the lastAdded cat to our newest cat object.
    // This way we can update it dynamically
    lastAdded = newCat;
    // return success
    res.json({ name: lastAdded.name, beds: lastAdded.bedsOwned });
  });

  // if error, return it
  savePromise.catch((err) => res.status(500).json({ err }));

  return res;
};

```

<hr>

### VI-B. Is it working?
- BTW - The above code is using JS *promises* instead of *callbacks* - conceptually they are similar, except promises allow for *method chaining* as opposed to *nesting*, which many developers regard as more clear code:
  - https://www.geeksforgeeks.org/javascript-promises/
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises
  - https://javascript.info/promise-basics
  - https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261
  - [Coding Train: Promises Part 1 - Topics of JavaScript/ES6](https://www.youtube.com/watch?reload=9&v=QO4NXhWo_NM)
- Now test **/setName** from **page2** and add some cat info
- Head to **page1** to see the change 
- Also double check `mongo` with `db.cats.find().pretty()` - you should see the cat you created!

<hr>

### VI-C. Searching for a name

- the **/findByName** endpoint currently doesn't work
- here's the code:

```js
// function to handle requests search for a name and return the object
const searchName = (req, res) => {
  // check if there is a query parameter for name
  // BUT WAIT!!?!
  // Why is this req.query and not req.body like the others
  // This is a GET request. Those come as query parameters in the URL
  // For POST requests like the other ones in here, those come in a
  // request body because they aren't a query
  // POSTS send data to add while GETS query for a page or data (such as a search)
  if (!req.query.name) {
    return res.status(400).json({ error: 'Name is required to perform a search' });
  }

  // Call our Cat's static findByName function.
  // Since this is a static function, we can just call it without an object
  // pass in a callback (like we specified in the Cat model
  // Normally would you break this code up, but I'm trying to keep it
  // together so it's easier to see how the system works
  // For that reason, I gave it an anonymous callback instead of a
  // named function you'd have to go find
  return Cat.findByName(req.query.name, (err, doc) => {
    // errs, handle them
    if (err) {
      return res.status(500).json({ err }); // if error, return it
    }

    // if no matches, let them know
    // (does not necessarily have to be an error since technically it worked correctly)
    if (!doc) {
      return res.json({ error: 'No cats found' });
    }

    // if a match, send the match back
    return res.json({ name: doc.name, beds: doc.bedsOwned });
  });
};
```

<hr>

### VI-D. Is it working?
- test it from the **/findByName?name=** GET endpoint
- test it from **page2**

<hr>

### VI-E. Updating an existing cat
- the **/updateLast** endpoint currently gives us a 404 page
- an update is a save on something that already exists
- here's the code:


```js
// function to handle a request to update the last added object
// this PURELY exists to show you how to update a model object
// Normally for an update, you'd get data from the client,
// search for an object, update the object and put it back
// We will skip straight to updating an object
// (that we stored as last added) and putting it back
const updateLast = (req, res) => {
  // Your model is JSON, so just change a value in it.
  // This is the benefit of ORM (mongoose) and/or object documents (Mongo NoSQL)
  // You can treat objects just like that - objects.
  // Normally you'd find a specific object, but we will only
  // give the user the ability to update our last object
  lastAdded.bedsOwned++;

  // once you change all the object properties you want,
  // then just call the Model object's save function
  // create a new save promise for the database
  const savePromise = lastAdded.save();

  // send back the name as a success for now
  savePromise.then(() => res.json({ name: lastAdded.name, beds: lastAdded.bedsOwned }));

  // if save error, just return an error for now
  savePromise.catch((err) => res.status(500).json({ err }));
};
```

<hr>

### VI-F. Is it working?
- test it from **page2**
  - The "Update Last" will work
  - If you try to "submit" the same name you'll get a "duplicate key" error in the browser, and on the command line
  - Note the `lastAdded.bedsOwned++;` line - which are doing just to demo that it's possible

<hr><hr>

## VII. Deploy to Heroku

### Deploy to Heroku

- connect to github
- resources tab:
    - look for mLab MongoDB
    - provision for server
- settings tab
    - config vars
- resource tab
    - I can view contents of MongoDB DB
