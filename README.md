# nginx-2420
1. Connect to Your Arch Linux Server

Connect to your Arch Linux server via SSH using your preferred terminal emulator.# nginx-2420

```
    ssh username@your_server_ip
```
2. Install Necessary Software

Update the package repository and install Vim and Nginx.

```
    sudo pacman -Sy vim nginx
```

3. Create Project Directory

Create a new directory /web/html/nginx-2420 to serve as your project root where website documents will be stored.

```
    sudo mkdir -p /web/html/nginx-2420
```

4. Configure Nginx

Create a new server block configuration file for your project. Open the file using Vim or any text editor.

```
    sudo vim /etc/nginx/sites-available/nginx-2420
```

If the directory structure /etc/nginx/sites-available is not set up by default in your Nginx installation, you can create it manually. Here's how:

Create the Directory: Use the mkdir command to create the sites-available directory inside the /etc/nginx directory.

```
    sudo mkdir -p /etc/nginx/sites-available
```

This command creates the sites-available directory, along with any parent directories if they don't already exist.

Verify the Directory: Confirm that the directory was created successfully.

```
    ls /etc/nginx
```

Add the following configuration to the file:

```
    server {
    listen 80;
    listen [::]:80;
    server_name domainname1.tld;
    root /usr/share/nginx/domainname1.tld/html;
    location / {
        index index.php index.html index.htm;
    }
}
```
Replace domainname1.tld with your domain name.

5. Enable the Server Block

Create a symbolic link from the sites-available directory to the sites-enabled directory to enable the server block.

```
    sudo ln -s /etc/nginx/sites-available/nginx-2420 /etc/nginx/sites-enabled/
```

If the /etc/nginx/sites-enabled directory doesn't exist, you'll need to create it before creating the symbolic link. Here's how you can do it:

```
    sudo mkdir -p /etc/nginx/sites-enabled
```

This command will create the sites-enabled directory within the /etc/nginx directory. After creating the directory, you can create the symbolic link as follows:

sudo ln -s /etc/nginx/sites-available/nginx-2420 /etc/nginx/sites-enabled/

Now the symbolic link will be created successfully, allowing Nginx to include the configuration file for the nginx-2420 server block.

6. Test Nginx Configuration

Check the Nginx configuration for syntax errors.

```
    sudo nginx -t
```

If there are no errors, reload Nginx to apply the changes.

```
    sudo systemctl reload nginx
```

7. Verify Nginx Service

Ensure that Nginx is running properly.

```
    sudo systemctl status nginx
```
8. Create Demo Document

Create a simple HTML demo document and place it in the project directory.

```
    sudo vim /web/html/nginx-2420/index.html
```

Add some sample content to the HTML file, for example:

```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>2420</title>
        <style>
            * {
                color: #db4b4b;
                background: #16161e;
            }
            body {
                display: flex;
                align-items: center;
                justify-content: center;
                height: 100vh;
                margin: 0;
            }
            h1 {
                text-align: center;
                font-family: sans-serif;
            }
        </style>
    </head>
    <body>
        <h1>All your base are belong to us</h1>
    </body>
    </html>
```

9. Access the Demo Page

Open a web browser and navigate to your server's domain name or IP address. You should see the demo page served by Nginx.

You are going to face the issue like this:

Apr 02 16:50:46 archy nginx[1059117]: 2024/04/02 16:50:46 [warn] 1059117#1059117: could not build optimal types_hash, you should increase either types_hash>

--> For this issue, you have to change the configuration file from
/etc/nginx/nginx.conf. You have to replace the original file from https://wiki.archlinux.org/title/Nginx to /etc/nginx/nginx.conf in your server:

```
    user http;
worker_processes auto;
worker_cpu_affinity auto;

events {
    multi_accept on;
    worker_connections 1024;
}

http {
    charset utf-8;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    server_tokens off;
    log_not_found off;
    types_hash_max_size 4096;
    client_max_body_size 16M;

    # MIME
    include mime.types;
    default_type application/octet-stream;

    # logging
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log warn;

    # load configs
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

Conclusion

You have successfully set up a fresh Arch Linux server on DigitalOcean to serve a demo document using Nginx. This tutorial covered the installation of necessary software, configuration of Nginx with a separate server block, and utilization of systemd components for managing services.