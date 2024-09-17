## LEMP Stack Setup on AWS EC2
This guide provides an easy-to-follow method for setting up a LEMP stack on an AWS EC2 instance. The LEMP stack includes Linux, Nginx, MySQL, and PHP, creating a robust platform for web applications.

### Introduction
The LEMP stack is popular for its performance and flexibility. Linux (Ubuntu), Nginx (web server), MySQL (database), and PHP (scripting language) work together to deliver dynamic web content efficiently.

### Prerequisites
- AWS account
- Basic knowledge of EC2 and SSH
- SSH key pair for instance access

#### Step 1: Launch an EC2 Instance
Log in to AWS: Open the AWS Management Console and go to EC2.
Launch Instance:
AMI: Select Ubuntu 22.04 LTS.
Instance Type: Choose t2.micro (free-tier eligible).
Key Pair: Select or create a key pair for SSH.
Security Group: Allow ports 22 (SSH), 80 (HTTP), and 443 (HTTPS).
Launch the instance and note its public IP address.
#### Step 2: Connect to the EC2 Instance
#### Step 3: Install Nginx
#### Step 4: Install MySQL
#### Step 5: Install PHP
#### Step 6: Configure Nginx for PHP
#### Step 7: Test PHP

Your LEMP stack is now set up and running. For further customization or advanced configurations, refer to Nginx, MySQL, and PHP documentation.

