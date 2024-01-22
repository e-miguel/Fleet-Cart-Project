# Fleet-Cart-Project (three-tier architecture)
Host a Dynamic Ecommerce Website on AWS

# Tech Stack
AWS - S3, VPC with public and private subnets, security groups, EC2 instances, NAT gateways, RDS, Application load balancer, Route 53, Auto scaling group, Certificate manager, EFS and more.
MySQL Workbench, Apache, and BASH/PHP.

# Objectives
1. Create VPC with public and private subnets
2. Create NAT gateways in the public subnets
3. Create the security group
4. Create the RDS instance
5. Create S3 buckets
6. Create an IAM role with S3 policy
7. Deploy eCommerce website
8. Import the dummy date for the website
9. Create an amazon machine image (AMI)
10. Create an application load balancer
11. Register a new domain name in route 53
12. Create a record set in route 53
13. Register for an SSL certificate in AWS certificate manager
14. Create an HTTPS listener
15. SSH into an EC2 instance in the private subnet
16. Update the ENV file
17. Create another AMI
18. Create auto scaling group
19. Clean up

# Reference Architecture

![image](https://github.com/e-miguel/Fleet-Cart-Project/assets/134418850/862fa1f2-97c9-41e0-9fed-457f99370d48)

1. VPC with public an private subnets in 2 availability zones
2. An internet gateway is used to allow communication between instances in VPC and the internet
3. We are using 2 availability zones for high availability and fault tolerance
4. Resources such as NAT gateway, bastion host, and application load balancer uses public subnets
5. We will put the webservers and database servers in the private subnets to protect them
6. The NAT gateway allows the instances in the private app subnets and private data subnets to access the internet
7. We are using mysql RDS database
8. We are using EC2 instances to host our website
9. Application load balancer is used to distribute web traffic across an auto scaling group of EC2 instances in multiple AZs
10. Using auto scaling group to dynamically create our EC2 instances to make our website highly available, scalable, fault-tolerant, and elastic
11. We are using route 53 to register our domain name and create a record set
12. We are using AWS S3 to store our web files
13. We will use IAM role to give EC2 permission to download web files from AWS S3
14. Once we have installed our website on an EC2 instance, we will use the EC2 instance we installed our website on to create an AMI

# Outcome
