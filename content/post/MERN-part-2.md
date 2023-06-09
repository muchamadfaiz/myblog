---
title: "Building MERN Stack Event Organization Web App 2023 | Part 2"
date: 2023-03-28T16:15:26+07:00
image : /img/test.jpg
draft: true
description: "Part 2: Building Data Models"
---
Hey everyone! Welcome back to the second part of our MERN Stack series. In the first part was all about configuring the project and gaining an understanding of the many different tools and technologies

and in case you missed it, here is the link to the first part:

**(LINK TO FIRST PART)**

In this second part, we will create our database schema using mongoose to connect to the MongoDBAtlas. Let’s take a problem statement and draw our data model diagram.

> There’s a Freelance platform web app. **Users** can have multiple  **gigs** . Before creating some gig users must register and log in to their accounts with their username and password.

To make it easy to describe, here is the ERD diagram for the model

![db](/img/MERN-Part-2/db.png)


We would then create a new folder named ‘models’ in our root directory**. **To represent our two models — users and gigs — we will create two files inside the ‘models’ folder. These files will serve as representations of our two models, providing a clear and structured way of organizing our data. Let’s create a user  model

### User Model

We will begin by creating our first model — the User Model. This model will serve as a means of storing and managing user data. To begin this process, we will create a file named ‘user.js’ in the ‘models’ folder that we created earlier. This file will contain the necessary code to define and implement the User Model. So, we will start by first invoking *mongoose *in our file.

```
import mongoose from 'mongoose';
const { Schema, model } = mongoose;
```

Next, we create our `userSchema` from the `Schema` we defined earlier.

```
const userSchema = new Schema({
  username: {
    type: String,
    required: [true, "username harus diisi"],
    unique: true,
  },
  email: {
    type: String,
    required: [true, 'Please enter an email'],
    unique: true,
  },
  password: {
    type: String,
    required: [true, 'Please enter a valid password'],
    minlength: [6, 'Minimum password length must be 6 characters'],
    maxlength: [20, 'Max password length must be 20 characters'],
  },
},
  { timestamps: true }
);
```

Now that we have built our UserSchema, we can proceed to construct the User model based on the schema and then export it. The model will be stored in a database with the name “User” and MongoDB will pluralize it to become “Users”.

```
export default model('User', userSchema)
```

Overall our code will look like this

```
import mongoose from 'mongoose';
const { Schema, model } = mongoose;

const userSchema = new Schema({
  username: {
    type: String,
    required: [true, "username harus diisi"],
    unique: true,
  },
  email: {
    type: String,
    required: [true, 'Please enter an email'],
    unique: true,
  },
  password: {
    type: String,
    required: [true, 'Please enter a valid password'],
    minlength: [6, 'Minimum password length must be 6 characters'],
    // maxlength: [20, 'Max password length must be 20 characters'],
  },
},
  { timestamps: true }
);

export default model('User', userSchema)
```

### Gig Model

In order to design the schema for the gigs in our web app, we need to create a model called the Gig Model. This model will be responsible for managing the gigs that are posted on our platform. To keep things simple, we will not include images in the items schema.

But for this series, we will have these five fields — title, description, price, and sales. We will build ourgigmodel in a file named `gig.js` inside the models folder. We start by requiring the *mongoose *which allows us to interact with our MongoDB database. We will then create a new `Schema` object that will define the fields that we want in our gig model.

```
import mongoose from 'mongoose';
const { Schema, model } = mongoose;
```

Next, we will start designing our  **GigSchema** . It has five fields and will build upon the Schema we defined earlier.

```
const gigSchema = new Schema({
  userID:{
    type:Number,
    required:true,
  },
  title:{
    type:String,
    required:true,
  },
  desc:{
    type:String,
    required:true,
  },
  price:{
    type:Number,
    required:true,
  },
  sales:{
    type:Number,
    required:true,
  },
});
```

After defining the schema, we will use the `mongoose.model` method to create a new model called `Gig`. This model will be based on the `gigSchema` that we just defined. Finally, we will export the `Gig` model using `module.exports`, which will allow us to use this model in other parts of our application as needed.

```
export default model('Gig', gigSchema)
```

Your `gig.js` file should look like the following.

```
import mongoose from 'mongoose';
const { Schema, model } = mongoose;

const gigSchema = new Schema({
  userID:{
    type:Number,
    required:true,
  },
  title:{
    type:String,
    required:true,
  },
  desc:{
    type:String,
    required:true,
  },
  price:{
    type:Number,
    required:true,
  },
  sales:{
    type:Number,
    required:true,
  },
});

export default model('Gig', gigSchema)
```

Great Job! we now finished building all the models we will use in our application, so we will end the second part here. In the next part, we will focus on the routes and controllers. We will define the endpoints for our API and the actions that should be taken when each endpoint is hit.

We will also create some custom middleware functions that will handle tasks such as authentication, authorization, and error handling. These middleware functions will be used throughout our application to ensure that our API is secure and robust.

**(Link to Third Part)**
