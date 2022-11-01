---
comments: true
date: "2019-06-26T22:00:00Z"
excerpt: If Docker + Powershell + AZ Cmdlets + Git is the answer, what is the question
  !?
image:
  feature: azdocker.png
tags:
- ARM
- Template
- Troubleshooting
- Azure
- Extensions
title: Build a Docker container with a standard deployment environment
---
<img src="/public/azdocker.png">   

# Practical user for Docker/Containers in infra as code (IaC) - a standard deployment environment

"Infrastructure as Code, Automate all the things, Containers are the future, DevOPs to the max!" has been a chorus of lots of people in tech for a while and as a non-app dev I never really saw applications for containers to help what I do.  
What do you do? Good question. Well it involves a lot of ARM template deployments and deploying blueprints and standard environments to run tests against.  

Other folks need to do that and quite often we come up against the "it doesn't work on my PC/MacbookPro!".  
When I heard that I remembered all the Docker/Container talks I'd been to that used that as an example or use case for Docker. A standard env no matter where you run it.  

So to standardise being able to deploy ARM templates, from a github repo I decided to create a Docker Image, based on CentOS, that has Powershell Core, AZ cmdlets and Git installed. Simples!  

Below is how I did it and how you can too (instructions for windows, but the theory's the same for mac/linux).  
NOTE: Before you start go to [https://hub.docker.com/](https://hub.docker.com/) and sign up for an account to be able to store your image publicly in later. My repo is called "pobx"...don't ask....  
  
1 Install docker for windows/mac/linux  
[https://docs.docker.com/docker-for-windows/install/](https://docs.docker.com/docker-for-windows/install/)  
It's a next next finish and reboot if required.  
2 Launch Docker Desktop and then get the centos base image by opening powershell and run  
```
docker pull centos
```
3 Start a container from the CentOS image so we can modify it and create a new image. We need to do this in *interactive* mode so we can run commands after each other and see the results.
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
What you have now is a CentOS containers, with Powershell and AZ Modules and Git Installed fresh.  
4 Now find the container (it's still running even though you exited bash) and it's amusing two word underscore separated name by running:  
```
docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
90194f94db40        centos              "/bin/bash"         26 minutes ago      Exited (0) 6 minutes ago                       nifty_pike
```  
5 Now we want to create the image of that container locally, but the local repo name we use should be the same as our Docker Hub repo we created before we started. In my case "pobx"
```
docker commit nifty_pike pobx/centos_az_ps_git_blog:v1
sha256:912b4df6971e8b4b872e26d27bfb92df60b7c4756b63e02964c131102418ecdb
```  
6 Now push it to docker hub, like this
```
docker push pobx/centos_az_ps_git_blog:v1
The push refers to repository [docker.io/pobx/centos_az_ps_git_blog]
260dddc15255: Pushed
d69483a6face: Mounted from pobx/centos_ps_az_git
v1: digest: sha256:2801d29dd86a7eb2c8fceb72442b42f4473d2ea80965e256c9aabfb9ed31a1d9 size: 742
```
And you can see it in docker hub, which means anyone can pull it down.
<img src="/public/dockerhub.png">  
7 Done. Now it's in docker hub.  Go to another pc and run
```
docker pull myRepo/centos_az_ps_git:v1
docker run -it myRepo/centos_az_ps_git:v1 pwsh
```  
Et voila  
<img src="/public/connected.png">  
Yes I get this is powershell inside powershell, but you could run it from a linux vm with no powershell core, or mac or windows without git commands or a windows machine with old AzureRM modules or no Azure modules.  
If you tested your templates and/or scripts run from this container, it will work when run on the other person's machine from this container too.
