---
title: Selecting a VPN for China
tags: []
id: '1835'
categories:
  - - uncategorized
alias: /2020/06/26/selecting-a-vpn-for-china/
date: 2020-06-27 00:20:37
---

Selecting a VPN for China is [not as simple as it used to be](https://benchodroff.com/2018/01/31/shadowsocks-docker-bypassing-chinas-great-firewall/). Basic VPN solutions from the west won't cut it and most don't even work. Running your own VPN node such as Shadowsocks (SSR) or V2ray often can't achieve 100Mbps+. Since late 2018, ensuring a self-hosted node stays reliable can be a full time job of outrunning detection by the Great Firewall (GFW) which employs an army of censors and increasing artificial intelligence recognition.
<!-- more -->
While I could say what I use today ([nexitallysafe.com](http://nexitallysafe.com), [wannaflix.com](http://wannaflix.com), and two backup v2ray servers in Aliyun and AWS using CloudFlare relaying websockets) these will likely all be dead soon enough. I'm going to teach you to fish and point you to the self proclaimed non-experts at [duyaoss.com](http://duyaoss.com) which regularly review the best providers and provide detailed technical analysis. Use Google Chrome to translate the site if you can't read Chinese.

![](https://benchodroff.com/wp-content/uploads/2020/06/image-1-287x300.png)

2020/06/27 - Shanghai to Hong Kong, using Nexitally and China Telecom

## Commercial Review

Not all internet service providers (ISP) will have the same performance with each VPN provider. The first way to getting reliable VPN speed is to use the right ISP where possible. For the best performance and least restrictions, I recommend China Unicom (few users) or China Telecom (very good). China Mobile should be avoided as their IPLC connection is terrible and they have too many users, but with the right VPN you can make the most of it. Let's cut to the chase and see the reviews:

[VPN Testing Results using China Unicom (and intro - read this first!)](https://www.duyaoss.com/archives/3/)

[VPN Testing Results using China Telecom](https://www.duyaoss.com/archives/1/)

[VPN Testing Results using China Mobile](https://www.duyaoss.com/archives/1031/)

## Run Your Own v2Ray

Running your own v2ray server outside China for use in China will never be as fast as the best commercial providers because you cannot buy a [relay node](https://www.duyaoss.com/archives/2741/) unless you happen to be mainland Chinese citizen, have a legitimate Commercial Business license (hard), and can obtain an Internet Data Center license (IDC) in China (impossible).

However, if you require a VPN which works even during a crackdown or want to have online privacy, I recommend running your own private v2ray servers and only using them in an emergency. How to setup a v2ray VPN is quite an advanced topic and you should use Google for better guides. However, you can see my v2ray configuration script here if it helps you: [https://github.com/benjaminchodroff/v2ray-connect/](https://github.com/benjaminchodroff/v2ray-connect/)

I highly recommend setting up v2ray with WebSockets and relaying it through CloudFlare for free. This ensures that even if your server gets banned, you can still access it via a CloudFlare relay which, while capped to <50Mbps, it is impossible for China to ban without China blocking much of the web. [I recommend reading this for analysis of cloud providers that are commonly used](https://www.duyaoss.com/archives/57/) for hosting VPN nodes for use with users in China.