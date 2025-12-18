# Secure AWS Architecture with ALB and private EC2 Instances
This repository showcases a cloud networking architecture built on AWS, focusing on secure traffic flow and high availability. The environment is deployed within a custom VPC, separating public-facing components from private compute resources. An Application Load Balancer handles inbound web traffic, while EC2 instances operate privately behind it. Outbound internet access for private instances is enabled through a NAT Gateway.

## Architecture Diagram

![AWS ALB Architecture](content/architecture.drawio.png)

## Components and Roles
• VPC – Provides network isolation and defines the IP address space for the environment.

• Public Subnets - Host internet-facing resources such as the Application Load Balancer and provide a route to the Internet Gateway.

• Private Subnets – Host EC2 instances running NGINX with no direct internet access.

• Internet Gateway (IGW) – Enables inbound and outbound internet connectivity for resources in public subnets.

• NAT Gateway – Allows private EC2 instances to initiate outbound internet connections while remaining inaccessible from the internet.

• Application Load Balancer (ALB) – Acts as the single entry point for inbound HTTP/HTTPS traffic and distributes requests across healthy backend instances.

• Target Group – Defines how traffic is routed from the ALB to EC2 instances and performs health checks.

• EC2 Instances (NGINX) – Serve application content privately behind the load balancer.

• Security Groups – The ALB security group allows inbound HTTP/HTTPS traffic from the internet, while the EC2 security group only permits HTTP traffic from the ALB, preventing direct public access to backend instances.

• Route 53 – Provides DNS resolution and routes domain traffic to the ALB using an ALIAS record.

• AWS Certificate Manager (ACM) – Manages SSL/TLS certificates used by the ALB to enable HTTPS.



## Resource Map

![Map showing connections](content/resource-map.png)
AWS VPC Resource Map highlighting public and private routing paths.

## Traffic Flow
• Client requests first resolve the application domain via Route 53, which routes traffic to the Application Load Balancer.

• Client requests enter the VPC through the Application Load Balancer in the public subnets.

• The ALB distributes incoming HTTP traffic to healthy EC2 instances in private subnets.

• Backend EC2 instances are not publicly reachable and only accept HTTP traffic from the ALB security group.

• Private instances use the NAT Gateway for outbound internet access (e.g., updates).

## Security Groups 
• ALB Security Group – Allows inbound HTTPS (443) and HTTP (80) traffic from the internet and forwards requests to backend targets. Optional HTTP (80) access can be used for redirection to HTTPS.

• EC2 Security Group – Allows inbound HTTP (80) traffic only from the ALB security group, preventing direct public access to private instances.

This security group design enforces least privilege access by ensuring all inbound traffic passes through the Application Load Balancer while keeping backend EC2 instances fully private and inaccessible from the internet.

# DNS and TLS Notes

DNS & TLS Notes

• Route 53 (DNS) – A public hosted zone is used to manage the domain. An ALIAS record is used in Route 53 to point the domain to the Application Load Balancer, so users can access the application via a friendly URL instead of the ALB DNS name.

ACM (TLS/SSL) – AWS Certificate Manager provides the SSL/TLS certificate used by the ALB’s HTTPS (443) listener. This enables encrypted connections from clients to the load balancer, while backend instances remain private and are reached only through the ALB.

