---
title: Getting started with LoRa using a LoPy for IoT
tags:
  - embedded python
  - iot
  - lopy
  - lora
id: '116'
categories:
  - - uncategorized
alias: /2017/02/18/getting-started-with-lora-using-a-lopy-for-iot/
date: 2017-02-18 14:10:18
---

When a colleague first told me about LoRa, I was skeptical that it was even real. I can use a small handheld device to transmit messages up to 40km to another device for free? It sounds too good to be true, but it's real. Of course, there is a catch - less bandwidth. But who cares? If all I need to send from my IoT device is perhaps some longitude, latitude, and basic sensor data -- LoRa is a great technology.
<!-- more -->
\[caption id="attachment\_150" align="aligncenter" width="300"\][![LoPy](http://www.benchodroff.com/wp-content/uploads/2017/02/lopy-300x300.jpg)](http://www.benchodroff.com/wp-content/uploads/2017/02/lopy.jpg) LoPy\[/caption\]

I backed the kickstarter project, [LoPy](https://www.kickstarter.com/projects/1795343078/lopy-the-lora-wifi-and-bluetooth-iot-development-p), which combines LoRa, Bluetooth, and WiFi into an embedded python microcontroller about the size of a stick of gum. For about $90 USD, I received two LoPy devices, antennas, and a development interface. There are much cheaper options to buy a raw LoRa radio, but I found this dev kit to be a great quick way to prototype ideas. Plus, I have always wanted to try embedded python!

When you first get the devices, make sure you take the time to update the firmware. There were numerous issues when mine first arrived, but as of early February 2017, all the issues I hit have been fixed. I've tested the bluetooth, wifi, and LoRa and have been very impressed. I was able to send and receive messages about a half a mile from my porch. No line of site and I live in a dense city. With a better antenna position, I bet it could easily transmit a few miles.

Here is the python code I built to test the LoRa radio range. Connect one of the LoPy's to be your transmitter and the other to be your receiver. You can use the development kit built in button to signal which LoPy will be the transmitter or receiver by pushing it during startup. If all goes well, the transmitter will send a series of colors as commands to the receiver. If the receiver successfully sees the message, it will set it's RGB led to the color it received, and send an acknowledgement message back to the transmitter. The transmitter will then set its RGB led to the color that was acknowledged as successfully received. This is a quick and dirty way to test the range because you can clearly see if messages get lost. Any time an error happens, the RGB will go blue or white to indicate the issue.  
main.py: