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

