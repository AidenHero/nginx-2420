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

## Step 2 - WIP