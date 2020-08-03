---
title: "Rat Signal Part 6: Red Alert"
date: 2020-08-03T14:42:04-05:00
slug: "rat-signal-part-6"
draft: false
tags: ["mqtt"]
project: "rat-signal"
description: ""
---

Now that the server software is complete, how about a little recap?

- Rat Signal is a collection of microservices that inject surf report data into my smart home
- The Broker service is the "post office" of the application
- The Endpoint service grabs data from Magic Seaweed every hour and sends it to the post office
- The Middleman service links Rat Signal to other external services
- Docker is used to coordinate the deployment of the Rat Signal services

Now that we've got the surf forecast data and it's easily accessible throughout the home, 
it's time to actually use it. As discussed in the first part of this series, I plan on having (at least) three devices consume the forecast data. 

This first device is simply a smart bulb that turns red whenever the surf is going off. Red Alert, all
hands on deck, you know the drill. I'm using a [Philips smart bulb](https://www.homedepot.com/p/Philips-Color-and-Tunable-White-A19-LED-60W-Equivalent-Dimmable-Smart-Wi-Fi-Wiz-Connected-Wireless-Light-Bulb-555607/310289028) - these things are great and can get nice and cozy without feeling like artificial lighting.
I'm a "hot wire running through glass" kind of guy and much prefer warm incandescent over LED.

![red-alert][red-alert]

[red-alert]: /notes/rat-signal-part-6/redalert.jpg "Red Alert"

I set up [If This Then That](https://ifttt.com/) to listen for a webhook that gets triggered by
the Middleman service. As mentioned in a previous post, this webhook is triggered whenever the forecast calls for rideable surf. Here's how the signal propagates down the chain:

```
1) Data originates on Magic Seaweed
2) Endpoint requests the data
3) Endpoint determines if surf is rideable and publishes state to the surf/rideable topic
4) Broker relays state to the Middleman
5) If surf is rideable, Middleman triggers a webhook
6) If This Then That listens for the webhook and turns on lamp whenever it is triggered
```

This device, "Red Alert", is just an off-the-shelf light bulb. The next two devices will be custom software
running on custom hardware and will display more meaningful data than just an indicator. Stay tuned.

---

# The Rat Signal Series:

- [Rat Signal Part 1: Overview](/notes/rat-signal-part-1)
- [Rat Signal Part 2: The Broker](/notes/rat-signal-part-2)
- [Rat Signal Part 3: The Endpoint](/notes/rat-signal-part-3)
- [Rat Signal Part 4: The Middleman](/notes/rat-signal-part-4)
- [Rat Signal Part 5: Deployment](/notes/rat-signal-part-5)
- *[Rat Signal Part 6: Red Alert](/notes/rat-signal-part-6)*
- Rat Signal Part 7: Numbers are Nice (TBA)
- Rat Signal Part 8: The OK Wave off Kanagawa (TBA)