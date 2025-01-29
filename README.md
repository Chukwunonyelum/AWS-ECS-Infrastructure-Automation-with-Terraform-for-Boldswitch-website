
# Terraform AWS Infrastructure Deployment

This repository contains Terraform configurations to deploy a comprehensive AWS infrastructure. The setup includes a **Virtual Private Cloud (VPC)**, **public and private subnets**, an **Application Load Balancer (ALB)**, **Auto Scaling Groups (ASG)**, an **ECS cluster**, a **Relational Database Service (RDS)**, **NAT Gateways**, **Simple Notification Service (SNS)**, **S3 buckets**, and various **security groups** to secure the environment.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Infrastructure Overview](#infrastructure-overview)
3. [Configuration Files](#configuration-files)
4. [Deployment Steps](#deployment-steps)
5. [Outputs](#outputs)
6. [Security Considerations](#security-considerations)
7. [Cleanup](#cleanup)

---

## Prerequisites

Before deploying the infrastructure, ensure you have the following:

- **Terraform**: Install Terraform from [here](https://www.terraform.io/downloads.html).
- **AWS CLI**: Install and configure the AWS CLI with the necessary credentials. Follow the instructions [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
- **AWS Account**: Ensure you have an AWS account with sufficient permissions to create the resources defined in the Terraform configurations.
- **SSH Key Pair**: Create an SSH key pair in the AWS region where you plan to deploy the infrastructure. This key pair will be used for EC2 instances.
- **Valid Email Address**: Required for SNS topic subscription to receive notifications.

---

## Infrastructure Overview

The infrastructure is designed to be **highly available**, **scalable**, and **secure**. Below is a high-level overview of the components:

### **Networking**
- **VPC**: A Virtual Private Cloud with public and private subnets across two availability zones.
- **Public Subnets**: Host the Application Load Balancer (ALB) and NAT Gateways.
- **Private Subnets**: Host EC2 instances (web servers), ECS containers, and the RDS database.
- **NAT Gateways**: Allow instances in private subnets to access the internet for updates and patches while remaining secure.

### **Compute & Orchestration**
- **ECS Cluster**: Manages containerized applications using AWS Fargate.
- **Auto Scaling Group (ASG)**: Automatically adjusts the number of EC2 instances based on demand.

### **Storage & Database**
- **S3 Bucket**: Stores Terraform state files and application data.
- **RDS Database**: A MySQL/Aurora database deployed in private subnets.

### **Load Balancing & Security**
- **Application Load Balancer (ALB)**: Distributes incoming traffic across EC2 instances in the private subnets.
- **Security Groups**: Restrict access to resources based on IP addresses and protocols.
- **IAM Roles**: ECS and EC2 instances have dedicated IAM roles for permissions.

### **Monitoring & Notifications**
- **CloudWatch Logs**: Captures logs for ECS containers and application logs.
- **SNS Topic**: Provides notifications for Auto Scaling Group events such as instance launches and terminations.

---

## Configuration Files

The repository includes the following Terraform configuration files:

### **Core Infrastructure**
- **`main.tf`**: Configures the AWS provider and Terraform backend.
- **`vpc.tf`**: Defines the VPC, subnets, internet gateway, and route tables.
- **`security-group.tf`**: Configures security groups for ALB, EC2, RDS, and ECS.

### **Compute & Orchestration**
- **`ecs.tf`**: Defines the ECS cluster, task definitions, and services.
- **`ecs-role.tf`**: Configures IAM roles for ECS tasks and execution.
- **`asg.tf`**: Configures Auto Scaling Groups and launch templates.

### **Networking & Load Balancing**
- **`alb.tf`**: Defines the Application Load Balancer (ALB), target groups, and listeners.
- **`nategateway.tf`**: Sets up NAT Gateways for private subnet internet access.

### **Database & Storage**
- **`rds.tf`**: Configures the RDS database instance and subnet group.
- **`s3.tf`**: Manages S3 buckets, including the remote Terraform state.

### **IAM & Permissions**
- **`backend.tf`**: Configures remote state storage using S3.
- **`providers.tf`**: Defines Terraform providers used in the configuration.

### **Logging & Notifications**
- **`sns.tf`**: Configures SNS topics and subscriptions.
- **`cloudwatch.tf`**: Manages CloudWatch log groups for ECS and EC2.

### **State Management & Variables**
- **`variable.tf`**: Defines Terraform variables.
- **`output.tf`**: Specifies Terraform outputs.
- **`.terraform.lock.hcl`**: Lock file for Terraform provider versions.

---

## Deployment Steps

### 1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

### 2. **Initialize Terraform**
   This command initializes the working directory and downloads the necessary providers.
   ```bash
   terraform init
   ```

### 3. **Review the Plan**
   Generate and review the execution plan to ensure the configuration will create the desired infrastructure.
   ```bash
   terraform plan
   ```

### 4. **Apply the Configuration**
   Apply the Terraform configuration to create the infrastructure.
   ```bash
   terraform apply
   ```
   Confirm the action by typing `yes` when prompted.

### 5. **Verify Deployment**
   Once the deployment is complete, Terraform will output the **VPC ID**, **public subnet IDs**, **ECS cluster details**, and the **website URL**. Use these outputs to verify the deployment.

### 6. **Subscribe to SNS Notifications**
   After the deployment, check your email for a subscription confirmation from AWS SNS. Confirm the subscription to start receiving notifications.

---

## Outputs

After a successful deployment, Terraform will output the following:

- **`vpc_id`**: The ID of the created VPC.
- **`public_subnet_az1_id`**: The ID of the public subnet in Availability Zone 1.
- **`website_url`**: The URL of the deployed website.
- **`ecs_cluster_name`**: The name of the ECS cluster.
- **`s3_bucket_name`**: The name of the S3 bucket used for remote state storage.

Example output:
```bash
vpc_id = "vpc-0123456789abcdef0"
public_subnet_az1_id = "subnet-0123456789abcdef0"
website_url = "https://www.cloudrookie.online"
ecs_cluster_name = "dev-ecs-cluster"
s3_bucket_name = "rookie-terraform-remote-state"
```

---

## Security Considerations

- **SSH Access**: SSH access to the EC2 instances is restricted to the IP address specified in the `ssh_location` variable.
- **Database Security**: The RDS database is deployed in private subnets and is only accessible from the web server security group.
- **ALB Security**: The Application Load Balancer only allows HTTP/HTTPS traffic on ports 80 and 443.
- **NAT Gateways**: NAT Gateways are used to allow outbound internet access for instances in private subnets while preventing inbound access.
- **IAM Policies**: ECS tasks and EC2 instances use least privilege IAM roles.
- **S3 Bucket Security**: The S3 bucket used for remote state storage is configured with encryption and access control.

---

## Cleanup

To avoid incurring unnecessary charges, destroy the infrastructure once it is no longer needed.

### 1. **Destroy the Infrastructure**
   ```bash
   terraform destroy
   ```
   Confirm the action by typing `yes` when prompted.

### 2. **Verify Deletion**
   Ensure that all resources have been deleted by checking the AWS Management Console.

---

## Conclusion

This README provides a comprehensive guide to deploying and managing the AWS infrastructure using Terraform. The modular structure allows for easy scaling, security enforcement, and automation.
