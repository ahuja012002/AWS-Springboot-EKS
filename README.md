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
