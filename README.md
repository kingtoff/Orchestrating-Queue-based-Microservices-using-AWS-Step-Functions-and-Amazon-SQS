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
... I logged into my AWS Management Console Account and clicked on "services". I typed SQS and selected Simple Queue Service to open the service console, when the SQS console landing page appeared, I scrolled to "Get Started" and clicked on "Create queue"

![Screenshot (243)](https://user-images.githubusercontent.com/99415191/224582700-748a3f1d-a2e1-46ac-b7f8-25591b979634.png)


![Screenshot (246)](https://user-images.githubusercontent.com/99415191/224583986-ce02e073-3b82-4ccc-ad5b-f09fcc5b1a6c.png)


... Next I configure my Queue named "MyOrder", I chose the queue type as standard and left other configuration settings such as visibility timeout, message retention period as default and created "MyOrder" queue


![Screenshot (247)](https://user-images.githubusercontent.com/99415191/224584887-b5144ea6-2cc9-4b16-91dd-916a704cfafd.png)


![Screenshot (248)](https://user-images.githubusercontent.com/99415191/224584954-3da2ac24-cecf-4dbb-b53a-95c11ffc367e.png)


![Screenshot (250)](https://user-images.githubusercontent.com/99415191/224584959-21007025-f7e7-44a5-95aa-4cf2aa08cd86.png)


After my Queue has been succesfully created, I moved to the next step of designing a workflow of how my I want the e-commerce order to be processed


![Screenshot (251)](https://user-images.githubusercontent.com/99415191/224584962-3a1afc8b-cb32-41c5-b484-17ca70e90f5e.png)

## Step 2: I designed a workflow that describes how I want my E-commerce order be processed using AWS Step Functions. 
Workflow describes a process as a series of discrete tasks that can be repeated again and again.
I designed my workflow in AWS Step Functions. The workflow will request verification of inventory from a microservice. Many microservices uses a queue to receive requests. I used an AWS Lambda function to represent the microservice in this project.

Next, I opened the AWS Step Function Console by typing and searching on the searchbar.

Thereafter, I clicked on "create state machine".

To define my state machine I selected "Write your workflow in code" because I have my custom definition.

![Screenshot (252)](https://user-images.githubusercontent.com/99415191/224594931-9f09b8ce-f160-4a15-b9a9-d74e0aed8296.png)

  I replaced the contents of the State machine definition window with the Amazon States Language (ASL) state machine definition in code file. Amazon States Language is a JSON-based, structured language used to define your state machine.
  
![Screenshot (253)](https://user-images.githubusercontent.com/99415191/224594865-029a1d33-46fe-402e-8bea-3dac245c0a8c.png)

"What the definition does? It uses a task state to put a message on an SQS queue. This task state is configured for a callback pattern. When you append .waitForTaskToken to your resource, Step Functions will add a task token to the JSON payload and wait for a callback. The microservice can return a result to Step Functions by calling the Step Functions API."
...I then copied the URL of my SQS queue from the SQS console and paste it into my State machine definition.
I then clicked the refresh butteon to allow the step function translate the ASL state machine into visual workflow

![Screenshot (255)](https://user-images.githubusercontent.com/99415191/224594918-69411b2b-fb7c-4172-968b-7ffedae47846.png)

I configured and named my state machine "Inventory-Sate_Machine"

![Screenshot (256)](https://user-images.githubusercontent.com/99415191/224594923-1ec33774-71d3-487d-a783-22071155b630.png)

My state machine is successfully created

![Screenshot (257)](https://user-images.githubusercontent.com/99415191/224594927-b56ea54c-4365-4d77-ae91-d86e802065b6.png)
