# nginx-2420

### Aiden Denike A01282232

Welcome to my guide that will take you from a fresh Arch Linux Server running on digitalOcean to serving the demo document. Good luck and have fun!

## Step 1 - Installing all the correct software

First things first, we will need to make sure you have everything installed. We will start with updating your packet manager (sudo) and then installing vim and then installing nginx

Heres the command to type into your terminal to update your packet manager

```bash
sudo pacman -Syu
```

Then here's the command to type into your terminal to install vim

```bash
sudo pacman -S vim
```

Then here's the command to type into your terminal to install nginx

```bash
sudo pacman -S nginx
```

## Step 2 - Making the directories and server block directories

First we will make the directory for all the file contents to be stored, and a seperate server block for our config file. 

Here's the command to run in your terminal that will create the directory that will act as your project root

```bash
sudo mkdir -p /web/html/nginx-2420
```

Next we will need to make the directories for sites-available and sites-enabled, in order for us to have fully seperate servers down to their config file

Run this command to make the sites-available directory

```bash
sudo mkdir -p /etc/nginx/sites-available
```

Run this command to make the sites-enabled directory

```bash
sudo mkdir -p /etc/nginx/sites-enabled
```

## Step 3 - Changing the conf files to allow the server block to work

Here we will be making all the conf files, and making them all work together

First we will edit the nginx.conf file to run everything in your sites-enabled directory when it runs. This will allow our seperate conf files to run!

Here's the command to go to the correct file

```bash
sudo vim /etc/nginx/nginx.conf
```

Then we will change the entire contents of this file to this

```bash
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

This script will be a great conf file to start from and will look for conf files in other folders, and specifically the last line 

```bash
include /etc/nginx/sites-enabled/*;
```

Will allow this to search for all files in the directory sites-enabled and use them as a config file.

Next we will make our servers conf file in our sites-available directory with this command

```bash
sudo vim /etc/nginx/sites-available/nginx-2420
```

And we will then put these contents in the file (change the ip address to yours)

```bash
server {
    listen 80;
    listen [::]:80;
    root /web/html/nginx-2420;
    location / {
        index index.php index.html index.htm;
    }
}
```

## Step 4 - Creating system links

The main way we can turn on and off websites will be enabling or disabling the links associated with it. Now that we have all the correct files made, we will make a symbolic link between our conf file in sites-available and sites-enabled

Here's the command to make the symbolic link between our files, ran in the terminal

```bash
sudo ln -s /etc/nginx/sites-available/nginx-2420 /etc/nginx/sites-enabled/nginx-2420
```

Here's the command to check if the syntax is right for everything, and more error checking. If you followed everything above correctly this should give no errors.

```bash
sudo nginx -t
```

Here's the command to start your nginx server. Run this now to turn it on.

```bash
sudo systemctl start nginx
```

Here's the command to have your nginx server always work from boot. Run this now to always have your server on, even if you were to restart your droplet.

```bash
sudo systemctl enable nginx
```

Here's the command to restart it when you need it. This can be used if you make other changes to your nginx server later

```bash
sudo systemctl restart nginx
```

## Step 5 - Making the html

Here we will be adding the html index so our page has something to load. You can have anything you want but I will give you a file you can paste in to verify everything is working to compare against my screenshot.

start by moving to the file we are making

```bash
sudo vim /web/html/nginx-2420/index.html
```

Then you can put these file contents in:

```html
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

Now try going to the site

it'll be 
the-ip-address-of-your-droplet
as the url for the website. I will have an image you can compare against in the git repository.


if you ever want to disable this website, you can simply unlink the file in the terminal with. Make sure you restart your nginx if you make any changes

```bash
sudo unlink /etc/nginx/sites-enabled/nginx-2420
```

Thank you for coming to my guide! Hopefully you got everything working!