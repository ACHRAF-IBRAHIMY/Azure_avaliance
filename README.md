# Azure
## Install the Azure CLI
 
Use the following commands if you're using Ubuntu.
```
sudo apt-get update  
sudo apt-get install azure-cli  
Run the login command. 
az login 
```


 
So this is what  we are going to do. Let's divide it into simple steps. 
## Step 1 - Create a simple 'Hello World' in Node.js and push it to GitHub
 
What is Node.js?
 
Node.js is an open-source, cross-platform, JavaScript runtime environment that executes JavaScript code outside a web browser. Node JS enables us to write server-side Javascript code. It is built on Chrome’s V8 JavaScript engine. With Node JS we can build different types of applications like web servers, command Line Applications, Rest APIs, and so on. To learn more you can go to here.
 
## Installing node js
 
To create a project it should be installed on your machine and to install node js please visit here and install the node js.
 
## Creating a Node Project
 
Create a new directory and initialize the node with the command.
```
npm init  
What is NPM?
``` 
Npm is a package manager where all the javascript packages reside. We use NPM to download all the javascript packages through npm.
```
mkdir nodeexample  
cd nodeexample/  
npm init -y  ```
After executing the command, a package.json file is generated in the project and this holds all the details relevant to the project.
 
## Configure Express
 
Express is used to create a server. The below command is used to create our server.
```
npm install express --save 
We are creating a new server that will be running on port 3000. I am creating a route that returns hello world. Here is the complete code.
const express = require('express')    
const app = express()    
const port = 3000    
app.get('/', (req, res) => {    
  res.send('Hello World!')    
})    
app.listen(port, () => {    
  console.log(`Example app listening at http://localhost:${port}`)    
})     
```
Now start the server using the below command.

npm start  

Open your browser and navigate to http://localhost:3000/. You should see Hello world getting displayed.
Dockerize the project 
 
What is docker?
 
Docker is a set of the platform as a service products that use OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries, and configuration files.
 
Create an empty file named Dockerfile in the root directory and paste the following code
# Create app directory    
WORKDIR /usr/src/app    
   
# Install app dependencies    
# A wildcard is used to ensure both package.json AND package-lock.json are copied    
# where available (npm@5+)    
COPY package*.json ./    
    
RUN npm install    
# If you are building your code for production    
# RUN npm ci --only=production    
   
# Bundle app source    
COPY . .    
    
EXPOSE 80    
CMD [ "node", "index.js" ]     
Build the docker image
docker build -t <your username>/nodeexample  
Run the docker image
docker run -p 49160:3000 -d <your username>/nodeexample  
if successful you can go to your browser and visit this address http://localhost:49160/You must see Hello World. You can learn more here and this is very detailed documentation. Now what you need to do is create a Github repository and push the code to Github.
 
Step 2 - Setup a pipeline in Azure DevOps 
 
Now go to Azure DevOps click on start free and log in using your email and password. After logging in click on New Project. 
 
Provide your project name and description and choose visibility.
