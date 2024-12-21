---
title: OpenCanary honeypot on Synology NAS
tags: []
id: '2041'
categories:
  - - uncategorized
alias: /2022/03/02/opencanary-honeypot-on-synology-nas/
date: 2022-03-02 18:45:07
---

Coal miners used to carry a canary in a cage when digging as the bird would sing, unless dangerous gasses collected in the mine which would weaken the bird and alert the miners of the imminent danger. The same concept has existed in computer security for a long time as honeypots naturally attract hackers and allow a network security engineer to use them to alert of the issue. A Synology NAS full of identification or financial documents is a target for hackers that should be fully secured on your intranet and always isolated from the public internet. However, what if your internal network is unknowingly compromised (malware, IoT device, etc.) and hackers start to try to launch attacks against your secure NAS? Reviewing the log files for all possible service attacks may be quite challenging and, by the time you notice the issue, may be too late to take action. A canary honeypot can help immediately alert you that a serious problem may exist within your security perimeter.
<!-- more -->
For this project, I used the "thinkst/opencanary" docker container on my Synology NAS. It creates a variety of psuedo-real services that log to a central file that can be easily monitored. Any bot, script kiddie, or even advanced persistent threat scanning the network may get curious, access this honeypot, and trigger the canary to issue an early warning alert. Thinkst also has an enterprise version and I'm willing to bet it's worth the money because the free version is amazing for a self-hosted enthusiast, but their paid version looks turnkey.

The Synology NAS makes hosting docker containers easy. This particular container is going to require a bit more setup because it will be attempting to run services on ports that conflict with Synology's own internal service ports, such as a tcp/80 webserver. We can use a docker "macvlan" network driver to create a special network interface that creates a unique IP address separate from your Synology NAS IP address.

The full instructions on [how to set up Macvlan in Synology can be found here](https://www.wundertech.net/how-to-use-docker-on-a-synology-nas/#2_Macvlan_Bridge_Network_Interface) and I suggest reading these first.

However, the commands I used are specific to my Synology setup (I use bond0 instead of eth0 because I have bonded my ethernet ports together, and I picked 192.168.50.200/32 as an unused ip address that my router DHCP will not assign). You must SSH to the Synology NAS to issue this command because it is currently not possible to create a macvlan network through the GUI:

`docker network create -d macvlan -o parent=bond0 --subnet=192.168.50.0/24 --gateway=192.168.50.1 --ip-range=192.168.50.200/32 canary_network`

I next created a docker bridge "canary\_bridge" network through the Synology GUI. This allows my Synology to communicate with the OpenCanary container using 192.168.10.2/32 IP address.

![canary_bridge](/2022/03/canary_bridge.jpg)

Finally, I downloaded the image "neomediatech/opencanary:latest" (it appears to be the best supported version of thinkst/opencanary in dockerhub), and launched it on both the "canary\_network" and "canary\_bridge" networks using the advanced settings during the launch. This ensures both my NAS and rest of the network can see this container. As the container is using macvlan, you do not need to map any ports in docker -- everything on the container is magically directly visible to the network. Leave the "port settings" page blank. Never expose this container directly to the public internet "for fun", as the canary services it runs are not secure and don't trust a docker container to isolate from a hacker.

![canary_advancednetwork](/2022/03/canary_advancednetwork.jpg)

In the volume settings, you can specify a /data directory for the log file, and optionally provide a custom opencanary.conf file mapped to /root/.opencanary.conf. [The example opencanary.conf template file can be found on the github repository - enable more of the services](https://github.com/thinkst/opencanary/blob/master/data/.opencanary.conf).

![canary_volume](/2022/03/canary_volume.jpg)

Once launched, you could port scan the IP address of the OpenCanary from another machine and see the services running (or use 192.168.10.1 if running the scan from the Synology):

`nmap -F 192.168.50.200`

![canary_ports](/2022/03/canary_ports.jpg)

When you access port 80 webserver, you will get a very convincing fake Synology NAS login page:

![canary_login](/2022/03/canary_login.jpg)

When a hacker attempts to login against these fake services, each attempt is logged. As nobody should ever need to login to these fake services, you can now use any log file alerting tool. You can also use the built in PyLogger SMTP service to relay these log messages to your email. Sadly, the SMTP log service has issues with TLS connections so you may need to fiddle with the settings... it took me a bit to get it all working.

![canary log](/2022/03/canary-log.jpg)

Now you all know my secrets but hopefully this idea helps protect someone else.