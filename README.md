

Terraform AWS Infrastructure Deployment
This repository contains Terraform configurations to deploy a comprehensive AWS infrastructure. The setup includes a Virtual Private Cloud (VPC), public and private subnets, an Application Load Balancer (ALB), Auto Scaling Groups (ASG), an ECS cluster, a Relational Database Service (RDS), NAT Gateways, Simple Notification Service (SNS), and various security groups to secure the environment.

Table of Contents
Prerequisites
Infrastructure Overview
Configuration Files
Deployment Steps
Outputs
Security Considerations
Cleanup
Prerequisites
Before deploying the infrastructure, ensure you have the following:

Terraform: Install Terraform from here.
AWS CLI: Install and configure the AWS CLI with the necessary credentials. Follow the instructions here.
AWS Account: Ensure you have an AWS account with sufficient permissions to create resources.
SSH Key Pair: Create an SSH key pair in the AWS region where you plan to deploy. This key pair will be used for EC2 instances.
Valid Email Address: Required for SNS topic subscription to receive notifications.
Infrastructure Overview
The infrastructure is designed to be highly available and secure. Below is an overview of the key components:

Networking
VPC: A Virtual Private Cloud with public and private subnets across two availability zones.
Public Subnets: Host the Application Load Balancer (ALB) and NAT Gateways.
Private Subnets: Host EC2 instances (web servers), ECS containers, and the RDS database.
NAT Gateways: Allow instances in private subnets to access the internet for updates and patches while remaining secure.
Compute & Orchestration
ECS Cluster: Manages containerised applications using AWS Fargate.
Auto Scaling Group (ASG): Automatically adjusts the number of EC2 instances based on demand.
Storage & Database
S3 Bucket: Stores state files and application data.
RDS Database: A MySQL/Aurora database deployed in private subnets.
Load Balancing & Security
Application Load Balancer (ALB): Distributes traffic across EC2 instances in private subnets.
Security Groups: Restrict access to resources based on IP addresses and protocols.
IAM Roles: ECS and EC2 instances have dedicated IAM roles for permissions.
Monitoring & Notifications
CloudWatch Logs: Captures logs for ECS containers and application logs.
SNS Topic: Provides notifications for Auto Scaling Group events such as instance launches and terminations.
Configuration Files
The repository includes the following Terraform configuration files:

Core Infrastructure
main.tf – Configures the AWS provider and Terraform backend.
vpc.tf – Defines the VPC, subnets, internet gateway, and route tables.
security-group.tf – Configures security groups for ALB, EC2, RDS, and ECS.
Compute & Orchestration
ecs.tf – Defines the ECS cluster and task definitions.
ecs-role.tf – Configures IAM roles for ECS tasks and execution.
asg.tf – Configures Auto Scaling Groups and launch templates.
Networking & Load Balancing
alb.tf – Defines the Application Load Balancer (ALB), target groups, and listeners.
nategateway.tf – Sets up NAT Gateways for private subnet internet access.
Database & Storage
rds.tf – Configures the RDS database instance and subnet group.
s3.tf – Manages S3 buckets, including the remote Terraform state.
IAM & Permissions
backend.tf – Configures remote state storage using S3.
providers.tf – Defines Terraform providers used in the configuration.
Logging & Notifications
sns.tf – Configures SNS topics and subscriptions.
cloudwatch.tf – Manages CloudWatch log groups for ECS and EC2.
State Management & Variables
variable.tf – Defines Terraform variables.
output.tf – Specifies Terraform outputs.
.terraform.lock.hcl – Lock file for Terraform provider versions.
Deployment Steps
1. Clone the Repository
bash
Copy
Edit
git clone <repository-url>
cd <repository-directory>
2. Initialize Terraform
This downloads necessary providers and sets up the working directory.

bash
Copy
Edit
terraform init
3. Review the Execution Plan
Check what changes Terraform will apply before deployment.

bash
Copy
Edit
terraform plan
4. Apply the Configuration
Deploy the AWS infrastructure:

bash
Copy
Edit
terraform apply
Confirm the action by typing "yes" when prompted.

5. Verify Deployment
After deployment, Terraform will output VPC ID, public subnet IDs, ECS cluster details, and the website URL.

6. Subscribe to SNS Notifications
Check your email for a confirmation link from AWS SNS. Confirm the subscription to start receiving notifications.

Outputs
After a successful deployment, Terraform will output the following:

vpc_id – ID of the created VPC.
public_subnet_az1_id – ID of the public subnet in AZ1.
website_url – URL of the deployed application.
Example output:

bash
Copy
Edit
vpc_id = "vpc-0123456789abcdef0"
public_subnet_az1_id = "subnet-0123456789abcdef0"
website_url = "https://www.cloudrookie.online"
Security Considerations
SSH Access: Limited to allowed IPs using ssh_location.
Database Security: The RDS instance is in a private subnet and only accessible via allowed security groups.
ALB Security: Only allows HTTP/HTTPS traffic on ports 80 and 443.
NAT Gateways: Provide outbound internet access for private instances without exposing them to the internet.
IAM Policies: ECS tasks and EC2 instances use least privilege IAM roles.
Cleanup
To avoid unnecessary charges, destroy the infrastructure when no longer needed.

1. Destroy Resources
bash
Copy
Edit
terraform destroy
Confirm the action by typing "yes" when prompted.

2. Verify Deletion
Ensure that all AWS resources have been deleted by checking the AWS Management Console.

Conclusion
This README provides a comprehensive guide to deploying and managing AWS infrastructure using Terraform. The modular structure allows easy scaling, security enforcement, and automation.


