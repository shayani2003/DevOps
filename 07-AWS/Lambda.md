# AWS Lambda

## Introduction

AWS Lambda is a serverless compute service that allows you to run code without provisioning or managing servers.

With Lambda, you only focus on:

```text
Writing Code
```

AWS handles:

```text
Servers

Scaling

Availability

Patching

Infrastructure
```

---

# What is Serverless Computing?

Serverless does NOT mean:

```text
No Servers
```

It means:

```text
Servers Managed By AWS
```

You don't manage them.

---

# Traditional Architecture

```text
Application
      |
EC2 Server
      |
Manage OS
Manage Patches
Manage Scaling
```

---

# Serverless Architecture

```text
Application
      |
Lambda
      |
AWS Manages Everything
```

---

# What is AWS Lambda?

AWS Lambda is an event-driven compute service.

It executes code when triggered by an event.

---

# Architecture

```text
Event
   |
Lambda Function
   |
Result
```

---

# Examples of Events

```text
S3 Upload

API Request

Database Change

CloudWatch Event

SNS Message
```

---

# Why Lambda?

Benefits:

```text
No Server Management

Automatic Scaling

Pay Per Use

High Availability
```

---

# Lambda Workflow

```text
Event
   |
Lambda Function
   |
Execution
   |
Response
```

---

# Real World Example

User uploads image:

```text
S3 Bucket
      |
Lambda Trigger
      |
Resize Image
```

Automatically.

---

# Lambda Components

Main Components:

```text
Function

Trigger

Event

Execution Role
```

---

# Lambda Function

A function contains your code.

---

# Supported Languages

```text
Python

Java

Node.js

Go

C#

Ruby
```

---

# Example

Python:

```python
def lambda_handler(event, context):

    return {
        "statusCode": 200,
        "body": "Hello World"
    }
```

---

# Trigger

A trigger starts Lambda execution.

---

# Examples

```text
S3

API Gateway

SNS

SQS

CloudWatch

EventBridge
```

---

# Architecture

```text
Trigger
    |
Lambda
```

---

# Event

Data sent to Lambda.

---

# Example

```json
{
  "name": "Shayani"
}
```

---

# Lambda receives:

```text
Event Data
```

and processes it.

---

# Context Object

Provides runtime information.

---

Contains:

```text
Function Name

Memory

Request ID

Execution Time
```

---

# IAM Execution Role

One of the most important interview topics.

---

# Why Needed?

Lambda needs permissions.

---

Example:

```text
Lambda
   |
Read S3
```

Requires:

```text
IAM Role
```

---

# Architecture

```text
Lambda
   |
IAM Role
   |
S3
```

---

# Example Policy

```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "*"
}
```

---

# Lambda Scaling

Automatic.

---

Example:

```text
1 Request
```

1 execution.

---

```text
1000 Requests
```

1000 executions.

---

# Architecture

```text
Users
   |
Lambda
   |
Auto Scaling
```

---

# Concurrency

## What is Concurrency?

Number of simultaneous Lambda executions.

---

Example:

```text
100 Requests
```

at same time.

---

```text
Concurrency = 100
```

---

# Cold Start

Very common interview topic.

---

# What is a Cold Start?

When Lambda starts a new execution environment.

---

Workflow:

```text
First Request
      |
Create Environment
      |
Execute Code
```

---

Slight delay occurs.

---

# Warm Start

Environment already exists.

---

Workflow:

```text
Request
   |
Reuse Existing Environment
   |
Faster Execution
```

---

# Lambda Timeout

Maximum execution time.

---

Range:

```text
1 Second

to

15 Minutes
```

---

# Example

```text
Timeout = 30 Seconds
```

---

# Lambda Memory

Configurable.

---

Range:

```text
128 MB

to

10 GB
```

---

# More Memory

Usually means:

```text
More CPU
```

also.

---

# Lambda Pricing

Pay only for:

```text
Invocations

Execution Duration
```

---

Example:

```text
No Requests
     |
No Cost
```

---

# Event Sources

Lambda can be triggered by:

```text
API Gateway

S3

DynamoDB

SNS

SQS

CloudWatch

EventBridge
```

---

# API Gateway + Lambda

Most common serverless architecture.

---

# Architecture

```text
User
  |
API Gateway
  |
Lambda
  |
Response
```

---

# Example

```text
GET /users
```

---

API Gateway:

```text
Calls Lambda
```

---

Lambda:

```text
Returns Data
```

---

# Lambda + S3

---

# Architecture

```text
Upload File
      |
S3
      |
Lambda Trigger
      |
Process File
```

---

# Example

Upload:

```text
image.jpg
```

---

Lambda:

```text
Create Thumbnail
```

---

# Lambda + DynamoDB

---

# Architecture

```text
Lambda
   |
Read / Write
   |
DynamoDB
```

---

# Lambda + SQS

---

# Architecture

```text
Message
    |
SQS
    |
Lambda
```

---

# Benefits

```text
Asynchronous Processing
```

---

# Lambda + EventBridge

---

# Architecture

```text
Scheduled Event
       |
EventBridge
       |
Lambda
```

---

# Example

Every day:

```text
9 AM
```

Run report generation.

---

# Monitoring

Use:

```text
CloudWatch Logs

CloudWatch Metrics
```

---

# Architecture

```text
Lambda
   |
CloudWatch
```

---

# Metrics

```text
Invocations

Errors

Duration

Throttles
```

---

# Environment Variables

Store configuration.

---

Example:

```text
DATABASE_URL

API_KEY
```

---

# Architecture

```text
Lambda
   |
Environment Variables
```

---

# Lambda Layers

Reusable code packages.

---

# Example

```text
Shared Libraries

SDKs

Dependencies
```

---

# Architecture

```text
Layer
  |
Multiple Lambda Functions
```

---

# Lambda vs EC2

Most common interview question.

| Feature | Lambda | EC2 |
|----------|---------|------|
| Server Management | No | Yes |
| Scaling | Automatic | Manual/ASG |
| Pricing | Pay Per Use | Pay For Running Server |
| Startup | Fast | Slower |
| Long Running Tasks | Limited | Better |

---

# Lambda vs ECS

| Feature | Lambda | ECS |
|----------|---------|------|
| Execution Model | Event Driven | Container Based |
| Infrastructure | Fully Managed | Container Management |
| Runtime Duration | Max 15 Minutes | Long Running |

---

# Complete Serverless Architecture

```text
User
  |
API Gateway
  |
Lambda
  |
DynamoDB
```

---

# Real Production Example

Image Processing System

Workflow:

```text
Upload Image
      |
S3
      |
Lambda
      |
Create Thumbnail
      |
Store Result
```

---

# Advantages

## No Server Management

AWS manages infrastructure.

---

## Automatic Scaling

Scales instantly.

---

## Cost Effective

Pay only when used.

---

## High Availability

Built-in redundancy.

---

# Disadvantages

## Cold Starts

Initial execution delay.

---

## Execution Limit

Maximum 15 minutes.

---

## Vendor Lock-In

AWS-specific service.

---

# Interview Question

## What is AWS Lambda?

### Answer

AWS Lambda is a serverless compute service that runs code in response to events without requiring users to provision or manage servers.

---

# Interview Question

## What is Serverless Computing?

### Answer

Serverless computing is a cloud computing model where the cloud provider manages the infrastructure, scaling, and maintenance, allowing developers to focus only on writing code.

---

# Interview Question

## What Triggers a Lambda Function?

### Answer

Lambda functions can be triggered by services such as API Gateway, S3, SNS, SQS, EventBridge, DynamoDB, and CloudWatch Events.

---

# Interview Question

## What is a Cold Start?

### Answer

A Cold Start occurs when AWS Lambda creates a new execution environment before running a function, causing a small startup delay.

---

# Interview Question

## Why Does Lambda Need an IAM Role?

### Answer

Lambda uses an IAM Execution Role to securely access AWS services such as S3, DynamoDB, and CloudWatch without storing credentials inside the code.

---

# Interview Question

## Difference Between Lambda and EC2

### Answer

Lambda is serverless and automatically scales based on events, while EC2 provides virtual servers that users must manage and scale themselves.

---

# Best Practices

- Use IAM Roles instead of credentials.
- Keep functions small and focused.
- Monitor with CloudWatch.
- Use Environment Variables.
- Minimize Cold Starts.
- Use Lambda Layers for shared code.
- Configure appropriate memory and timeout values.

---

# Memory Trick

```text
Event
  |
Lambda
  |
Response
```

Common Triggers:

```text
API Gateway

S3

SNS

SQS

EventBridge
```

---

# Summary

AWS Lambda provides:

```text
Serverless Computing

Automatic Scaling

Event-Driven Execution

Pay Per Use
```

Architecture:

```text
Trigger
   |
Lambda
   |
AWS Services
```

Most Common Architecture:

```text
User
  |
API Gateway
  |
Lambda
  |
DynamoDB
```

AWS Lambda is one of the most important AWS services because it enables serverless architectures, event-driven applications, automation workflows, and cost-efficient cloud solutions, making it a very common interview topic in AWS, Cloud, and DevOps roles.
