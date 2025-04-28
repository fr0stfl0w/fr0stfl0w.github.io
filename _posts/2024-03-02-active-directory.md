---
title: Active Directory
author: erika
date: 2024-03-02 20:55:00 +0800
categories: [tech]
tags: [tech, ctf, red team, pentesting]
pin: false

image:
  path: /assets/img/active-directory.PNG
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: duffle bag for pentest

--- 
# Namespace Notes

This is our layout

[Network Layout]

- 192.168.5.1: Router
- 192.168.5.2: Bare Metal Host Machine (windows) This is the actual computer hosting all the virtual machines
- 192.168.5.3: Domain Controller 1
- 192.168.5.4: Domain Controller 2
- 192.168.5.10: Appsec Box
- 192.168.5.20* - User workstations

We're going to be alternating between domain controllers to handle license expiration. 


# Making an Active Directory
Scenario: Setup a Microsoft Active Directory Server on VirtualBox

1. Download the Windows Server Evaluation iso image
(microsoft.com/en/-us/evalcenter/download-windows-server-2022)
ISO Downloads > 64 Bit Edition

2. Create a Virtual Machine using the ISO image downloaded
Note: i had to have it empty and add the iso later on to avoid the licensing requirement

3. Configure this Windows AD server to our needs & make it into a DC (Domain Controller)

4. 16:20 - Server Manager Dashboard
  - Select Step 2: Add Roles & Features
  -   Server Manager > Dashboard 
      At this point I also changed the name of the pc (settings)
      Also checking network settings to be static
      In the virtualbox settings, it needs to be set to a bridged adapter
      
  - Role-based or feature-based installation

  - Now, when you get to Server Roles, it will have File and Storage Services

  [interject to show the server manager and dashboard screenshot] 

5. 16:42 - Active Directory Domain Services
6. Installation succeeded, time to promote it to a domain controller
7. Forest: Collection of domains
8. Root Domain Name: scavengers-repo.local
9. Set password
10. Custom Install (Windows Server 2022 Desktop Standard)
11. Login after the restart
12. Part 2: Setting up the network by creating a user




## Forests, Trees, and Domains

[Screenshot of AD]

- Active Directory's Logical Divisions
- Domains: Group of network objects (ex.) with the same Active Directory database
- Tree: a group of domains stemming from those root domains.
- Forest: A whole bunch of these root domains


## Setup DNS on Active Directory
https://www.youtube.com/watch?v=-TsqAHUWdQU&ab_channel=MSFTWebCast
