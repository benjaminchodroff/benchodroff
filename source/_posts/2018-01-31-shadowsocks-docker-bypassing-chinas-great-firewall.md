---
title: Shadowsocks in Docker for bypassing China's Great Firewall
tags: []
id: '360'
categories:
  - - uncategorized
alias: /2018/01/31/shadowsocks-docker-bypassing-chinas-great-firewall/
date: 2018-01-31 16:49:08
---

If you have travelled in China, you likely know that many websites such as Google, Facebook, and Twitter are blocked on most internet connections. Typically I have purchased a VPN service to access blocked sites. Unfortunately, many VPN services are being detected and blocked by China's "Great Firewall.” The best solution I have found is either to purchase a premium VPN solution, such as ExpressVPN which is specifically designed to work in China, or to use an open source solution called Shadowsocks hosted on a remote server. This tutorial provides a detailed setup of how to run Shadowsocks in a docker container deployed on a Synology NAS.
<!-- more -->
You could rent a cloud server from Digital Ocean or AWS, but because I already own a Synology NAS at home I decided to use it instead. If you own a Synology NAS, you may not be aware that it can run docker containers. Docker will allow you to tap into a repository community of images that have already been prebuilt for a wide variety of applications including Shadowsocks. I already loved my Synology NAS, but once I started using docker it became indispensable.

## **Synology NAS Configuration**

If you have not installed Docker on your Synology NAS before you simply need to go into the Synology Package Center on the device, search for "Docker", and click to install it.

\[caption id="attachment\_364" align="aligncenter" width="640"\]![Synology Docker Install](https://benchodroff.com/wp-content/uploads/2018/01/docker_install-1024x204.png) Synology Docker Install\[/caption\]

Now you can open the Docker service on the Synology NAS and go to “Registry” to search for “shadowsocks.” The first result will be “mritd/shadowsocks." Select this image, click Download, and when prompted download the latest.

\[caption id="attachment\_365" align="aligncenter" width="640"\]![Docker Registry Shadowsocks Search](https://benchodroff.com/wp-content/uploads/2018/01/docker_registrysearch-1024x423.png) Docker Registry Shadowsocks Search\[/caption\]

Once downloaded, you can deploy this image by going into the "Image" section, selecting the shadowsocks image, and clicking "Launch." Set the following options in "Advanced Properties":

*   Enable auto-restart
*   Under "port settings", create a port mapping the local port 6500 to the docker port 6500 for UDP traffic
*   Under "port settings", create a port mapping the local port 6443 to the docker port 6443 for TCP traffic
*   Under "environment", create or modify the following environment variables:
    *   SS\_MODULE: ss-server
    *   KCP\_FLAG: true
    *   KCP\_CONFIG: -t 127.0.0.1:6443 -l :6500
    *   KCP\_MODULE: kcpserver
    *   SS\_CONFIG: -s 0.0.0.0 -p 6443 -m aes-256-cfb -k test123

 

\[caption id="attachment\_366" align="aligncenter" width="640"\]![Shadowsocks Docker Ports](https://benchodroff.com/wp-content/uploads/2018/01/shadowsocks_ports_docker-1024x289.png) Shadowsocks Docker Ports\[/caption\]

\[caption id="attachment\_367" align="aligncenter" width="640"\]![Shadowsocks Docker Environment](https://benchodroff.com/wp-content/uploads/2018/01/shadowsocks_environment_docker-1024x524.png) Shadowsocks Docker Environment\[/caption\]

These properties specify that we want to run shadowsocks in server mode "ss-server". We are enabling the optional KCP tuning for UDP relay support running on TCP port 6443 and UDP port 6500 using the kcpserver module. We are starting shadowsocks to listen on all available ip addresses 0.0.0.0 for 6443/TCP with aes-256-cfb encryption and a password of "test123" (pick a better password). It is important to know that a single shadowsocks tcp/udp port combination can support as many simultaneous devices as you wish, but everyone who knows the password can connect to your home internet. Be careful who you share your password with!

Save, and start the image.

You will need to ensure your home router allows port forwarding for both the 6500/UDP and 6443/TCP port to the local IP address of your Synology NAS. If you have enabled a firewall on your Synology NAS under the Control Panel Security, you may also need to open these ports on the Synology too. Additionally, you will likely want to set up a dynamic DNS hostname service either on your home router or, better yet, use the built in dynamic DNS hostname mapping feature of the Synology NAS found in Control Panel, "External Access - DDNS".

Now you need to configure your browser clients. I will provide examples for iOS and macOS, but clients for Linux and Windows also exist.

## **iOS Configuration**

I use the "Potatso Lite" application for my iPhone iOS which you can find for free in the App Store. Once installed, add a server with the following information:

*   Type: shadowsocks
*   Host: \[the dynamic DNS hostname of your NAS\]
*   Port: 6443
*   Password: test123 (or the password you set)
*   Encryption: aes-256-cfb
*   Remark: (leave blank)

In advanced, enable “Forward UDP” - it turbocharges the performance using kcpserver. In the general settings, I recommend using the "smart routing" feature because it significantly improves performance (although at a tradeoff of allowing China to see local country traffic).

## **macOS Configuration**

For my Mac laptop I use shadowsocksR which you can download for free here: [https://github.com/qinyuhang/ShadowsocksX-NG-R/releases](https://github.com/qinyuhang/ShadowsocksX-NG-R/releases)

This is a special China version of ShadowsocksR which is enhanced specifically for the great firewall. Once installed and running, it will automatically set up your Mac to have a proxy server which you can populate with your Synology NAS shadowsocks server. Add a server by navigating to "Server Preferences" and add the following details:

*   Address: \[the dynamic DNS hostname of your NAS\]
*   Port: 6443
*   Password: test123 (or the password you set)
*   Encryption: aes-256-cfb
*   Remark: (leave blank)
*   Leave the rest of the settings unchanged

Then add the following properties in the ShadowsocksR menu:

*   "Auto Mode by PAC"
*   In "Advanced Preferences", check the "Enable the Udp relay" feature for enhanced performance using kcpserver.

How does this all work? Your browser is now configured to connect to a local proxy server (your shadowsocks client) which then connects to the shadowsocks server running in the docker container, which then relays the traffic to the destination. The shadowsocks server then sends the response back to your shadowsocks client using UDP and TCP, which your browser then receives via a standard proxy connection.

The solution is quite magical. The performance is vastly superior all the commercial VPN solutions I used. I can stream YouTube, post large photos to Instagram, and do just about anything as if I was at home.