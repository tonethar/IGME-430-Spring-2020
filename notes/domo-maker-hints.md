# Domomaker HW Hints/Tips
- See the myCourses dropboxes (aka Assignments) for due dates and submission instructions
- The instructions are in PDF form and are in the myCourses content section



## Domomaker-A  - Tips
- pretty straight forward, just type the code in, carefully ...
- run the project with  `npm run nodemon` so that it re-boots every time you make an edit
- also run `npm test` a lot to catch your typos early



## Domomaker-B - Tips
- follow the instructions, and run the app with  `npm run nodemon` whenever it tells you to test the app to be sure it works
- also run `npm test` a lot to catch your typos early
- if you get ‘undefined’ errors, your imports and exports are likely off, or you misspelled a property. Logging out values like this will help with debugging:
  - `console.log(\`Domo = ${Domo}, filename=${__filename}\`);`
- In step #11, you’ll need to put that code into a new file in the **models** folder named **Domo.js**
- In step #15, you’ll need to put that code into the **Domo.js** file located in the **controllers** folder 
- in step #17, you need to login (after the server restarts) before you can add a new Demo. You can make sure you are getting new domos added by heading to terminal/gitbash/powershell/whatever:
    - run `mongo` in another shell window
    - `use DomoMaker`
    - `db.domos.find().pretty()`
    - if you try to add a demo without logging in, it throws an error
- the rest is pretty straightforward
