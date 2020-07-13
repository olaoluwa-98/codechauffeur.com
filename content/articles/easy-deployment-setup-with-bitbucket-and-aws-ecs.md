---
title: "Easy Deployment Setup With Bitbucket and AWS ECS"
date: 2020-07-13T18:58:41+01:00
draft: false
images: ["images/easy-deployment-with-bitbucket-and-ecs/easy-deployment-with-bitbucket-and-ecs.png"]
tags: [CI/CD,Deployment,DevOps,AWS,Bitbucket,Docker]
---

Deployment is one of the most important parts of product development and launch. The better the deployment process is the faster it is. There are many deployment tools that make things easier today.

In this article, I will take you through a deployment setup with Bitbucket and AWS Elastic Container Service (ECS).

## Requirements

- Basic knowledge of `git` version control
- Basic knowledge of `Docker`
- An AWS account
- A Bitbucket account

[Bitbucket](https://bitbucket.org/) is a web-based version control repository hosting service owned by Atlassian. They have built-in Continuous Integration and Delivery/Deployment (CI/CD).

We'll deploy a simple express app. Let's generate an express app

```sh
npx express-generator
```

If you don't have npx, you can install `express-generator` globally

```sh
npm install -g express-generator
express
```

Now we have our express app, let's create a docker file that we would use for deployment

```sh
touch Dockerfile
```

Copy this and paste in your `Dockerfile`

```docker
FROM node:8-alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD [ "npm", "start" ]
```

## Setup AWS ECS

Login to your AWS account and navigate to [ECS](https://console.aws.amazon.com/ecs/home).

![AWS ECS Home page](/images/easy-deployment-with-bitbucket-and-ecs/ecs-home-page.png)

Navigate to **Repositories** under Amazon Elastic Container Registry (ECR).

AWS ECR is a container registry for docker. It's similar to Docker Hub. Container registries are used to store and distribute docker images.

Create a repository by clicking the 'Create repository' button then give it a name like `my-express-app`.

![AWS ECR](/images/easy-deployment-with-bitbucket-and-ecs/create-repository-page.png)

Now we have our repository URI: `XXXXXXXXXXX.region.amazonaws.com/my-express-app`

## Pushing to AWS ECR

We will build our docker image and push it to our newly created repository on ECR.
For this article, we'll use `docker build`. If you have a more complex project setup, you may want to consider [docker-compose](https://docs.docker.com/compose/)

cd into the folder where the `Dockerfile` is and run

```sh
docker build -t my-express-app .
```

If the command ran well, you should have this:

![Run docker build](/images/easy-deployment-with-bitbucket-and-ecs/docker-build-example.png)

To push the image to ECR, you have to first install [aws cli tool](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

Run `aws2 configure` to enter your AWS access ID and secret access key. Once you're done, run this command:

```sh
$(aws2 ecr get-login --no-include-email --region us-east-1)
```

Then push the docker image

```sh
docker tag my-express-app:latest XXXXXXXXXXX.region.amazonaws.com/my-express-app:latest
docker push XXXXXXXXXXX.region.amazonaws.com/my-express-app:latest
```

## Setup ECS Cluster

Navigate to the Clusters page. Click Create Cluster.

![Creating a cluster](/images/easy-deployment-with-bitbucket-and-ecs/ecs-create-cluster.png)

We will use EC2 Linux + Networking.

You can use the following configuration to create the cluster:

- Give your cluster a name.
- Create an empty cluster: Unchecked
- Provisioning Model: On-Demand Instance
- EC2 instance type: t2.nano (You can use whatever will fit your application needs)
- Number of instances: 1
- EC2 Ami Id: Amazon Linux 2 AMI [ami-08b26b905b0d17561]
- Root EBS Volume Size (GiB): 30
- Key pair: You can create anyone and attach it. This is the key you can use to ssh into the ec2 instance.
- Networking: You can create a new VPC or use an existing one.
  - Auto-assign public IP: Use subnet settings
  - Security group: Create a one that opens up port 80
- CloudWatch Container Insights: Unchecked

## Create an Application Load Balancer

An Application Load Balancer allows us to dynamically assign applications to routes using pattern matching. This will be used in the ECS Service

To create one, navigate to the load balancers tab on [EC2](console.aws.amazon.com/ec2) and click on Create Load Balancer

![Create Load Balancer](/images/easy-deployment-with-bitbucket-and-ecs/creating-load-balancer.png)

Give it a name, fill the Availability Zones, and add a security group that exposes port 80 and 443 (if you want to enable SSL).

In Step 4: Configure Routing, you'd create a target group. You can skip Step 5.

## Setup ECS Task Definition & Service

A task definition specifies the container information for our application. Navigate to Task Definitions and click on Create new Task Definition.

Select EC2 as launch type compatibility

![Task definition](/images/easy-deployment-with-bitbucket-and-ecs/create-ecs-task-def.png)

You can use the following configuration to create the task definition:

- Task Definition Name: my-express-app
- Task Role: Leave empty
- Network Mode: default
- Task memory (MiB): 128 (You can use whatever size that fits your application needs)
- Task CPU (unit): 128 (You can use whatever size that fits your application needs)
- Container: Click on Add container
  - Container name: my-express-app
  - Image: XXXXXXXXXXX.region.amazonaws.com/my-express-app:latest
  - Port mappings: Host - 0, Container port: 3000
  - Configure the remaining to fit your application needs
  - You can enable Log configuration so you can view the application logs on [CloudWatch](https://console.aws.amazon.com/cloudwatch/home).

Once you're done, click on Create. Next, we'll create a Service.
A service lets you specify how many copies of your task definition to run and maintain in a cluster.

Click the `Actions` dropdown and select Create Service

![Creating an ecs service](/images/easy-deployment-with-bitbucket-and-ecs/create-service-from-task-def.png)

You can use the following configuration to create the service:

- Launch type: EC2
- Task Definition: my-express-app
- Cluster: my-express-app (or whatever cluster you created)
- Service name: MyExpressAppService
- Service type: Replica
- Number of tasks: 1 (Or whatever number fits your application needs).
- Minimum healthy percent: 100
- Maximum percent: 200
- Deployment type: Rolling update
- Placement Templates: AZ Balanced Spread
- Load balancing: Application Load Balancer
  - Load balancer name: Select the load balancer we created earlier
  - Container to load balance: Click on Add to load balancer
  - Select the target group we created earlier or create a new one here
- Use default in the remaining steps or configure to your application needs

Once the service is created, it'll run the task in the cluster.

## Setup CD with Bitbucket

We are going to create a new repository on Bitbucket. You can name it anything. I named mine `my-express-app`.

![Creating bitbucket repository](/images/easy-deployment-with-bitbucket-and-ecs/create-repository-bitbucket.png)

Automated CI/CD comes built-in with Bitbucket. To enable it, navigate to Repository Settings > Pipelines > Settings and switch it on.

## Bitbucket pipeline file

Bitbucket uses a file named: bitbucket-pipelines.yml for automating CI/CD. Create the file.

```yml
definitions:
  services:
    push-image: &push-image
      name: Build and Push Docker Image
      image: atlassian/pipelines-awscli
      caches:
        - docker
      services:
        - docker
      script:
        - export BUILD_ID=$BITBUCKET_BRANCH_$BITBUCKET_COMMIT_$BITBUCKET_BUILD_NUMBER
        - export DOCKER_URI=$DOCKER_IMAGE_URL:latest
        # Login to docker registry on AWS
        - eval $(aws ecr get-login --no-include-email)
        # Build image
        - docker build -t $DOCKER_URI .
        # Push image to private registry
        - docker push $DOCKER_URI

    deploy-to-ecs: &deploy-to-ecs
      name: Deploy to ECS
      image: atlassian/pipelines-awscli
      script:
        - pipe: atlassian/aws-ecs-deploy:1.1.0
          variables:
            AWS_ACCESS_KEY_ID: $AWS_ACCESS_KEY_ID
            AWS_SECRET_ACCESS_KEY: $AWS_SECRET_ACCESS_KEY
            AWS_DEFAULT_REGION: $AWS_DEFAULT_REGION
            CLUSTER_NAME: 'my-express-app'
            SERVICE_NAME: 'MyExpressAppService'
            TASK_DEFINITION: "task-definition.json"

pipelines:
  branches:
    master:
      - step: *push-image
      - step: *deploy-to-ecs

```

You'd notice `TASK_DEFINITION`. This is needed to update the ECS service for deployment. So let's create one:

```sh
touch task-definition.json
```

Paste this in the task definition file
```json
{
  "containerDefinitions": [
    {
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/my-express-app",
          "awslogs-region": "us-east-1",
          "awslogs-stream-prefix": "ecs"
        }
      },
      "portMappings": [
        {
          "hostPort": 0,
          "protocol": "tcp",
          "containerPort": 3000
        }
      ],
      "cpu": 0,
      "image": "XXXXXXXXXXX.region.amazonaws.com/my-express-app:latest",
      "essential": true,
      "name": "my-express-app"
    }
  ],
  "memory": "128",
  "family": "my-express-app",
  "requiresCompatibilities": ["EC2"],
  "cpu": "128"
}
```

Now we need to put the following environment variables on our bitbucket repository:

- DOCKER_IMAGE_URL
- AWS_DEFAULT_REGION
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY

Navigate to Repository Settings > Repository variables > 
![Bitbucket repository variables](/images/easy-deployment-with-bitbucket-and-ecs/bitbucket-repository-variables.png)

Once you add them, commit and push to bitbucket to trigger the pipeline and the deployment process is run.

The following things occur:
 
- The docker image is built and pushed to the AWS ECR repository you created
- The task definition is updated with the `task-definition.json` file
- The service (MyExpressAppService) is updated
- The service automatically registers a new instance of the task and begins draining the old task running
- The new task is dynamically registered on the Application Load Balancer
  
You check the full bitbucket repository [here](https://bitbucket.org/olaoluwa-98/my-express-app)
