# Using Mongoose in conjunction with Node.js <!-- omit in toc -->

## Table of contents <!-- omit in toc -->

- [What is Mongoose?](#What-is-Mongoose)
- [MongoDB](#MongoDB)
- [Terminology](#Terminology)
- [Setting up the database](#Setting-up-the-database)
  - [Creating a schema](#Creating-a-schema)
  - [Adding a record](#Adding-a-record)
  - [Retrieving a record](#Retrieving-a-record)
- [Conclusion](#Conclusion)
    - [sources:](#sources)

## What is Mongoose?

Mongoose is a library focused on Data modeling, made for MongoDB and Node.js.

You could view Mongoose as an extra layer in between the database (MongoDB) and the project logic (Node.js).

Mongoose lets you communicate with your MongoDB using Javascript.

## MongoDB

So as I stated above Mongoose creates a link between MongoDB and your Javascript application, but what is MongoDB?

MongoDB is a database structure, setting itself apart by being less strict than for example SQL databases. This means that, for example, the structure of documents in a Mongo database can vary, whereas a lot of databases enforce a structure.

This makes MongoDB a flexible database to work with when prototyping, as you aren't always sure exactly what kind of data you will be handling.

## Terminology

- Collection: A Mongo collection is where you store your different data objects, saved as JSON objects.
- Documents: Documents are the records in your database, the objects that you will be saving.
- Fields: Fields are the different attributes of a document, for example the username of a user document.
- Schema: In a schema you can define the properties of a document. You can define these properties however strict you want them to be, e.g.:
  - A required String with a maximum length of 100 characters
  - A Number
  - An object containing a String and a Number
- Model: A model is a wrapper on the created schema, allowing your application to interact with the database using queries.

## Setting up the database

Firstly one would install the needed dependencies, including Node.js and Mongoose. A lot of tutorials can be found on this subject.

For our project we decided to use mlab to store our data. They host free databases (not suited for production, but very easy to use for prototyping).

By storing the data online we could rapidly prototype and test on all sorts of devices without having to deploy our local database.

### Creating a schema

A first step if we want to save documents to our database would be defining the structure of this document globally.

For example if we are planning to work with users, we could define a very basic user schema:

```javascript
// requiring the mongoose dependency
let mongoose = require('mongoose')

// defining the schema
var userSchema = new mongoose.Schema({
    username: String
    password: String
})

// defining a model, referencing the schema
module.exports = mongoose.model('User', userSchema)
```

### Adding a record

After this we could populate the database using the model. First we have to create an instance of User:

```javascript
// we required the User model as a variable
var user = new User({
  username: 'test',
  password: 'hunter123'
})
```

After which we can save that object to the actual database:

```javascript
user.save()
```

Mongo automatically assigns a unique id to newly added documents, so you do not have to define that field yourself.

### Retrieving a record

To retrieve an existing record from the database Mongoose offers multiple queries, the most basic one being 'find'.

For example if we were to retrieve our newly created user, we could search it using the following code:

```javascript
User.find({
  username: 'test'
}).then(retrievedDocument => {
  console.log(retrievedDocument)
})
```

The above code would print the found result(s) to the console.

If you plan on more extensive use of Mongoose, I recommend checking out the [documentation](https://mongoosejs.com/docs/queries.html) to see which queries are available to you and how to use them.

In my experience Mongoose is quite simple to learn while also offering a lot of freedom regarding the structure and usage of your database.

## Conclusion

If you are looking for an easy to set up, Node.js compatible database that you can communicate with directly using Javascript, I'd say Mongoose is the way to go.

I have personally used it for the prototyping of an application for which the structure of my datamodel was constantly changing, and because of the flexible nature Mongoose was the tool for the Job.

#### sources:

- [The Mongoose documentation](https://mongoosejs.com/docs/queries.html)
- [Introduction to Mongoose](https://www.freecodecamp.org/news/introduction-to-mongoose-for-mongodb-d2a7aa593c57/)
- [mlab](https://mlab.com)
