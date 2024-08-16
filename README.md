# localstack-ec2-actions

This repository provides a complete guide and Terraform configuration for securely deploying and managing a LocalStack instance on AWS EC2. It includes step-by-step instructions to set up SSH access using a PEM file and automate interactions with LocalStack through GitHub Actions, ensuring secure and controlled access to your testing environment.

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

