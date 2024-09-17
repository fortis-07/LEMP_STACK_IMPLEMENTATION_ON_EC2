# LEMP_STACK: Introduction

The LEMP stack is a widely adopted open-source web development framework, known for its efficiency in deploying and managing dynamic web applications. LEMP stands for **Linux**, **Nginx**, **MySQL**, and **PHP** (though sometimes **Perl** or **Python** are used instead of PHP). Each component plays a crucial role in ensuring the stack's stability and performance. This technical documentation provides a detailed guide on how to install, configure, and manage the LEMP stack for optimal performance.

- **Linux**: The foundation of the stack, Linux is the operating system responsible for managing hardware resources and software execution. In this guide, we utilize **Ubuntu**, a Debian-based Linux distribution, to streamline package installations, network configurations, and system-level processes through the use of tools like `apt`.

- **Nginx**: Nginx serves as a high-performance, asynchronous web server and reverse proxy. It efficiently handles HTTP requests, serving static files like HTML, CSS, and JavaScript. Additionally, Nginx forwards dynamic content requests (e.g., PHP-based applications) to the appropriate application server, such as **PHP-FPM**. Its ability to manage large volumes of concurrent connections makes it ideal for scaling web applications.

- **MySQL**: This component is a **Relational Database Management System (RDBMS)** used to manage and organize data in structured tables. MySQL efficiently handles CRUD operations (Create, Read, Update, Delete) for dynamic applications and supports SQL queries to interact with the data. Additionally, it can be tuned for security, availability, and performance, utilizing replication, indexing, and caching mechanisms.

- **PHP**: A server-side scripting language responsible for generating dynamic content. PHP interacts with MySQL to retrieve and manipulate data, which is then served to users through the Nginx web server. Modern PHP deployments often utilize **PHP-FPM (FastCGI Process Manager)** for efficient request handling, better resource management, and faster execution, especially under heavy load.

By combining these components, the LEMP stack offers a robust, scalable, and flexible architecture for developing web applications, facilitating seamless interaction between the server, application code, and database.

## Prerequisites

- **AWS Account**: A registered AWS account with permissions to create and manage EC2 instances and associated resources.
- **EC2 and SSH Knowledge**: Familiarity with Amazon EC2, security group configurations, and basic SSH commands for secure server access.
- **SSH Key Pair**: An existing SSH key pair for authenticating and connecting to your EC2 instance, with the private key stored locally for secure access.

## Step 1: **Provisioning a new EC2 instance and Connection**:
 - Log in to AWS: Go to the AWS Management Console.
- Navigate to EC2: From the services menu, select EC2 (t2-micro, Ubuntu 22.04 LTS).
- Key Pair: Select or create a key pair for SSH access.
Security Group: Open the following ports:
SSH (22)
HTTP (80)

- Launch a new instance:

![WhatsApp Image 2024-09-16 at 16 08 02_929650c2](https://github.com/user-attachments/assets/d0ce4a07-8a67-4219-ad3a-90fd9b3e7c5f)

Once the instance is running, connect to it via SSH.

```bash
ssh -i /path/to/your-key.pem ubuntu@<your-ec2-public-ip>
```

![WhatsApp Image 2024-09-17 at 11 01 23_6ae2ebff](https://github.com/user-attachments/assets/36960b9a-cd4b-4bd7-a54c-2337eba2a022)

## Step 2: Nginx Web Server Installation
In order to display web pages to site visitoers,we would install Nginx ,Ahigh-performance web server using the apt package manager to install it.

```bash
sudo apt update
sudo apt install nginx
```
To verify the installation and confirm that Nginx is running on Ubuntu server use the command:

```bash
sudo systemctl status nginx
```

![WhatsApp Image 2024-09-16 at 23 02 55_172f33f6](https://github.com/user-attachments/assets/03ad162a-e105-4b6f-b8bb-4e4685559e88)

Proceed to access the server locally from the internet with the addresss:

```bash
http://44.200.28.108:8O
```
![WhatsApp Image 2024-09-16 at 23 04 52_9b38cd2a](https://github.com/user-attachments/assets/337cdfc8-7ed5-4219-be0f-aacbcc06c737)

## Step 3: MySQL installation
With the Web Server running,we install a Database Management System to store anad manage data  in a relational database and for this MySQL is a popular choice within PHP environment.

```bash
sudo apt install mysql-server
```
After installation is finished,login to the Database by typing

```bash
sudo mysql
```

![WhatsApp Image 2024-09-16 at 23 10 54_7ba7f27d](https://github.com/user-attachments/assets/6ff9e6bf-3efc-426e-bead-bf394b34f85c)

Exit the MyQlshelll by running the command:

```bash
exit
```
## Step 4: Installation and Configuring PHP-FPM with Nginx for PHP
Unlike Apache, which has a built-in PHP interpreter for each request, Nginx relies on an external program to handle PHP processing. This program, **PHP-FPM** (PHP FastCGI Process Manager), acts as a bridge between the web server and the PHP interpreter, enabling better performance for PHP-based websites. However, this requires additional configuration. We will need to install **php-fpm** and configure Nginx to forward PHP requests to it. Additionally, we'll install the **php-mysql** module, which allows PHP to interact with MySQL databases. Core PHP packages will be automatically installed as dependencies.
To install these 2 packages at once, run:

```bash
sudo apt install php-fpm php-mysql
```
![WhatsApp Image 2024-09-16 at 23 13 06_4b2d662b](https://github.com/user-attachments/assets/28c87ca1-d844-4097-a718-b9127eb515e6)

## Step 5: Configuring Nginx to Use PHP Processor
### Setting Up Nginx Server Blocks for Multiple Domains

When using Nginx, we can create **server blocks** (similar to Apache’s virtual hosts) to manage configuration settings for multiple domains on a single server. In this example, we'll use `projectLEMP` as the domain name.

By default, on **Ubuntu 24.04**, Nginx is configured to serve content from `/var/www/html`, which works for a single site. However, when hosting multiple domains, managing everything in this single directory becomes cumbersome. Instead of modifying the default location, we’ll create a new directory structure for each domain within `/var/www`. This keeps `/var/www/html` as the fallback for requests that don’t match any specific site.

For instance, we'll set up a new directory structure at `/var/www/your_domain` to serve the `projectLEMP` site, making it easier to manage different websites independently while keeping the default configuration intact.

- Create a root web directory for your_domain

```bash
sudo mkdir /var/www/projectLEMP
```
Assigning ownership so that default user able to execute projectLEMP directory:

```bash
sudo chown -R $USER:$USER /var/www/projectLEMP
```
Open a new configuration file in Nginx’s “sites-available” directory.

```bash
sudo nano /etc/nginx/sites-available/projectLEMP
```
After creating a new blank file.Paste in the following bare-bones configuration

```bash

#/etc/nginx/sites-available/projectLEMP

server {
  listen 80;
  server_name projectLEMP www.projectLEMP;
  root /var/www/projectLEMP;

  index index.html index.htm index.php;

  location / {
    try_files $uri $uri/ =404;
  }

  location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
  }

  location ~ /\.ht {
    deny all;
  }
}
```
#### Explanation of blocks and directives within the bare-metal configuration

- **server { ... }**: This defines a **server block**, which is a set of configuration rules for handling specific domain requests.

- **listen 80;**: Nginx listens for incoming HTTP requests on port 80, the default port for non-secure web traffic.

- **server_name projectLEMP www.projectLEMP;**: This specifies the domain names that this server block will respond to. Requests for both `projectLEMP` and `www.projectLEMP` are handled here.

- **root /var/www/projectLEMP;**: This defines the root directory for the site, where Nginx will look for files to serve, like HTML or PHP files.

- **index index.html index.htm index.php;**: This tells Nginx which file to serve as the default when a user visits the root of the website (e.g., `index.html`, `index.php`).

- **location / { ... }**: This block handles all requests to the root directory. The `try_files` directive checks if the requested file exists; if not, it tries the directory, and if neither exists, it returns a 404 error.

- **location ~ \.php$ { ... }**: This block handles requests for PHP files. It includes the FastCGI configuration (`snippets/fastcgi-php.conf`) and sends the PHP requests to the **PHP-FPM** process running at the socket `php8.1-fpm.sock`.

- **location ~ /\.ht { ... }**: This block denies access to any `.ht` files, typically used for Apache configurations, for security purposes.

  *This configuration ensures that Nginx handles static files, PHP processing, and secures certain sensitive files.*

- **Activate the configuration by linking to the config file from Nginx sites-enabled directory:**

```bash
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
*Test the configuration for syntax errors by typing:

```bash
sudo nginx -t
```
You should get a message like this:

![WhatsApp Image 2024-09-16 at 16 51 43_baa410d3](https://github.com/user-attachments/assets/0b9f8434-2784-4c77-b1fb-43bece269cb7)

Disable the default Nginx host that currently configured to listen on port 80

```bash
sudo unlink /etc/nginx/sites-enabled/default
```

Reload Nginx to apply the changes

```bash
sudo systemctl reload nginx
```
The new website is now active but the web root /var/www/projectLEMP is still empty. Create an index.html file in this location so to test the virtual host work as expected.

```bash
sudo echo "Hello LEMP from hostname"
``

Go to your browser and try to open th websitre URL using the IP address or the curl command from your Terminal:


```bash
http://44.200.28.108:8O
```

![WhatsApp Image 2024-09-17 at 00 08 08_2eedf1ba](https://github.com/user-attachments/assets/2ab1cb88-7a47-4a86-8429-0603907a56f3)

![WhatsApp Image 2024-09-17 at 00 09 10_d7dc9ef7](https://github.com/user-attachments/assets/72b4e443-a40e-4df9-8e8a-1456f29dab7a)

This file can serve as a temporary landing page for the application until you set up the `index.php` file. Once the `index.php` file is ready, remove or rename the `index.html` file in the document root, as `index.html` will be loaded first by default.

The LEMP stack is now fully set up. At this point, everything is installed and the stack is ready for use.

Open you browser and access it to confirm the set-up and be certain the index.html is up and working.

## Step 6:
Test the LEMP stack to validate that Nginx can handle the .php files off to the PHP processor.

- Create a test PHP file in the document root. Open a new file called info.php within the document root.

```bash
sudo nano /var/www/projectLEMP/info.php
```

```bash
<?php
phpinfo();
```
Go to your browser and try to open the website URL using the IP address:

```bash
http://44.200.28.108/info.php
```
You will see a web page containing detailed information of your server:

![WhatsApp Image 2024-09-17 at 00 27 29_1e6747df](https://github.com/user-attachments/assets/70ed921e-804e-4e83-989d-ecefb3d47be4)

Scrolling down with give us more information:

![WhatsApp Image 2024-09-17 at 00 27 56_84afa846](https://github.com/user-attachments/assets/5728b4d0-9098-434a-9698-f388dbe51823)

After confirming and checking the information about the PHP Server it is best to remove the file you have created as it contains sensitive information about the PHP environment.You can use `rm` command to remove the file:

```bash
sudo rm /var/www/projectLEMP/info.php
```

## Step 7: Retrieving data from MySQL databsae with PHP

We will create a test database with a simple "To-Do list" and configure it so that the Nginx website can query data from the database and display it.

Currently, the MySQL PHP library (mysqlnd) does not support **caching_sha2_authentication**, which is the default method in MySQL 8. To connect PHP to the MySQL database, we need to create a new user using the **mysql_native_password** authentication method.

- **First, connect to the MySQL console using the root account.**

```bash
sudo mysql -p
```
- ** Create a database,run the following command in the MySQL Console:
- 
```bash
CREATE DATABASE version1_db;
```

Create a new user and grant full priviledges on the database you have created

```bash
CREATE USER 'version1_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
Give this user permission over the `version1_db`

```bash
GRANT ALL ON version1_db.* TO 'version1_user'@'%';
```
![WhatsApp Image 2024-09-17 at 00 43 10_8f3a694e](https://github.com/user-attachments/assets/3d722510-b852-40ad-a0fb-e0416fe63862)

Now exit the MySQL shell:

```bash
exit
```
- **Verify the new user has proper permissions by logging into the MySQL console again using the custom user credentials.

```bash
mysql -u version1_user -p
```

- **In the MySQL console,onfirm that you have access to the version1_db you have created:**

```bash
SHOW DATABASE;
```

You will get the following output:

![WhatsApp Image 2024-09-17 at 00 48 21_0a4d6655](https://github.com/user-attachments/assets/2d524a4d-7aef-4627-9f74-fefb6c519e7d)

Now we will create a 'todo_list' from the MySQL console:

``bash
CREATE TABLE todo_database.todo_list (
  item_id INT AUTO_INCREMENT,
  content VARCHAR(255),
  PRIMARY KEY(item_id)
);
```

- **Insert a few rows of content to the test table.**

```bash
INSERT INTO version1_db.todo_list (content) VALUES ("My first important item");

INSERT INTO version1_db.todo_list (content) VALUES ("My second important item");

INSERT INTO version1_db.todo_list (content) VALUES ("My third important item");

INSERT INTO version1_db.todo_list (content) VALUES ("and this one more thing");
```

To confirm that the data was successfully saved to your table run:

```bash
SELECT * FROM version1_db.todo_list;
```
This should be the output:

![WhatsApp Image 2024-09-17 at 01 07 37_e207d681](https://github.com/user-attachments/assets/305fb25f-6da2-4350-9d31-f2ccb4a33725)

Now we can create a PHP script that will connect to MySQL and query for content

```bash
nano /var/www/projectLEMP/todo_list.php
```


The script connects to the MySQL database and queries for the content of the todo_list table

![WhatsApp Image 2024-09-17 at 01 16 33_e4ffad2d](https://github.com/user-attachments/assets/ac6052e7-f57c-4651-9ce9-bd5b5c37b9af)

Save the file and acess this page in your web browser by visiying the public IP addresss configured for your website,followed by todo_list.php:

```bash
http://44.200.28.108/todo_list.php
```
This should be the output showing the content you have in the test table:

![WhatsApp Image 2024-09-17 at 01 18 30_063df431](https://github.com/user-attachments/assets/ecff68aa-eccc-4b0a-be95-95f1e90c8ccb)

## This means your PHP environment is ready to connect and interact with your MySQL Server
