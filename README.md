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








    
    
