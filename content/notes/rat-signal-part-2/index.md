---
title: "Rat Signal Part 2: The Broker"
date: 2020-08-02T10:17:44-05:00
slug: "rat-signal-part-2"
draft: false
tags: ['mqtt']
project: "rat-signal"
description: ""
---

In an MQTT-based system, a central "post office" is needed to manage the flow of data. This is the job
of the *broker*. A broker manages the subscriptions of the edge devices, or clients, and pushes the right
data to the right clients whenever data is published to the right *topic*.

Topics can be organized into a number of different levels. There aren't any rules for topic
hierarchy and you are free to organize your topics however you want. It makes sense, though, to logically organize topics into a hierarchy of increasing specificity. For example, if my home was loaded with
different sensors, my topics might look like:
```javascript
home/bedrooms/guest/temp
home/bedrooms/master/lights/closet
home/garage/humidity
home/appliances/coffeemaker/status
```
I could have a client device that subscribes to the `home/bedrooms/master/lights/closet` topic to check
if the bedroom's closet lights are on. Using wildcards, I could subscribe to all of the bedroom's sensors
by subscribing to `home/bedrooms/master/#`.

## The Mosquitto Broker

[Eclipse Mosquitto](https://mosquitto.org/) is an open source and lightweight MQTT broker that is
used as the "post office" for Rat Signal. I'm only interested in certain data points for a certain surf
break so my message topics will be organized as such:

```javascript
surf/time/timestamp
surf/time/timeText
surf/height/minHeight
surf/height/maxHeight
surf/windSpeed
surf/period
surf/rating/solidRating
surf/rating/fadedRating
surf/rideable
```

> These topic names correspond with data provided by Magic Seaweed. More on that in the next post.

Mosquitto is deployed to my home server using [Docker](https://hub.docker.com/_/eclipse-mosquitto). More
on deployment in a later post.

---

# The Rat Signal Series:

- [Rat Signal Part 1: Overview](/notes/rat-signal-part-1)
- *[Rat Signal Part 2: The Broker](/notes/rat-signal-part-2)*
- [Rat Signal Part 3: The Endpoint](/notes/rat-signal-part-3)
- [Rat Signal Part 4: The Middleman](/notes/rat-signal-part-4)
- [Rat Signal Part 5: Deployment](/notes/rat-signal-part-5)
- [Rat Signal Part 6: Red Alert](/notes/rat-signal-part-6)
- Rat Signal Part 7: Numbers are Nice (TBA)
- Rat Signal Part 8: The OK Wave off Kanagawa (TBA)
