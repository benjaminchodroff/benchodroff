---
title: Security, standards, and frameworks for industrial IoT
tags:
  - advanced threat
  - compliance
  - failure
  - industrial
  - internet of things
  - iot
  - iso 26262
  - predictive maintenance
  - quality
  - reliability
  - safety
  - security
  - standards
id: '17'
categories:
  - - uncategorized
alias: /2017/02/24/security-standards-and-frameworks-for-industrial-iot/
date: 2017-02-24 11:35:09
---

As the world moves towards embracing the internet of things for industrial assets, the importance of considering security cannot be understated. Industrial IoT poses unique challenges to security in combining Information Technology security with Operational Technology security. Many of these assets were designed to serve critical functions and their reliability is paramount. All of the systems that power our modern lives are undergoing a revolution where they will be controlled or interact with software. Assets will be connected to the internet for updates, pushing data, or even remote control. How can we make these industrial IoT systems secure by design?
<!-- more -->
Multiple attacks, most notably Stuxnet, have proven that even an isolated "air gapped" network is not sufficient at preventing an attack which can compromise a software controlled asset. Additional protections must be accounted for in all processes and controls within any software controlled system. Could the use of signed encrypted code binaries provide a stronger defense for industrial IoT systems? The stakes are even higher for implementing a secure solution if the asset's vendor wishes to provide remote automated software upgrades for an IoT asset.

If a software system is compromised remotely it does not necessarily mean an industrial IoT asset should be able to catastrophically fail. The idea of a resilient design decision that asset vendors should carefully consider in their requirements. I love digital technology and it is incredible how affordable digital sensors have become. Unfortunately, digital technology is no substitutes for a mechanical safety controls, good process, and a deep understanding of how to implement security. Does this mean we should avoid implementing industrial IoT systems?

Absolutely not. In fact, adding such digital sensors and controls, when designed correctly, can improve safety considerably by providing quicker and deeper knowledge about how the system is operating. Such data can be used to increase the performance, decrease costs, detect issues, or even predict when issues could happen. We are continually finding new ways to implement digital capabilities while improving the security and safety of the design as necessary. In many cases, these digital systems should be considered an addition and not necessarily a replacement of any existing mechanical failsafe.

Best practice application security practices have been developing in the Information Technology (IT) space for years, but are still failing to be consistently implemented in the embedded systems space. How do we ensure best practices are implemented? How can we possibly merge the operational technology (OT) securities world with the complexities of IT security?

One of the best ways we can ensure best practices are applied in industrial IoT systems is through capturing standards. One industry that has been and remains on the edge of providing secure industrial IoT systems is automotive. Safety standards, such as [ISO 26262](http://www.iso.org/iso/catalogue_detail?csnumber=43464), should serve as a guide to all industries on how to define requirements, design, develop, test, monitor, and support secure industrial IoT systems throughout their lifecycle. Unfortunately, each industry and IoT use case is unique and requires continual security review.

\[caption id="attachment\_236" align="aligncenter" width="300"\][![Industrial IoT Trust](http://www.benchodroff.com/wp-content/uploads/2017/02/trustworthiness2-300x300.png)](http://www.benchodroff.com/wp-content/uploads/2017/02/trustworthiness2.png) Industrial IoT Trust\[/caption\]

The other way to foster better security is through sharing knowledge. [AT&T](https://www.business.att.com/enterprise/Portfolio/internet-of-things/ "AT&T"), [Cisco](https://http://www.cisco.com/c/en/us/solutions/internet-of-things/overview.html "Cisco Systems"), [General Electric](https://www.ge.com/digital/industrial-internet "General Electric"), [IBM](https://www.ibm.com/internet-of-things/ "IBM"), and [Intel](http://www.intel.com/content/www/us/en/internet-of-things/overview.html "Intel") formed the [Industrial Internet Consortium](http://www.iiconsortium.org/) (IIC) almost three years ago and now boasts over 269 member organizations (02/23/2017). While IIC is not a standards organization, they are actively promoting shared knowledge through resources that promotes security along with many other topics. Their first contribution has produced an [Industrial Internet Security Framework](http://www.iiconsortium.org/IISF.htm) technical report. This report is an excellent read for everyone involved in making industrial IoT trustworthy.

Standards and frameworks are necessary. Our most critical infrastructure and systems depend on vendors and customers ensure they understand and adopt new security standards in this rapidly evolving industrial IoT landscape.