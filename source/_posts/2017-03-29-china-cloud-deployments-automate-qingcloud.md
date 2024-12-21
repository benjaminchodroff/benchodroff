---
title: China cloud deployments and how to automate QingCloud
tags:
  - aliyun
  - api
  - automate
  - azure
  - beian
  - bluemix
  - china
  - CLI
  - cloud
  - gcp
  - ibm
  - ICP
  - mandarin
  - microsoft
  - python
  - qingcloud
  - sdk
  - softlayer
id: '286'
categories:
  - - uncategorized
alias: /2017/03/28/china-cloud-deployments-automate-qingcloud/
date: 2017-03-29 04:06:42
---

I have explored multiple cloud providers in China, including QingCloud, over the past two years. QingCloud focuses on private enterprises in China but also caters to many startups too. I wanted to find a way to automate a Virtual Private Cloud (VPC) setup and thought I would share my script to build out a bastion host in a secure VPC. Automating their infrastructure is incredibly simple given their powerful API partnered with their private network capabilities. Having worked with clouds in Amazon, Google, IBM, Microsoft, VMWare, Aliyun... I am impressed with how easy QingCloud makes establishing a VPC either from their web portal or automated script. Here is my automated deployment script.
<!-- more -->
\[caption id="attachment\_293" align="aligncenter" width="640"\][![QingCloud VPC](https://benchodroff.com/wp-content/uploads/2017/03/qingcloudvpc-1024x527.png)](https://benchodroff.com/wp-content/uploads/2017/03/qingcloudvpc.png) QingCloud VPC\[/caption\]

Here is the sample code to both set things up and tear them back down. If you find this helpful or ever want more details about cloud deployments in China, let me know in the comments.