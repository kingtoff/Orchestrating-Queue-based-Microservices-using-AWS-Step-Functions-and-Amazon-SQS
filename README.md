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
