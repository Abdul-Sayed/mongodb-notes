# MongoDB Atlas Cheat Sheet

Create New Project. Choose free plan. This generates a cluster. When generated,
click database access
add new user, choose defaults
click network access, add ip address, add current ip address or allow access from anywhere (less secure)
click clusters, connect, connect with applocation, copy standard connection string
Replace the mLab string with this new string; atlas uri

Serve your app

Create a keys_dev.js in the server config folder with
module.exports = {
mongoURI: 'YOUR_OWN_MONGO_URI', => atlas uri, with password inserted
secretOrKey: 'YOUR_OWN_SECRET'
}

# MongoDB Node Driver CheatSheet

npm init
npm install mongodb --save

# MongoDB CLI Cheat Sheet

In bin directory, mongo.exe is the mongo CLI, and mongod.exe is what runs the local mongoDB server.

## Opening Mongo Shell

cd into directory of mongo.exe

    ```
    ./mongo.exe //=> for linix
    ```

starts mongo on port mongodb://127.0.0.1:27017

exit to leave shell

## Show All Databases

Only databases with collections/documents will show

    ````
    show dbs
    ```

## Show Current Database

    ```
    db
    ```

## Create Or Switch to new dbDatabase

    ```
    use acme
    ```

## Drop

Delete current database

    ```
    db.dropDatabase()
    ```

## Show Collections/Documents

For current database

    ```
    show collections
    ```

## Create Collection/Document

db.createCollection('documentName')

    ```
    db.createCollection('posts')
    ```

## Insert Row/Object into collection/document

R click to paste

    ```
    db.posts.insert({
    title: 'Post One',
    body: 'Body of post one',
    category: 'News',
    tags: ['news', 'events'],
    user: {
      name: 'John Doe',
      status: 'author'
    },
    date: Date()
    })
    ```

## Insert Multiple Rows/Objects

    ```
    db.posts.insertMany([
    {
      title: 'Post Two',
      body: 'Body of post two',
      category: 'Technology',
      date: Date()
    },
    {
      title: 'Post Three',
      body: 'Body of post three',
      category: 'News',
      date: Date()
    },
    {
      title: 'Post Four',
      body: 'Body of post three',
      category: 'Entertainment',
      date: Date()
    }
    ])
    ```

## Get All Rows/Objects in Collection/Document

Dont use .pretty() when passing data to another function

    ```
    db.posts.find().pretty()
    ```

## Find Rows

    ```
    db.posts.find({ category: 'News' })

    ```

## Sort Rows

Will sort document alphabetically by object key

    ```
    # ascending

    db.posts.find().sort({ title: 1 }).pretty()

    # descending

    db.posts.find().sort({ title: -1 }).pretty()

`````

## Count Rows

    ```
    db.posts.find().count()
    db.posts.find({ category: 'news' }).count()
    ```

## Limit Rows

    ```
    db.posts.find().limit(2).pretty()
    ```

## Chaining

    ````
    db.posts.find().limit(2).sort({ title: 1 }).pretty()
    ```

## Foreach

Cannot use ES6 syntax

    ```
    db.posts.find().forEach(function(doc) {
    print("Blog Post: " + doc.title)
    })
    ```

## Find One (first) Row/Object

Returns first object with that field

    ```
    db.posts.findOne({ category: 'News' })
    ```

## Find Specific Fields

```

db.posts.find({ title: 'Post One' }, {
title: 1,
author: 1
})

```

## Update Row

Replaces or rewrites the object if found, else upsert:true will upload the new object in case

    ```
    db.posts.update({ title: 'Post Two' },
    {
    title: 'Post Two',
    body: 'New body for post 2',
    date: Date()
    },
    {
    upsert: true
    })
    ```

## Update Specific Field

Set some specific field, leaving other fields of object intact

    ```
    db.posts.update({ title: 'Post Two' },
      {
        $set: {
          body: 'Body for post 2',
          category: 'Technology'
        }
      }
    )
    ```

## Increment Field (\$inc)

Increment a field to new value. Create the field if not there.

    ```
    db.posts.update({ title: 'Post Two' },
      {
        $inc: {
          likes: 5
        }
      }
    )
    ```

## Rename Field

Rename likes field to views

    ```
    db.posts.update({ title: 'Post Two' },
      {
        $rename: {
          likes: 'views'
        }
      }
    )
    ```

## Delete Row

Remove object by field value

    ```
    db.posts.remove({ title: 'Post Four' })
    ```

## Sub-Documents

Add new property, with key comments, and value nested array to object with field title: 'Post One'

    ```
    db.posts.update({ title: 'Post One' },
      {
        $set: {
          comments: [
            {
              body: 'Comment One',
              user: 'Mary Williams',
              date: Date()
            },
            {
              body: 'Comment Two',
              user: 'Harry White',
              date: Date()
            }
          ]
        }
      }
    )
    ```

## Find By Element in Array ($elemMatch)

  Return the object that has a comments field where an object in it has "user" : "Mary Williams"

    ```
    db.posts.find({
      comments: {
        $elemMatch: {
          user: 'Mary Williams'
        }
      }
    })
    ```

## Add Index

    ```
    db.posts.createIndex({ title: 'text' })
    ```

## Text Search

Search object's values for a string 'Post O'

    ```
    db.posts.find({
      $text: {
        $search: "\"Post O\""
      }
    })
    ```

## Greater & Less Than

    ```
    db.posts.find({ views: { $gt: 2 } })
    db.posts.find({ views: { $gte: 7 } })
    db.posts.find({ views: { $lt: 7 } })
    db.posts.find({ views: { $lte: 7 } })
    ```




/*  Helper Methods */
Array.prototype.unique = function () {
  return this.filter(function (value, index, self) {
    return self.indexOf(value) === index;
  });
}

Array.prototype.diff = function (arr2) { return this.filter(x => arr2.includes(x)); }
