---
layout: post
title: Getting started with the NooElec NESDR Nano on OS X Yosemite
tags:
- SDR
---

This is how I did it. Your mileage may vary.

Prerequisites:

- [XCode with command line tools](http://guide.macports.org/#installing.xcode)
- [MacPorts](http://www.macports.org/install.php)
- cmake, autoconf, automake, libtool, libusb*, rtl-sdr

All programs listed on the last item above came from MacPorts**, with `sudo port install PROGRAM` with the exception of libusb. The first four on the list are required for compiling the last two from source -- and in that order, because rtl-sdr depends on libusb. Unfortunately, compiling from source did not work for me: rtl-sdr kept failing to find libusb. Installing them from MacPorts did the trick. Upshot: save yourself some trouble, use MacPorts. Other upshot: it's entirely possible that if you get rtl-sdr from MacPorts you don't need any of the first five programs in that list.

\* For libusb I did `sudo port install libusb +universal` because of [this thread](https://code.google.com/p/simple-openni/issues/detail?id=44).

** After you install MacPorts you can check `port version` but _you have to do it in a fresh terminal window_. You will find it, then you can do `sudo port selfupdate` just to get into the habit. Three more things: 

1. Every single thing you install with `sudo port install` will have to be launched from a fresh terminal window.
2. Do `sudo port selfupdate` often.
3. See also your other `sudo port` options: `clean`, `uninstall`, `upgrade`.

I think I'm on my way to putting this [thing](http://www.amazon.com/dp/B00JQZU1ZO/ref=sr_ph?ie=UTF8&qid=1422775781&sr=1&keywords=nooelec) to use. See below:

```
$ rtl_test -t
Found 1 device(s):
  0:  Realtek, RTL2838UHIDIR, SN: 00000001

Using device 0: Generic RTL2832U OEM
Found Rafael Micro R820T tuner
Supported gain values (29): 0.0 0.9 1.4 2.7 3.7 7.7 8.7 12.5 14.4 15.7 16.6 19.7 20.7 22.9 25.4 28.0 29.7 32.8 33.8 36.4 37.2 38.6 40.2 42.1 43.4 43.9 44.5 48.0 49.6 
Sampling at 2048000 S/s.
No E4000 tuner found, aborting.
```

Alright, I'm sure that this "No E4000 tuner found, aborting." is some cause for concern, but this note is about getting started.
