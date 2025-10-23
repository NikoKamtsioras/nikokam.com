---
layout: single
title: "How i built a Satellite Tracker for Amsat"
date: 2025-10-19
categories: [Projects]
---

One of the things I have wished to do once I get my amateur radio callsign is to communicate with others using Low Earth Orbiting (LEO) satellites. But doing so was not going to be so easy. I will be explaning the challenges faced with LEO Satellites and how my soultion was implemented.

# Existing solutions

I looked what others use to do satwork and well...

- There is other hobbyest projects, like the [SatNOGS Rotator v3](https://wiki.satnogs.org/SatNOGS_Rotator_v3), But it needs a lot of parts aside from 3d-printing, and i wanted to make a tracker for even less than the 220$ it costs.

- Professional and high-end amateur satellite ground stations rely on heavy-duty rotators from companies like Yaesu or Kenwood, these cost thousands to setup.

# My Idea:

My key Idea to dramatically lower the cost and complexity of the mechanical system was to leverage 3D printing for almost all of my system. This also means that others can make it without many tools or other gear.

# The Problems

## Power

Most amateur radio satellites are tiny, university-built CubeSats. These micro-sats can be as small as 10 cm×10cm×10cm! Their minimal surface area severely limits solar power generation and the battery capacity. So the desiners have to lower the RF output to fit all the hardware into the body. 

![A Cubesat](/assets/images/CubeSat_in_hand.jpg)

Due to the very low power these cubesat use for the radio downlink, in the order of milliwatts(0.1 watt) to 1-5 watts, the antenna system will need very high gain antennas to receive the signal, and there is two main desines:

### Yagi-Uda
A standard, multi-element directional antenna. It offers high gain (necessary for weak signals) in a linear polarization. While its high gain is a major advantage, its linear polarization makes it highly susceptible to the **Fading Problem** discussed below.

![Diagram of a Yagi-Uda antenna](/assets/images/Yagi-uda-antenna-example.png)

### Moxon
A compact, two-element directional antenna. It is much smaller than a comparable Yagi-Uda and offers a very good front-to-back ratio. It provides moderate gain but, like the standard Yagi, is fundamentally a linearly polarized antenna, making it a poor choice for mitigating fading. **BUT**, unlike the Yagi, it does not need a matching system for tranmiting, so it is usefull for a uplink antenna.

![Moxon](/assets/images/I-LG-040_M.te_Aiona_(4874577865).jpg)

## Speed and Frequency Problem

The satellite's speed (∼28,000 km/h!) creates two major challenges:

1. Doppler Shift: This high velocity causes a significant shift in frequency over time. On the 70 cm Ham band,close to 440Mhz, the total shift can be ±10 kHz or more during a pass. Since amateur modes like Single Sideband (SSB) or FM are narrow, the signal will quickly drift out of the receiver's passband, requiring constant re-tuning.

![Doppler Shift](/assets/images/de.jpg)

2. Tracking: LEO passes are short, lasting only 10-15 minutes. To capture the weak signal with high-gain directional antennas, the antenna must be continuously and precisely aimed/Moved using an Azimuth and Elevation (Az/El) rotator system.

## The Fading Problem

CubeSats typically use simple linear antennas and tumble as they orbit. This constant shift in the signal's polarization causes momentary signal strength fluctuations, or fading. When the satellite's antenna polarization is perfectly misaligned with the ground station's antenna, this results in a small but noticeable dip in signal strength. A successful antenna system must mitigate this effect.

## Tracking Software

The brain of the entire satellite ground station is the Tracking Software. This software performs the complex astronomical and mathematical calculations needed to precisely aim the antenna and correct for the frequency shift. The software also can predict when and how the Satellite will pass overhead.

One site i found was [N2YO](https://www.n2yo.com/), and while it can give the pass times, i need sofware that can be interfaced with the tracker's hardware.

# My Implementation

---

# Testing!

## The International Space Station

I first tested with the moxon, and setup the sofware to track the 
ISS's APRS digipeater on 145.825Mhz, and i got a few packets of data!

[vidio of sdr here!S]

APRS (Automatic Packet Reporting System) is an amateur radio system that uses digital packets to transmit and display real-time information, primarily including automatic location reporting (position reports), weather data, and short text messages. 

And the digital repeater receives an amateur radio APRS packet, and then retransmits it, often including its own callsign in the path, to extend the communication range between two stations. And with it being in space, the range can be VERY far!

## RS-44

RS-44 is a small scientific satellite created by specialists of the company Information Satellite Systems (ISS) Reshetnev and students of the Siberian State Aerospace University (SibSAU) Krasnoyarsk.

![RS-44](/assets/images/dosaaf-95-rs-44-antennas.jpg)

It has the following radios, with ONLY 5 watts shared between them.


- Beacon: 435.605 MHz – transmits CW call sign RS44

- Inverting transponder:
Earth-to-Space: 145.965 MHz +/- 30 kHz
Space-to-Earth: 435.640 MHz +/- 30 kHz

### Inverting what?

An inverting transponder is a type of amateur radio satellite repeater that, upon receiving a signal on its uplink band, reverses or "flips" the signal's frequency relationship and sideband before retransmitting it on the downlink band.

This Means that any type of signal can be used with the transponder like CW, Voice with SSB, or digi-modes like FT4 can all be found on RS-44!

So i changed the moxon out for my 440Mhz yagi, and...

Heard not just the CW, but the SSB Voice and FT4 also!

[vidio of sdr here!S]
