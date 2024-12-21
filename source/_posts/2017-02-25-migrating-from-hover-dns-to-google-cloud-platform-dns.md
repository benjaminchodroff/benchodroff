---
title: Migrating from Hover DNS to Google Cloud Platform DNS
tags:
  - automate
  - dns
  - gcloud
  - gcp
  - google
  - hover
id: '239'
categories:
  - - uncategorized
alias: /2017/02/25/migrating-from-hover-dns-to-google-cloud-platform-dns/
date: 2017-02-25 10:30:49
---

I recently helped migrate a Hover DNS domain into Google Cloud Platform DNS. Hover does not provide any public API or supported export functionality. Google has no import function built into their cloud console and provides no assistance. In case anyone else is looking to do a similar migration, here are the steps I used to automate migrating all DNS records with no disruption to any services.
<!-- more -->
Plan:

1.  1.  Login to your [Hover](https://www.hover.com/domains) domain - Extract your entries using the following javascript (I used Google Chrome developer tools to paste this into the console). The results will look similar to the "tool-man.org.zone" output:
    2.  Login to [Google Cloud Console](https://console.cloud.google.com/) and create a new DNS zone for your intended domain (From your Project, go to Networking -> Cloud DNS -> Create Zone)
    3.  Import your domain into the zones - Launch the gcloud command line tool and execute: [gcloud dns record-sets import](https://cloud.google.com/sdk/gcloud/reference/dns/record-sets/import) \[yourzonefile\] --zone-file-format -z \[theNameOfTheZoneInGoogle\] Note: you can easily upload the zone file export into Google's cloud shell in your browser -- no need to install gcloud command line!
    4.  In Hover, go to: [https://www.hover.com/nameservers](https://www.hover.com/nameservers) and update NS entries to point to the new NS records for your domain defined in the zone within Google's DNS
    5.  Test from your local Linux/macOS command line: dig \[yourdomain.com\] 8.8.8.8 Confirm that your NS records reflect that Google's DNS is the authority - this could take less than an hour, normally You can ask the same question to 8.8.8.8, ns1.hover.com, and other DNS servers to monitor the propagation of this change across the internet
    
    Rollback:
    1.  Go to [https://www.hover.com/nameservers](https://www.hover.com/nameservers) and rollback the NS entries. This could take up to 2 days for the default TTL, but typically is done in less than 4 hours for most of the internet.