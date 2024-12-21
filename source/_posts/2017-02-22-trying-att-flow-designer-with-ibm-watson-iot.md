---
title: Trying AT&T Flow Designer with IBM Watson IoT
tags:
  - azure
  - bluemix
  - flow
  - ibm
  - iot
  - javascript
  - mqtt
  - Node-RED
  - salesforce
  - softlayer
  - watson iot
id: '198'
categories:
  - - uncategorized
alias: /2017/02/21/trying-att-flow-designer-with-ibm-watson-iot/
date: 2017-02-22 05:35:22
---

The AT&T IoT Platform's "Flow" at [https://flow.att.io](https://flow.att.io) has recently announced they support IBM Bluemix with Watson IoT Platform. This is a great hybrid cloud strategy for IoT. After taking a quick look, the flow platform is an impressively customized version of [Node-RED](https://nodered.org/). Let's take it for a spin!
<!-- more -->
1.  Register with AT&T Flow here: [https://flow.att.io/](https://flow.att.io/) -- it was very painless and free to get started
2.  Make sure you have a Bluemix account and an active Watson IoT platform example - I recommend the quickstart but I will use a real device for this tutorial: [https://internetofthings.ibmcloud.com/](https://internetofthings.ibmcloud.com/)
3.  Create your first project in Flow
    
    \[caption id="attachment\_199" align="aligncenter" width="300"\][![AT&T IOT Platform](http://www.benchodroff.com/wp-content/uploads/2017/02/helloworld-300x285.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/helloworld.png) AT&T IOT Platform\[/caption\]
    
4.  On the left hand side of the canvas, under IBM, drag "IBMIOT In" onto the canvas, and double click it. If you can't find it, you can use the "Search" to quickly find it (there are a lot!)
5.  Configure the IBM Watson IoT input with the following information from your account, or alternatively use the quickstart to get started
    
    \[caption id="attachment\_204" align="aligncenter" width="245"\][![IBM IoT Configuration](http://www.benchodroff.com/wp-content/uploads/2017/02/ibmiotconfig-245x300.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/ibmiotconfig.png) IBM IoT Configuration\[/caption\]
    
    1.  Authentication: API Key
    2.  API Key: Enter in your key details
    3.  Input Type: Device Event (default, but check out the others such as "Device Status" for device online presence information!)
    4.  Device Type: check the "All" box
    5.  Device ID: check the "All" box (unless you are using the quickstart - use your device id provided)
    6.  Event: check the "All" box
    7.  Format: json (my messages are in json format)
    8.  Name: IBM IoT
    9.  Save!
6.  From the output list, drag "Debug" onto the canvas
7.  Connect your IBMIOT to the Debug, and click "Deploy" in the upper right hand corner
    
    \[caption id="attachment\_202" align="aligncenter" width="640"\][![AT&T Flow with IBM Watson IoT](http://www.benchodroff.com/wp-content/uploads/2017/02/flowiotwatson-1024x565.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/flowiotwatson.png) AT&T Flow with IBM Watson IoT\[/caption\]
    
8.  You should now be able to click the "Debug" in the lower left hand corner of the canvas. You will see a console open and begin to see your json IoT messages (using MQTT) within the AT&T IoT Platform.
9.  Success!
10.  Next steps: explore the multitude of inputs, outputs, and analysis you can perform

I have used Node-RED over the past couple years to automate my freshwater fishtank with a variety of device events, status, and control messages using a Raspberry Pi. Using the AT&T Iot Platform lets you send text messages, control device billing, provision new devices into your IoT solution, and much more. I'm very impressed and see potential.

\[caption id="attachment\_203" align="aligncenter" width="640"\][![Node-RED Fishtank](http://www.benchodroff.com/wp-content/uploads/2017/02/noderedfishtank-1024x680.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/noderedfishtank.png) Node-RED Fishtank\[/caption\]