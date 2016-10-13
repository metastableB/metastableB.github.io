---
layout: post
title:  "Introduction: Bluez, GATT and BLE Devices"
date:   2016-08-31 16:24:33 +0530
custom_css: style.css
categories: Exploring-BLE
excerpt: The first of a series of articles on working with BLE devices under Linux. This article includes a very brief overview of GATT Profile and BLE.
---

As part of an internship I did in the summer of 2016, at the Center for Smart Systems, SUTD Singapore, I had to get my hands dirty with a lot of BLE devices. For someone with no knowledge of how the Bluetooth stack works, this was like [insert doing something challenging]. This here, is my attempt to document what I have learned at SUTD.

{: style="color:red; font-size: 80%;"}
\begin{face-saving-rant}

I do not, even in my wildest dreams, claim that whatever I'm going to write down is correct nor do I claim this is going to help anyone. These are unverified and my own conclusion. This is for myself and only myself - in the highly likely event that I forget how I got everything working! Trod at your own risk!

{: style="color:red; font-size: 80%;"}
\end{face-saving-rant}

With that out of the way, let me outline how this series of blog articles (thats right, series!) are structured. 



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


## Background on BLE/GATT


We need a little fundamental understanding of the BLE/GATT profile before we begin. This is particularly useful if you have to work with undocumented devices or when debugging device information.

A little extra reading hurts no one, right ?

Let's start off by reading up on [General Attribute Profile](https://www.bluetooth.com/specifications/adopted-specifications), [services](https://developer.bluetooth.org/gatt/services/Pages/ServicesHome.aspx), [characteristics](https://developer.bluetooth.org/gatt/characteristics/Pages/CharacteristicsHome.aspx) and [characteristics configuration](https://developer.bluetooth.org/gatt/descriptors/Pages/DescriptorViewer.aspx?u=org.bluetooth.descriptor.gatt.client_characteristic_configuration.xml). You don't really need to dig deep into these and you probably can get away with a basic understanding. I for one, read just about enough for me to understand what these are and that has been enough for me.

For the lazy ones, *characteristics*, as evident form the naming, are defined attribute types that contain a single logical value. For example, the battery level, heart rate or say age. These are defined and adopted by the Bluetooth Special Interests Group, the guys currently responsible for designing and marketing the BLE standard. [Here](https://developer.bluetooth.org/gatt/characteristics/Pages/CharacteristicsHome.aspx) you can find a list of all adopted characteristics. Each characteristic has a unique ID (assigned number) assigned to it which we will use later to interact with the BLE device. Further, each characteristic has a certain set of configuration options.

For example, consider the [navigation (0x2a58)](https://developer.bluetooth.org/gatt/characteristics/Pages/CharacteristicViewer.aspx?u=org.bluetooth.characteristic.navigation.xml) characteristic specification. It lists a row whose name is _flags_. It is stated as a mandatory 16 bit field, meaning that any device that supports this characteristic will have this field. Further, the additional information section has information regarding what individual bit values mean. Later, we will see how we can modify these bits.

**TODO: Add screen shots**

A group of characteristics that share a logical relation form a service. For example the characteristics that provide information about a device forms the [device information service](https://developer.bluetooth.org/gatt/services/Pages/ServiceViewer.aspx?u=org.bluetooth.service.device_information.xml). As you can see, the device information service has characteristics like `manufacture_name_string`, `nodel_number_string` etc. Note that, like characteristics, services are also assigned a UUID, which we will later use to identify the characteristic on the devices.

Remember that the listed characteristics and services are not exhaustive and you will certainly find other UUIDs not mapped to any characteristic or service. These are manufacture specif services/characteristics.

Was that a little too much raw info? Let me try to restate it in a plane and simple manner. Each BLE device can perform a particular set of tasks(measurements), like measuring temperature or measuring heart rate and so forth. We can ask the BLE device for a list of measurements it supports (how we do this will be discussed later). The BLE device will respond back with some set of logically grouped functions. Logically grouped in the sense that, measuring temperature, pressure and humidity are features which can be grouped as 'environmental measurements', forming a logical collection. Similarly, number of steps, pace, heart rate and calories burned can be grouped as 'fitness/health measurements'. These groupings are called services and we can query (at least in theory) a BLE device to know what services it provides. 

The individual measurements, like the heart rate measurement in the 'health/fitness service' are called characteristics in GATT terminology. I hope this gives a better picture of what characteristic and services are. How we use and interact with them, or a BLE device in general will be taken up (with examples) in other articles.

---
*Note that the examples for characteristics and services used here are not from the adopted standard. They were constructed to convey a conceptual meaning and their usefulness and validity exists to satisfy this cause only.*
