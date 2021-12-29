---
title: "xMas 2021"
date: 2021-12-29T10:32:00-06:00
slug: "xmas-2021"
draft: false
tags: ["hardware", "firmware", "mcu", "pcb"]
project: "xmas-2021"

inProgress: false
started: 2021-11-01
completed: 2021-12-25

description: "Christmas Mix 2021"
---

My Christmas ornaments the last few years have been pretty basic
and this year I wanted to do something a little different.
Earlier this year I learned about reverse-mount LEDs through a [very cool article on Hackaday](https://hackaday.io/page/6081-using-side-view-leds-in-place-of-reverse-mount-leds)
and I knew I wanted to use them for *something*.
I had also been thinking about making an ornament that played music and naturally this was the first thing that came to mind.

These ornaments play snippets of five different Christmas songs. There's a button on the backside the cycles through these songs.

![final][final]

I designed this PCB to match the X and Y dimensions of a real cassette tape (100mm x 63mm).
This worked out perfectly because my [PCB manufacturer](https://www.elecrow.com/pcb-manufacturing.html) 
offers a great deal for small boards up to 100mm x 100mm (10 boards for $10!).

I waited until the last minute to design the boards
and I wasn't able to prototype the circuit on a breadboard before sending off the gerbers for manufacturing.
This made me uncertain about a few things in the circuit - namely the piezo buzzer -
so I included a few contingency plans in the design. More on that in a bit.

This was my first time ever using a piezo buzzer and I had no idea what I needed to order.
I discoverd that all of the buzzers in the parts catalog specify their operating frequency.
Some of the buzzers were listed as "internally driven" - that is, you supply the piezo with a DC voltage
and the buzzer vibrates at the specified frequency.
Other buzzers were listed as "externally driven" and require you to design your own drive circuitry.
This was a big source of confusion - it makes sense to specify the frequency of the internally driven
buzzers but what about the externally driven kind?
From what I gathered, these buzzers are designed to work *best* at the specified frequency.
Some of the datasheets plotted the buzzer's frequency response and this (sort of) assured me that 
it would work for what I needed it to do.

The piezo buzzer in this design is driven by a pulse-width modulation signal pumped out by a microcontroller.
A PWM signal at 50% duty cycle is just a square wave and a square wave gives you that classic, cheesy, 8-bit sound.
Each song is just an array of values that correspond to the frequencies of musical notes - 
the note A, for example, is just a 440Hz wave.
The firmware on the microcontroller loops through the array (5 notes per second) and sets the PWM frequency to that value.

The piezo buzzer is actually driven by *two* PWM signals in a push-pull configuration.
That is, when one signal is high, the other is low.
This was suggested by one of the datasheets to increase the output power of the buzzer.
I wasn't sure if this would work so I designed in a jumper pad (JP1) to route one side of the piezo
to ground as a contingency plan
(in hindsight I definitely could have just set the pin to ground in the firmware).
I also wasn't sure how much current the buzzer could take so I included pads for a resistor (R3)
if I determined it needed one.
It did not, so I just jumped the pad with a blob of solder.

![schematic][schematic]

I think the reverse-mount LEDs ended up looking really neat. 
These LEDs shine *into* the board instead *out from*.
I had to get all cute with the board to make this work.

A simple 2-layer PCB is basically a sandwich.
In the center you've got a non-conductive material known as the substrate.
Outside of the substrate, on both sides, 
you've got your copper layer, your soldermask layer (typically green), and then your silkscreen layer.
You are able to get several different colors on a board by exposing or stacking these different layers:

1) Exposed substrate (typically brown-ish and semi-opaque)
2) Exposed copper (typically silver or gold depending on the plating)
3) Solder mask over copper (typically dark green)
4) Solder mask *not* over copper (typically light green)
5) Silk screen (typically white)

For this project I went with black soldermask and white silkscreen.
For these LEDs, I went with option 1 here. 
Because the substrate is semi-opaque, I was able to shine the LED through the board
by making sure there was no copper or soldermask in the areas I wanted to see the LEDs glow.
For the backside of the board, I had to make sure there were no copper or soldermask
directly beneath the LED (which I failed to do for one of the LEDs and manually had to drill out the copper).
The tape semi-circles are just exposed copper plated with tin.

![pcb][pcb]

I'm pretty happy with how things turned out this year.
The source code and design files can be found in my [Github repo](https://github.com/bruthaearl/chrimbus2021).
And, if you want, you can watch the assembly process below.

{{< youtube txI6bzVMPFU >}}

[schematic]: /projects/xmas-2021/schematic.png "Schematic"
[pcb]: /projects/xmas-2021/pcb.png "PCB"
[final]: /projects/xmas-2021/final.jpg "Final Assembly"
