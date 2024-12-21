---
title: >-
  Using a raspberry pi with a microphone to hear an audio alarm using FFT in
  python
tags:
  - alarm
  - audio
  - code
  - FFT
  - fire
  - flood
  - leak
  - numpy
  - python
  - raspberry pi
  - sensor
  - smoke
  - water
id: '157'
categories:
  - - uncategorized
alias: >-
  /2017/02/18/using-a-raspberry-pi-with-a-microphone-to-hear-an-audio-alarm-using-fft-in-python/
date: 2017-02-18 14:56:57
---

If your smoke alarm or, in my case, water alarm goes off you want to know right away - even if you are currently half way across the world traveling in China. I run a fish tank. I take many precautions but you really can't be too safe. I bought a set of Honeywell water sensors which I highly recommend. Sadly, this particular alarm is not IoT enabled. In fact, last I checked all the IoT alarm systems were terribly reviewed and overpriced. Hopefully that gets fixed soon. Until then, I needed to make do with what I had.
<!-- more -->
[![](http://www.benchodroff.com/wp-content/uploads/2017/02/51ZtsQWq7vL._SL1000_-300x300.jpg)](http://www.benchodroff.com/wp-content/uploads/2017/02/51ZtsQWq7vL._SL1000_.jpg)Rather than ripping the alarms open and hard wiring in a detection, I wanted to use an audio detection method so I could listen to many devices around the house. I wanted to receive a text message if any alarm went off and my raspberry pi could hear it. I saw some commercial devices for sale but what is the fun in that, honestly? I scoured github and google for some example code, but did not find much. So, here is my quick and dirty audio alarm sensor code.

DISCLAIMER: Under no circumstances should this code be used in any safety or production setting. Using an audio mechanism to listen for safety alerts is just plain dumb and could easily malfunction causing serious injury or death. This is for education purposes only.

This code works on a set of moving windows to detect confirmed alarm beeps. My alarm beeps at 3500Hz with a regular interval. I used a fast fourier transform with numpy in python to isolate the most intense sounds. I then use quadratic interpolation to determine the frequency of the most intense sound wave. If enough "blips" fill a window to detect a "beep," and enough "beeps" fill a larger window, then the program indicates the alarm is active. If enough non-beep detected windows pass, it will clear the alarm detection.

At some point I'll need to learn machine learning and teach it how to automatically recognize different types of alarms! Anyone have some tips?
<script src="https://gist.github.com/benjaminchodroff/7d33a34e389c31a11aaa1ad16ba4c159.js"></script>

