---
title: Volatilty (Part 1)
author: erika
date: 2023-09-26 20:55:00 +0800
categories: [tech]
tags: [tech, volatility forensics, plague doctor]
pin: false
img_path: '/assets/img'

image:
  path: gallery-ink-executioner.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Basic intro to Volatility and Host-Based Forensics in General

---

Annotations of various tutorials on starting out in Volatility, a python-based tool for Host-Based Forensics and Incident Responders. This tool is for digital investigation, and requires the repository up on Github and a .vmem file for collection.s

## Tutorials Used

<!-- Video Embedding Format

https://img.youtube.com/vi/4lURQHslmMc/maxresdefault.jpg

<p align="center"> <a href="https://youtu.be/hjWVUrf7Obk" alt="13Cubed: MemProcFS - This Changes Everything" target="_new"><img src="http://img.youtube.com/vi/hjWVUrf7Obk/0.jpg" height="100"/></a>
</p> -->
<!-- 
<p> <a href="https://youtu.be/4lURQHslmMc?si=8-kgV6uInHoXbui8" alt="Professor K: Memory Forensics Using the Volatility Framework
" target="_new"><img src="https://img.youtube.com/vi/4lURQHslmMc/maxresdefault.jpg" width="300"/></a>
<br>Professor K: Memory Forensics Using the Volatility Framework
</p> -->


| Tutorial                                                                                          | Author | Comments           |
|----------------------------------------------------------------------------------------------------|-----------|:--------------|
| [Memory Forensics Using the Volatility Framework](https://youtu.be/4lURQHslmMc?si=8-kgV6uInHoXbui8) | Professor K | the most straightforward video |
| [VOLATILITY 3 CHEATSHEET](https://blog.onfvp.com/post/volatility-cheatsheet)         | Ashley Pearson | Volatility 2 vs 3 differences  |
| [(EASIEST) Install Volatility on Windows (Standalone)](https://www.youtube.com/watch?v=lhxEqOh22g0)         | Computer Science Basics| Skip to 0:45 seconds  |
| [How to Use Volatility for Memory Forensics and Analysis](https://www.varonis.com/blog/how-to-use-volatility)    | Neil Fox| Goes a bit more in depth on malfind and code injection |
| [Rapid Windows Memory Analysis with Volatility 3](https://www.youtube.com/watch?v=EqGoGwVCVwM)         | John Hammond | Going a bit further (needs BTLO)  |


These are the resources in order that I used to get more familiar with the tool. They're all pretty straightforward, and unlike MemProcFS, it required minimal setup


## Ingredients
- Python 3 (to run the vol.py script)
- [Volatility 3](https://github.com/volatilityfoundation/volatility3) (use the .zip file in the github repo)
- [.vmem Cadaver](https://github.com/volatilityfoundation/volatility/wiki/Memory-Samples)

## 0. Installation

Using Volatility 3, download the [.zip file from their Github Repo](https://github.com/volatilityfoundation/volatility3/releases)

Github Repo > **Releases** > Source Code (.zip)

cd into the repository and run 

``vol.py -f <.VMEM SAMPLE> windows.info``

## 1. Quick Command Toolbox

``vol.py -f <.vmem sample> <module>``

Note: I used the [cridex.vmem](https://github.com/volatilityfoundation/volatility/wiki/Memory-Samples) example that Professor K showcased in his tutorial

Note: I used python 3
```sass
python vol.py -f cridex.vmem windows.info

python vol.py -f cridex.vmem windows.pslist

python vol.py -f cridex.vmem windows.pstree

python vol.py -f cridex.vmem windows.psscan

python vol.py -f cridex.vmem windows.netscan

python vol.py -f cridex.vmem sockets

python vol.py -f cridex.vmem -o . window.dumpfiles --pid 1640 
(this gets the exe and associated dlls)

python vol.py -cridex.vmem -o . windows.memmap --dump --pid 1640

strings 1640.dmp | grep -Fi "weird thing, etc" -C 5

python python vol.py -f cridex.vmem windows.malfind 
```

## 2. Annotations 

Highly recommend that you actually watch these videos or read through them, these are more of a quick memory jog and so while they make sense to me since I've watched them, they're probably not as insightful as at least giving the video a watch.
### Professor K

<p> <a href="https://youtu.be/4lURQHslmMc?si=8-kgV6uInHoXbui8" alt="Professor K: Memory Forensics Using the Volatility Framework
" target="_new"><img src="https://img.youtube.com/vi/4lURQHslmMc/maxresdefault.jpg" width="300"/></a>
</p>

Probably the most straightforward out of all the tutorials, he just dives right into it. Keep in mind that he uses a **Linux** host to examine a .vmem image of an infected Windows machine. He is also using Volatility 2. I didn't have much trouble getting past this on a Windows workstation using Volatility 3 and Python 3, but you may need to pull up 
[Ashley Pearson's Volatility 2-3 cheatsheet](https://blog.onfvp.com/post/volatility-cheatsheet). 

### Varonis

<p> <a href="(https://www.varonis.com/blog/how-to-use-volatility" alt="Varonis - how to use Volatility" target="_new"><img src="https://info.varonis.com/hubfs/Blog_Volatility_BlogHero_202203_FNL.png" width="300"/></a>
</p>

Unlike the other tutorials, this one goes more into malfind in the **Identifying Injected Code** part

```shell
This displays a list of processes that Volatility suspects may contain injected code based on the header information displayed in hex, the permissions, and some extracted assembly code, just because a process is listed in the output it doesn't mean the process is 100% malware.

```



