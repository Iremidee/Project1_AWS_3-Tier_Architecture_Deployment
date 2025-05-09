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

AWS has updated the VPC console to include a preview that allows us to visually see our subnets, availability zones, route tables, and network connections as you create them. You can hover over a subnet, and it will highlight in blue the route that will occur, making it easy to see your connections.

![VPC Preview](https://github.com/user-attachments/assets/0f9bdcf2-4b94-4efb-bc4e-bd67ba130c3d)

When ready, click “Create VPC.” And here is the real-time updates of the workflow being created.

![VPC Workflow](https://github.com/user-attachments/assets/ed6e792e-d4bc-4d59-b8ea-7dc0909be3e1)
![VPC Workflow3](https://github.com/user-attachments/assets/8c9ad7bc-3cf8-43b4-8340-282603f1441d)

When all of the VPC resources have finished creating, click “View VPC.”

![VPC Created](https://github.com/user-attachments/assets/49153834-326a-48b4-8d55-89784d085ae5)

I just created a VPC, 2 public subnets, 4 private subnets, a NAT gateway, an internet gateway, and some default route tables. 
Before I move further, I navigated to “Subnets” on the left side menu of the VPC console and selected one of the subnets I have created. Under the “Actions” tab, expanded the down arrow and selected “Edit subnet settings”.

![Subnets](https://github.com/user-attachments/assets/4ed81440-8fb4-4201-babb-5766808013d1)

Click “Enable auto-assign IPv4 address” and “Save”, I did this for all 6 the created subnets.

![Subnets 1](https://github.com/user-attachments/assets/583e547e-5210-48b6-9178-236b8afd517d)
![Subnets2](https://github.com/user-attachments/assets/2ad2a51a-9e21-4dcc-a7e3-0bfe10a7030d)

# Creating a Web Server Tier - Part 2
The web server tier also know as the presentation tier houses the user interface, such as the website that a user or client navigates to. It can also be thought of as the “front end”.

In this section I will create my first tier that represents the front end user interface. I need to create an EC2 instance that will host a custom webpage.

## Launch an Instance
Navigate to the EC2 console, click “Instances,” then “Launch instances.” Give the instance a name, select an AMI, select an instance type, and key pair.

![EC1](https://github.com/user-attachments/assets/d8d215f8-8ba4-461f-8748-5c1142d09f8b)
![EC2](https://github.com/user-attachments/assets/c8bcb4c6-3a22-4230-879c-baef2196fc9f)
![EC3](https://github.com/user-attachments/assets/2e9810b3-8f91-4c8c-b05d-d8d05526a924)

Under the Network settings you will see that the default VPC is selected. Click “Edit” and select the VPC you just created. Then select one of the public subnets you just created. For “Auto-assign public IP,” choose “Enable.”

![Network](https://github.com/user-attachments/assets/1fd2bfb0-ebd0-4327-8817-853a0b3af91b)

Under the Firewall settings we want to create a web tier security group that allows inbound internet traffic. Select “Create security group,” give it a name and description, and the select “HTTP” for type and “Anywhere” for source type. This will allow all inbound traffic on port 80. I also like to allow HTTPS (port 443) and SSH (port 22).

![Firewall1](https://github.com/user-attachments/assets/42a53e92-44b1-489d-a15d-0330d7153c44)
![Firewall2](https://github.com/user-attachments/assets/af0d44e6-1fff-446d-a32a-572328db7f9a)

Under the Configuration storage settings you can leave it as is. Open the Advanced details section and scroll all the way to the bottom where you see user data. We want our public IP address to point to a custom company site. We can have Apache web server launch at the time of the instance launch by putting a script in the user data field. The following script will install Apache and lead to a custom site that says “Company Website.”

![User Data](https://github.com/user-attachments/assets/7fc500fc-6b50-4d1f-a46b-530f77091011)


```
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd

cd /var/www/html

sudo echo "<h1>Company Website</h1>" >index.html
```
We are now ready to click “Launch instance.”

![EC4](https://github.com/user-attachments/assets/fc7372b7-6519-4af9-be86-6ba98950b2ea)

You should see that your instance is running, but this may take a few moments. Select the running instance and copy and paste the public IPv4 address in a web browser to see if you are met with a custom webpage.

## Create a Launch Template and an Auto Scaling Group
Ok so now we have a running EC2 instance with a functioning website. We want to attach an auto scaling group to the instance to increase our availability and reliability. When creating an auto scaling group, you need to define a launch template that tells EC2 what resources to use to launch the on-demand instances.

In the EC2 console, select “Auto Scaling Groups” from the left side bar menu, then click the orange “Create Auto Scaling group” button. Give the auto scaling group a name and click the blue “Create a launch template” link.

![ASG1](https://github.com/user-attachments/assets/4619600e-53bb-4b29-be9a-29f90399e86e)

This will open a new window where you can select the specifics of your launch template. I’m creating a template with the Amazon Linux 2 AMI and t2.micro instance type. Give the launch template a name and description.

![LT3](https://github.com/user-attachments/assets/d8371fdc-b08e-4183-a8bd-a3872e47a947)
![LT2](https://github.com/user-attachments/assets/e8bbfeaa-3a03-4d7e-a649-e7f175b6ee53)
![LT1](https://github.com/user-attachments/assets/b282e28e-5155-4091-9a93-40e428c41b4a)

Select an existing key pair or create a new one. Under Network settings, select the security group that you created earlier for the web tier. You can add additional EBS volumes if desired.

![LT4](https://github.com/user-attachments/assets/335b5d04-f293-4df9-89b8-2c83663704cf)
![LT5](https://github.com/user-attachments/assets/4f8e3c47-de6f-40bb-b0ae-cec195c6939c)
![LT6](https://github.com/user-attachments/assets/91ad0d6f-51b7-4f25-a208-7d02c2dd50e3)

Click “Create launch template.” Ok now, navigate back to the auto scaling group page, refresh the launch templates, then select the launch template you just created. This launch template is the blueprint the instance will use to create additional instances when it needs to scale up. Click “Next” when ready to proceed.

![ASG_LT](https://github.com/user-attachments/assets/4ca7a6ac-d607-4804-bee0-b0004d8c33df)

The next screen asks about network settings and has the default VPC selected. We want our web tier auto scaling group to use the VPC we created earlier, and scale across the two public subnets that we created. Select the appropriate VPC and subnets and click “Next.”

![Instance Launch Option](https://github.com/user-attachments/assets/0ff620f6-7f29-4203-bfcd-f3d06254b955)
![Instance Launch Option 2](https://github.com/user-attachments/assets/3d1ae15b-ce11-4327-9f34-8220c8bd9fa2)

You are now given the option to add a load balancer. A load balancer is a good idea to distribute traffic amongst the instances. Since we haven’t created one yet, we will select “Attach to a new load balancer” and choose “Application Load Balancer” because we are using HTTP connections. Give the load balancer a name and select “Internet-facing” for the scheme. This will allow us to communicate to the internet, not just internally. Under Network mapping, you should see the VPC that you created and the two public subnets in our auto scaling group. The load balancer needs a listener and target group. We can see HTTP is selected (port 80) and we will create a new target group.

![LB](https://github.com/user-attachments/assets/c0fe8632-7ca1-401d-b412-7ea52d853325)
![New LB1](https://github.com/user-attachments/assets/6fc92791-e396-48ee-8158-7e828e879791)
![New LB2](https://github.com/user-attachments/assets/35864084-6ce6-43d8-b05c-37d51f3e787e)

Click “ELB” under Health checks and “Enable group metrics collection within CloudWatch.” Both of these are optional settings. When ready, click “Next.”

![Health Check](https://github.com/user-attachments/assets/d8f22207-e38b-4d5d-a062-021f9dbf8667)

Next we configure the group size and scaling policies. We want a desired capacity of 1 instance, minimum capacity of 1 instance, and a maximum capacity of 2 instances. Selecting “Target tracking scaling policy” will allow you to apply a policy that resizes your Auto Scale group according to changes in demand. You can set when you want scaling to occur based on percentage of CPU utilization. In the below example, I’ve selected “Target tracking scaling policy” to scale when the average CPU utilization reaches 50%. When you are finished adjusting these settings, click “Next.”

![Configure1](https://github.com/user-attachments/assets/0a7730d5-25ef-4ef8-8525-e96656fe4bb6)
![Scaling1](https://github.com/user-attachments/assets/3968951f-6f24-4921-bcbb-35bd18f6e0c1)
![Scaling 2](https://github.com/user-attachments/assets/c0b92fb7-4ed0-4c6e-8001-8454ecb5192d)
![Scaling3](https://github.com/user-attachments/assets/faf27a50-99ce-4b0e-ade4-c38b55fd2ad5)

You have the option to add notifications (SNS topics) or tags, but I’ve skipped both of these. After reviewing your settings for the auto scaling group, click “Create auto scaling group.”

## Update Tier 1 Public Route Tables
Ok at this point, we’ve successfully created our web tier. We have a tier with a VPC, two public subnets with an autoscaling group, a security group, an internet gateway, and user data that will install Apache and create a custom company website landing page. The last thing we need to do for the web tier is make sure that the route tables that were automatically created when we created the VPC and subnets are associated correctly. 

Navigate back to the VPC dashboard and on the left side menu select “Route tables.” Select the web tier public route table and view the associated subnets. It should be the two public subnets we created.

![RouteTables](https://github.com/user-attachments/assets/15aa7eda-e1bb-4aa5-a3af-aca7dc9be23e)

This looks correct, so our web tier is now complete.

# Application Tier - Part 3
For the application tier, we will use the same VPC as tier 1 as well as 2 of the private subnets that we previously created. We want to again create an EC2 instance with an auto scaling group up to 2, and can use the same steps that we did to create our tier 1 instance and auto scaling group. We are going to change up the security group permissions to limit access from the public. Go ahead an launch an instance using the instructions from earlier.

## Launch an Instance

![EC2 App Tier1](https://github.com/user-attachments/assets/718fce92-87a5-47e7-9e5d-751f42e77f88)
![EC2 App Tier2](https://github.com/user-attachments/assets/a46431c5-f90b-4fdb-b201-15a0f3fe2ed7)
![EC2 App Tier3](https://github.com/user-attachments/assets/34ee7981-8020-44a6-b521-cc589566766d)

For Network settings, we want to limit access to the application tier for security purposes. You wouldn’t want any public user being able to access the application tier or back end, so we will adjust these permissions by creating a new security group. We want to allow SSH, HTTP, and ICMP traffic from our web tier security group. The ICMP rule will allow us to ping the application tier from the web tier. When adding your security groups, choose “Custom” source type, and this will allow you to select your web tier security group as the source. You do not need to auto-assign public IP for this tier, since it is a private subnet.

![EC2 App Net](https://github.com/user-attachments/assets/a315ebc5-a053-420c-8dc0-a0e7fdb71c5f)
![EC2 App Net2](https://github.com/user-attachments/assets/90cac34a-d283-48ea-aaeb-6ae26b1a4d2b)
![EC2 App Net3](https://github.com/user-attachments/assets/707fe7e2-8f86-4ba5-8051-d4538667d639)

After completing the network settings, go ahead and launch the instance.

## Create a Launch Template and an Auto Scaling Group
Next, let’s create our application tier auto scaling group. Navigate to the EC2 console and on the left side menu click “Auto Scaling Groups.” Make sure you create a new launch template since the security group permissions will be different for the application tier than they were for the web tier. Use the same process we did before to create an auto scaling group, and it should look something like this:

![App LT1](https://github.com/user-attachments/assets/93722595-bea3-44c1-acf3-c73701c32a1f)
![App LT2](https://github.com/user-attachments/assets/e8bc4bdc-4ae1-4012-a6df-ed390a879012)
![App LT3](https://github.com/user-attachments/assets/694d9655-d4aa-4d0d-b24d-2cb3b9f9682d)

Under Network settings, select the application tier security group you created previously.

![App LT Net](https://github.com/user-attachments/assets/32dd4c66-14f7-42f6-85c8-f8e065cacb1e)
![App LT4](https://github.com/user-attachments/assets/e7a24785-5b63-4b59-918d-b315c8a4aad9)

We aren’t adding any User data to this instance template, so go ahead and click “Create template.” Now that our application tier launch template is created, let’s go create our application tier auto scaling group. Navigate back to the EC2 console in the Create Auto Scaling group page refresh the launch templates. You should now see the application launch template you just created. Select the application template and click “Next.”

![App asglt1](https://github.com/user-attachments/assets/86723ee0-594e-4fea-8eea-3d74ca3e69a3)
![App asglt2](https://github.com/user-attachments/assets/e8b292cf-8c09-484d-aa42-97c5f82d4de9)

Choose the correct VPC and two of your private subnets (I used private-1-us-east-1a and private-2-us-east-1b).

![App asglt3](https://github.com/user-attachments/assets/95e3d3bb-2f18-428e-acc7-4a53cdaaea9e)
![App asglt4](https://github.com/user-attachments/assets/3f3724a3-f986-4bb0-918f-3f93d0e7c550)
![App asglt5](https://github.com/user-attachments/assets/b55bbf51-359d-4dc0-9a0d-6a461c243bf2)

After clicking “Next,” you are given the option to select a load balancer. Since the application tier uses private subnets, we can select an internal application load balancer. Ensure that the VPC and private subnets look correct.

![App ASGLT6](https://github.com/user-attachments/assets/3a66ca42-115c-4ebb-95fb-10bd2f843e37)
![App ASGLT7](https://github.com/user-attachments/assets/3281673f-cb51-4f7e-ab1c-abeb0d4cfe97)
![App ASGLT8](https://github.com/user-attachments/assets/eb56c18b-e892-4c26-acb0-7ad4ea7b72b2)

You have the option to include health checks and enable monitoring. Click “Next” to proceed.

![App ASGLT9](https://github.com/user-attachments/assets/e802c32d-b731-420e-9649-9ed597e933f6)

Choose how you want your application tier auto scaling group to scale. The configuration below will always keep one instance running and scale up to 2 when CPU utilization reaches 50%.

![App ASG Size](https://github.com/user-attachments/assets/8dcacea5-3c98-4611-94a1-11c4b432fc78)

Click “Next.” Add notifications and tags if you want, then review your settings and click “Create Auto Scaling group.”

## Update Tier 2 Private Route Tables
Navigate to the VPC console and on the left side menu select “Route tables.” Click one of your private route tables that was automatically created (mine is W9webtier-rtb-private1-us-east-1a) and look at the Subnet associations. Make sure this private route table is associated with two of the private subnets (I’ve chosen private-1-us-east-1a and private-2-us-east-1b).

![Route Table2](https://github.com/user-attachments/assets/e3e913af-87db-4932-ab7d-441524b70146)


# Database Tier - Part 4
Alright guys, we’re in the home stretch. We have successfully created two of our three tiers, and now we want to focus on our database. AWS offers several different types of databases, but for this example we will use a MySQL RDS database.

## Create a DB Subnet Group
The first thing we have to do is create our subnet groups, so navigate to the Aurora & RDS console and on the left side menu click “Subnet Groups” and then the orange “Create DB subnet group.” This part can be a bit tricky so bear with me. Start by giving your database subnet group a name and description. Then select the VPC that we have been using for this entire tutorial.

![DT1](https://github.com/user-attachments/assets/809a7b1d-01ad-4e52-af1b-9f702a34f5df)

Here’s the tricky part. Under Add subnets, you must first select the availability zones. You need to know which availability zones you used previously to house your third and fourth private subnets. If you don’t remember, head over to the VPC console, click “Subnets” on the left side menu, and then select your last two private subnets (make sure not to select the private subnets that you already used in tier 2). On the subnet page you can see the availability zones used.

![Subnetss](https://github.com/user-attachments/assets/837b5377-d98a-49bb-9ecf-b1c1f919809b)

Now go back to the RDS Create DB subnet group page and you will see the Add subnets section.
Select the availability zones that house your private subnets you’re going to use.

After selecting the correct availability zones, you can click the down arrow on Subnets to expand your available options. This is the trickiest part. Note that the subnets listed only show their subnet ID, not their name. You need to know the subnet ID of your last two private subnets so that you select the correct ones for tier 3. Go back to the VPC console and Subnets tab where you just found your availability zones, and grab the subnet IDs from there.

Once you have them, select them in the list and click “Create.”

![DT2](https://github.com/user-attachments/assets/70f91520-6dc6-4612-8b2e-60e486ff14c6)

## Create a MySQL Datatbase
Your database subnet group is now created, so we can now click on “Databases” on the left side menu of the RDS console, and then “Create database.” Select “Standard create” and “MySQL.”

![Database](https://github.com/user-attachments/assets/669fd784-8883-421b-a969-2cfb3ff67dc7)

You have the option to create a multi-availability zone cluster that consists of three DB instances (one primary instance and two readable standby instances). This is very useful if a database instance fails, you have two backups ready to launch. For this example, we do not need this feature.

![Database1](https://github.com/user-attachments/assets/22c1862c-724d-4e19-9807-f41eb5c9858c)

We will use the free tier template. If you were launching this database in Production mode or Dev/Test mode you would have access to the availability and durability deployment options. However, for our example, this is also not needed.

![Database 2](https://github.com/user-attachments/assets/53acf917-64f2-46fa-97d2-b38bba519426)
![Database3](https://github.com/user-attachments/assets/6d3cb25c-2f78-44de-ac12-85023f0a4d47)

Under the Settings section, give the DB instance a name and create a master username and password for the database. This username and password should be different than your AWS account user login, as it is specific to the database you are creating.

![Database setting](https://github.com/user-attachments/assets/8a78eb7b-88f4-4429-ac13-0ea3458e20a4)

Under Instance configuration, the burstable classes option is the only one available for the free tier, thus it is pre-selected. You can adjust your instance type, and I’ve chosen db.t2.micro, which is enough for one VPC. Adjust storage as needed, but for this example I’ve left it as the default settings.

![Database Config](https://github.com/user-attachments/assets/809e3832-4b07-405e-874a-83a59bcff23b)
![Database Storage](https://github.com/user-attachments/assets/49b20e29-2a72-461d-884f-80adc1140afa)

The next section is Connectivity and there are a lot of options here. Under Compute resource you can choose if you want to set up a connection to a compute resource for this database. This option automatically sets up your VPC and related network settings during database creation to enable a secure connection between the EC2 instance and the RDS database. For my example, I want to setup my network settings manually, so I’ve selected “Don’t connect to an EC2 compute resource.” The Network type is IPv4, and then you can select the VPC that we have been using for this tutorial. Next select the DB subnet group that you recently created. Since this is our company database and we don’t want the public accessing it, we’re going to select “No” for Public access. Under VPC security group, we want to create a new security group. Give the security group a name and select a preferred availability zone. You also have the option to create an RDS Proxy, but this does incur additional costs, so we will leave that option unselected.

![Database Connect](https://github.com/user-attachments/assets/aec7102d-bb2e-4a38-9bcf-4188c73fed2c)
![Database Connect2](https://github.com/user-attachments/assets/550bfea4-e5e1-416d-a825-7cb1c7ab6bc6)

For Database authentication you have three options. Then you have the option for enhanced monitoring and additional configurations. When ready to proceed, click the “Create database” button.

![Database auth](https://github.com/user-attachments/assets/c79877b2-bc8d-489d-a18f-600d54dde3cb)

## Update Database Tier Security Group
We created a new security group for the database tier, but weren’t given the opportunity to adjust the permissions. Navigate over to the VPC console, select Security groups on the left side menu, and then find the database tier security group you just created.

You can see that by default, the database security group has an inbound rule to allow MySQL/Aurora traffic on port 3306 from your IP address. Delete this rule.

We want to allow inbound traffic for MySQL from the application tier security group instead. Click “Add rule,” select “MySQL/Aurora” for Type (this will insert port 3306), choose custom Source, and then select your application tier security group and “Save rules.”

![Database  Inbound](https://github.com/user-attachments/assets/32cb9fce-c7a1-416b-9458-b8079630d390)

## Update Tier 3 Private Route Tables
The last thing to do for the database tier is to adjust the route tables that were created by default earlier. Navigate to the VPC console and click “Route tables” on the left side menu. Select the private route table that you want to associate with your database tier private subnets, then click the Subnet associations tab to see if they are correctly associated. If the are not, you can edit subnet associations to add the correct subnets.

![Route Tables 3](https://github.com/user-attachments/assets/f6179d83-d4bd-4df7-8e9c-8577634c391d)

# Testing - Part 5
Now that the architecture is built, let’s test it.
Let’s make sure we can connect to our web server instance via SSH:


