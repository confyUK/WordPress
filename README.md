# Host a WordPress Website on AWS

## Overview
This project demonstrates how to deploy a scalable and highly available WordPress website on AWS using a well-architected reference design. The infrastructure is built with a combination of EC2 instances, RDS, Elastic File System (EFS), and an Application Load Balancer for distributing traffic.

## Architecture Diagram
Refer to the uploaded architecture diagram for a visual representation of the infrastructure setup.

## AWS Services Used
- **Amazon VPC**: Custom Virtual Private Cloud with public and private subnets.
- **EC2 Instances**: Hosts the WordPress application.
- **Auto Scaling Group**: Automatically manages EC2 instances.
- **Elastic Load Balancer**: Distributes incoming traffic.
- **Amazon RDS**: Managed database service for WordPress.
- **Elastic File System (EFS)**: Shared file storage for EC2 instances.
- **Amazon Route 53**: Domain name resolution.
- **AWS Certificate Manager**: Manages SSL/TLS certificates.
- **Security Groups**: Restricts access to resources.
- **Simple Notification Service (SNS)**: Alerts on Auto Scaling events.

## Deployment Steps
### 1. Setup VPC and Networking
- Create a VPC with public and private subnets across two availability zones.
- Deploy an Internet Gateway for public access.
- Configure Security Groups to allow necessary traffic.

### 2. Deploy EC2 Instances
- Launch EC2 instances in private subnets.
- Use a Launch Template for Auto Scaling.

### 3. Install and Configure WordPress
SSH into the EC2 instance and run:
```bash
sudo yum update -y
sudo yum install -y httpd php php-mysqlnd php-cli php-json php-xml php-mbstring php-gettext
sudo systemctl enable httpd
sudo systemctl start httpd
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
sudo cp -r wordpress/* /var/www/html/
```

### 4. Configure Auto Scaling Group
- Use the provided launch template for automated scaling.

### 5. Setup Amazon RDS
- Create an RDS instance for the WordPress database.
- Update `wp-config.php` with database credentials.

### 6. Configure EFS for Shared Storage
```bash
EFS_DNS_NAME=<your-efs-dns>
sudo mount -t nfs4 "$EFS_DNS_NAME":/ /var/www/html
```

### 7. Setup Load Balancer and Route 53
- Configure an Application Load Balancer.
- Register a domain in Route 53.
- Point the domain to the load balancer.

## Security Considerations
- Restrict database access using security groups.
- Use AWS Certificate Manager for SSL.
- Regularly update and patch instances.

## Scaling and Maintenance
- Auto Scaling ensures high availability.
- CloudWatch monitors application performance.
- SNS sends notifications for key infrastructure events.

## Conclusion
By following this guide, you can deploy a production-ready WordPress website on AWS, ensuring scalability, reliability, and security.

