# MERN-STACK-IMPLEMENTATION
A MERN stack is a bundle of four different software technologies that developers use to build websites and web applications. MERN is an acronym for the NoSQL document database, MongoDB; the server side/backend, Express.js; the client side/frontend, React.js; and the Javascript runtime environment, Node.js. This technology stack allows the creation of a 3-tier architecture that includes frontend, backend, and database using JavaScript and JSON. Before attempting this project, I had already created an AWS account and launched an instance running Ubuntu Server OS. The steps to deploy MERN are as follows;

**STEP 1 - SET UP AN SSH AND CONNECT TO UBUNTU SERVER ON AWS VIA TERMINAL**

**STEP 2 - BACKEND CONFIGURATION**

*Update Ubuntu*

    sudo apt update
    
*Upgrade Ubuntu*

    sudo apt upgrade
    
*Get the location of Node.js software from Ubuntu repositories*

    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -

*Install Node.js with the command below*

    sudo apt-get install -y nodejs

*Verify the node installation with the command below*

    node -v

*Verify the node installation with the command below*

    npm -v

*Create a new directory for your To-Do project:*

    mkdir Todo

*Verify that the Todo directory is created with **ls** command*

*Change your current directory to the newly created one*

    cd Todo

*Use the command **npm init** to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.*

*Run the command **ls** to confirm that you have package.json file created.*

<img width="566" alt="1" src="https://github.com/ifyyegwim/MERN-STACK-IMPLEMENTATION/assets/134213051/9cfcf48d-c278-4c5a-96e6-a44686338d52">

**STEP 3 - INSTALL EXPRESS.JS**

*Install express using npm*

    npm install express

*Create a file index.js with the command below*

    touch index.js

*Run ls to confirm that your index.js file is successfully created*

*Install the dotenv module*

    npm install dotenv

*Open the index.js file with the command below*

    vi index.js

*Paste:*

    const express = require('express');
    require('dotenv').config();

    const app = express();

    const port = process.env.PORT || 5000;

    app.use((req, res, next) => {
    res.header("Access-Control-Allow-Origin", "\*");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
    next();
    });

    app.use((req, res, next) => {
    res.send('Welcome to Express');
    });

    app.listen(port, () => {
    console.log(`Server running on port ${port}`)
    });
    
*Open your terminal in the same directory as your index.js file and type:*

    node index.js

<img width="560" alt="2" src="https://github.com/ifyyegwim/MERN-STACK-IMPLEMENTATION/assets/134213051/9c894114-4e25-49dc-aca2-084b24418ada">

*Add a rule in your EC2 configuration to open inbound configuration through port 5000 and set the source to 0.0.0.0/0*

<img width="995" alt="3" src="https://github.com/ifyyegwim/MERN-STACK-IMPLEMENTATION/assets/134213051/cdb6cd6f-45c1-42c9-b11d-26c50a3d1d7a">

*Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:*

    http://<PublicIP-or-PublicDNS>:5000
    
<img width="1123" alt="4" src="https://github.com/ifyyegwim/MERN-STACK-IMPLEMENTATION/assets/134213051/e3255f0d-0fc1-4343-aef8-6099d2a0bed0">

**STEP 4 - CREATE ROUTES**

There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes

    mkdir routes
    
*Change directory to routes folder*

cd routes

*Create a file api.js with the command below*

touch api.js

*Open the file with the command below*

vi api.js

*Paste:*

    const express = require ('express');
    const router = express.Router();

    router.get('/todos', (req, res, next) => {

    });

    router.post('/todos', (req, res, next) => {

    });

    router.delete('/todos/:id', (req, res, next) => {

    })

    module.exports = router;

**STEP 5 - CREATE A MODEL**
    
Since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model. A model is at the heart of JavaScript based applications, and it is what makes it interactive. We will also use models to define the database schema. This is important so that we will be able to define the fields stored in each Mongodb document. In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties. To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.

*In your Todo folder, install Mongoose*

    npm install mongoose

*Create a new folder 'models':*

    mkdir models

*Change directory into the newly created ‘models’ folder with:*

    cd models

*Inside the models folder, create a file and name it todo.js*

    touch todo.js

**Tip:** All three commands above can be defined in one line to be executed consequently with help of && operator, like this:

    mkdir models && cd models && touch todo.js
    
*Open the file created with vim todo.js then paste the code below in the file:*

    const mongoose = require('mongoose');
    const Schema = mongoose.Schema;

    //create schema for todo
    const TodoSchema = new Schema({
    action: {
    type: String,
    required: [true, 'The todo text field is required']
    }
    })

    //create model for todo
    const Todo = mongoose.model('todo', TodoSchema);

    module.exports = Todo;
    
*Update your routes from the file api.js in ‘routes’ directory to make use of the new model. In Routes directory, open api.js with vi api.js, delete the code inside with :%d command and paste the code below into it then save and exit*

    const express = require ('express');
    const router = express.Router();
    const Todo = require('../models/todo');

    router.get('/todos', (req, res, next) => {

    //this will return all the data, exposing only the id and action field to the client
    Todo.find({}, 'action')
    .then(data => res.json(data))
    .catch(next)
    });

    router.post('/todos', (req, res, next) => {
    if(req.body.action){
    Todo.create(req.body)
    .then(data => res.json(data))
    .catch(next)
    }else {
    res.json({
    error: "The input field is empty"
    })
    }
    });

    router.delete('/todos/:id', (req, res, next) => {
    Todo.findOneAndDelete({"_id": req.params.id})
    .then(data => res.json(data))
    .catch(next)
    })

    module.exports = router;
    
**STEP 6 - CREATE MONGODB DATABASE**

We need a database where we will store our data. On the mongobd website, sign up for a shared clusters free account, which is ideal for our use case. Follow the sign up process, select AWS as the cloud provider, and choose a region near you. Then, allow access to mongodb from anywhere. In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

*Create a file in your Todo directory and name it .env*

    touch .env
    
*Open the file:*

    vi .env
    
*Add the connection string to access the database in it, just as below:*

    DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'

*Ensure to update <username>, <password>, <network-address> and <database> according to your setup on mongodb*
    









    
    
