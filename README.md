# Orchestrating-Queue-based-Microservices-using-AWS-Step-Functions-and-Amazon-SQS
## What This Project Does
This project will simulate inventory verification requests from incoming orders in an e-commerce application as part of an order processing workflow. Step Functions will send inventory verification requests to a queue on SQS. An AWS Lambda function will act as your inventory microservice that uses a queue to buffer requests. When it retrieves a request, it will check inventory and then return the result to Step Functions. When a task in Step Functions is configured this way, it is called a callback pattern. Callback patterns allow you to integrate asynchronous tasks in your workflow, such as the inventory verification microservice of this project
### I understood how to use AWS step functions and SQS to design and run a serverless workflow that orchestrates a message queue-based microservices
## What is AWS Step Functions?
AWS step functions is a virtual workflow service that helps developers use AWS services to build distributed applications, automate processes, orchestrate microservices and create data and machine learning pipelines
## AWS Step Functions Workflow 
## What is Amazon Simple Queue Service?
Amazon Simple Queue Service(SQS) provides queues for high-throughput, system-to-system messaging. It uses queues to decouple heavyweight processes and to buffer and batch work. It stores messages until microservices and serverless aplications process them.
## What is AWS lambda?
AWS lambda is serverless computing resource. It is used to run codes without provisioning/managing servers. It maintains and operates all of the compute resources.
## Procedures
## Step 1: Creating and Configuring an SQS queue
1. I logged into my AWS Management Console Account and clicked on "services". I typed SQS and selected Simple Queue Service to open the service console, when the SQS console landing page appeared, I scrolled to "Get Started" and clicked on "Create queue"

![Screenshot (243)](https://user-images.githubusercontent.com/99415191/224582700-748a3f1d-a2e1-46ac-b7f8-25591b979634.png)


![Screenshot (246)](https://user-images.githubusercontent.com/99415191/224583986-ce02e073-3b82-4ccc-ad5b-f09fcc5b1a6c.png)


2. Next I configure my Queue named "MyOrder", I chose the queue type as standard and left other configuration settings such as visibility timeout, message retention period as default and created "MyOrder" queue


![Screenshot (247)](https://user-images.githubusercontent.com/99415191/224584887-b5144ea6-2cc9-4b16-91dd-916a704cfafd.png)


![Screenshot (248)](https://user-images.githubusercontent.com/99415191/224584954-3da2ac24-cecf-4dbb-b53a-95c11ffc367e.png)


![Screenshot (250)](https://user-images.githubusercontent.com/99415191/224584959-21007025-f7e7-44a5-95aa-4cf2aa08cd86.png)


3. After my Queue has been succesfully created, I moved to the next step of designing a workflow of how my I want the e-commerce order to be processed


![Screenshot (251)](https://user-images.githubusercontent.com/99415191/224584962-3a1afc8b-cb32-41c5-b484-17ca70e90f5e.png)

## Step 2: I designed a workflow that describes how I want my E-commerce order be processed using AWS Step Functions. 
Workflow describes a process as a series of discrete tasks that can be repeated again and again.

1. I designed my workflow in AWS Step Functions. The workflow will request verification of inventory from a microservice. Many microservices uses a queue to receive requests. I used an AWS Lambda function to represent the microservice in this project.

2. Next, I opened the AWS Step Function Console by typing and searching on the searchbar.

3. Thereafter, I clicked on "create state machine".

4. To define my state machine I selected "Write your workflow in code" because I have my custom definition.

![Screenshot (252)](https://user-images.githubusercontent.com/99415191/224594931-9f09b8ce-f160-4a15-b9a9-d74e0aed8296.png)

 5. I replaced the contents of the State machine definition window with the Amazon States Language (ASL) state machine definition in code file. Amazon States Language is a JSON-based, structured language used to define your state machine.
  
![Screenshot (253)](https://user-images.githubusercontent.com/99415191/224594865-029a1d33-46fe-402e-8bea-3dac245c0a8c.png)

"What the definition does? It uses a task state to put a message on an SQS queue. This task state is configured for a callback pattern. When you append .waitForTaskToken to your resource, Step Functions will add a task token to the JSON payload and wait for a callback. The microservice can return a result to Step Functions by calling the Step Functions API."

6. I then copied the URL of my SQS queue from the SQS console and paste it into my State machine definition.

7. I then clicked the refresh butteon to allow the step function translate the ASL state machine into visual workflow

![Screenshot (255)](https://user-images.githubusercontent.com/99415191/224594918-69411b2b-fb7c-4172-968b-7ffedae47846.png)

8. I configured(added an IAM role) and named my state machine "Inventory-Sate_Machine"

![Screenshot (256)](https://user-images.githubusercontent.com/99415191/224594923-1ec33774-71d3-487d-a783-22071155b630.png)

9. My state machine is successfully created

![Screenshot (257)](https://user-images.githubusercontent.com/99415191/224594927-b56ea54c-4365-4d77-ae91-d86e802065b6.png)

## Step 3: Creating IAM roles for my workflow(Roles for services are used when you want a service to access or interact with another service or services)

1. I opened the IAM management console to create an IAM role that allows lambda access Step Functions and SQS

![Screenshot (258)](https://user-images.githubusercontent.com/99415191/224697775-30e8ddfe-42c7-42aa-89dc-050392327854.png)

2. I scrollled to Roles and clicked on it, created a role
![Screenshot (259)](https://user-images.githubusercontent.com/99415191/224697789-9f3fb161-7e52-408f-8098-62ffb6a51393.png)

3. I selected AWS service and then selected  lambda, then click on "Next:Permissions"

 ![Screenshot (260)](https://user-images.githubusercontent.com/99415191/224697795-7323dd89-023a-4dde-b8c0-5840a0c271f3.png)
4. In the permissions policies, I searched and attached "AmazonSQSFullAccess" and "AmazonSQSFullAccess" policies to it
![Screenshot (261)](https://user-images.githubusercontent.com/99415191/224697801-e96553ff-a325-4a2b-b49b-6d679880fc74.png)

![Screenshot (262)](https://user-images.githubusercontent.com/99415191/224697806-f25dcc4e-9b2e-43e2-a489-1a80caa22f53.png)
5. I clicked on "Create role"
 ![Screenshot (264)](https://user-images.githubusercontent.com/99415191/224697821-2a9399af-fa7e-45fd-8dd3-755d82e1d7d5.png)
 
6. I entered the role name as "inventory-lambda-role"

7. My "inventory lambda role" is successfully created

![Screenshot (265)](https://user-images.githubusercontent.com/99415191/224697824-50a1d1cd-dde4-4de9-9bfd-633408bb0fff.png)

## Step 4: Creating Microservices with AWS lambda Functions. ###The AWS lambda function will act as an inventory microservices. The lambda function retrieves messages for SQS and will return a message to Step Function that represents the result of the request. The lambda function is in the code file
1. I clicked services, then click on compute then clicked on lambda
![Screenshot (266)](https://user-images.githubusercontent.com/99415191/224697830-00ef706f-283f-44bf-bed3-9b056610bd49.png)
2. I then clicked on "Create a function"
3. I left it at "Author from scratch"
![Screenshot (267)](https://user-images.githubusercontent.com/99415191/224697834-7b0fc17f-4753-4151-ad50-543ffd6e296e.png)
4. I configured my lambda function setting my runtime as "Node.js 18x"
![Screenshot (268)](https://user-images.githubusercontent.com/99415191/224697839-2c2a0620-e58e-4a0d-aa0f-5582121d5b4d.png)

5. For my Role, I clicked on  "Use an existing role" and I selected "inventory-lambda-role"
6. 6. I gave my function name as "Inventory"
![Screenshot (269)](https://user-images.githubusercontent.com/99415191/224697840-5bbcb892-c788-429f-a3c6-6f9d7602aaa9.png)

7. My lambda function is successfully created
![Screenshot (270)](https://user-images.githubusercontent.com/99415191/224697848-a4c81631-9e6d-4cdc-ab82-53b14397e049.png)
