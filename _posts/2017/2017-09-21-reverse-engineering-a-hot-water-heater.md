---
layout: post
title: Reverse Engineering a Hot Water Heater
date: '2017-09-21 22:25'
comments: true
categories:
  - reverse engineering
  - rinnai
  - hot water
  - automation
  - MQTT
---

I have a [Rinnai Infinity gas hot water heater](https://rinnai.co.nz/water-heating-gas-hot-water-systems-rinnai-infinity-vt) at home. Rinnai sells controllers that allow you to set the hot water temperature and automatically fill baths, but they're about $250. There are a few things wrong with them though:

- They don't speak MQTT.
- $250 is too much to pay for a 7-segment display and a microcontroller.

## First look

The first step involved buying a US controller from eBay (because they're a lot cheaper). I suspected the only difference was the units displayed on the thing (which turned out to be true), but putting in a setting or switch to change units would be way too hard so why not make two separate products? I removed the case from the unit next, mostly just to see what's inside. Rinnai kindy decided to make these things unservicable, by plastic welding the case shut.

The controllers connect to the heater with only two wires, which means the power and data are both sent over only 2 wires. Since the controller can report faults from the heater itself, we also know that the communication is 2-way. I connected up an oscilloscope to check out what was happening on the lines.

![This, turns out](images/2017/09/NewFile2.png)

The positive wire carries +12V, and a 2.5V p-p AC signal.
After some disconnecting and reconnecting of the controller I worked out the following:

- The power is delivered by 12V DC. Data is superimposed as pulses of 400kHz AC. Short and long pulses indicate 0s and 1s.
- Data is sent every 250ms, even if nothing has changed.
- The heater unit sends 2 packets of 6 bytes each, whether the controller is connected or not.
- When the controller is connected, it replies with 6 bytes of it's own.

So when the controller is connected there are 3x6 bytes at a time to record to start reverse-engineering the protocol.

## Reverse Engineering

In the interest of speed I found a pin on the Rinnai controller where the AC signal had already been decoded into a TTL digital signal. I whipped up an arduino sketch to start recording the data using [PulseIn](https://www.arduino.cc/en/Reference/PulseIn) on a digital pin to record the length of pulses, and surprisingly it worked really well off the bat. I used a trick I saw on HackADay to visualise the protocol and help me work out what's going on.

![A temperature sweep](images/2017/09/Untitled.png)

The above image shows a sweep through the temperature settings available on the unit. Each row is a single 'packet' of data from the heater. You can see that there is a binary number going up and down around the middle of the image.

Through this method I've managed to reverse engineer all of the day-to-day workings of the water heater - setting the temperature, setting the bath-fill temperature and volume, beginning a bath-fill, and detecting how much is left/when a bath-fill is done. What i haven't done yet is decode any errors that the unit might display, or the weird control hand-over thing that happens between controllers when you have multiple (but then if you're making it wifi enabled you don't need more than one).

## Hardware

It turns out that separating the AC and DC components of the signal is not straight-forward, especially when you then want to inject your own AC signal onto the line. I have some ideas for how to do this, but have not had the time yet to test. I need to do more to understand how the controller I bought does it. More to come!
