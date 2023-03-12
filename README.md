# Orchestrating-Queue-based-Microservices-using-AWS-Step-Functions-and-Amazon-SQS
## What This Project Does
This project will simulate inventory verification requests from incoming orders in an e-commerce application as part of an order processing workflow. Step Functions will send inventory verification requests to a queue on SQS. An AWS Lambda function will act as your inventory microservice that uses a queue to buffer requests. When it retrieves a request, it will check inventory and then return the result to Step Functions. When a task in Step Functions is configured this way, it is called a callback pattern. Callback patterns allow you to integrate asynchronous tasks in your workflow, such as the inventory verification microservice of this project
### I understood how to use AWS step functions and SQS to design and run a serverless workflow that orchestrates a message queue-based microservices
AWS step functions is a virtual workflow service that helps developers use AWS services to build distributed applications, automate processes, orchestrate microservices and create data and machine learning pipelines
## AWS Step Functions Workflow 

