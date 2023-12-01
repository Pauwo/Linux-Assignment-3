﻿# Linux Assignment 3

## 1 Create a New Regular User
Log in to your server as the root user
  ssh root@your_server_ip

Create a new user with administrative privileges // User has bash as login shell
  useradd -ms /bin/bash <user-name>

Add the new user to the sudo group // User can perform administrative tasks
  usermod -aG sudo <user-name>

Exit from the root session
  exit


## 2 Configure SSH Access for the New User
Log in to the server with the new user // User can access the server via SSH
  ssh your_username@your_server_ip


## 3 Prevent the root user from connecting to the server via SSH
Open the SSH daemon configuration file
  sudo vim /etc/ssh/sshd_config
  
Find the line **PermitRootLogin yes** and change it to **PermitRootLogin no**. Save and exit the editor using the code ':wq'.

Restart the SSH service to apply the changes
  sudo systemctl restart ssh


## 4 Install Nginx 
Update the package index
  sudo apt update

Install Nginx
  sudo apt install nginx


## 5 Configure nginx to Serve a Sample Website
Create a sample HTML file
  echo "<html><head><title>Sample Website</title></head><body><h1>Hello, this is a sample website!</h1></body></html>" | sudo tee /var/www/html/index.html

Create a new server block configuration file for Nginx
  sudo vim /etc/nginx/sites-available/sample_website

Add the following configuration (replace server_name with your domain or IP address):
  server {
      listen 80;
      server_name your_domain_or_ip;
  
      location / {
          root /var/www/html;
          index index.html;
      }
  }

Save and exit the editor.

Create a symbolic link to enable the server block
  sudo ln -s /etc/nginx/sites-available/sample_website /etc/nginx/sites-enabled/

Test Nginx configuration
  sudo nginx -t

Reload Nginx to apply the changes
  sudo systemctl reload nginx


Now, you should be able to access your server's sample website by navigating to your domain or IP address in a web browser.

Congratulations! You've successfully set up a Debian 12 server on DigitalOcean, created a new user with administrative privileges, configured SSH access, prevented root user SSH access, installed Nginx, and configured it to serve a sample website.
