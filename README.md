# AWS-Springboot-EKS
We will deploy the Spring boot Application in Kubernetes using AWS EKS.

For this, We will use the Employee Management Application System Spring boot application developed in the below repository:
https://github.com/ahuja012002/AWS-Springboot-3tierApp

We are not going to revisit the Spring boot Application again here :

## What is EKS

Amazon Elastic Kubernetes Service (Amazon EKS) is a fully managed Kubernetes service that enables you to run Kubernetes seamlessly in AWS Cloud .

In order to run our application on EKS, first thing we need to do is containerize our Spring boot Application i.e. Create Docker image for the application.

To Create Docker image, we need to create dockerFile at the root of our spring boot project as shown in the screenshot :
<img width="297" alt="Screenshot 2025-01-23 at 12 03 17 PM" src="https://github.com/user-attachments/assets/7201087b-1f2d-4565-aa0b-ad64cd257883" />

Below will be the content of dockerfile :

<img width="457" alt="Screenshot 2025-01-23 at 12 04 03 PM" src="https://github.com/user-attachments/assets/9d188ed5-282f-459b-a068-f7eb79407ce8" />

This says, we are going to build one image for this application with below features :
Open JDK version 17

It should create directory /app in the container

it should copy the SpringUIService-0.0.1-SNAPSHOT.jar from target folder and place it in the app directory.

It should expose the application at port 8080

It should finally run the application using java -jar SpringUIService-0.0.1-SNAPSHOT.jar

Once we add the Docker file, we should build the application using mvn clean install

# Creating Repository in AWS Console
Lets now login to AWS Console to go to ECR. Click Create Repository 

<img width="1430" alt="Screenshot 2025-01-23 at 9 14 49 PM" src="https://github.com/user-attachments/assets/8afd673d-acb7-4c02-bbeb-537228992d06" />


<img width="1710" alt="Screenshot 2025-01-23 at 9 15 54 PM" src="https://github.com/user-attachments/assets/f1ba680b-85a9-4fce-a5a8-c060d7f9d031" />

Give any name to the repository and Click Create. This should create a repository.

Next step is to build the docker image of our code and push it to ECR.

Click on view Push Commands :

<img width="813" alt="Screenshot 2025-01-23 at 9 17 59 PM" src="https://github.com/user-attachments/assets/b3b0c325-1dcf-4fba-8897-374fdd8e6235" />

Run each command for your operating system.

This will create docker image for your application.

Tag the Docker Image

Push the Image to ECR

Now We need to Copy Image URI

## Creating EKS Cluster

Before we create eks cluster, we need to setup eksctl and kubectl in our system.

Here is the official AWS Documentation to set this up :

https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

Once they are installed , its time to create a cluster. There are two ways to create Cluster

Through Console (We need to perform multiple actions like adding IAM roles, adding addons, etc.)
Through CLI using eksctl (This will execute cloud formation script and perform all the operations.)

We will use CLI to create our cluster.

Below command is used to create cluster with name springboot-eks-demo with Kubernetes version 1.31 , One node and node type as t2.small in the region us-east-1

eksctl create cluster --name springboot-eks-demo --version 1.31 --nodes=1 --node-type=t2.small --region us-east-1

We can also give command like :

eksctl create cluster springboot-eks-demo

This will take default values and create nodetype as m5.large. So, just be careful as it can incur some cost.

<img width="988" alt="Screenshot 2025-01-22 at 8 08 56 PM" src="https://github.com/user-attachments/assets/aafb8f94-24aa-4d3f-9300-60c5cfac8646" />

Once, the cluster is created, we can check it in AWS Console in EKS view.

<img width="1481" alt="Screenshot 2025-01-22 at 8 16 04 PM" src="https://github.com/user-attachments/assets/5278cc6b-f692-4560-a24e-b6f9fe5c5d25" />

Let us now deploy our application into the cluster :

For this we need to create a manifest file :

It will be something like below . Make sure to change Image URI with the one we created in ECR

<img width="632" alt="Screenshot 2025-01-23 at 9 28 59 PM" src="https://github.com/user-attachments/assets/9241ab35-60fb-4257-8b15-354d26b60d3d" />

Once the manifest file, is created, lets execute the below command : 

<img width="524" alt="Screenshot 2025-01-22 at 9 15 12 PM" src="https://github.com/user-attachments/assets/71d628e1-d413-453e-9d23-0a69ce003bc5" />

This will create pods and and also add load balancer service .

To check status , we can execute below command : 

<img width="993" alt="Screenshot 2025-01-22 at 9 15 02 PM" src="https://github.com/user-attachments/assets/e4bae651-65af-4e20-a388-34ef9827a42f" />

Pods may take couple of minutes to comeup, once they are up, u can see them as READY 1/1

Now, Check your external IP and browse your application using it :

If everything goes well, we should be able to see our application as below : 

<img width="1223" alt="Screenshot 2025-01-22 at 9 14 50 PM" src="https://github.com/user-attachments/assets/13144bf1-cb8f-4e85-9645-2bcab0195a07" />

<img width="1335" alt="Screenshot 2025-01-22 at 9 14 41 PM" src="https://github.com/user-attachments/assets/f5c157e5-6319-4bb0-90d3-3553e236ad00" />

So, Congratulations ! We have now successfully ran our Spring boot application on EKS Cluster.

Make sure to delete the Cluster !

<img width="560" alt="Screenshot 2025-01-22 at 9 17 50 PM" src="https://github.com/user-attachments/assets/7148f6a5-2723-4684-a161-e924857dd297" />


  type: LoadBalancer

