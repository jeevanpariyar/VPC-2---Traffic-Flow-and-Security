## Introducing Today's Project!

### ✍️ What are we here to do today?

In this project, I will demonstrate;

Part 1: Build a Virtual Private Cloud\
Part 2: You are here!\
Part 3: Creating a Private Subnet\
Part 4: Launching VPC Resources\
Part 5: Testing VPC Connectivity\
Part 6: VPC Peering\
Part 7: VPC Monitoring with Flow Logs\
Part 8: Access S3 from a VPC\
Part 9: VPC Endpoints

I'm doing this project to learn VPC connections and security of VPC.

### ✍️ What are we doing in this step?

In this step, I will ;\
Create a VPC.\
Create subnets.\
Create an internet gateway.

because this will help  us th understand how VPC is created an configured and how to secure resources in VPC.

### ✍️ What are we doing in this step?

In this step, I will create a route Table .

because Even though you've created an internet gateway and attached it to your VPC, you still have to tell the resource in your public subnet how to get to the internet.

You'll have to set up route tables to direct traffic from your resource to your internet gateway!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is an AWS service that allows you to create a logically isolated virtual network in the AWS Cloud, where you can launch and manage resources such as EC2 instances, databases, and applications.

Why is it useful?

\- Network isolation: Keeps your AWS resources in a logically isolated network.\
\- Security: Controls traffic using security groups and Network ACLs (NACLs).\
\- IP address management: Lets you define IPv4 or IPv6 address ranges using CIDR blocks.\
\- Traffic control: Uses route tables, Internet Gateways, and NAT Gateways to manage network traffic.\
\- Flexible architecture: Supports public and private subnets for hosting web servers, application servers, and databases.

### How I used Amazon VPC in this project

In today's project, I created an Amazon VPC and configured its networking components, including subnets, route tables, an Internet Gateway, security groups, and Network ACLs (NACLs). I configured a route to allow internet-bound traffic through the Internet Gateway and set security rules to control inbound and outbound traffic. This hands-on project helped me understand how to build a secure, isolated network environment in AWS and manage communication between cloud resources.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was that creating a VPC alone does not provide internet connectivity. I learned that I also need to configure subnets, route tables, an Internet Gateway, and appropriate security rules to enable secure communication between AWS resources and the internet.

### This project took me...

This project took me approximately 1 -2 hours to complete, including creating the VPC, configuring subnets, route tables, an Internet Gateway, security groups, and NACLs, and reviewing the concepts I learned during the hands-on practice.

## Route tables

Route Tables in AWS: A route table contains a set of rules called routes that determine where network traffic from a subnet or gateway should go, such as to an Internet Gateway, NAT Gateway, or another network.\
Example:\
\- 0.0.0.0/0 → Internet Gateway (IGW): Allows internet-bound traffic to route through the IGW.\
\- 10.0.0.0/16 → local: Allows communication within the VPC.\
In this project: We configure route tables and associate them with subnets to control how traffic flows between our VPC, the internet, and other networks.

We need a route table to make a subnet public because it defines how network traffic travels from the subnet to the internet. By adding a route 0.0.0.0/0 → Internet Gateway (IGW) and associating the route table with the subnet, we enable internet-bound traffic to reach the Internet Gateway. Note: An EC2 instance also needs a public IPv4 address and appropriate security group and network ACL rules for internet connectivity.

## Route destination and target

\- Destination: The range of IP addresses where the traffic is going (e.g., 0.0.0.0/0 means all IPv4 addresses).\
\- Target: The destination resource that forwards the traffic, such as an Internet Gateway (IGW), NAT Gateway, or local VPC route.\
Example: 0.0.0.0/0 → igw-12345678 means all IPv4 traffic not matched by a more specific route is directed to the Internet Gateway.

The route in my route table that directed internet-bound traffic to my internet gateway had a destination of 0.0.0.0/0  and a target of igw-0fed825c546fbb635

### ✍️ What are we doing in this step?

In this step, I will Create a security group.

Security Group in AWS: A security group acts as a virtual firewall for AWS resources such as EC2 instances. It controls inbound (incoming) and outbound (outgoing) traffic based on rules that specify protocols, ports, and IP addresses.\
Example:\
\- Inbound rule: Allow SSH (port 22) from your IP address to access an EC2 instance.\
\- Inbound rule: Allow HTTP (port 80) to host a public website.\
\- Outbound rule: Allow traffic from the EC2 instance to the internet.\
In this project: We configure security groups to control which traffic can reach our EC2 instances and what traffic they can send, helping keep our VPC resources secure.

![Route destination and target](https://nextwork.ai/intense_gray_jolly_kokako/uploads/fe5ed473-5874-57e1-a58f-d9b3d652330c_0a07b191)

## Security groups

A security group acts as a virtual firewall for AWS resources such as EC2 instances. It controls inbound (incoming) and outbound (outgoing) traffic based on rules that specify protocols, ports, and IP addresses.

### Inbound vs Outbound rules

Inbound rules control the incoming traffic allowed to reach resources, such as EC2 instances, associated with a security group.\
I  configured an inbound rule that allows HTTP (Port 80): Allows incoming web traffic from anywhere (0.0.0.0/0) to access the hosted website.

Outbound rules control the outgoing traffic allowed from resources, such as EC2 instances, associated with a security group.

By default, my security group's outbound rule is All traffic (All protocols and ports): Allows outgoing traffic to any IPv4 address (0.0.0.0/0).

### ✍️ What are we doing in this step?

In this step, I will configure NACL .

Think of Network ACLs as traffic cops stationed at every entry and exit point of your subnet, checking each data packet against a table of ACL rules before allowing them through

![Security groups](https://nextwork.ai/intense_gray_jolly_kokako/uploads/fe5ed473-5874-57e1-a58f-d9b3d652330c_92b0b0b4)

## Network ACLs

NACL (Network Access Control List) in AWS is a security layer that acts as a firewall at the subnet level. It controls inbound and outbound traffic using allow and deny rules.\
\- Inbound rules: Control incoming traffic to the subnet.\
\- Outbound rules: Control outgoing traffic from the subnet.\
\- Stateless: Return traffic must be explicitly allowed by the appropriate rules.\
\- Rule evaluation: Rules are evaluated in ascending numerical order; the first matching rule applies.\
NACL: Denies all traffic by default until you add rules.

Security Group vs. NACL: A security group operates at the resource level and is stateful, while a NACL operates at the subnet level and is stateless.

### Security groups vs. network ACLs

Security Group vs. NACL: A security group operates at the resource level and is stateful, while a NACL operates at the subnet level and is stateless.

## Default vs Custom Network ACLs

### Similar to security groups, network ACLs use inbound and outbound rules

Default NACL: Allows all inbound and outbound traffic by default.

In contrast, a custom ACL’s inbound and outbound rules are automatically set to Deny All Trafic.

![Default vs Custom Network ACLs](https://nextwork.ai/intense_gray_jolly_kokako/uploads/fe5ed473-5874-57e1-a58f-d9b3d652330c_4faeb056)

## Tracking VPC Resources

I created additional Resources using AWS CLI . Instead of my usual region, I used Another region. Teams would use multiple regions to launch resources.

EC2 Global View is an AWS feature that allows you to view and monitor your EC2 resources across multiple AWS Regions from a single dashboard.\
You can find resources such as EC2 instances, VPCs, subnets, security groups, EBS volumes, and key pairs across different Regions.\
In this project: I use EC2 Global View to get a centralized overview of my AWS infrastructure and check which resources are available in each Region.

Now that I've learnt about EC2 Global View, I'd use it again to when I need to monitor and manage AWS resources across multiple Regions from one place, verify resource configurations, identify existing VPCs and EC2 instances, and troubleshoot infrastructure issues without switching between Regions manually.
