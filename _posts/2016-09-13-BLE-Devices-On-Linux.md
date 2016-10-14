---
layout: post
title:  "BLE Devices On Linux - Brief Intro"
date:   2016-08-31 16:24:33 +0530
custom_css: style.css
categories: Exploring-BLE
tags: Exploring-BLE, BLE-On-Linux, GATT, BlueZ
excerpt: This article covers basics of setting up a BLE development environment on a specific Linux machine.
---

This article is part of a series of blog posts I am doing, on working with BLE devices. This particular article is the second in the series. You can find the first article, which gives a brief overview of the required theory, [here](/exploring-ble/2016/08/31/Introduction-Bluez-GATT-and-BLE-Devices.html). The overall structure of the blog posts is as follows:


{:style="background-color:#fff6f6;
    padding-top:15px;
    padding-bottom:15px;
    margin:0;
    padding-left:10px;
    color:#000000;
    font-size:140%"}
Structure

{:style="background-color:#fff6f6; 
    font-size:100%;
    padding-top:1px;
    padding-bottom:15px;
    margin:0;"}
1. [Introduction: Bluez, GATT and BLE Devices](/exploring-ble/2016/08/31/Introduction-Bluez-GATT-and-BLE-Devices.html)
2. [BLE devices on Linux](/exploring-ble/2016/08/31/BLE-Devices-On-Linux.html)
3. `gatttool` interactive mode with MIO Global Fuse
4. `gatttool` non interactive mode with MIO GLobal FUse
5. How to use `gatttool` and BLE devices : Sensor Tag


For the purpose of this article, I am going to assume a fundamental understanding of the theory involved. I am also going to assume a familiarity with the Linux environment.

# Setting Up

I am going to walk through the process of setting up your BLE environment on Linux.

## Installing BlueZ
For the Linux system to interact with BLE, we need to install the BlueZ library. The distribution specific installation instructions can be found in the [BlueZ website](http://www.bluez.org/download/). Make sure the version of BlueZ you have installed, or had previously installed has BLE support.

There are various ways of checking the BlueZ version. On debian based systems you can use `dpkg` to print out BlueZ version. Try something like:

    dpkg --status bluez | grep '^Version:'

Alternatively, bluez provides a shared library libbluetooth.so located in `/usr/lib64/`. You can look for the version in the symlink here. Specifically, use

    ls -al /usr/lib64/libbluetooth.so 


![IMG](/img/blog/bluez-version.png){: .blog-post-center-image .blog-post-div-shadow } 

My version of BlueZ is 3.18. Make sure your version of BlueZ has BLE support.

## Making Sure Bluetooth Module is UP and Running
Next we need to make sure that the **hci device** (our bluetooth dongle) is up. Look at the output of `hciconfig` to get a list of available bluetooth devices as well as their current status.
    
    $ hciconfig 

![hciconfig-down](/img/blog/hciconfig-down.png){: .blog-post-center-image .blog-post-div-shadow } 

As you can see, I have a single device indicated by `hci0` with mac address 00:1A:7D:DA:71:13. There could be multiple devices listed as well, usually as `hci0`, `hci1` and so forth. The third line of the output indicates that my **hci device** is currently down. Run the following to bring it back up (you might need to use `sudo` or use appropriate means to elevate your privileges). Don't forget to replace `hci0` with your device name.

    $ hciconfig hci0 up

![hciconfig-up](/img/blog/hciconfig-up.png){: .blog-post-center-image .blog-post-div-shadow } 

Your device should be up now as indicated by `UP` in the third line.

If you are experience problems with the device itself, a good place to look for error reports is `dmesg`. Try disabling the device (remove it, if its a USB device) and run `tail dmesg`. Now plug it back in and run `tail dmesg`. You should see some new messages related to the plugged in device. Check to see if you can find any errors here. The fixes for any errors are certainly device specific and googling the error messages is your best bet of solving them! 

Also, though extremely unlikely, it is possible that `hciconfig` is unable to enable your bluetooth device due to a software block by `rfkill`. If this is reported, you will have to use `rfkill` to unblock your device. Do,

    $ rfkill list

to view a list of RF devices on your system. Your bluetooth device should be listed here corresponding to some index - 

    3: hci0: Bluetooth
    Soft blocked: yes
    Hard blocked: no

Here you can see, my hci device `hci0`, listed against 3 has a software block. You can unblock it using 

    $ rfkill unblock 3

Make sure you replace `3` with the correct index of your device. 


## Checking for BLE Support

The next thing to do before proceeding is making sure that your Bluetooth adapter/module, be it an in build Bluetooth module or a USB dongle, supports Bluetooth 4.0 (BLE). Unfortunately, there is no one-fits-all way of doing this and you can try the following.

If you have the latest [BlueZ](http://www.bluez.org/download/), or any BlueZ version with BLE support for that matter, installed, you can use the `hciconfig` command to check support for BLE. Type,

    hciconfig hci0 lestates

and see if LE States are supported. You can get more information on this [SO link](http://stackoverflow.com/questions/36741207/how-can-i-check-if-my-bluetooth-module-supports-ble-capability-in-linux).

The easiest way to accomplish this though, is to to look into the device specifications and see if you have a bluetooth BLE supported device. Alternatively, a simple google seach with the device manufacturers name and model name should give you the required iformation.

## Testing Our BLE Module

We are now ready to talk to Bluetooth devices in our vicinity. For testing purposes, first we will try to talk to a non BLE device. Fire up the Bluetooth on your mobile phone or any other device you may have an run the following command on your terminal,

    $ hcitool scan

Note that this scan does not report BLE devices. The output you will see will only be from regular Bluetooth devices. 

![hcitool-scan](/img/blog/hcitool-scan.png){: .blog-post-center-image .blog-post-div-shadow } 

Now to scan for BLE devices, we use the `lescan` option. Run,

    $ hcitool lescan 

to scan for BLE devices. Make sure your BLE devices are turned on.
![hcitool-lescan](/img/blog/hcitool-lescan.png){: .blog-post-center-image .blog-post-div-shadow } 

You can ignore the unknown devices in my output. We now have a working environment for BLE development on Linux. Next lets try talking to some BLE devices. In the [next]() article, I am going to use my BLE setup to talk to a [Mio Fuse](http://www.mioglobal.com/en-us/Mio-FUSE-Heart-Rate-Training-Activity-Tracker/Product.aspx) health band I have. I am going to use the `gatttool` to work with Mio Global Fuse in interactive and non-interactive mode.

This article was about setting up the tools required for BLE development under Linux. 
