# Secure AWS Architecture with ALB and private EC2 Instances
This repository showcases a cloud networking architecture built on AWS, focusing on secure traffic flow and high availability. The environment is deployed within a custom VPC, separating public-facing components from private compute resources. An Application Load Balancer handles inbound web traffic, while EC2 instances operate privately behind it. Outbound internet access for private instances is enabled through a NAT Gateway.

## Architecture Diagram

![AWS ALB Architecture](content/architecture.drawio.png)


## Resource Map

![Map showing connections](content/resource-map.png)
AWS VPC Resource Map highlighting public and private routing paths.
