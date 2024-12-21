---
title: >-
  Using Apache NiFi on macOS to connect to IBM Bluemix Watson IoT using MQTT to
  IoT
tags: []
id: '36'
categories:
  - - uncategorized
alias: >-
  /2017/02/15/using-apache-nifi-on-macos-to-connect-to-ibm-bluemix-watson-iot-using-mqtt-to-iot/
date: 2017-02-16 05:31:47
---

I tried out Apache NiFi and liked how easy it was to act as an integration to bridge disparate Internet of Things systems together. I have previously tried NodeRED which I still highly recommend. Where I see NiFi exceeds is its ability to track each communication throughout the chain of custody.  This concept is marketed as provenance and tracks the exact data transformation that occurs from input to output.
<!-- more -->
\[caption id="attachment\_41" align="aligncenter" width="289"\][![ConsumeMQTT NiFi Queue](http://www.benchodroff.com/wp-content/uploads/2017/02/consumeMQTTnifiQueue-289x300.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/consumeMQTTnifiQueue.png) ConsumeMQTT NiFi Queue\[/caption\]

Enough with the marketing, how do you get started? I am using macOS Sierra 10.12.3 with Homebrew. I also have Java 1.8.0\_92 installed from Oracle. Within Terminal:

> brew install nifi

Once nifi is installed, you will need to ensure your JAVA\_HOME environment is set up.

> export JAVA\_HOME="$(/usr/libexec/java\_home)"

You may now launch nifi as either a command:

> nifi run

Or as a background service:

> nifi start

Now that nifi is running, you may wish to check the logs, which default to this folder:

> /usr/local/Cellar/nifi/1.1.1/libexec/logs

In order to connect to NiFi you will need to find which ip address it is running on. The default is http://localhost:8080/nifi but I found I needed to use http://127.0.0.1:8080/nifi in order to connect. The logs may provide additional details

Now that you are connected via your web browser, you can begin to establish MQTT connections. I will assume you already have set up a MQTT device using Watson IoT out on bluemix.net.

Within NiFi, drag the "Processor" Icon onto the template:

\[caption id="attachment\_39" align="aligncenter" width="525"\][![NiFi Processor](http://www.benchodroff.com/wp-content/uploads/2017/02/nifiprocessor-1024x153.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/nifiprocessor.png) NiFi Processor\[/caption\]

You can then pick ConsumeMQTT from the list of processors and select "Add." Right click on the new ConsumeMQTT processor and select "Configure" and go to the "Properties" tab.

\[caption id="attachment\_40" align="aligncenter" width="525"\][![ConsumeMQTT NiFi](http://www.benchodroff.com/wp-content/uploads/2017/02/consumemqttnifi-1024x927.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/consumemqttnifi.png) ConsumeMQTT NiFi\[/caption\]

*   Broker URI: tcp://\[orgID\].messaging.internetofthings.ibmcloud.com:1883 \[To use SSL you must set a different port with an SSL Context Service\]
*   Client ID: a:\[orgID\]:\[anything\]
*   Username: Your API Key
*   Password: Your API Auth Token
*   Topic Filter: iot-2/type/+/id/YOURDEVICEID/evt/+/fmt/json
*   Max Queue Size: 2048

Once you have configured your consumer, you may click "OK," right click on the consumer and select "Start." If there are any issues with connecting, you will see in the upper right hand corner of the consumer a list of the latest debug outputs. Otherwise, you should begin to see the "Out" messages increment if the consumer sees any new MQTT topics matching the filter.

You may then drag and drop an "Output Port" onto the template and connect your consumeMQTT to your output port. Once you begin receiving MQTT messages, you should see them as queued.

If you right click on the queud messages and select "List Queue" you can navigate the provenance of each message. You can click each messages "i" icon to download or view the message content:

\[caption id="attachment\_43" align="aligncenter" width="525"\][![NiFi Queue](http://www.benchodroff.com/wp-content/uploads/2017/02/nifiqueue-1024x193.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/nifiqueue.png) NiFi Queue\[/caption\]

Viewing a message:

\[caption id="attachment\_44" align="aligncenter" width="525"\][![NiFi MQTT Message](http://www.benchodroff.com/wp-content/uploads/2017/02/NiFi-MQTT-Message-1024x101.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/NiFi-MQTT-Message.png) NiFi MQTT Message\[/caption\]

Now that you have successfully connected the IBM Watson IoT Foundation to MQTT, you could envision how NiFi could be used to do "ETL" - Extract, Transform, Load - on messages while providing transparency on the data lineage.