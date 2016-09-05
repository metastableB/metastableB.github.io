---
layout: post
title:  "Bluez, GATT and BLE Devices"
date:   2016-08-31 16:24:33 +0530
custom_css: style.css
categories: Exploring-BLE
---
As part of an internship I did in the summer of 2016, at the Center for Smart Systems, SUTD Singapore, I had to get my hands dirty with a lot of BLE stuff. For someone with no knowledge of how the bluetooth stack works, this was like [insert doing something challengeing]. This here, is my attempt to document what I have learned at SUTD. 

{: style="color:red; font-size: 80%;"}
\begin{face-saving-rant}

I do not, even in my wildest dreams, claim that what ever I'm going to write down is right nor do I claim this is going to help anyone. These are unvarified and my own conclusion. This is for myself and only myself - in the highly likely event that I forget how I got everything working! Trod at your own risk!

{: style="color:red; font-size: 80%;"}
\end{face-saving-rant}

With that out of the way, let me outline how this series of blog articles (thats right, series!) are structured. 


- Introduction [this article] and some background on GATT and BLE
- Setting up BLE devices on Linux
- `gatttool` interactive mode with BLE device MIO Global Fuse
- `gatttool` non interactive mode with BLE device : MIO GLobal FUse
- How to use `gatttool` and BLE devices : Sensor Tag

Before we begin, I do not have any of the GATT devices at my desposal anymore and I cannot verify anything or debug anything. sorry!

## Background on BLE/GATT

