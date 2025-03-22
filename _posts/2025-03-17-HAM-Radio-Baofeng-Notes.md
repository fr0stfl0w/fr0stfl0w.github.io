---
title: HAM Radio Notes
author: erika
date: 2025-03-17 20:55:00 +0800
categories: [amateur radio, radio]
tags: [radio, ham radio, amateur radio, electronics]
pin: false

image:
  path: /assets/img/hzd-tallneck-preview.png

--- 
# Baofeng Menu Reference

![Desktop View](/assets/img/tutorials/radio/CHIRP-HAM-Radio-settings.png)

This is documentation for how to use a Baofeng Radio. I am using a UV-5R8W. Many far more technical and qualified people have done extensive tutorials, so you should check them out. But if you don't have much of a radio background to begin with or just need a quick brush up, this should be the guide for you. 

You will need 

- One Baofeng Radio 

- Connecting Cables

- CHIRP

## CHIRP
CHIRP is an open source python software for programming Baofengs and other radios with channels and other settings. The fields are as follows:

### Frequency
![Desktop View](/assets/img/tutorials/radio/frequency.jpeg)


This is the primary receive frequency that your radio is set to *listen* to. 

In a repeater setup, this would be the frequency the *repeater* transmits on (which is why you hear others talking on this frequency).

### Name
Completely self set, this could be anything you want but generally it should just be the name of the station

### Tone Mode

How the radio handles squelch tones, such as CTCSS or DCS

https://forums.radioreference.com/threads/tone-settings-in-chirp.331278/

- "For general information though, the "Tone Mode" chosen determines which of the columns are going to be written to the radio."
![Desktop View](/assets/img/tutorials/radio/ToneMode.png)
 
Notice you can pick \[Blank\], Tone, TSQL, DTCS, and Cross

#### Blank

No tone is being used, radio will transmit without sending a CTCSS or DCS tone. Your radio will receive all transmissions on that frequency even if they have a tone or squelch setting. If a repeater requires a tone to activate, your transmission will not trigger it unless you set the correct tone mode. 


When to Leave Tone Mode Blank?
- If you're using a simplex frequency (direct radio-to-radio communication).
- If you're listening to a frequency that doesn’t use a tone-based squelch.
- If you're scanning a frequency and want to hear everything, including signals without tones.

When NOT to Leave It Blank?
- If you're using a repeater that requires a CTCSS/DCS tone.
- If you're getting no response from a repeater (it may require a tone).
- If you want to filter out unwanted transmissions.

#### Tone

Tone: radio sends a CTCSS tone when transmitting, but does not require a tone to receive

#### TSQL 

#### DTCS


### Tone
### Tone Squelch
### DTCS
### RX DTCS
### DTCS Polarity
### Cross Mode
### Duplex
### Offset/TX Freq
### Mode
### Skip
### Power

