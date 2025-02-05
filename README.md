# Project1_AWS_3-Tier_Architecture_Deployment
## Architecture Overview
The three-tier architecture is the most popular implementation of a multi-tier architecture and consists of a single presentation/web tier, logic tier, and data tier. This pattern divides the infrastructure into 3 separate layers: one public and 2 private layers. The idea is that the public layer acts as a shield to the private layers. Anything in the public layers is publicly accessible but resources in the private layers is only accessible from inside the network.


![Capture 3](https://github.com/user-attachments/assets/56d0874f-1c28-4f3a-853e-557ba2fac9c4)


The architectural diagram above provides a clear, visual representation of the system's structure and components, showcasing how they interact to achieve the desired functionality. 
* It serves as a blueprint for stakeholders, enabling them to understand the overall design, relationships, and data flow.
* For the technical teams, it ensures alignment, simplifies communication, and guides the implementation of a secure, scalable, and efficient solution.
* In this cloud-based project, the architectural diagram highlighted key services, network configurations, and security measures, ensuring the architecture meets business and operational objectives.
## Project Outline
This project is a guide for the process of creating a highly available and fault tolerant web application architecture leveraging various AWS resources.
In this documentation, I will set up and deploy a three tier architecture using AWS. The architecture includes:
* The First Tier (Web Tier): The public subnet which holds the Application Load Balancer.
* The Second Tier (App Tier): The private subnet which holds the web servers such as EC2 Instance.
* The Third Tier (Database Tier): The private subnet which holds the database.
## AWS Services Used
* VPC (Virtual Private Cloud)
* Public Subnets
* Private Subnets
* Databases
* Web Servers
* Application Load Balancers
* Internet Gateway
* Route 53
## Scenario
A rapidly growing fintech startup in Africa wants to design a highly available and scalable AWS 3-tier architecture for their web app. They require seamless access control for their employees and customers, with security measures in place to protect sensitive data and prevent unauthorized access.
## Project Goal
* Design and deploy a highly available and scalable AWS 3-tier architecture. 
## Requirements
* An AWS account
## Summary
This architecture ensures high availability, scalability, and reliability by distributing the load, monitoring instance health, and scaling resources dynamically. The web tier serves the front-end and routes API calls, the application tier handles business logic and interacts with the database, and the database tier provides robust data storage and retrieval.
## Documentation
* I will be using the official AWS documentation.
* Reference: https://workshops.aws/card/Highly%20Available%20Web%20Application%20Workshop 
