# Linux Assignment 3

## Create a New Regular User

### User has bash as login shell

1. Log in to your server as the root user. Specify the path to your key and change the ip address to your server ip address.
```
    ssh -i C:\Users\rjpau\.ssh\do-key root@146.190.159.125
```

2. Create a new user with a home directory and has bash as login shell. Replace ```<user-name>``` with any username you want.
```
    useradd -ms /bin/bash <user-name>
```

3. Give the new user a password with ```passwd```.
```
    passwd <user-name>
```

### User can perform administrative tasks

1. Add the new user to the sudo group.
```
    usermod -aG sudo <user-name>
```

### User can access the server via SSH

1. Copy the .ssh directory to the new user's home directory.
```
    cp -r /root/.ssh /home/<user-name>
```

2. Change ownership of the .ssh directory and its contents.
```
    chown -R <user-name>:<user-name> /home/<user-name>/.ssh
```

3. Exit from the root session.
```
    exit
```

4. Log in as your new user. Remember to replace the ip address with your own server ip.
```
    ssh -i C:\Users\rjpau\.ssh\do-key <user-name>@146.190.159.125
```

5. Set proper permissions on the .ssh directory and its contents.
```
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/*
```

## Prevent the root user from connecting to the server via SSH

1. Open the SSH configuration file.
```
    sudo vim /etc/ssh/sshd_config
```

2. Find the line ```PermitRootLogin yes``` and change it to ```PermitRootLogin no```. Save and exit the editor using the code ```:wq```.
   
![PermitRootLogin](/images/permitrootlogin.png)

3. Restart the SSH service to apply the changes.
```
    sudo systemctl restart ssh
```

## Install Nginx 

1. Update the package index.
```
    sudo apt update
```

2. Install nginx.
```
    sudo apt install nginx
```

3. Start the nginx service.
```
    sudo systemctl start nginx
```

4. Enable nginx to start on boot.
```
    sudo systemctl enable nginx
```

5. Check nginx status.
```
    sudo systemctl status nginx
```

![nginx status](/images/nginxstatus.png)

## Configure Nginx to Serve a Sample Website

1. Create a sample HTML file for your website content.
```
    sudo vim /var/www/html/index.html
```

2. Copy this sample website or create your own.
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

3. Create an nginx server block configuration.
```
    sudo vim /etc/nginx/sites-available/sample_website
```

4. Copy this in the configuration.
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

5. Create a symbolic link to enable the server block configuration.
```
    sudo ln -s /etc/nginx/sites-available/sample_website /etc/nginx/sites-enabled/
```

6. Unlink the default to avoid errors.
```
    sudo unlink /etc/nginx/sites-enabled/default
```

7. Test nginx configuration. Ensure there are no errors in the configuration.
```
    sudo nginx -t
```

![Test Nginx Configuration](/images/testnginxconfig.png)

8. Reload nginx to apply changes.
```
    sudo systemctl reload nginx
```

9. Access your website on your web browser. Use your own server domain or ip address.
```
    http://146.190.159.125
```


Congratulations! You've successfully set up a Debian 12 server on DigitalOcean, created a new user with administrative privileges that has bash as login shell, configured SSH access, prevented root user SSH access, installed Nginx, and configured it to serve a sample website.
