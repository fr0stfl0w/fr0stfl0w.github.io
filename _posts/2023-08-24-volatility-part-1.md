---
title: Volatilty
author: erika
date: 2023-09-26 20:55:00 +0800
categories: [tech]
tags: [tech, volatility forensics, plague doctor]
pin: false
# img_path: '/assets/img'

image:
  path: /assets/img/volatility.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Basic intro to Volatility and Host-Based Forensics in General

---

Annotations of various tutorials on starting out in Volatility, a python-based tool for Host-Based Forensics and Incident Responders. This tool is for digital investigation, and requires the repository up on Github and a .vmem file for collection.s

## Raw Notes

![Desktop View](/assets/img/tutorials/volatility-part1/volpart1.jpeg){: width="500" height="589" }
_Raw Notes 1_
![Desktop View](/assets/img/tutorials/volatility-part1/volpart2.jpeg){: width="572" height="589" }
_Raw Notes 2_
![Desktop View](/assets/img/tutorials/volatility-part1/volpart3.jpeg){: width="572" height="589" }
_Raw Notes 3_
![Desktop View](/assets/img/tutorials/volatility-part1/volpart4.jpeg){: width="500" height="589" }
_Raw Notes 4_


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

`` pip3 install -r requirements.txt``

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

python vol.py -f cridex.vmem windows.malfind 
```

## 2. Annotations 

Highly recommend that you actually watch these videos or read through them, these are more of a quick memory jog and so while they make sense to me since I've watched them, they're probably not as insightful as at least giving the video a watch.
### Professor K

<p> <a href="https://youtu.be/4lURQHslmMc?si=8-kgV6uInHoXbui8" alt="Professor K: Memory Forensics Using the Volatility Framework
" target="_new"><img src="https://img.youtube.com/vi/4lURQHslmMc/maxresdefault.jpg" width="300"/></a>
</p>

Probably the most straightforward out of all the tutorials, he just dives right into it. Keep in mind that he uses a **Linux** host to examine a .vmem image of an infected Windows machine. He is also using Volatility 2. I didn't have much trouble getting past this on a Windows workstation using Volatility 3 and Python 3, but you may need to pull up 
[Ashley Pearson's Volatility 2-3 cheatsheet](https://blog.onfvp.com/post/volatility-cheatsheet). 


#### Timestamps 
```python
1:08 - Cridex Sample ( I already downloaded this and put it in my volatility folder for ease of access)
1:52 - Extract to memdump (user created)
2:45 - Runs volatility as a command, but I just did python vol.py
3:03 - volatility 3-2.4.1\vol.py -f cridex_memdump\cridex.vmem image.info
----> -f: use the following file (cridex.vmem) image.info
----> image.info is a basic Volatility module
----> vol.py -f cridex.vmem windows.info
----> this module gives basic system info, like when it was taken,  mount system 
3:46 - vol.py -f cridex.vmem windows.info
4:15 - vol.py -f cridex.vmem --profile=WinXPSP2x86 pslist
----> vol.py -f cridex.vmem windows.pslist, be aware of any that catch your attention
4:45 - All the processes running on this machine
5:12 - Takes note of explorer.exe & an unknown reader_s1.exe
5:40 - volatility -f cridex.vmem --profile=WinXPSP2x86 pstree
----> pip3 install -r .\requirements.txt -vv
5:46 vol.py -f cridex.vmem windows.pstree 
----> take note of explorer.exe to reader_s1.exe
6:08 View processes currently trying to hide
----> vol.py -f cridex.vmem profia vle = Win... psxview
----> there's not quite a volatility equivalent , but just gets lumped with pstree and pslist
----> vol.py -f .vmem windows.psscan
6:20 - The 1st two columns, the pslist & psscan. If any of these register as fake
----> If an entry is found in psscan but not psslist, it is attempting to hide
6:50 - Check for any sockets or open connections ( in 3 steps) 
----> vol.py -f cridex.vmem connscan (windows.netscan)
----> vol.py -f cridex.vmem sockets ("All the sockets currently running + PID")
----> Communication link between 2 programs
7:26 - All the sockets currently running + PID
8:09 - netscan
8:20 - Three Other Plugins
vol.py -f cridex.vmem  cmdline --> windows.cmdline.Cmdline
8:36 - We know from our original pstree that PID 1484=suspicious 
---> path: C:\Program Files \Adobe\Reader 9.0\Reader\Reader_S1.exe
9:10 - cmdline should give us a complete path for how these two programs are actually run
9:21 - Dump the memory area that runs this executable
---> volatility -f cridex.vmem --profile=WinXPSP2x86 procdump -p 1640
10:01 - In his video, he gave a dumpfile of 1640, the file was very dlls named this
10:10 - Addressable memory being used by the executable 
vol.py -f cridex.vmem -o . windows.memmap --dump --pid 1640
10:40 - Look closer at the memory of what was going on inside of this reader.exe
10:50 - strings 1640.dmp | grep -FI "41.168.5.140" -c 5
compare this with thirteen cubed, which had 
vol.py -f cridex.vmem windows.pslist | grep i ()
vol.py -f cridex.vmem windows.pslist --pid
vol.py -f cridex.vmem windows.cmdline --pid 1484
11:25 - Here in the .dmp strings we can see communication with this IP Address using a POST, potentially exfiltrating info.
12:17 - We could sscroll through the entire dmp file, in the video he found various banking urls
12:40 - we could possibly reverse engineer it or upload the .dmp to VirusTotal
13:45 - What is launching this malware? Dive into the registry
14:12 - Most malware will look to see if infected files are still present at system startup + communicate with that .exe every few minutes
14:25 - If that .exe has been removed, the payload will rebuild the executable but with a different name. Need to find the payload 
15:18 - volatility -f cridex.vmem hivelist
15:27 - What is hiding in one of the keys? Running as an exe at start
16:55 - See my association with this exe and 1640 weird process
17:20 - strings 1640.dmp | grep -Fi "Kb000207877.exe"
```

### Varonis

<p> <a href="(https://www.varonis.com/blog/how-to-use-volatility" alt="Varonis - how to use Volatility" target="_new"><img src="https://info.varonis.com/hubfs/Blog_Volatility_BlogHero_202203_FNL.png" width="300"/></a>
</p>


[Please check out the original tutorial, it's one of the few non video formats and goes more into malfind in the **Identifying Injected Code** part](https://www.varonis.com/blog/how-to-use-volatility)


> "...This displays a list of processes that Volatility suspects may contain injected code based on the header information displayed in hex, the permissions, and some extracted assembly code, just because a process is listed in the output it doesn't mean the process is 100% malware."
{: .prompt-tip }

#### Identify Malicious Network Connections
```sass
python vol.py -f <> windows.netscan
```
#### Identify Injected Code
> Things Volatility only suspects to be injected code

we're looking for PAGE_EXECUTE_ReadWrite permissions

Header of a Windows Executable is ```MZ```
OR ```4D 5A```

```sass
python vol.py -f cridex.vmem windows.malfind 
```
#### Procdump
extracts exe and all dlls associated

```sass 
python vol.py -f <> -o <location> windows.dumpfiles --pid []
```

#### Identify Malicious Processes 
pslist 
pstree - for spotting malicious processes masquerading as valid ones.

> "... Windows processes always run from set locations on disk. The parent process is usually a set process."


```
-k: specifies a group of services to run
```

