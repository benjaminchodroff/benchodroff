---
title: Using Freeboard.io to visualize devices with IBM Watson IoT MQTT messages
tags: []
id: '49'
categories:
  - - uncategorized
alias: >-
  /2017/02/15/using-freeboard-io-to-visualize-devices-with-ibm-watson-iot-mqtt-messages/
date: 2017-02-16 06:00:41
---

If you are looking for a quick and powerful way to visualize your IBM Watson IoT device messages using MQTT, I would highly recommend trying out freeboard.io. It is 100% open source and they even provide free online hosting if you do not wish to host it yourself.
<!-- more -->
You can see my live demo of my IoT freshwater aquarium dashboard on freeboard.io here: [https://freeboard.io/board/l1XbFY](https://freeboard.io/board/l1XbFY)

\[caption id="attachment\_54" align="aligncenter" width="525"\][![MQTT Freeboard.io IBM Watson IoT](http://www.benchodroff.com/wp-content/uploads/2017/02/mqttfreeboardmqttibm-1024x312.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/mqttfreeboardmqttibm.png) MQTT Freeboard.io IBM Watson IoT\[/caption\]

Unfortunately, there is no direct integration from Freeboard.io to support MQTT, but luckily they have a javascript developer console where you can integrate to anything! I have taken the Paho MQTT client and some similar open source code to make it work for my purposes. You can see the code here:

> [https://github.com/benjaminchodroff/freeboard-mqtt](https://github.com/benjaminchodroff/freeboard-mqtt)

If you would like to use this custom plugin, here is how to hook it up.

1.  Make sure you are signed into freeboard
2.  Click "Developer Console"
    
    \[caption id="attachment\_51" align="aligncenter" width="525"\][![Freeboard.io Developer Console](http://www.benchodroff.com/wp-content/uploads/2017/02/developerconsole-1024x197.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/developerconsole.png) Freeboard.io Developer Console\[/caption\]
    
3.  Add the following two plugins and then click OK:
    1.  https://rawgit.com/benjaminchodroff/freeboard-mqtt/paho-mqtt-default/ibm.iotfoundation.plugin.js
    2.  https://rawgit.com/benjaminchodroff/freeboard-mqtt/paho-mqtt-default/paho.mqtt.plugin.js
4.  Under datasources, click "Add" and select the new "IBM IoT Foundation" datasource
5.  Configure the new datasource with your details, including which device to listen to
    
    \[caption id="attachment\_52" align="aligncenter" width="525"\][![Watson IoT Freeboard.io Configuration](http://www.benchodroff.com/wp-content/uploads/2017/02/IBMIoTFoundationFreeboard-1024x587.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/IBMIoTFoundationFreeboard.png) Watson IoT Freeboard.io Configuration\[/caption\]
    
    Now that your Datasource is configured, you should start to see it update automatically every time a new MQTT message is received. You can add widgets which can visualize this data by adding a Pane, then adding a widget, and then selecting data from your MQTT message.
    

I would recommend using JSON encapsulated messages so that it is easy to extract and manipulate the data. For example, I have a message which contains the pH of water and I can add this to a widget by using the value:

`(datasources["IOT"]["b827eb14cce9"]["water_ph"]).toFixed(2)`

\[caption id="attachment\_53" align="aligncenter" width="652"\][![IoT MQTT pH value](http://www.benchodroff.com/wp-content/uploads/2017/02/pH.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/pH.png) IoT MQTT pH value\[/caption\]