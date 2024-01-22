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
![image](https://github.com/e-miguel/Fleet-Cart-Project/assets/134418850/53484d54-be53-417b-b10f-a4c4bd54be48)
![image](https://github.com/e-miguel/Fleet-Cart-Project/assets/134418850/02d493f1-0090-47ed-8741-40b0cf6e527a)

# Deploy E-Commerce Website Commands
#1. update ec2 instance
sudo su
sudo yum update -y

2. install apache 
sudo yum install -y httpd httpd-tools mod_ssl
sudo systemctl enable httpd 
sudo systemctl start httpd

3. install php 7.4
sudo amazon-linux-extras enable php7.4
sudo yum clean metadata
sudo yum install php php-common php-pear -y
sudo yum install php-{cgi,curl,mbstring,gd,mysqlnd,gettext,json,xml,fpm,intl,zip} -y

4. install mysql5.7
sudo rpm -Uvh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
sudo yum install mysql-community-server -y
sudo systemctl enable mysqld
sudo systemctl start mysqld

5. set permissions
sudo usermod -a -G apache ec2-user
sudo chown -R ec2-user:apache /var/www
sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
sudo find /var/www -type f -exec sudo chmod 0664 {} \;

6. download the FleetCart zip from s3 to the html derectory on the ec2 instance
sudo aws s3 sync s3://fleetcart-webfiles0 /var/www/html

7. unzip the FleetCart zip folder
cd /var/www/html
sudo unzip FleetCart.zip

8. move all the files and folder from the FleetCart directory to the html directory
sudo mv FleetCart/* /var/www/html

9. move all the hidden files from the FleetCart diretory to the html directory
sudo mv FleetCart/.DS_Store /var/www/html
sudo mv FleetCart/.editorconfig /var/www/html
sudo mv FleetCart/.env /var/www/html
sudo mv FleetCart/.env.example /var/www/html
sudo mv FleetCart/.eslintignore /var/www/html
sudo mv FleetCart/.eslintrc /var/www/html
sudo mv FleetCart/.gitignore /var/www/html
sudo mv FleetCart/.htaccess /var/www/html
sudo mv FleetCart/.npmrc /var/www/html
sudo mv FleetCart/.php_cs /var/www/html
sudo mv FleetCart/.rtlcssrc /var/www/html

10. delete the FleetCart and FleetCart.zip folder
sudo rm -rf FleetCart FleetCart.zip

11. enable mod_rewrite on ec2 linux, add apache to group, and restart server
sudo sed -i '/<Directory "\/var\/www\/html">/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/httpd/conf/httpd.conf
chown apache:apache -R /var/www/html 
sudo service httpd restart

# Import the Dummy Data for the Website Commands
sudo su
sudo aws s3 sync s3://fleetcart-dummydata0 /home/ec2-user
sudo unzip dummy.zip
sudo mv dummy/* /var/www/html/public
sudo mv -f dummy/.DS_Store /var/www/html/public
sudo rm -rf /var/www/html/storage/framework/cache/data/cache
sudo rm -rf dummy dummy.zip
chown apache:apache -R /var/www/html 
sudo service httpd restart
