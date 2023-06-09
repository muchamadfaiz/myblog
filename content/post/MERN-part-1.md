---
title: "Building MERN Stack Event Organization Web App 2023 | Part 1"
date: 2023-03-28T13:28:22+07:00
draft: true
description: "Part 1: Initial set-up"
image : /img/test.jpg
categories : [
    "Javascript"
]
tags: [
  "Javascript"
]
---
### Introduction

The MERN stack is a popular stack of technologies nowadays for building a modern single-page application. The main purpose of using the MERN stack is to develop apps using javascript only. This is because the four technologies that make up the technology stack are all JavaScript-based. Thus, if one knows JavaScript (and JSON, the backend, frontend, and database can be operated easily.

The MERN stack consists of the following technologies:

* **M -** MongoDB: A document-based database.
* **E -** Express: A server-side web framework.
* **R -** React: A client-side web framework.
* **N -** Node.js: JavaScript run-time environment that executes JavaScript code outside of a browser (such as a server).

Using (MERN) makes us easily build a three-tiers architecture (front end, back end, and database) entirely using JavaScript and JSON.

![firtsimage](/img/test2.png)

In this tutorial, you will learn the MERN stack by building an event organizer web app. the main features we would have in the application that we would be building are:

* Authentication using JSON Web Tokens (JWT)
* Authorization
* The users can create, edit, view, and delete all the events on the cart

### Before we get started

**Prerequisites**

Before we get started, make sure you have the following installed:

* Text Editor (in this tutorial I will use VS code)
* Node (you should install it in our system as we’ll use `npm` to install packages its like `pip` on python, )
* MongoDB (for a vast and robust tutorial I will use MongoDBAtlas)
* Postman (for API testing)

So far we have an overview of what we will build, so now let’s start building the project.

### Setting Up The Project

Assuming that you have installed Node.js and npm, then open up a terminal and move into the folder of your choice where we would create a new directory for our Express.js project and make it as your working directory.

```
mkdir myapp
cd myapp
```

Then, move into the created folder and initialize the project using `npm`

```
npm init
```

it would show us some question to create `package.json` file. you can go through into it and leave all as default but in this tutorial i would like to change the entry point to server.js instead of the default `index.js` I would like to name it `server.js` because it will work like a server so naming it as such is more convenience.

Afterwards, it will create a `package.json` in our working directory. Open up the file with your desired text editor, here i’am using VS code. Install the following dependencies using `npm`

```
npm install express mongoose dotenv jsonwebtoken bcrypt
```

and here is the explanation of these packages we just installed.

* `express`: This is the library we would use to make routing, request handling, and responding easier to code.
* `mongoose`: This is the library to connect nodejs with mongoDB
* `jsonwebtoken`: This is the library to make authorization and authentication
* `bcrypt`: this library is to change our secret key into a hash
* `dotenv:` this library is to store our credential information

Next, we will install nodemon globally

```
npm install -g nodemon
```

nodemon will make our development process easier, it will restart the running code if there is a change in our code. Make sure to use `nodemon` instead of `node` when you run your code for development purposes.

we would then add this following script to `package.json `file

```
  "type":"module",
```

```
  "scripts": {
    "dev": "nodemon server.js",
},
```

Your `package.json` file should look like the following at this step.

```
{
  "name": "seminar",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "type":"module",
  "scripts": {
    "dev": "nodemon server.js"
  },
  "keywords": [],
  "author": "Faiz",
  "license": "ISC",
  "dependencies": {
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "mongoose": "^7.0.2"
  }
}
```

so we can start the apps in development with `nodemon dev` and we no longer use “require” but we will use “import” sentence

The next step is to create the backend server, but before we go into it, we’ll host our database in the cloud using MongoDB Atlas. So, lets create an account at MongoDBAtlas [here](https://cloud.mongodb.com/).

Get logged in to your account and create the new project. Afterwards, create a database by clicking the green button as the image bellow.

![mongo-1](/img/mongo-1.png)

then configure the new cluster by choosing your Cloud Provider, the region you want your data to be stored in and the specification of database.

![mongo-2](/img/mongo-2.png)

After the cluster is created, you will have to configure IP Whitelist addresses and a database user. For the IP Whitelist, just add your current IP address.

![mongo-3](/img/mongo-3.png)

Once those steps have been completed, in our dashboard it will be filled with the cluster we just created

![mongo-4](/img/mongo-4.png)

Then click connect and this window bellow will be pop up

![mongo-5](/img/mongo-5.png)

Here is a few different ways that we provide information to connect to MongoDBAtlas.

* Connect with the MongoDB shell — connect into MongoDB CLI
* Connect your application — connect directly into our apps
* Connect using MongoDB Compass — connect into MongoDB GUI
* Connect using VSCode — connect directly into VScode

In this tutorial we will choose “Connect Your Application” . So after we click it you will see information on getting a connection string

![mongo-6](/img/mongo-6.png)

Later on the code, we will use the connection string and the password that was created earlier. Now let’s create a file named `server.js` in the root directory, import all the packages

```
import express from 'express'
import mongoose from 'mongoose'
import dotenv from 'dotenv'
```

invoke our express app and dotenv package

```
const app = express();
dotenv.config();
```

Afterwards, make the server listen to port 3001 and create `.env` file. The .env file is a file that is generally used to store apps configurations. By using the dotenv package and creating a `.env `file, you can keep your sensitive credentials separate from your codebase and avoid accidentally sharing them in version control. Create a new file in the root of your project and name it `.env` and then add this code into it

```
PORT=3001
MONGO=
```

MONGO variable will be filled by our database link. to see our link lets go to MongoDBAtlass and copy the URI. Then paste it into MONGO variables on `.env` , so your file will look like this

```
PORT=3001
MONGO=mongodb+srv://muchamadfaiz:<password>@cluster0.jmccb6u.mongodb.net/?retryWrites=true&w=majority
```

replace the `<password>` with your password for your database

Next, add this line of code for mongoose settings and set the `port` for our server to run and have our app listen on this port

```
// MONGOOSE SETUP
const URI = process.env.Mongo
const connect = async () => {
    try {
        await mongoose.connect(URI);
        console.log(`Connected to MongoDB Succesfull`)
    } catch (error) {
        console.log('Could not conect to MongoDB...', error)
    }
}

// LISTENING
const PORT = process.env.PORT
app.listen(PORT, ()=> {
    connect()
    console.log(`Backend server is running! on port:${PORT}`)
})
```

Your `server.js` file should look like the following at this stage.

```
import express from "express";
import mongoose from "mongoose";
import dotenv from "dotenv";

// CONFIGURATIONS
const app = express()
dotenv.config();

// MONGOOSE SETUP
const URI = process.env.Mongo
const connect = async () => {
    try {
        await mongoose.connect(URI);
        console.log(`Connected to MongoDB Succesfull`)
    } catch (error) {
        console.log('Could not conect to MongoDB...', error)
    }
}

// LISTENING
const PORT = process.env.PORT
app.listen(PORT, ()=> {
    connect()
    console.log(`Backend server is running! on port:${PORT}`)
})
```

Let’s run our server for now by typing ` nodemon dev`. The following output should be show on terminal.

![result](/img/result.png)

Perfect! You've set up a server using node and express and succesfully connected to MongoDB Atlas 
In the next part, we would then re-structured the folder to better organize the files thus make the codebase more maintainable. Then, we will create our models, routes and controllers.
I'am very exciting about the upcoming part and i hope you are too.
