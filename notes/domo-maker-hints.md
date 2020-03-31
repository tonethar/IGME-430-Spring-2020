# Domomaker HW Hints/Tips
- See the myCourses dropboxes (aka Assignments) for due dates and submission instructions
- The instructions are in PDF form and are in the myCourses content section



## Domomaker-A  ... things to watch for
- pretty straight forward, just type the code in, carefully ...
- run the project with  `npm run nodemon` so that it re-boots every time you make an edit
- also run `npm test` a lot to catch your typos early
- after you deploy to Heroku, if things are not working, check the app logs from your console with:
  - `heroku logs -t --app APP-NAME`



## Domomaker-B ... things to watch for
- follow the instructions, and run the app with  `npm run nodemon` whenever it tells you to test the app to be sure it works
- also run `npm test` a lot to catch your typos early
- if you get ‘undefined’ errors, your imports and exports are likely off, or you misspelled a property. Logging out values like this will help with debugging:
  - ``console.log(`Domo = ${Domo}, filename=${__filename}`);``
- In step #11, you’ll need to put that code into a new file in the **models** folder named **Domo.js**
- In step #15, you’ll need to put that code into the **Domo.js** file located in the **controllers** folder 
- in step #17, you need to login (after the server restarts) before you can add a new Domo. You can make sure you are getting new Domos added by heading to terminal/gitbash/powershell/whatever:
    - run `mongo` in another shell window
    - `use DomoMaker`
    - `db.domos.find().pretty()`
    - PS - if you try to add a Domo without logging in, it throws an error
- the rest is pretty straightforward
- after you deploy to Heroku, if things are not working, check the app logs from your console with:
  - `heroku logs -t --app APP-NAME`
  
## Domomaker-C ... things to watch for 

- Before you get started, follow the instructions in the “Setting up Redis for Local use” PDF in myCourses

- In step #5, don’t forget to start up `mongod`, and when you head to the browser, go to the “/“ path to login. 

- Note: The instructions in step #5 refer to the fact that you need Redis “running for this”, and later on in step #6 it says “BUT keep Redis open”, and later on in step #8 it refers to Redis “running as a service”. You can ignore these directives because we have set up Redis the the cloud, not on our local machine,  per the “Setting up Redis for Local use” PDF.

- In step #7, to run 2 instances of the server at the same time on different ports, do the following:
    - kill the server, and then restart it with `npm start`  (NOT `npm run nodemon`)
    - change the port number in the source code
    - make a second powershell window and start the server with `npm start`


- In steps #8-#10, ignore these steps. They are only applicable if you have downloaded and installed Redis

- after you deploy to Heroku, if things are not working, check the app logs from your console with:
  - `heroku logs -t --app APP-NAME`
  
  
  ## Domomaker-D ... things to watch for
  
  - Prior to beginning the Domomaker-D, watch the React videos by Austin:
    - [React FSCs](https://youtu.be/kAMb0sEp9js) 
    - [React Class Components](https://youtu.be/EzgxSVN-AzI)
