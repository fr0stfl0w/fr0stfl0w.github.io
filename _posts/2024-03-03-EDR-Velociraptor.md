---
title: EDR Velociraptor
author: erika
date: 2022-02-27 20:55:00 +0800
categories: [tech]
tags: [tech, ctf, red team, pentesting]
pin: false

image:
  path: /assets/img/gallery-ink-executioner.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: duffle bag for pentest

--- 
## Video Demo
https://www.youtube.com/watch?v=2kgC9Ushk9g&ab_channel=TaylorWalton


## Velociraptor Release Link
https://github.com/velocidex/velociraptor/releases/

## Surprisingly Simple Documentation

https://docs.velociraptor.app/docs/deployment/clients/


## Tutorial Followed
https://socfortress.medium.com/free-incident-response-with-velociraptor-bedd2583415d


## Docker Version 
https://github.com/weslambert/velociraptor-docker

"velociraptor-docker
Run Velocidex Velociraptor server with Docker

Install
Ensure docker-compose is installed on the host

git clone https://github.com/weslambert/velociraptor-docker

cd velociraptor-docker

Change credential values in .env as desired

docker-compose up (or docker-compose up -d for detached)

Access the Velociraptor GUI via https://<hostip>:8889

Default u/p is admin/admin
This can be changed by running:
docker exec -it velociraptor ./velociraptor --config server.config.yaml user add user1 user1 --role administrator

Notes:
Linux, Mac, and Windows binaries are located in /velociraptor/clients, which should be mapped to the host in the ./velociraptor directory if using docker-compose. There should also be versions of each automatically repacked based on the server configuration.

Once started, edit server.config.yaml in /velociraptor, then run docker-compose down/up for the server to reflect the changes

Docker image
To pull only the Docker image:

docker pull wlambert/velociraptor

To pull a specific version of the Docker image:

docker pull wlambert/velociraptor:0.6.8-2"



## Weird Errors Encountered

https://stackoverflow.com/questions/68251632/yaml-unmarshal-errors-when-using-go
