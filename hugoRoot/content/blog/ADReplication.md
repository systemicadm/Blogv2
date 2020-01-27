---
# Common-Defined params
title: "If it's not DNS, then it has to be the network"
publishDate: "01-11-2020"
sidebar: true
toc: false
description: "Troubleshooting an AD replication issue"
categories:
  - "Active Directory"
  - "Monitoring"
  
tags:
  - "Active Directory"
  - "Microsoft"

# Theme-Defined params
comments: false # Enable Disqus comments for specific page
---

A few months back, I came across a percular issue in my production network. We had 3 datacenters with WAN links between them. Each datacenter had a Domain Controller located on site with one data center having two (A regular and a RODC). The senior tech reported that there was an intermittent replication issue that was found only after one of the DC's failed, of course, not long before my arrival. They had it on the overall plan to setup some form of monitoring, but my predecessor had never gotten around to it. So, like many other projects, it was added to my technical debt list...
When I was ready to tackle the issue, shortly after promoting the RODC, I decided to use a powershell script to monitor replictaiton between DC's and shoot off an email or alert if something was amiss. This worked great! A few minor tweeks and we were rocking and rolling. After about a week of monitoring, we noticed that there were almost regular interruptions everynight. At the time, our networking team was doing some large overhaul projects incluing re-aligning the WAN so we chaulked it up to maintanance. It wasn't until a few months had gone by and networking had completed their work that we noticed the alerts were still firing. I started investigating and during the intial troubleshooting, the issue never seemed to happen during the daytime. Only at night or during off hours when I wasn't online. I casually ignore it until one day, the primary DC is affected and isn't replicating *AT ALL* between the other DC's. We find this out when we hire a new tech and create his account only to find it's not working in half the domain. So, back to troubleshooting. This time, the issue (Error 1722, RPC communication error) is almost consistently happening. I begin all basic troubleshooting and some advanced. Splunk doesn't show anything being blocked. I see the traffic hitting the Windows host firewall about every 3rd try. My immediate thought, besides firewall, was DNS. We tear through the DNS records and sure enough find that the DC's DNS is all jacked up with weird IPv6 addresses that are completely out of scope for our network. 


---------



