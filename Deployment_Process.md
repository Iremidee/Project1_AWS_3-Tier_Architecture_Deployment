# Networking and Security - Part 1
## The Underlying Network Architecture 
Looking at our architecture diagram, this is the base environment upon which the 3-tier application architecture will be built.
This base network consists of:
* A VPC
* Two (2) public subnets spread across two availability zones (Web Tier)
* Two (2) private subnets spread across two availability zones (Application Tier)
* Two (2) private subnets spread across two availability zones (Database Tier)
* One (1) public route table that connects the public subnets to an internet gateway

## Creating a VPC and Subnets
After logging into the AWS console, navigate to the VPC resource. Click “Create VPC”, there are two options: “VPC only” or “VPC and more.” 
Because I'm creating a VPC with multiple subnets, availability zones, and more, I choose “VPC and more.” 
Tag auto-generation, which will tag all of the resources of the VPC. This helps to stay organized.


![VPC1](https://github.com/user-attachments/assets/d3b32414-ad98-4d27-85a5-4883942ec19a)

![VPC2](https://github.com/user-attachments/assets/53f58861-a5d8-435a-bd43-24342e6b4b7f)
![VPC3](https://github.com/user-attachments/assets/d915532e-5cec-4f84-a67e-89a61a8355ac)
![VPC4](https://github.com/user-attachments/assets/b44302ab-dd9b-47a3-869f-0d6c30503f2b)

AWS has updated the VPC console to include a preview that allows you to visually see your subnets, availability zones, route tables, and network connections as you create them. You can hover over a subnet, and it will highlight in blue the route that will occur, making it easy to see your connections.
![VPC Preview](https://github.com/user-attachments/assets/0f9bdcf2-4b94-4efb-bc4e-bd67ba130c3d)

When ready, click “Create VPC.” You will get real-time updates of the workflow being created.
![VPC Workflow](https://github.com/user-attachments/assets/ed6e792e-d4bc-4d59-b8ea-7dc0909be3e1)
![VPC Workflow3](https://github.com/user-attachments/assets/8c9ad7bc-3cf8-43b4-8340-282603f1441d)

When all of your VPC resources have finished creating, click “View VPC.”
![VPC Created](https://github.com/user-attachments/assets/49153834-326a-48b4-8d55-89784d085ae5)
We’ve now created a VPC, 2 public subnets, 4 private subnets, a NAT gateway, an internet gateway, and some default route tables. Before we move further, navigate to “Subnets” on the left side menu of the VPC console and select one of the subnets you have created. Under the “Actions” tab, expand the down arrow and select “Edit subnet settings.”
![Subnets](https://github.com/user-attachments/assets/4ed81440-8fb4-4201-babb-5766808013d1)
Click “Enable auto-assign IPv4 address” and “Save.” Do this for all 6 of your subnets.
![Subnets 1](https://github.com/user-attachments/assets/583e547e-5210-48b6-9178-236b8afd517d)
![Subnets2](https://github.com/user-attachments/assets/2ad2a51a-9e21-4dcc-a7e3-0bfe10a7030d)

# Web Server Tier - Part 2

# Application Tier - Part 3

# Database Tier - Part 4

# Testing - Part 5
