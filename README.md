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


Step 3 - Automatically create a docker image and push it to docker hub

Search for Docker and choose docker.

![image](https://user-images.githubusercontent.com/96426756/173195062-90e183c6-4bfe-481a-9a64-e21c7b3bc175.png)

Choose your docker registry. We have already set up our service connection to docker then you must see the connection name so choose the connection and give Container repository a name. Leave everything default and click on Add. It will add another task.

![image](https://user-images.githubusercontent.com/96426756/173195066-8020781e-e9ea-4bc0-854a-3a31295bafb8.png)

Our previous task only builds the docker image but it doesn't push to the docker hub so we are going to remove or replace the task with the task we have just created. Your YAML file should look something like this. 
```
# Docker    
# Build a Docker image     
# https://docs.microsoft.com/azure/devops/pipelines/languages/docker    
    
trigger:    
- main    
    
resources:    
- repo: self    
    
variables:    
  tag: '$(Build.BuildId)'    
    
stages:    
- stage: Build    
  displayName: Build image    
  jobs:      
  - job: Build    
    displayName: Build    
    pool:    
      vmImage: 'ubuntu-latest'    
    steps:    
    - task: Docker@2    
      inputs:    
        containerRegistry: 'docker connection'    
        repository: 'achraf/demodckreg'    
        command: 'buildAndPush'    
        Dockerfile: '**/Dockerfile'    
        tags: |    
          $(tag)   
```
Now click on Save and Run. Commit directly to the main branch.

![image](https://user-images.githubusercontent.com/96426756/173195067-5a69cc84-c38d-42de-9eff-a4bb202de499.png)

You will be presented with the below screen and you will see the build running.

![image](https://user-images.githubusercontent.com/96426756/173195068-e3bdfa7e-e1b7-4d12-8638-691cb53359a2.png)

If you click on the build then you can see the full log of the build.

![image](https://user-images.githubusercontent.com/96426756/173195243-94b640b2-8d57-44be-afa7-8e284c62e1ed.png)

# Step 4 - Set up Terraform for automatically creating resources in Azure and deploying docker image.
 
## So what is Terraform?
 
Terraform is an open-source infrastructure as a code software tool that provides a consistent CLI workflow to manage hundreds of cloud services. You can learn more here.
 
## What is Infrastructure as Code?
 
Infrastructure as code (IaC) is the process of managing and provisioning resources on the Cloud rather than logging in to your cloud provider and doing it manually. You can write codes which will interact with your cloud provider and can create, modify, and delete resource automatically without visiting the portal.
 
Create a file named main.tf in the root directory of the project. This file holds the terraform configuration code. Add the following code.
```
provider "azurerm" {    
    version = "2.5.0"    
    features {}    
}     

resource "azurerm_resource_group" "terraform_test" {    
  name = "ans-tes-grp"    
  location = "westus2"    
}   
```
As we need a resource group in azure for our resources the above code will create a resource group named ans-test-grp in west us. Let us test if everything is fine and to do so we need to initialize terraform. Use the following command for that.

```
provider "azurerm" {    
    version = "2.5.0"    
    features {}    
}    
    
terraform {    
    backend "azurerm" {    
        resource_group_name  = "achrafRG"    
        storage_account_name = "terraformstfl"    
        container_name       = "tfstate"    
        key                  = "terraform.tfstate"    
    }    
}    
    
variable "imagebuild" {    
  type        = string    
  description = "Latest Image Build"    
}    
    
resource "azurerm_resource_group" "terraform_test" {    
  name = "ans-tes-grp"    
  location = "Australia East"    
}    
    
resource "azurerm_container_group" "tfcg_test" {    
  name                      = "node-taste-anish"    
  location                  = azurerm_resource_group.anish_terraform_test.location    
  resource_group_name       = azurerm_resource_group.anish_terraform_test.name    
    
  ip_address_type     = "public"    
  dns_name_label      = "achraf"    
  os_type             = "Linux"    
    
  container {    
      name            = "node-tes-anish"    
      image           = "achraf/nodetest:${var.imagebuild}"    
        cpu             = "1"    
        memory          = "1"    
    
        ports {    
            port        = 80    
            protocol    = "TCP"    
        }    
  }    
}       
} 
```
We need to provide access to terraform in order to work remotely --  I mean from pipeline not from our local machine -- and to do so head over to Azure and search for Azure Active directory. Click on App registration and click on new registration. 

![image](https://user-images.githubusercontent.com/96426756/173195548-2addab3d-0b25-419b-b6e3-0ed22a4e25ab.png)

Give it a name and choose Accounts in this organizational directory only (Default Directory only - Single-tenant).Click on the register and once it is complete copy the following.
- Client ID
- Tenant ID
Head over to certificates and secrets and click on New client secret. Give it a name and copy the value.

![image](https://user-images.githubusercontent.com/96426756/173195550-85a10ee9-4a73-4feb-b622-5b2fef204da1.png)

Now go back to your subscription and copy the subscription ID. Go to your pipeline and click on Library for adding these four values for terraform to communicate with Azure. Add all four values here like below.

![image](https://user-images.githubusercontent.com/96426756/173195553-423784f6-ac56-401e-8486-ea6f3584f3a2.png)

Once you're done go to your azure-pipeline.yml file and add another stage.
```
- stage: Provision  
  displayName: 'Terraforming on Azure...'  
  dependsOn: Build  
  jobs:  
  - job: Provision  
    displayName: 'Provisioning Container Instance'  
    pool:  
      vmImage: 'ubuntu-latest'  
    variables:   
    - group: terraformvars // Library name in your pipeline  
    steps:  
    - script: |  
        set -e  
        terraform init -input=false  
        terraform apply -input=false -auto-approve  
      name: 'RunTerraform'  
      displayName: 'Run Terraform'  
      env:  
        ARM_CLIENT_ID: $(ARM_CLIENT_ID)  
        ARM_CLIENT_SECRET: $(ARM_CLIENT_SECRET)  
        ARM_TENANT_ID: $(ARM_TENANT_ID)  
        ARM_SUBSCRIPTION_ID: $(ARM_SUBSCRIPTION_ID)  
        TF_VAR_imagebuild: $(tag)  
```
Now we are ready to test the workflow. Now go to the code and make any changes and push it to GitHub. Once done head back to DevOps. Now go to the code and make any changes and push it to GitHub. Head back to DevOps and you will see one build running with two states something like below and of course you can see the complete log when you click on it. Once it is done you can go back to azure and check if everything is completed.

![image](https://user-images.githubusercontent.com/96426756/173195554-13b1402b-0a44-4b2a-980c-84f6dfdd0b51.png)

So now you don't need to build your docker image and push it to the docker hub. You don't need to create resources manually. You just need to focus on your code and whenever you push any changes it will do the rest for you. Now if everything is complete you can go to Azure and check if the Azure container instance is running.
 
 ![image]()
