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



# Web Server Tier - Part 2

# Application Tier - Part 3

# Database Tier - Part 4

# Testing - Part 5
