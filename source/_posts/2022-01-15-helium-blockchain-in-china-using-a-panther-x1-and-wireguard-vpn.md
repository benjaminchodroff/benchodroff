---
title: Helium blockchain in China using a Panther X1 and Wireguard VPN
tags: []
id: '1962'
categories:
  - - uncategorized
alias: /2022/01/15/helium-blockchain-in-china-using-a-panther-x1-and-wireguard-vpn/
date: 2022-01-15 14:33:04
---

Helium is a blockchain which creates a decentralized [LoRaWAN](https://lora-alliance.org/) global wireless network for IoT devices that rewards users based on Proof of Coverage. I came across the idea years before, but was concerned that they only allowed a single device to provide the LoRaWAN coverage! Helium has now opened up to support many approved device manufactures. I decided to purchase a Panther X1 unit to try it out. While using a blockchain in China for personal purposes (not as a company) is not illegal, China has heavily censored many blockchain related services.
<!-- more -->
The Helium relies on libp2p. China's Great Firewall (GFW) has very effective mechanisms that recognize libp2p domestically and inject connection resets during the SSL handshake. While you could put a commercial VPN in front of your Helium device, most of these solutions won't allow opening a port. Helium is supposed to work around this problem by providing a relay service, but this relay mechanism is also being disrupted inside China. This will cause your node to remain fully synchronized but never participate in Proof of Coverage (witness/beacons). If you want to decentralize in China, you need to build your own bridge out of China to participate. I implemented a bridge to a Virtual Private Server (VPS) in the cloud outside of China by using a self hosted Wireguard VPN and client. Each Helium device would require its own VPS (or more specifically, its own unique external to China IP address with port 44158 opened, because relays are broken). Here are the steps to take to reproduce my setup.

The Panther X1 is internally a Raspberry Pi 4B with a LoRaWAN adapter. The Raspberry Pi is locked down but allows SSH connections. In order to install a VPN client on this device and troubleshoot it effectively, we will need to break into the device. Luckily, the Panther X1 isn't that secure (note: the Panther X2 implements additional security).

Open the device up, remove the four rubber pads from the device, remove the four screws in the case, remove all the screws holding down the LoRaWAN adapter, and gently pull up on the adapter towards the antennas. You will find it separates from the pins on the Raspberry Pi board.

![](http://www.benchodroff.com/wp-content/uploads/2022/01/IMG_8254-1.jpg)

Once the LoRaWAN adapter is disconnected (you may keep the antennas connected, just move the board aside), remove all the screws for the Raspberry Pi. On the underside you will find a micro SD Card. Remove this card, and install it into a USB micro SD card reader on a Linux computer (on older MacOS you can use exFuse, and there may be a similar program for Windows - let me know in the comments). Find the device such as "/dev/sdb2". The letter may be different on your computer, but it is the second partition which is formatted ext4 that we want. 

```
# Mount the device to a mountpoint that is availablesudo mount /dev/sdb2 /mnt
```

Once we have modified this file, we can now reinstall the micro SD card back into the Raspberry Pi. Now is a great time to install heatsinks on the Raspberry Pi CPU, RAM, and bridge -- as the device gets to 80C otherwise. Finally, reattach the LoRaWAN adapter - pay attention to ensure the pins line up correctly. Reinstall the screws for the case and rubber feet. You can boot your Panther X1 back up and find that your SSH root account now accepts logins using your private key.

To install wireguard server and client, I [followed this guide](https://www.cyberciti.biz/faq/ubuntu-20-04-set-up-wireguard-vpn-server/) with some modifications below:

## Wireguard VPN Server on VPS

```
sudo apt updatesudo apt upgraderebootsudo apt install wireguard vim fail2bansudo -icd /etc/wireguard/umask 077; wg genkey  tee privatekey  wg pubkey > publickeycat privatekeycat publickeysudo vim /etc/wireguard/wg0.conf# Replace PrivateKey with the server private key# We will generate the Peer client PublicKey later... leave the section commented out for now
```

\[Interface\]   
\## My VPN server private IP address (Don't change this unless required)   
Address \= 192.168.12.1/24    
   
\## My VPN server port ##   
ListenPort \= 41194     
  
\## VPN server's private key i.e. /etc/wireguard/privatekey   
PrivateKey = REPLACE ME  
  
PostUp = /etc/wireguard/helper/add-nat-routing.sh  
PostDown = /etc/wireguard/helper/remove-nat-routing.sh  
  
\# (come back and uncomment this once you set up the client!)  
#\[Peer\]  
\## Client public key   
\# PublicKey = REPLACE ME  
#  
\## client VPN IP address (note the /32 subnet) - (Don't change this unless required) ##  
#AllowedIPs = 192.168.12.2/32  

`**sudo mkdir /etc/wireguard/helper**  
**sudo vim /etc/wireguard/helper/add-nat-routing.sh**`

#!/bin/bash  
\# https://www.cyberciti.biz/faq/ubuntu-20-04-set-up-wireguard-vpn-server/  
IPT="/sbin/iptables"  
#IPT6="/sbin/ip6tables"   
IN\_FACE="eth0" \# NIC connected to the internet  
WG\_FACE="wg0" \# WG NIC  
SUB\_NET="192.168.12.0/24" \# WG IPv4 sub/net aka CIDR  
WG\_PORT="41194" \# WG udp port  
HELIUM\_PORT="44158" \# Helium tcp port  
HELIUM\_IP="192.168.12.2" \# Helium VPN client IP address  
#SUB\_NET\_6="fd42:42:42:42::/112" # WG IPv6 sub/net  
  
\## IPv4 ##  
$IPT \-t nat -I POSTROUTING 1 \-s $SUB\_NET \-o $IN\_FACE \-j MASQUERADE  
$IPT \-I INPUT 1 \-i $WG\_FACE \-j ACCEPT  
$IPT \-I FORWARD 1 \-i $IN\_FACE \-o $WG\_FACE \-j ACCEPT  
$IPT \-I FORWARD 1 \-i $WG\_FACE \-o $IN\_FACE \-j ACCEPT  
$IPT \-I INPUT 1 \-i $IN\_FACE \-p udp --dport $WG\_PORT \-j ACCEPT  
  
\# outside the wall you can achieve anything  
$IPT \-t nat -A PREROUTING -i $IN\_FACE \-p tcp --dport $HELIUM\_PORT \-j DNAT --to-destination $HELIUM\_IP:$HELIUM\_PORT  
  
\## IPv6 (Uncomment if ipv6 is required) ##  
\## $IPT6 -t nat -I POSTROUTING 1 -s $SUB\_NET\_6 -o $IN\_FACE -j MASQUERADE  
\## $IPT6 -I INPUT 1 -i $WG\_FACE -j ACCEPT  
\## $IPT6 -I FORWARD 1 -i $IN\_FACE -o $WG\_FACE -j ACCEPT  
\## $IPT6 -I FORWARD 1 -i $WG\_FACE -o $IN\_FACE -j ACCEPT

**`sudo vim` `/etc/wireguard/helper/remove-nat-routing.sh`**

#!/bin/bash  
IPT="/sbin/iptables"  
#IPT6="/sbin/ip6tables"   
IN\_FACE="eth0" \# NIC connected to the internet  
WG\_FACE="wg0" \# WG NIC   
SUB\_NET="192.168.12.0/24" \# WG IPv4 sub/net aka CIDR  
WG\_PORT="41194" \# WG udp port  
HELIUM\_PORT="44158"  
HELIUM\_IP="192.168.12.2"  
#SUB\_NET\_6="fd42:42:42:42::/112" # WG IPv6 sub/net  
  
\# IPv4 rules #  
$IPT \-t nat -D POSTROUTING -s $SUB\_NET \-o $IN\_FACE \-j MASQUERADE  
$IPT \-D INPUT -i $WG\_FACE \-j ACCEPT  
$IPT \-D FORWARD -i $IN\_FACE \-o $WG\_FACE \-j ACCEPT  
$IPT \-D FORWARD -i $WG\_FACE \-o $IN\_FACE \-j ACCEPT  
$IPT \-D INPUT -i $IN\_FACE \-p udp --dport $WG\_PORT \-j ACCEPT  
$IPT \-t nat -D PREROUTING -i $IN\_FACE \-p tcp --dport $HELIUM\_PORT \-j DNAT --to-destination $HELIUM\_IP:$HELIUM\_PORT  
  
\# IPv6 rules (uncomment if ipv6 is required) #  
\## $IPT6 -t nat -D POSTROUTING -s $SUB\_NET\_6 -o $IN\_FACE -j MASQUERADE  
\## $IPT6 -D INPUT -i $WG\_FACE -j ACCEPT  
\## $IPT6 -D FORWARD -i $IN\_FACE -o $WG\_FACE -j ACCEPT  
\## $IPT6 -D FORWARD -i $WG\_FACE -o $IN\_FACE -j ACCEPT  

```
sudo chmod +x /etc/wireguard/helper/*.shsudo ufw allow 41194/udp# Allow the helium port to route traffic to wireguard client IPsudo ufw route allow proto tcp to 192.168.12.2 port 44158sudo ufw allow sshsudo ufw enablesudo ufw statussudo systemctl enable wg-quick@wg0sudo systemctl start wg-quick@wg0sudo systemctl status wg-quick@wg0sudo wg# Make sure you have opened 41194/udp in your cloud provider security group! # Now, we need to update the server to allow relaying trafficsudo vim /etc/sysctl.conf# append the following to the end of that file:
```

**\# Wireguard  
net.ipv4.ip\_forward = 1  
net.ipv6.conf.all.forwarding=1  
**

**`# reload the /etc/sysctl.conf file:  
sysctl -p`**

## Wireguard Client on Panther X1

```
sudo apt updatesudo apt upgraderebootsudo apt install wireguard vim fail2bansudo -icd /etc/wireguard/umask 077; wg genkey  tee privatekey  wg pubkey > publickeycat privatekeycat publickey# Go back to server /etc/wireguard/wg0.conf, fill in the public key into the [Peer] section# Uncomment peer section, and then run on server: sudo systemctl restart wg-quick@wg0# Set up client on Panther # Replace PrivateKey and PublicKey - pay attention to server vs client# Replace Endpoint with your server's public IP address or DNSsudo vim /etc/wireguard/wg0.conf
```

\[Interface\]  
\## This Desktop/client's private key ##  
PrivateKey = REPLACE ME  
  
\## Client ip address ##  
Address = 192.168.12.2/24  
  
\[Peer\]  
\## Ubuntu 20.04 server public key ##  
PublicKey = REPLACE ME  
  
\## set ACL ##  
AllowedIPs = 0.0.0.0/0  
\## Your Ubuntu 20.04 LTS server's external address and port "156.283.123.9:41194" ##  
Endpoint = REPLACEME:41194  
  
\## Key connection alive ##  
PersistentKeepalive = 15`**  
sudo systemctl enable wg-quick@wg0**  
**sudo systemctl start wg-quick@wg0  
sudo systemctl status wg-quick@wg0  
sudo wg  
sudo vim /lib/systemd/system/wg-quick@.service**`

Modify the following lines to include network.target:

After=network-online.target nss-lookup.target network.target   
Wants=network-online.target nss-lookup.target network.target

Double check everything is working. I also experience occasional drops, so I have a watch script on the client which restarts the wireguard client if we lose connectivity, or starts it upon reboots. Don't enable this until you are certain everything is working first. 

**sudo vim /root/wireguard\_watch.sh**

#!/bin/bash  
/usr/bin/ping -c 1 192.168.12.1 &> /dev/null && (/usr/bin/logger "INFO: Wireguard alive")  (/usr/bin/logger "ERROR: Wireguard dead, restarting wg-quick@wg0" ; /usr/bin/systemctl restart wg-quick@wg0)

```
sudo chmod +x /root/wireguard_watch.shsudo crontab -e
```

You will see an existing line in this file. Ignore it, and leave it alone. Add a new line which contains:   
\* \* \* \* \* /root/wireguard\_watch.sh > /dev/null

I also installed the [Panther X1 Dashboard](https://github.com/Panther-X/PantherDashboard):

**`sudo -i  
cd /root  
git clone https://github.com/Panther-X/PantherDashboard.git`**  
**`cd PantherDashboard`**  
**`sudo sh install.sh`**

You can now access the dashboard at https://YourInternalPantherIPAddress using admin/admin as the login. 

From SSH, you can also explore the setup and troubleshoot the logs:

**`tail -f /opt/miner_data/logs/console.log`**

**`sudo docker exec helium-miner miner info height  
sudo docker exec helium-miner miner peer listen`**

We can see the miner info height returns the epoch and block height which you can compare with https://explorer.helium.com. You can use the miner peer listen to see what your IP address is - it should show your Wireguard external server IP address if successful. You can safely ignore the 172.xxx IP address, as it is just the docker container. 

Outside the wall, we can achieve anything. Have fun!