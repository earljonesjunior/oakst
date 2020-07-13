---
title: "MIDIoT"
date: 2019-02-25
slug: "midiot"
draft: false
tags: ["hardware", "firmware", "iot"]
project: "midiot"
completed: 2019-03-01
started: 2019-02-01

description: "I wanted a way to play my cheap Casio keyboard over the internet. "
---

I wrote a webapp and built a device that allows me (or anyone else) to play a MIDI keyboard over the internet using the magic of IoT. MIDI + IoT = MIDIoT. Pretty clever, right? 

![display][display]

The app and the device connect to each other using the very cool [Losant IoT platform](https://www.losant.com/iot-platform). The platform workflow validates the note played by the web app and then sends that note to the device. 

![flow][flow]

I cheated and used an Adafruit Huzzah Feather M0 as a development platform. Arduino libraries already exist for WiFi, MIDI, and connecting to the Losant platform so why reinvent the wheel for a one-off, you know? I did design a little PCB that fits over the Feather for I/O and LED connectivity so as to feel like an engineer. 

![guts][guts]

The webapp is no-longer up and running but the source code, board gerbers, and etc can be found on my [Github](https:\/\/github.com\/bruthaearl\/midiot).

[display]: /projects/midiot/display.jpg "Final product"
[guts]: /projects/midiot/guts.jpg "Gut shot"
[flow]: /projects/midiot/flow.png "Losant Flow"