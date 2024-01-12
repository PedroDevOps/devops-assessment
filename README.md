# AWS CloudFormation Stacks Deployment Guide

This guide provides step-by-step instructions for creating a Node.js application, building a Docker image and updating it to Amazon ECR. It also covers deploying three different AWS CloudFormation stacks: a basic VPC, an ECS cluster, and an auto-scaling Fargate service.

## Table of Contents
- [AWS CloudFormation Stacks Deployment Guide](#aws-cloudformation-stacks-deployment-guide)
  - [Table of Contents](#table-of-contents)
  - [Prerequisites](#prerequisites)
  - [Creating a Node.js Application](#creating-a-nodejs-application)
  - [Building and Updating Docker Image to ECR](#building-and-updating-docker-image-to-ecr)
  - [Deploying CloudFormation Stacks](#deploying-cloudformation-stacks)
    - [VPC CloudFormation Stack](#vpc-cloudformation-stack)
    - [ECS Cluster CloudFormation Stack](#ecs-cluster-cloudformation-stack)
    - [Container with auto scalling CloudFormation Stack](#container-with-auto-scalling-cloudformation-stack)
    - [Delete CloudFormation Stacks](#delete-cloudformation-stacks)


## Prerequisites
1. AWS account and programatic access.
2. AWS CLI installed
   1. aws configure
3. Docker installed
   1. to build images
4. packages npm and nodejs

## Creating a Node.js Application
1. **Initialize Project**: Create a new directory for your project and initialize it with `npm init`.
2. **Install Express**: Use `npm install express` to add Express.js for web server functionality.
3. **Application Code**: Create an `index.js` file and write your server code.
4. **Test Locally**: Run your application locally using `node index.js` to ensure it's working.

## Building and Updating Docker Image to ECR
1. **Create ECR Repository**: In AWS ECR, create a new repository for your Docker image.
2. **Authenticate Docker**: Authenticate your Docker CLI to the ECR registry using the AWS CLI command `aws ecr get-login-password`.
3. **Build Docker Image**: From your project directory, build your Docker image using `docker build -t <repository-name> .`.
4. **Tag Your Image**: Tag your built image with the ECR repository URI using `docker tag <repository-name> <ecr-repository-uri>`.
5. **Push to ECR**: Push your Docker image to ECR using `docker push <ecr-repository-uri>`.

## Deploying CloudFormation Stacks

To the deploy the container-with-auto-scalling stack you could use different vpc and a ecs-cluster stack. This will allow you different configurations for differente enviroments.

### VPC CloudFormation Stack

To create the VPC CloudFormation stacks, use the following AWS CLI commands:

```
aws cloudformation create-stack --template-body file://$PWD/vpc.yml --stack-name vpc

or with specifc parameters:

aws cloudformation create-stack --template-body file://$PWD/vpc.yml --stack-name vpc \
--parameters ParameterKey=BasicVpcCidr,ParameterValue=10.200.0.0/16 \
ParameterKey=PubSubACidr,ParameterValue=10.200.0.0/24 \
ParameterKey=PubSubBCidr,ParameterValue=10.200.1.0/24 \
ParameterKey=PrivSubACidr,ParameterValue=10.200.2.0/24 \
ParameterKey=PrivSubBCidr,ParameterValue=10.200.3.0/24
```

### ECS Cluster CloudFormation Stack

To create the ECS Cluster CloudFormation stacks, use the following AWS CLI commands:

```
aws cloudformation create-stack --template-body file://$PWD/ecs-cluster.yml --stack-name ecs-cluster --capabilities CAPABILITY_IAM

or with specifc parameters:

aws cloudformation create-stack --template-body file://$PWD/ecs-cluster.yml --stack-name ecs-cluster --capabilities CAPABILITY_IAM \
--parameters ParameterKey=VpcStack,ParameterValue=other-vpc-stack 

```

### Container with auto scalling CloudFormation Stack

To create the Container with auto scalling CloudFormation stacks, use the following AWS CLI commands:

```
aws cloudformation create-stack --template-body file://$PWD/container-with-auto-scalling.yml --stack-name container-with-auto-scalling --capabilities CAPABILITY_IAM

or with specifc parameters:

aws cloudformation create-stack --template-body file://$PWD/container-with-auto-scalling.yml --stack-name container-with-auto-scalling --capabilities CAPABILITY_IAM \
--parameters ParameterKey=VpcStack,ParameterValue=other-vpc-stack \
ParameterKey=EcsClusterStack,ParameterValue=other-cluster-stack \
ParameterKey=Image,ParameterValue=other-image \
ParameterKey=MaxContainers,ParameterValue=max-containers-number

```

### Delete CloudFormation Stacks

To delete the CloudFormation stacks and cleanup your enviroment, use the following AWS CLI commands:

```
aws cloudformation delete-stack --stack-name vpc
aws cloudformation delete-stack --stack-name ecs-cluster
aws cloudformation delete-stack --stack-name container-with-auto-scalling
```