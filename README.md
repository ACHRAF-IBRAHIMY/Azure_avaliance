# Azure
## Install the Azure CLI
 

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
npm init -y  
```
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

![image](https://user-images.githubusercontent.com/96426756/173149777-a6011c78-f06a-4656-9ecd-e52bd2fa4c8c.png))

## Dockerize the project 
 
What is docker?
 
Docker is a set of the platform as a service products that use OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries, and configuration files.
 
Create an empty file named Dockerfile in the root directory and paste the following code
Create app directory :  WORKDIR /usr/src/app    
   
 Install app dependencies    
 A wildcard is used to ensure both package.json AND package-lock.json are copied    
 where available (npm@5+)    
COPY package*.json ./    
  ```  
RUN npm install    
RUN npm ci --only=production    
   ```
## Bundle app source    
```
COPY . .      
EXPOSE 80    
CMD [ "node", "index.js" ]     
Build the docker image
docker build -t <your username>/nodeexample  
Run the docker image
docker run -p 49160:3000 -d <your username>/nodeexample  
 ```
if successful you can go to your browser and visit this address http://localhost:49160/ 

 

# Step 2 - Setup a pipeline in Azure DevOps 
 
Now go to Azure DevOps click on start free and log in using your email and password. After logging in click on New Project. 
 
Provide your project name and description and choose visibility.

![image](https://user-images.githubusercontent.com/96426756/173149791-75c66de2-3413-4a26-8f9e-a14541eb2f6a.png)

I am choosing Git as version control since I have my code on GitHub. After successfully creating Now go to project settings.

![image](https://user-images.githubusercontent.com/96426756/173149795-393f4d93-811c-4f21-8066-3f3b127d2ced.png)

Choose a service connection.

![image](https://user-images.githubusercontent.com/96426756/173149801-61fe07a0-b5dd-4e7b-bbcb-fae3bffd0038.png)

You may ask why are we doing this. So what we are going to do is create a docker image and push that image to docker hub so we need to connect Azure DevOps to docker hub. Now choose the Docker registry.

![image](https://user-images.githubusercontent.com/96426756/173149805-d1b2a411-dbf3-4290-b5b3-f957ee600261.png)

Provide your credentials and after that click on verify; if verified then enter the service name and click on it.

![image](https://user-images.githubusercontent.com/96426756/173149808-b7995e18-08d8-4d0e-92ca-f9ee0f183030.png)

Again click on New service connection and click on Azure Resource Manager and click Next.

![image](https://user-images.githubusercontent.com/96426756/173149814-b8a22177-072f-4330-ba07-c456104ad7ff.png)

Choose service principal automatic.

![image](https://user-images.githubusercontent.com/96426756/173149817-5dd6bd05-3fc8-43df-b289-b1a520f2394e.png)

Choose your subscription and provide your service connection name and then click on next.

![image](https://user-images.githubusercontent.com/96426756/173149822-2599c011-00b6-40ba-a759-9a2996c0027c.png)

Go back to Pipeline and click on Create Pipeline. Now choose Git and sign in to GitHub if needed. After signing in you will see a list of your repository and you need to choose the repository which contains your node js code. You may need to provide permission to Azure Pipeline to access your code.
 
Based on the code and file in the project, it will automatically suggest many options to configure our pipeline with. We are going to choose Docker in this case. Click on validate and configure.

![image](https://user-images.githubusercontent.com/96426756/173149826-71f2a55e-b7b5-46ae-8dde-9c3854793c52.png)

You will be presented with an azure-pipeline.yml file to review. This file contains pretty much what we need to do but we are going to add something more to this file. wW are going to add a different task to this file and for that click on Show Assistant and search for Docker and choose docker.


