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

# Practical user for Docker/Containers in infra as code (IaC) - a standard deployment environment

"Infrastructure as Code, Automate all the things, Containers are the future, DevOPs to the max!" has been a chorus of lots of people in tech for a while and as a non-app dev I never really saw applications for containers to help what I do.  
What do you do? Good question. Well it involves a lot of ARM template deployments and deploying blueprints and standard environments to run tests against.  

Other folks need to do that and quite often we come up against the "it doesn't work on my PC/MacbookPro!".  
When I heard that I remembered all the Docker/Container talks I'd been to that used that as an example or use case for Docker. A standard env no matter where you run it.  

So to standardise being able to deploy ARM templates, from a github repo I decided to create a Docker Image, based on CentOS, that has Powershell Core, AZ cmdlets and Git installed. Simples!  

Below is how I did it and how you can too (instructions for windows, but the theory's the same for mac/linux).

1. Install docker for windows/mac/linux  
https://docs.docker.com/docker-for-windows/install/  
It's a next next finish and reboot if required.

2.  Launch Docker Desktop <img src=/public/docker.png> and then get the centos base image by opening powershell and run  
```
docker pull centos
```
<img src=/public/pullcentos.png>

3. Start a container from the CentOS image so we can modify it and create a new image. We need to do this in *interactive* mode so we can run commands after each other and see the results.
```
docker run -it centos /bin/bash
```

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
