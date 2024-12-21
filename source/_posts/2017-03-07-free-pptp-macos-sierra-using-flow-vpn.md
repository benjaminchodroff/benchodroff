---
title: Free PPTP in macOS Sierra using Flow VPN
tags:
  - macos
  - pptp
  - sierra
  - softlayer
  - vpn
id: '264'
categories:
  - - uncategorized
alias: /2017/03/07/free-pptp-macos-sierra-using-flow-vpn/
date: 2017-03-07 10:22:39
---

Apple disabled native PPTP VPN in macOS Sierra for good reason - it is highly insecure. However, sometimes for reasons completely outside of your control you need to use a PPTP VPN. Luckily, there is a free option through Flow VPN: [https://www.flowvpn.com/download-mac/](https://www.flowvpn.com/download-mac/)
<!-- more -->
\[caption id="attachment\_267" align="aligncenter" width="300"\][![Flow VPN](https://benchodroff.com/wp-content/uploads/2017/03/flowvpn-300x89.png)](https://benchodroff.com/wp-content/uploads/2017/03/flowvpn.png) Flow VPN for PPTP\[/caption\]

Once you download the client, simply type in the VPN address such as "pptpvpn.dal01.softlayer.com" with your username and password. PS - You can find a full list of SoftLayer PPTP VPN urls at the bottom of this guide: [https://knowledgelayer.softlayer.com/procedure/set-pptp-vpn-mac-os-x-10x](https://knowledgelayer.softlayer.com/procedure/set-pptp-vpn-mac-os-x-10x)

I did observe a defect while working with SoftLayer and Flow VPN. Your password cannot have the "#" special character in it or you will get invalid password errors. I couldn't tell if the defect is with Flow VPN or SoftLayer.