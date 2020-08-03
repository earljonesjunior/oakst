---
title: "Rat Signal"
date: 2020-08-03T18:45:12-05:00
slug: "rat-signal"
draft: false
tags: ['mqtt', 'iot', 'node', 'node-red', 'docker']
project: "rat-signal"

inProgress: true
completed: 
started: 2020-08-01
description: "Surf report data injected into my smart home."
---

Earlier this year, I decided to finally do the one thing every young man dreams about: 
I built my own home sever. I didn't have any real plans for the server when I built it but it has done
a pretty good job taking up space and collecting dust. 

I finally came up with a project for the server's christening. I call it the *Rat Signal*.

## MQTT and the Smart Home

[MQTT](https://en.wikipedia.org/wiki/MQTT) is a networking protocol used extensively in the world of the Internet of Things. Devices with limited available power and bandwidth use MQTT to efficiently 
communicate back home and to other fellow resource-constrained devices.

Here's the gist of how it all works. Data is organized by *topic*. Devices can *publish* data to a topic,
 *subscribe* to a topic to receive published data, or both. 
 An MQTT server, also called a *broker*, is responsible for managing and maintaining publications and subscriptions.

MQTT fits well into the smart home ecosystem. Sensors like thermometers and motion detectors and publish data to an MQTT broker while devices like indicators and alarms subscribe to it.

## The Rat Signal

I wanted a way to bring surf report data into my smart home. If the conditions are good and there is some rideable surf, I wanted to be alerted. I also wanted a way to make the report data a visual part of my home.

Using MQTT, Rat Signal pushes forecast data from [Magic Seaweed](https://magicseaweed.com/Okaloosa-Pier-Surf-Report/954/) to different devices around my connected home. Magic Seaweed is my favorite
forecasting website and I've been checking their report since high school.

Rat Signal is a modern application following the paradigms of microservice architecture. All this means
is tha Rat Signal is composed of several different independent pieces with each piece performing a specific
job. From my professional experience I've found this method of development much more enjoyable than managing
one major monolithic application.

Rat Signal can be broken apart into the back-end and front-end. On the back-end, the Rat Signal server runs
an MQTT broker, a data endpoint, and a "middleman". The front-end is composed of three different different
client devices.

The first device is an off-the-shelf smart bulb that lights up whenever there's some rideable surf. 
The second device will display the numerical forecast data. I will be designing the device's hardware and software.
The third device is more of a wildcard proof of concept. I haven't fleshed out all of the details just yet but rest assured I WILL be appropriating traditional Japanese art.

![flow][flow]

[flow]: /notes/rat-signal-part-1/flow.png "Rat Signal system overview"

---

# The Rat Signal Series:

- [Rat Signal Part 1: Overview](/notes/rat-signal-part-1)
- [Rat Signal Part 2: The Broker](/notes/rat-signal-part-2)
- [Rat Signal Part 3: The Endpoint](/notes/rat-signal-part-3)
- [Rat Signal Part 4: The Middleman](/notes/rat-signal-part-4)
- [Rat Signal Part 5: Deployment](/notes/rat-signal-part-5)
- [Rat Signal Part 6: Red Alert](/notes/rat-signal-part-6)
- Rat Signal Part 7: Numbers are Nice (TBA)
- Rat Signal Part 8: The OK Wave off Kanagawa (TBA)



 