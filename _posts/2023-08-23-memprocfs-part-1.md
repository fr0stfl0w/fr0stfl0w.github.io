---
title: MemProcFS (Part 1)
author: erika
date: 2023-09-28 11:33:00 +0800
categories: [tech]
tags: [forensics, memprocfs, annotations, 13cubed]
pin: false
math: true
mermaid: true
image:
  # path: /commons/devices-mockup.png
  path: /assets/img/banner-crow.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Responsive rendering of Chirpy theme on multiple devices.
---

Annotations and quick copy-pastes for MemprocFS, based on 13Cubed's tutorial. I don't see a whole lot of other people using this tool but it has been useful so I guess here we are. 

## TLDR SpeedRun
### Commands
```css
> MemProcFS.exe -device memdump.vmem -forensic 1

/sys/proc
- pstree output
/sys/net
- netstat output

/registry/HKLM/SYSTEM/ControlSet001/Control/ComputerName 
/registry/HKLM/Software/Microsoft/Windows/Current Version/Run
/registry/hive_files
- list of the PIDs by name (dash the actual PID#)

/name/(randomprocess)
/name/(randomprocess)/files/handles 
    - what files was this process interacting with 
/name/(randomprocess)/files/modules
    - what dlls was this process interacting with
/name/(randomprocess)/files/vads
    - how Windows keeps track of what process memory is allocated to a process

/pid

```


![Desktop View](/assets/img/tutorials/memprocfs-part1/raw notes 1.jpeg){: width="500" height="589" }
_Raw Notes 1_
![Desktop View](/assets/img/tutorials/memprocfs-part1/raw notes 2.jpeg){: width="572" height="589" }
_Raw Notes 2_
![Desktop View](/assets/img/tutorials/memprocfs-part1/raw notes 3.jpeg){: width="572" height="589" }
_Raw Notes 3_

## Part 0: AKA Installation

This was the trickiest part IMO especially if you are some casual like me who has NO IDEA what forensics tools are like.

You will need an infected .vmem file, such as the cridex one in [Volatility's](https://github.com/volatilityfoundation/volatility/wiki/Memory-Samples).

You will need to [ download MemProcFS](https://github.com/ufrisk/MemProcFS/releases) off of their site along with [installing Dokany .msi](https://github.com/dokan-dev/dokany/releases) if you're doing this on a Windows host

For me, I did this all on a Windows host. 

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial1.png){: width="972" height="589" }
_Installing Dokany_

Download the .zip off of the [github releases](https://github.com/ufrisk/MemProcFS/releases) AND the [Dokany msi](https://github.com/dokan-dev/dokany/releases) 

Unzip the file and then run the .exe from commandline/terminal/powershell


> "...equating to the directories and files related to the MemProcFS forensic sub-system. The forensic sub-system is a collection of more thorough batch-oriented analysis tasks that may be undertaken on memory dumps." 

[https://github.com/ufrisk/MemProcFS/wiki/FS_Forensic](MemprocFS Wiki)

> Take Note: that .vmem file is that same cridex.vmem sample we used with Volatility. 
{: .prompt-tip }



## Part 1: Running MemProc

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial2.png){: width="972" height="589" }
_Running Memproc_
```css
.\MemProcFS.exe -device '[.VMEM SAMPLE]' -forensic 1
```
> -forensic Starts a forensic scan of the physical memory

> -device [the .vmem image to examine]

> MemProc will take a bit for the /forensics folder to show up but it will contain info regarding things like timelines of processes and dlls loaded. 
{: .prompt-tip }

When you run this it will seem like nothing is happening, and you can for now ignore the python warning. But if you look at your Windows directory, you will see the :\M image loaded for you to browse through like a file directory.


![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial3.png){: width="972" height="589" }
_Using the .vmem John Hammond used in his walkthrough_


:M\ directory appearing

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial4.png){: width="972" height="589" }
_The image getting mounted_


## Part 2: Running Through

5:30 is the approximate timestamp for when this starts.

Relevant directories: 
```css
/sys
/registry
/name
/pid
```

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial4-2.png){: width="972" height="589" }
_folder directory_

### \sys\ Folder
#### \sys\proc
Process hierarchy, textfile roughtly corresponding to a pstree output in Volatility

note: explorer should not be the parent process for svchost.exe

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial5.png){: width="972" height="589" }
_examining proc_

#### \sys\net 

Netstat output/netscan in volatility
![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial6.png){: width="972" height="589" }
_folder directory_


### \registry\ Folder

Similar to regedit

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial7.png){: width="972" height="589" }
_folder directory_

#### \registry\...\Run\RunOnce 

The Run/RunOnce registry keys make the program run once the user logs in

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial8.png){: width="972" height="589" }
_folder directory_

```M:\registry\HKLM\Software\Microsoft\Windows\Current Version\Run```

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial9.png){: width="972" height="589" }
_folder directory_

```M:\registry\hive_files```

list of the PID's by name (dash the actual PID #)

> Registry Hives: logical group of keys, subkeys, and values in the registry. Each time a new user logs on, a new hive gets created for that user with a separate file for the user
{: .prompt-tip } 

### \name\ Folder 

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial10.png){: width="972" height="589" }
_folder directory_

/files/handles will tell you the files this process was interacting with 

/files/modules will tell you all the dlls with which the process was interacting

/files/vads (virtual address descriptor) is how windows keeps track with what process memory is allocated to a process 

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial11.png){: width="972" height="589" }
_handles_

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial14.png){: width="972" height="589" }
_folder directory_


## Part 3: M:/forensic Folder

![Desktop View](/assets/img/tutorials/memprocfs-part1/memprocfs tutorial15.png){: width="972" height="589" }
_folder directory_

Forensic > Timeline is a list of processes in memory.
```console
M:\forensic\timeline\timeline_registry

M:\forensic\timeline\timeline_net
```

Registry paths and timestamps associated with them

```console
forensic\findevil
```
Remember, this forensic folder may or may not be 