# localstack-ec2-actions

This repository provides a complete guide and Terraform configuration for securely deploying and managing a LocalStack instance on AWS EC2. It includes step-by-step instructions to set up SSH access using a PEM file and automate interactions with LocalStack through GitHub Actions, ensuring secure and controlled access to your testing environment.

### Motivation
- **Spin-Up Time**: Running LocalStack directly in CI/CD pipelines can be time-consuming. By deploying LocalStack on an EC2 instance, we reduce the spin-up time significantly.
- **Cost Considerations**: Running LocalStack on a private GITHUB or in CI environments can become expensive. Using Spot Instances on AWS EC2 helps manage costs effectively.
- **Branch Testing**: This setup allows testing on feature branches before merging into the main branch, ensuring higher code quality and fewer integration issues.


## Features
- Create an S3 bucket for Terraform state storage.
- Enable versioning and server-side encryption on the S3 bucket.
- Set up a DynamoDB table for state locking.
- Configure Terraform to use the S3 bucket for state storage.
- Configure Terraform to use the DynamoDB table for state locking.
- Terraform configuration for deploying LocalStack on AWS EC2.
- Secure SSH access setup using PEM files.
- Automating interactions with LocalStack via GitHub Actions.
- Step-by-step instructions for setup and deployment.

## Infrastructure Overview

### Terraform State Management

Create an S3 bucket for Terraform state storage:

```plaintext
+----------------------------------------------------+
|                    AWS                             |
|  +----------------------------------------------+  |
|  |                                              |  |
|  |  +------------------+    +----------------+  |  |
|  |  |                  |    |                |  |  |
|  |  |   S3 Bucket      |    |  DynamoDB      |  |  |
|  |  | (State Storage)  |    |   (Locking)    |  |  |
|  |  |                  |    |                |  |  |
|  |  +------------------+    +----------------+  |  |
|  |        |                         |           |  |
|  |        v                         v           |  |
|  |  +----------------------------------------+  |  |
|  |  |                                        |  |  |
|  |  |   Terraform Backend Configuration      |  |  |
|  |  |                                        |  |  |
|  |  |  - Uses S3 for State Storage           |  |  |
|  |  |  - Uses DynamoDB for State Locking     |  |  |
|  |  |                                        |  |  |
|  |  +----------------------------------------+  |  |
|  |                                              |  |
|  +----------------------------------------------+  |
|                                                    |
+----------------------------------------------------+

+-----------------------------------------------+
|                GitHub Actions                 |
|  +-----------------------------------------+  |
|  |                                         |  |
|  |  +-----------------------------+        |  |
|  |  |  GitHub Actions Workflow    |        |  |
|  |  |  - Checkout Code            |        |  |
|  |  |  - Set Up SSH Key           |        |  |
|  |  |  - SSH to EC2 Instance      |        |  |
|  |  +------------+----------------+        |  |
|  |               |                         |  |
|  +---------------|-------------------------+  |
|                  |                            |
|                  v                            |
|  +-----------------------------------------+  |
|  |              AWS EC2 Instance           |  |
|  |  +-----------------------------------+  |  |
|  |  |  +-----------------------------+  |  |  |
|  |  |  |   LocalStack                 | |  |  |
|  |  |  |  - S3                        | |  |  |
|  |  |  |  - DynamoDB                  | |  |  |
|  |  |  |  - Other AWS Services        | |  |  |
|  |  |  +-----------------------------+  |  |  |
|  |  |                                   |  |  |
|  |  |  +-----------------------------+  |  |  |
|  |  |  |   Secure SSH Access (PEM)    | |  |  |
|  |  |  +-----------------------------+  |  |  |
|  |  +-----------------------------------+  |  |
|  +-----------------------------------------+  |
+-----------------------------------------------+

