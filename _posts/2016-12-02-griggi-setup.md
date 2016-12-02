---
layout: post
title: How to setup Griggi on your Openwrt router
excerpt: Griggi can be installed on any Openwrt router. Follow the steps on this article to do so.
---
<p style="text-align:center">
	<i class="fa fa-wrench fa-6" style="font-size: 10em; color: #62cb31"></i>
</p>

Griggi is developed as package(s) that can be compiled & installed on any unix based operating system. Openwrt seems to be the most popular flavor of opensource router OS now a days, so we provide Griggi pre-bundled Openwrt firmware image that you can directly flash on your Openwrt router. 

> To know if your router supports Openwrt, head over to [Openwrt Table of Hardware](https://wiki.openwrt.org/toh/start) & search for your router model

So far, we do not have need to support all Openwrt routers as most of the time, we suggest an appropriate router to our users, or we get one of that, follow the instructions & ship a pre-bundled Griggi router. 


### 1. Download Griggi firmware ###

You are here mostly because you chose to install Griggi yourself on a router we suggested. 

To get Griggi pre-bundled Openwrt firmware image on your router

* head over to [app.griggi.com](http://app.griggi.com) & login with your phone number
* navigate to 'SETUP' option on the left sidebar
* choose 'Add a Router', enter details & download firmware for your newly added router
<style>
  img { border : 0.5px solid #bababa; max-width:100%; }
</style>
<img src="/images/setup/login.png" style="width:400px;">
<img src="/images/setup/setup.png" style="width:200px"><br/>
<img src="/images/setup/router-list.png" style="width:300px;">
<img src="/images/setup/new-router-form.png" style="width:300px;"><br/>
<img src="/images/setup/download-firmware.png" style="width:200px;">
<img src="/images/setup/select-router.png" style="width:400px;"><br/>

Select your router from list of supported routers. Make sure you pick the right hardware version from the sticker at the back of the router. 

<img src="/images/setup/TP-Link-1043ND-v3.jpg" style="width:400px;"><br/>

### 2. Flashing Griggi on the router ###

Power your router. Connect to it through internet cable or WiFi. Navigate to the router dashboard. Usually it is at **http://192.168.0.1** . It will show up a login page.

> If it is a new router, the login credentials are on the sticker at the back of the router

Post login, locate the **firmware upgrade** page as shown below. Upload the firmware downloaded in previous step. 

<img src="http://dslrcontroller.com/guide-wifi_mr3040/setup-flash.png" style="width: 600px"><br/> 

> Ensure that power does not go off while the firmware uploads. If it does, it might *brick* your router

Post flasing the new firmware, your router will reboot. Wait for the WiFi to come back up. There is a WiFi named **griggi** to which you can go ahead & connect.

> If you are using **TP Link 1043ND** router - the WiFi on Griggi is disabled as there is known issue with Openwrt & this particular model. In this case, you need to connect to the router through internet cable through one of the LAN ports (yellow color ports).<br/><br/>Dont connect to port 1 as it is converted to secondary WAN port for load balancing. Use one of 2 to 4 marked yellow port to connect to the router.

### 3. Configure Griggi router ###

A few configuration need to be manually done to router before you start to use Griggi. 

> If you navigate to **http://192.168.7.1** & dont see login page, we have not bundled luci in the Openwrt image as the router has too low flash space. You may ignore the rest of the configuration steps. 

#### 1. Setup admin password ####

Navigate to htttp://192.168.7.1, you will see Openwrt login page. There is no password by default. Setup a password first thing.

<img src="/images/setup/set-password.png" style="width: 600px"><br/> 

#### 2. PPPoE settings (selectively applicable) ####

If your ISP asks you to login when you connect to internet, there is PPPoE applicable in your setup. ISPs like ACT, Tikona, Beam etc have this implemented. 

Griggi does not work well with another authentication system. There is a way for PPPoE login to be done by the router itself.

* Navigate to **Network** -> **Interfaces**
* Click 'Edit' on 'WAN' interface. In 'Protocol' drop down menu select **PPPoE**, then click 'Switch Protocol'`.

<img src="/images/setup/pppoe.png" style="width: 600px"><br/>

* Enter the username & password provided by the ISP in the appropriate field & save the settings. 

> To test if your settings are working, plug in the ISP line in the WAN interface, connect to the WiFi (or through wire if you are on 1043ND), you should see griggi authentication popup. If it does not, try manually going to griggi.com on the browser & you should see the login page.<br/><br/> If none of this happen, either there is software or hardware configuration problem. Sort this out before proceeding.

### 4. Advance Settings ###

> Advance settings are applicable to only those devices that have router dashboard showing up at http://192.168.7.1

If you have come this far, congrats. You have Griggi router setup in a usable state. You will be able to limit data at user level. But there are a few other cool things that Griggi can do. 

* automatically prioritizing traffic so that bulk traffic (download, youtube, torrents) dont clog your internet bandwidth
* Connect multiple ISPs for load balancing & failover to have 100% internet uptime.

#### 1. Configure Prioritization ####

You need to be aware of your general upload & download speed of your ISP which prioritization software uses to avoid bandwidth clogging. 

If you are not sure, try running speedtest & note upload and download values. 

* Navigate to **Network** -> **SQM QoS**

<img src="/images/setup/sqm-qos-speed.png" style="width: 400px"><br/>

* Enable SQM

* Figure which interface is the WAN interface by navigating to Network -> Interfaces. It usually is eth0 or eth1. Select the WAN interface in SQM.

* Specify download & upload speed in kbps. 1 Mbps should go as 1000. Save settings. 

> To test if your prioritization is configured right, launch a terminal & run a continuous ping to google.com . Notice the delay in ping. Start a long download, youtube video or speedtest. The delay in ping should not increase significantly. Neither there should be any ping packet drops.<br/><br/>You can run the same test after disabling SQM. You should see difference in ping times.


#### 2. Configure Load Balance ####

You can plug 2 internet lines to a Griggi router. The traffic gets divided on two lines based on metric(weight) that you set on each of the wan interface. Eg. if one interface support 100 Mbps speed & the other 10 Mbps, you should put metric 10 for 100 Mbps line & 1 for 10 Mbps line. 

In case if one of the internet line goes down, all traffic starts to route through the other line. 

> In Griggi router Load Balancer is enabled by default. 
> The first LAN port (port marked 1) is the secondary internet port which we have named as WWAN.
> All you need to do is follow below steps to assign metrics to WAN and WWAN.

* Setup interface metric by navigating to **Networks** -> **Interfaces** -> WAN/WWAN -> Advanced Settings . Set **Use gateway metric** for the appropriate metric value for the interface.

<img src="/images/setup/set-metric.png" style="width: 400px"><br/>

> To test if load balance is configured right, plug internet lines in both WAN & WWAN ports. 

* Navigate to **Network** -> **Load Balance**

> The Load Balance on router dashboard should show both interface green. 

<img src="/images/setup/mwan-status.png" style="width: 400px"><br/>

