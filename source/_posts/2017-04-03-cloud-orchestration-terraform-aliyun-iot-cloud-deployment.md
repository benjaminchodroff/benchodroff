---
title: Cloud orchestration with terraform in Aliyun for an IoT cloud deployment
tags:
  - alibaba
  - alicloud
  - aliyun
  - automation
  - china
  - cloud
  - hashicorp
  - iot
  - orchestration
  - terraform
id: '298'
categories:
  - - uncategorized
alias: /2017/04/03/cloud-orchestration-terraform-aliyun-iot-cloud-deployment/
date: 2017-04-03 13:18:50
---

Automating cloud deployments around the world can become very tedious because each cloud provider API has quirks. Luckily, the world is moving beyond declarative cloud deployment automation into procedural orchestration. I'll show how to [Hashicorp's terraform](https://www.terraform.io/) for an [example IoT cloud deployment](https://github.com/benjaminchodroff/terraform-iot-aliyun) in the fastest growing cloud in the world: Alibaba's "[Aliyun](http://www.aliyun.com/)" cloud, based out of China.
<!-- more -->
  
[Hashicorp's terraform](https://www.terraform.io/) allows DevOps to move beyond declarative infrastructure automation into procedural infrastructure automation. Rather than stating step by step how to set up a cloud deployment, wouldn't you rather say "I want the cloud to look like this" and then have the utility figure out how to deploy that? The significant benefit to this model is that most cloud deployments are rarely static. As changes happen, you want to change control "what is the cloud deployment," not the exact steps of how to get there.

[Aliyun](http://www.aliyun.com/) is a Chinese based cloud provider that operates globally with datacenters currently in China, USA, Singapore, Australia, Europe, UAE, and Japan. They offer many similar capabilities that can be found in other providers including Amazon Web Services, Microsoft Azure, Google Cloud Platform, and IBM Bluemix. However, their biggest strength is in the mainland China market where they are the dominant IaaS player.

Here is a reference IoT cloud deployment architecture diagram which includes a virtual private cloud, routers, load balancers, backend MQTT workers and https controllers.

\[caption id="attachment\_301" align="aligncenter" width="640"\][![Aliyun IoT Example](https://benchodroff.com/wp-content/uploads/2017/04/iotexample-1024x746.png)](https://benchodroff.com/wp-content/uploads/2017/04/iotexample.png) Aliyun IoT Example\[/caption\]

[Here is the link to my full "terraform-iot-aliyun" source code](https://github.com/benjaminchodroff/terraform-iot-aliyun) \-- check out the README.md to see how to get started. You can then follow "main" example to see how it all comes together. By editing the main/variables.tf, we can spin up new mqtt and https instances to scale from a small deployment to a full production workload. The best part is that you can configure the load balancers and all other components to dynamically react to this change without having to tediously declare these changes.

Here is a basic "minimal" example that may help you see the basics of terraform with an Aliyun provider: