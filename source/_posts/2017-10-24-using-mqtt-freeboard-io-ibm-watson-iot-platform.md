---
title: Using MQTT on Freeboard.io with IBM Watson IoT Platform
tags:
  - freeboard
  - ibm
  - iot
  - javascript
  - messaging
  - mqtt
  - watson iot
id: '327'
categories:
  - - uncategorized
alias: /2017/10/23/using-mqtt-freeboard-io-ibm-watson-iot-platform/
date: 2017-10-24 04:55:47
---

I helped add a javascript MQTT plugin to the free IoT visualization platform [freeboard.io](https://freeboard.io). [MQTT](http://mqtt.org/) is an ISO standard publish-subscribe protocol for fast and secure IoT communications over TCP/IP. While there are many options for messaging, MQTT is excellent for many common IoT use cases. The [IBM Watson IoT Platform](https://internetofthings.ibmcloud.com/) provides a way to scale from free to full featured with many advanced security and integration features built in to their MQTT broker. Here is a quick demo of how to get started using MQTT IoT messaging in freeboard without requiring any programming, installation, or cost.
<!-- more -->
1.  Go the IBM Watson IoT Platform Quickstart: [https://quickstart.internetofthings.ibmcloud.com/iotsensor/](https://quickstart.internetofthings.ibmcloud.com/iotsensor/)
2.  Copy the device ID found in the upper right hand corner, such as "cd2b389949ff". Keep this page open.
    
    \[caption id="attachment\_328" align="aligncenter" width="199"\][![Watson IoT Platform Quickstart](https://benchodroff.com/wp-content/uploads/2017/10/quickstart-199x300.png)](https://benchodroff.com/wp-content/uploads/2017/10/quickstart.png) Watson IoT Platform Quickstart\[/caption\]
    
3.  In a new page, go to [freeboard.io](https://freeboard.io) and register a new free account.
4.  Login to freeboard.io with your account and create a new board.
5.  Click "Add" under "datasources" and select "MQTT"
6.  Enter a Name of "example"
7.   In the Topic field, replace "DEVICEID" with the device ID you copied from the quickstart page
    
    \[caption id="attachment\_329" align="aligncenter" width="300"\][![Freeboard MQTT](https://benchodroff.com/wp-content/uploads/2017/10/freeboard_mqtt-300x263.png)](https://benchodroff.com/wp-content/uploads/2017/10/freeboard_mqtt.png) Freeboard MQTT\[/caption\]
    
8.  You should not change any of the other default settings for the quickstart. Click "Save" and if everything is working, you should see the datasource get updated every few seconds. If it says "Last updated: never" you are not receiving any MQTT messages and have an issue.
9.  In freeboard.io click "Add Pane" and in the new pane click the "+" icon to add a "Gauge" widget
10.  In the gauge widget configuration, set the following and replace "DEVICEID" with the device from the quickstart, and click save
    *   Title: Temperature
    *   Value: datasources\["example"\]\["DEVICEID"\]\["d"\]\["temp"\]
    *   Units: Celcius
    *   Minimum: 0
    *   Maximum: 100
        
        \[caption id="attachment\_330" align="aligncenter" width="300"\][![Freeboard Widget](https://benchodroff.com/wp-content/uploads/2017/10/freeboard_temperature-300x222.png)](https://benchodroff.com/wp-content/uploads/2017/10/freeboard_temperature.png) Freeboard Widget\[/caption\]
        

Your freeboard is now connected to the IBM Watson IoT Platform Quickstart. The changes in the temperature on the quickstart page are set in a JSON message via the IBM Watson IoT Platform MQTT broker. The freeboard MQTT javascript plugin subscribes to the broker in your browser, and receives the messages associated with a topic. These messages are parsed and beautifully represented  in the gauge widget. The quickstart page represents a virtual device, but if you read further on IBM Watson IoT platform, you will find many ways to integrate to real world devices in a variety of programming languages.

As you get deeper in MQTT, you can find ways to provide confirmed delivery, persistence with queues, last will and testament, client X509 certificates, and so much more. The best part about MQTT is how far you can go with it!