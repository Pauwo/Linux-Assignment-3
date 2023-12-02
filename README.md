# Linux Assignment 3

## Create a New Regular User
1. Log in to your server as the root user. Specify the path to your key and change the ip address to your server ip address.
```
    ssh -i C:\Users\rjpau\.ssh\do-key root@146.190.159.125
```
2. Create a new user with a home directory and has bash as login shell. Assume that I want to make a new user named "Pau", the command would be
```
    useradd -ms /bin/bash Pau
```
Note: You can replace "Pau" with any other username that you want.


3. Give the user a password with ```passwd```
```
    passwd Pau
```
4. Add the new user to the sudo group so that the user can perform administrative tasks.
```
    usermod -aG sudo Pau
```
Note: ```-a``` appends the user to the specified group without removing the user from any other groups they might already belong to. ```-G``` is used to specify the supplementary groups of which the user is a member. In this command, it adds the user to the 'sudo' group.

5. User can access the server via SSH

Copy the .ssh directory to the new user's home directory
```
    cp -r /root/.ssh /home/your_username
```

Change ownership of the .ssh directory and its contents
```
    chown -R your_username:your_username /home/your_username/.ssh
```

Exit from the root session
```
    logout
```

Log in as your new user
```
    ssh -i C:\Users\rjpau\.ssh\do-key Pau@146.190.159.125
```

Set proper permissions on the .ssh directory and its contents
```
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/*
```


## Prevent the root user from connecting to the server via SSH
Open the SSH daemon configuration file
```
    sudo vim /etc/ssh/sshd_config
```
Find the line **PermitRootLogin yes** and change it to **PermitRootLogin no**. Save and exit the editor using the code ```:wq```.

Restart the SSH service to apply the changes
```
    sudo systemctl restart ssh
```

## Install Nginx 
Update the package index
```
    sudo apt update
```

Install Nginx
```
    sudo apt install nginx
```

Start the Nginx service
```
    sudo systemctl start nginx
```

Enable Nginx to start on boot
```
    sudo systemctl enable nginx
```

Check nginx status
```
    sudo systemctl status nginx
```



## Configure nginx to Serve a Sample Website
Create a sample HTML file for your website content
```
    sudo vim /var/www/html/index.html
```

copy this sample website for now
```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>2420</title>
        <style>
            body {
                display: flex;
                align-items: center;
                justify-content: center;
                height: 100vh;
                margin: 0;
            }
            h1 {
                text-align: center;
            }
        </style>
    </head>
    <body>
        <h1>Hello, World</h1>
    </body>
    </html>
```

Create an Nginx Server Block Configuration
```
    sudo vim /etc/nginx/sites-available/sample_website
```

Copy this in it
```
    server {
    	listen 80 default_server;
    	listen [::]:80 default_server;
    	
    	root /var/www/html;
    	
    	index index.html index.htm index.nginx-debian.html;
    	
    	server_name _;
    	
    	location / {
    		# First attempt to serve request as file, then
    		# as directory, then fall back to displaying a 404.
    		try_files $uri $uri/ =404;
    	}
    }
```

Create a Symbolic Link to Enable the Server Block
```
    sudo ln -s /etc/nginx/sites-available/sample_website /etc/nginx/sites-enabled/
```

unlink the default
```
    sudo unlink /etc/nginx/sites-enabled/default
```

Test Nginx Configuration
```
    sudo nginx -t
```
Ensure there are no errors in the configuration

Reload Nginx to Apply Changes
```
    sudo systemctl reload nginx
```

Access your website on your web browser using
```
    http://146.190.159.125
```
use your own server domain or ip



Now, you should be able to access your server's sample website by navigating to your domain or IP address in a web browser.

Congratulations! You've successfully set up a Debian 12 server on DigitalOcean, created a new user with administrative privileges, configured SSH access, prevented root user SSH access, installed Nginx, and configured it to serve a sample website.
