---
title: "Rat Signal Part 4: The Middleman"
date: 2020-08-02T12:42:24-05:00
slug: "rat-signal-part-4"
draft: false
tags: ['node-red', 'mqtt']
project: "rat-signal"
description: ""
---

I'm a sucker for smart home stuff. I've got several different smart lights, smart plugs, Chromecasts, 
and a Google Home Mini in every room. If I'm cooking dinner and my hands are dirty, it's so damn handy
being able to ask my Home "how many grams in a teaspoon of baking soda?". If I'm already upstairs in bed, 
I can turn off my downstairs lights using my phone.

While some of these devices may use MQTT internally, these messages are aren't explicitly accessible. These
devices all use their own proprietary data formats and methods and a central service is needed to link 
everything together.

I use Google Home to tie together all of my smart devices. One thing Google Home lacks, however, is
conditional-based events. This is where [If This Then That](https://ifttt.com/) comes in. Using IfTTT,
my smart watch monitors my sleep cycles and tells my bedside lamp to gradually brighten when it's most 
optimal for me to wake up. How cool is that?

Rat Signal needs a way to talk to my off-the-shelf smart devices that don't use MQTT. A middleman is needed
to talk to IfTTT to then relay the information to these devices. 

## Node-RED

[Node-RED](https://en.wikipedia.org/wiki/Node-RED) is a visual flow-based programming tool used to wire together different messaging services and APIs. A Node-RED instance runs on my home server and subscribes
to the `surf/rideable` topic. If rideable, Node-RED makes a call to a IfTTT webhook which then be linked
to devices like my smart bulbs and outlets. This is what the Node-RED flow looks like:

![flow][flow]

[flow]: /notes/rat-signal-part-4/flow.png "Node-RED flow"

So why not just make the call to the webhook from the Endpoint? Why is Node-RED even needed? 

Well, Node-RED really isn't needed at all. I wanted to stick to the microservice paradigm of separation of 
duty. The Endpoint is only used to fetch and publish data. The Middleman is only used to relay that data.
As long as the inputs and outputs are well-defined, maintaining these separate pieces is often easier than maintaining one monolithic application.

The Middleman is deployed using Docker. More on that in the next post.

---

# The Rat Signal Series:

- [Rat Signal Part 1: Overview](/notes/rat-signal-part-1)
- [Rat Signal Part 2: The Broker](/notes/rat-signal-part-2)
- [Rat Signal Part 3: The Endpoint](/notes/rat-signal-part-3)
- *[Rat Signal Part 4: The Middleman](/notes/rat-signal-part-4)*
- [Rat Signal Part 5: Deployment](/notes/rat-signal-part-5)
- [Rat Signal Part 6: Red Alert](/notes/rat-signal-part-6)
- Rat Signal Part 7: Numbers are Nice (TBA)
- Rat Signal Part 8: The OK Wave off Kanagawa (TBA)

