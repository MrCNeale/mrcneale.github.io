---
layout: post
title: Build a Docker container with a standard deployment environment
excerpt: If Docker + Powershell + AZ Cmdlets + Git is the answer, what is the question !?
comments: true
date: 2019-06-26T22:00:00+00:00
image:
  feature: azdocker.pnb
tags: 
  - ARM
  - Template
  - Troubleshooting
  - Azure
  - Extensions
---
<img src="/public/azdocker.png">   

# What can you do to find out more about why a VM extension is failing or not working quite right?


install docker for windows
url, download, next next finish reboot/relogins

get centos image
----------------
docker pull centos

start container from centos image in interactive mode at a bash prompt (to make the changes) 
----------------
docker run -it centos /bin/bash

install git bash
----------------
yum install -y git

install powershell
------------------
curl https://packages.microsoft.com/config/rhel/7/prod.repo | tee /etc/yum.repos.d/microsoft.repo
yum install -y powershell

run powershell
--------------
pwsh

install azure modules
---------------------
install-module az -force

exit twice from pwsh and bash
-----------------------------
exit
exit

find the running container name (look at the NAMES column on the right for word1_word2)
-------------------------------
docker ps -a

now create new image from that container
----------------------------------------
docker commit word1_word2 myRepo/centos_az_ps_git:v1

now push it to docker hub (optional)
------------------------------------
docker push myRepo/centos_az_ps_git:v1

Done it's in docker hub.  Go to another pc and run
docker pull myRepo/centos_az_ps_git:v1
docker run -it myRepo/centos_az_ps_git:v1 pwsh
