---
layout: post
title: How to Setup Access Points to extend WiFi range
excerpt: Access Point setup is needed to cover a space that can not be entirely covered by one WiFi device alone. This blog talks about setting up Access Points with a Griggi router but steps are generic enough & will work with any router.
---

Access Points (also referred as AP) are devices that are hooked to the main router (through wire or wirelessly) & solely work to either extend WiFi network or support more number of devices than a single router/access point can support. 

AP are also called **wireless or wired bridge** in more technical terms. 

Unlike office or large corporates or WiFi enabled institutes that have an extensive AP deployment, it can be done for small office or even your home. Typical usecase is your main router being set in a hall/room whose WiFi does not reach to yourroom that is upstairs. You could setup an AP somewhere between, hook it up wirelessly (or with wire) to the main router & eventually this AP will extend the WiFi for your rooms that were earlier could not be reached by your router.

APs are dumb device that do only WiFi extending. All other network related things like IP assignment, traffic handling, NAT etc is done by the router itself. Some APs do provide a way to limit bandwidth at device level etc in enterprise grade APs, but guess that is something your network consultant will be able to guide you through. 

### Types of APs

1. **Enterprise Grade** - these are devices explicitly sold as Access Point. [Ubiquiti UnFi series](https://www.ubnt.com/unifi/unifi-ap/) is one good range of AP that starts at around $100 . One AP is able to take around 100 devices. 

2. **Router converted to AP** - you could convert any (home) router to work as AP. The advantage of AP configuration is, you could wirelessly extend the WiFi network which you will not be in the default state. 

### AP setup 

The AP needs connection to main router through a ethernet wire or without it (wireless). The wired setup has **advantage of high speed**. If you have an internet speed > 50-60 Mbps, you should go for wired setup as the traffic between router & AP will get throttled to that value because of the speed limit in 2.4 Ghz WiFi channel. 

The wireless setup has advantage that there is no issue of laying cable connecting the router to the AP. Although, you need to ensure that an AP has WiFi reach from at least another AP which in-turn has access to the router (directly or through another AP). Else, you might have a set of APs that might be able to interact with each other but not able to send/receive traffic from the router. 

Each AP can have its own WiFi SSID & password. Usually, its advised to keep the same SSID & password across APs as it will lead to less confusion for your users to decide which WiFi to connect to. 

**1. Wired AP setup (wired bridge)**

> The wired AP should be connected to main router **through one of the LAN port of the main router**

If you have Enterprise grade APs, you are set here. They usually come with one port & you need to just jack in the ethernet cable into it. There could be multiple ports too in which you need to figure out the right one. 

If you have a consumer grade router that you want to convert & use as wired AP, you first need to **disable DHCP** (which in turn will disable NAT also) on the router. You should be able to do the same by logging into router admin dashboard. 

> Once you have disabled DHCP on your router, you will not get IP once you connect to the router WiFi. Hence there is no way to reach admin dashboard except to reset your router. Most routers have a reset button or pin hole to do so.  

You have to connect ethernet wire into **one of the LAN ports** & not WAN port of the newly turned router turned AP.

Note that you could still use router without DHCP disabling, in which case, the wire from the router to the AP is plugged into the WAN port. In regular setup, this would just work fine. 

> With Griggi on the main router, the setup where DHCP isnt disabled & wire plugged into the WAN port is going to hurt you. Reason being, for the data accounting at user level to work, the mac address of the connecting device should reach Griggi router. In this setup, the Griggi router sees the mac address of the AP router & hence whoever logs in first time on the AP WiFi, all traffic on this AP router will start to get reflected on that person account. 

**Note** - to **test wired bridging** with Griggi router, you should connect to the AP WiFi with a device *that has already been connected to Griggi WiFi before*. If you still see a login page, that means there is a configuration issue. To double check, look at the mac address in the login page url. The mac address should be of the AP. 

**2. Wireless AP setup (wireless bridge)**

This mode is usually discouraged in enterprise AP setup. If you investing into enterprise AP, the assumption is, you are aiming at providing high speed internet & as explained above, hooking up AP to the main router through WiFi is going to throttle internet speed. 

Usually this setup is preferred with the cheap home router turned AP, typically because you are interested in extending WiFi without the hassle of wiring & not worrying much about speed. 

Most of the home routers come with **WDS client setup**. WDS stands for wireless distribution system. Your main router to which you are going to hookup the AP should have WDS support turned on. This is a one step process & preferred process to enable wireless bridging. Log into your router dashboard & you should see this option under 'Wireless' section . You should see an option to scan available WiFi. Select your main router WiFi among the scan result (you would need to provide the main WiFi password) & once you have done this, you are set.

<img src="/images/setup/wds-bridging.png" style="width:800px"/><br/>

> Its a little tricky to test wireless bridging as both main router & the AP has the same WiFi name. You can either install the 'Wifi Analyzer' android app. Open the app & select 'channel graph' to see the graph of all WiFi SSIDs. Your WiFi should show up twice on the graph.  

<img src="/images/setup/wifi-analyzer.jpg" style="width:200px"/><br/>

The other way you could test is to go to a location where you had earlier not received the WiFi signal from the main router but can potentially get the WiFi from the AP. 


### Running out of ports in main router ?

If you have done wired setup, you would soon realize that if you are connecting APs to the main router, you could only connect APs as many as LAN port available on your main router. 

To connect more APs, you could try one of the setup

1. You can connect a new AP to one of the LAN port of existing AP. You do not need to draw wire from the router all the time.

2. You could get a switch. They cost as less as $5 in the market. You could connect one port of the switch from the LAN port of the router & rest of the ports to one AP each. 

> You need to note that you can not have wires > 100m in length between router & AP or between 2 APs, as it would lead to heavy signal loss. You should either get an AP or a switch within less than 100m hop to counter this problem.
