# MongoDB - Intro to mongo shell

- The video for this demo is here --> [MongoDB - Intro to mongo shell (17:51)](https://video.rit.edu/Watch/rm2-intro-to-mongodb-shell)
- also see:
  - the above video demo is using the `mongo` & `mongod` applications in Terminal (Powershell, etc), but this semester we are doing everything in the Mongo Cloud, so you need to do the following:
    - Complete the "MongoDBCloudSetup" PDF in  Week 7 of myCourses - this walks you though setting up a cloud account for Mongo. Be sure to download and setup the **MongoDB Compass** application as specified in the instructions. For now you can skip over the last section of the PDF that shows how to hook up MongoDB with Heroku.
    - At the bottom of the **MongoDB Compass** Application, go ahead and open up the provided Mongo shell - see "_MongoSH Beta" in the screenshot below:
    
[!screeshot](./_images/intro-mongo-shell-1.png)
  
  
<hr>
  

## I. MongoDB Demo

- We're going to run through the C.R.U.D. (Create, Read, Update, Delete) operations of MongoDB:
  - https://docs.mongodb.com/manual/crud/
 
<hr>

1) Install MongoDB
    - https://docs.mongodb.com/manual/administration/install-community/
    - don't miss the step where you create a "data/db" folder 
    - also see "Setting up Mongo on Lab Machines" PDF in mycourses if you get stuck anywhere  

<hr>

2) Launch mongod
    - `mongod` is the actual mongo *server*
    - see install instructions above on how to launch for your OS
    - On OS X I'll launch `mongod` by typing: `brew services start mongodb-community@4.2`

<hr>

3) Launch mongo
    - the `mongo` shell is how we talk to the `mongod` server
    - see install instructions above on how to launch for your OS
    - on OS X I'll launch mongo by typing `mongo` 

<hr>

4) See the default databases
    - type: `show dbs`
    - you should see `admin`, `local`, and `config`
    - databases are used to group related *collections*
    - each application we make is going to have its own database
    - each database will have multiple collections

<hr>

5) Create or switch databases
    - type: `use game` - this creates a new database named `game` if it doesn't already exist
    - type: `db` to see what the current database is
    - You can delete the current database with `db.dropDatabase()`

<hr>

6) Create and show a collection
    - if you are familiar with SQL, a *collection* is analogous to a *table* (also to the columns of an Excel spreadsheet):
      - https://www.cs.montana.edu/~halla/csci440/n3/figure-3-1.png
    - type: `db.createCollection('players')` to create a new collection named `players`
    - type: `show collections` to see the collections for the current database

<hr>

7) Insert a *document*
    - if you are familiar with SQL, a *document* is analogous to a *tuple* (or *row* or *record*)
    - https://docs.mongodb.com/manual/core/document/
    - `db.<collection-name>.insert()` will add a document to a collection
    - a document is a collection of *field : value* pairs
  
  ```js
  db.players.insert({
	name: 'Dak',
	class: 'Ranger',
	hitpoints: 100,
	dateCreated: Date(),
	weapons: ['Long Sword', 'Dagger'],
	armor: {
		name: 'Plate Mail',
	 	resistance: 8,
	 	weight: 60
	}
  })
  ```
  
  - above we inserted a document with `name`, `class`, `hitpoints`, `dateCreated`, `weapons`, and `armor` fields
  - the data *types* we used were string, string, integer, datetime, array, and object
  - the format of the data is very similar to JSON and is called BSON ("Binary JSON") - the primary difference between the 2 formats is that BSON supports more datatypes that JSON does:
    - https://en.wikipedia.org/wiki/BSON

<hr>

8) View the contents of a collection
    - type: `db.players.find()`
    - type: `db.players.find().pretty()` // "pretty printing"
    - note the unique `_id` field

<hr>

9) Insert another *document*

  ```js
  db.players.insert({
	name: 'Lothar',
	class: 'Wizard',
	hitpoints: 12,
	magicpoints: 20
  })
  ```

  - note that unlike when you work with database tables, when we add documents to a collection, they can have additional fields, or missing fields, compared to the other entries. This makes noSQL databases like Mongo much more flexible
  - `db.players.find().pretty()`
  - `db.players.find().count()`
  - `db.players.find({class:'Wizard'}).pretty()`
  - `db.players.find({_id: ObjectId('<objectID>')}).pretty()`
  - `db.players.find().sort({hitpoints: 1}).pretty()` // sorted by `hitpoints` ascending
  - `db.players.find().sort({hitpoints: -1}).pretty()` // sorted by `hitpoints` descending
  - `db.players.find({hitpoints: { $gt: 12}}).pretty()` // those with `hitpoints` greater than 12
  - `db.players.find({hitpoints: { $gt: 12}}).count()`
 
<hr>

10) Insert multiple documents

    - use `db.<collection-name>.insertMany()`

  ```js
  db.monsters.insertMany([
	{
		species: 'Orc',
		hitdice: 2
	},
	{
		species: 'Goblin',
		hitdice: 1
	},
	{
		species: 'Bugbear',
		hitdice: 4
	}
  ])
  ```

  - above we actually created a new collection named `monsters`
  - `db.monsters.find()`
  - `db.monsters.find().limit(2).pretty()`  // just get the first 2

<hr>

11) foreach

  ```js
  db.monsters.find().forEach(function(doc) {
    print("Monster species: " + doc.species)
  })
  ```

<hr>

12) update document

  - first match:

  ```js
  db.monsters.update({ species: 'Goblin' },
    {
	$set:{
  	hitdice: 3,
    }
  })
  ```
  - all matches:
  
  ```js
    db.monsters.updateMany({ species: 'Goblin' },
    {
      $set:{
	hitdice: 10,
      }
    })
  ```

<hr>

13) Increment a field

  ```js
  db.players.update({ name: 'Dak' },
  {
    $inc: {
      hitpoints: 10
    }
  })
  ```

<hr>

14) Delete a document

  ```js
  db.monsters.remove({ species: 'Bugbear' })
  ```

<hr>

15) Quit the shell

    - type: `exit`

<hr>

## II. Homework

- Super Easy, barely an inconvenience:
  - add a `magicitems` collection
  - documents in the `magicitems` collection will have 3 fields: `name` (a string), `magicStrength` (an integer), `valueInGold` (an integer)
  - create 3 documents and add them to `magicitems` - initialize the field values to something meaningful
  - execute a `find().pretty()` command so that their values are dumped to the console
  - take a screen shot of the console
  - post the screenshot to myCourses

