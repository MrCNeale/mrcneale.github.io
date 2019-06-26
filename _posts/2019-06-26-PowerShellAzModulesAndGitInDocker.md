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
2.  Launch Docker Desktop <img src="/public/docker.png"> and then get the centos base image by opening powershell and run  
```
docker pull centos
```
3. Start a container from the CentOS image so we can modify it and create a new image. We need to do this in *interactive* mode so we can run commands after each other and see the results.
```
docker run -it centos /bin/bash
```
Then install the things
```
yum install -y git  
curl https://packages.microsoft.com/config/rhel/7/prod.repo | tee /etc/yum.repos.d/microsoft.repo  
yum install -y powershell  
pwsh  
install-module az -force  
exit  
exit  
````
<img src="/public/installs1.png"><img src="/public/installs2.png"><img src="/public/installs3.png">  
What you have now is a CentOS containers, with Powershell and AZ Modules and Git Installed fresh.  So we need to grab a new image of that.
4. Now find the container (it's still running even though you exited bash) by running
```
docker ps -a
```

now create new image from that container
----------------------------------------
docker commit word1_word2 myRepo/centos_az_ps_git:v1

now push it to docker hub (optional)
------------------------------------
docker push myRepo/centos_az_ps_git:v1

Done it's in docker hub.  Go to another pc and run
docker pull myRepo/centos_az_ps_git:v1
docker run -it myRepo/centos_az_ps_git:v1 pwsh
