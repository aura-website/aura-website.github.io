---
layout: post
title: How to revert back to original/stock firmware from Openwrt, DD-Wrt, Gargoyle, Tomtato 
excerpt: A lot of small routers support Openwrt but there might be some quirks with some models out there. In that case, you might like to revert back to original/stock firmware. This blog post talks about steps needed for that.
---

This blog post is for those who installed an Opensource router OS like Openwrt, DD-wrt, Gargoyle or Tomato on your router, but now looking to revert the OS back to the original/stock firmware which came by default on the router. 

Ideally, the process should involve downloading the stock firmware & flashing it on your router. The flashing part should not be alien to you as you would have already done it earlier to get the opensource OS. Most of the router manufacturer websites list the routers & their corresponding firmware for download.

The tricky bit is - the stock firmware comes with bootloader + firmware(OS) combined in a single file. The way the 'upgrade section' in the stock firmware works is to take this combined firmware image & over-write both bootloader as well as OS part. 

The way most of the Opensource router upgrade process works is - they only expect the OS part to be uploaded & over-write only the OS section. The bootloader part is largely the same for any OS, so they dont fiddle with the part. 

Hence to flash stock OS on top of a working opensource OS router, you need to get hold of a bootloader stripped stock firmware image. Or, you need to find a way to strip the bootloader part from the firmware image. 

If you are one of the Griggi users, you would have a TP Link router. [http://www.friedzombie.com/tplink-stripped-firmware/](http://www.friedzombie.com/tplink-stripped-firmware/) is a good compilation of stripped down images for TP Link routers. It does not list TP Link 1043ND **v3** firmware image which you can get [here](/images/setup/tplink-1043nd-v3-stripped.bin). 

**1. Reverting a working router**

If you have a working router, you can simply go to the system update location on your router dashboard, upload the stripped firmware & flash it. On router reboot post flashing, you have the stock OS back with you. 

**2. Reverting a bricked router**

If you have a router that had got *bricked* while setting up one of the opensource OS, the process is going to be a little tricky. 
 
Different router manufacturers have different ways to tackle the 'unbricking' problem. Most probably, you might be having a TP Link router. TP Link routers have a way to recover using tftp. Below are the steps

1. Get a tftp server software on your PC. You would need a PC with ethernet port as well which will be used to connect the bricked router for it to interact with tftp server. 

2. Download the appropriate bootloader stripped down firmware image for your router model & hardware version. Rename it as *&lt;model name&gt;_&lt;hw version&gt;_tp_recovery.bin* . Eg. for TP Link 1043ND v3 router, the name will be *wr1043nd_v3_tp_recovery.bin*.

3. Launch tftp server. In the UI, you will be able to see the default location from where the router is going to pick up the recovery file. Copy the renamed file in the said area. The ethernet interface ip on your PC should be hardcoded to **192.168.0.66** . Once you set this ip, you should be able to confirm the same on the tftp server UI. If it isnt reflecting, restart tftp server. This is the ip where the router looks for tftp server in recovery mode & tries to download the recovery file. 

4. Connect **LAN port** of the router & ethernet port of your PC through a cable. Hold the reset button (or reset pin hole), switch on the router while holding it & leave once you see the 'lock' led on the TP Link router start to blink. You need to wait till router reboots. Should take 30 sec to 1 minute max. Post reboot, your router will have the stock OS.  
