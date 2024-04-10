# Assignment 3 part 2

Aiden Denike A01282232

## Adding a firewall and a Reverse Proxy Server

Welcome One and All to the BEST (potentially false) guide you'll see. In this Guide i will be showing you how to add a firewall and reverse proxy server to the one I previouslly showed you how to set up.

## Step 1 - downloading the binary backend and putting it onto your computer

First you will download the hello-server file (in this folder) onto your host computer

Next you will connect with your linux machine with sftp (ran on your host computer)

Replace the IP with your own machines IP, and the user with your own username. This is how you would connect.

```shell
sftp -i ~/.ssh/do-key aiden@64.23.155.88
```

once in sftp, you will be downloading the hello-server file onto your computer with put. Change the file path to where hello-server is located on your computer, but here is an example

```shell
put C:\Users\dell\Documents\term2\asignment3part1\assignment3p2/hello-server
```

Congrats! You have now moved the file onto your linux server.

## Step 2 - Creating a firewall

Here i will be showing you how to create a firewall with ufw. Super easy and simple

First things first we will update out package manager

```bash
sudo pacman -Syu
```

Next we will install ufw

```bash
sudo pacman -S ufw
```

Next we will enable the ufw.service for later, but not start it until we have everything necessary

```bash
sudo systemctl enable --now ufw.service
```

Next we will add to the UFW rules to allow SSH traffic. VERY IMPORTANT MUST DO BEFORE YOU TURN ON UFW!!!

you can run any of the commands below, or multiple. They all do the same thing.

```bash
sudo ufw allow ssh
sudo ufw allow SSH
sudo ufw allow 22
```

Next we will limit the amount of ssh attempts someone can make, in order to make your device more secure

```bash
sudo ufw limit ssh
```

Next we will allow http (in order for the website to still exist)

```bash
sudo ufw allow http
```

We will also allow 8080 that we will be using later

```bash
sudo ufw allow 8080
```

Great! now we have everything necessary to turn it on! Run the command to turn it on, make sure you enabled SSH

```bash
sudo ufw enable
```

In order to check that everything is working you can run this command below, and i will have an image to compare against labelled ufw_verbose.

```bash
sudo ufw status verbose
```



## Step 3 - Moving the binary backend into the correct folder and turning it into a service

Now that we have the file, we are going to move it into a reasonable folder and turn it into a service

First we will start by making a new folder to store the file

```bash
sudo mkdir /web/binary_backend
```

Next we are going to move the file we downloaded (will be in your users home directory, will need to be changed to your user)

```bash
sudo mv /home/aiden/hello-server /web/binary_backend/hello-server
```

Now we are going to make a service out of this file. First we will create the file

```bash
sudo vim /etc/systemd/system/hello-server.service
```

Next are the contents of this file

```bash
[Unit]
Description=hello_server

[Service]
ExecStart=/web/binary_backend/hello-server

[Install]
WantedBy=multi-user.target
```

Now that we have made the service file and its in the correct location, we will reload services with this command

```bash
sudo systemctl daemon-reload
```

Next we will give executable rights to the hello-server

```bash
chmod +x /web/binary_backend/hello-server
```

Then we are going to start the service

```bash
sudo systemctl start hello-server.service
```

next we are going to enable the service, so it'll always run on start up.

```bash
sudo systemctl enable hello-server.service
```

## step 4 - Adding the reverse proxy

Now we are going to add the reverse proxy to our website!

```bash
sudo vim /etc/nginx/sites-available/nginx-2420
```


And we will be changing the contents of the file into this to allow reverse proxy.

```bash
server {
    listen 80;
    listen [::]:80;
    root /web/html/nginx-2420;
    location / {
        index index.php index.html index.htm;
    }
location /hey {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

location /echo {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Now we will restart nginx with this command:

```bash
sudo systemctl restart nginx
```

Congrats it should be all running!


## step 5 - Testing the reverse proxy

I will be using postman for these tests

First i sent a get request to the website and the response should look like "Hey there". An image will be included called hey_test.png. Remember to use your ip address!

```
64.23.155.88/hey
```

Next i sent a post request to the website and the response should look like whatever you set the body of your request is. A screenshot will be included called echo_test.png. It will echo back whatever you send the contents of your request as.

```
64.23.155.88/echo
```