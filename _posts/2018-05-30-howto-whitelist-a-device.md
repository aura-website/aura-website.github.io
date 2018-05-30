---
layout: post
title: How to whitelist a device
excerpt: In somes cases as admin you might like to bypass some of the devices from login authentication and data limit. Follow the below steps to achieve that.
---

**1. Connect to Griggi network**

You need to be in the premise for this task. Connect to the network via WiFi or cable.

**2. SSH to the router**
1. If you are using Linux, use the terminal command: ssh root@192.168.7.1
2. If you are using Windows, use Putty. Select protocol as ssh or ensure port is 22.

**3. Add the mac address to be whitelisted**
1.  Open the config file using command: vi /etc/wifidog.conf
2.  Scroll down until you find a section TrustedMACList.
3.  Add the mac address following the example given under the section. 
4.  If any mistake in editing the file, quit by pressing escape key and :q! and re-open the file.

**3. Restart the router**
1.  Your device is whitelisted now. 
