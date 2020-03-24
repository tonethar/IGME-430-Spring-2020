# Express Handlebars Demo

## I. Overview

- Handlebars works similar to Vue.js (330), *except* the rendering is happening on the server-side instead of the client-side
- Today's start code was adapted from here:
  - https://github.com/ericf/express-handlebars
- Head to myCourses for the ZIP 

## II. Handlebars Demo

- Start Code:
  - https://github.com/IGM-RichMedia-at-RIT/simple-mvc-templates
  - https://github.com/IGM-RichMedia-at-RIT/simple-mvc-templates-done

Reference:
- handlebars:
  - https://handlebarsjs.com
  - https://www.npmjs.com/package/express-handlebars
  - https://github.com/ericf/express-handlebars
- express:
  - https://www.npmjs.com/package/express
  - https://expressjs.com/en/api.html

I. Look at Start Code
- we have a new dependency in package.json:
  - express-handlebars
- file structure:
  - app.js (needs code)
  - router.js (all done!)
  - controllers/index.js has the (stubbed in) functions that the router calls
  - see the .handlebars files in the views folder
- cd into folder
- npm i

II. Head to app.js
- import express-handlebars:
  - `const expressHandlebars = require('express-handlebars');`
- FYI - in app.use() note that we are putting all the static files under the /assets endpoint
- set the "template engine"
  - http://expressjs.com/en/4x/api.html#app.engine

// here we are saying "don't use a layout" (a layout is a "super template")
// A layout is simply a Handlebars template with a {{{body}}} placeholder. Usually it will be an HTML page wrapper into which views will be rendered.
app.engine('handlebars', expressHandlebars({
  defaultLayout: '',
}));

// set up the view (V of MVC) to use handlebars
// Setting the app's "view engine" setting will make that value the default file extension used for looking up views
// https://expressjs.com/en/api.html#app.set
app.set('view engine','handlebars');

// set the views path to the template directory
// https://expressjs.com/en/api.html#app.set
app.set('views',`${__dirname}/../views/`);
III. Head to views/index.handlebars
- this is a template
- just like in 330's Vue.js, {title} will be replaced by the contents of a `title` variable that gets passed in

IV. Head to controllers/index.js
- index.js, complete `hostIndex`
- remember last time we had `res.send()` and `res.sendFile()`?
 - this time we have `res.render()`


// https://expressjs.com/en/api.html#res.render
const hostIndex = (req, res) => {
// res.render takes a name of a page to render.
// These must be in the folder you specified as views in your main app.js file
// Additionally, you don't need .handlebars because you registered the file type
// in the app.js as handlebars. Calling res.render('index')
// actually calls index.handlebars. A second parameter of JSON can be passed
// into the handlebars to be used as variables with #{varName}
  res.render('index', {
    currentName: name,
    title: 'Home',
    pageName: 'Home Page',
  });
};
- npm run nodemon and test in browser - index will be served - the page title will be what we passed in under the `title` key (we're not using the other 2 yet)

V. Head to index.handlebars

- Add this and note the changes in the browser:

<h1>Welcome to {pageName}</h1>
<p>The current set name is {currentName}</p>
VI. Go to controllers/index.js

// A. for `hostPage1()`
res.render('page1', {
  title: 'Page 1',
});
- now go to page1 in browser - where's the title?
- edit page1.handlebars to utilize the title

// B. for `hostPage2()`
// not passing any data
res.render('page2');
- now go to page2 in browser

// C. for `notFound()`


// send back status code of 404
// pass in the name of the page the user typed into the location box
// https://expressjs.com/en/api.html#res.status
// https://expressjs.com/en/api.html#req.originalUrl
res.status(404).render('notFound', {
page: req.url,
});
- test in browser with bad route
- Fix typo! - to get image to show up, change the static file hosting from client to hosted

VII. Still in controllers/index.js
- implement `getName()`


// one-liner! bodyParser handles this for us 
// we've used res.json() before
// https://expressjs.com/en/api.html#res.json
res.json({ name });

- test /getName endpoint in browser location box


VIII. Still in controllers/index.js
- implement `setName()` - which is a POST request


// check if the required fields exist
// normally you would also perform validation to know
// if the data they sent you was real
if (!req.body.firstname || !req.body.lastname) {
  // if not respond with a 400 error
  // (either through json or a web page depending on the client dev)
  res.status(400).json({ error: 'firstname and lastname are both required' });
} else {
  // if required fields are good, then set name
  name = `${req.body.firstname} ${req.body.lastname}`;

  // respond with our new name updated.
  // This could just be a 200 response to say the name was good
  // but we will also throw in the json of the new name just for convenience
  res.json({ name });
}

- test from page2 form:
  - now we can see the entered name on the home page and at /getName
  - look at page2.handlebars to see how data is passed from the <form> to /setName
