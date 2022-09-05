---
layout: post
title: How to Troubleshoot Azure Self-Hosted Devops Agent issues
excerpt: Common problems with Azure's Self-Hosted devops agent
comments: true
date: 2022-09-05T09:00:00+00:00
image:
  feature: ado.jpg
tags: 
  - Azure
  - Devops
  - Troubleshooting
  - Selfhosted
  - Agent
  - Fix
---

![](/public/ado.jpg)

# Why Use Self-Hosted Agents for Devops jobs? 
We recently had issues with some Devops tasks queuing. This was happening only with the tasks in the job that we run on our self-hosted agents.  
Why run our own agents?  It's a reasonable question. Microsoft provide agents (VMs) pre-configured and ready to run your tasks for you, so why mess about setting up your own?  
There is usually a problem or configuration that cannot be solved using the Microsoft Pool of agents as they are selected at random and the only known quantity about them is that they will be selected from a pool that is in the same region as your DevOps Organisation. That's fine if everything you're doing is "public" and just requires access to publicly available APIs and URLs such as the Azure Resource Manager or other 3rd party https links.  
However part of our pipeline deploys a container to a private AKS cluster. As soon as you make the cluster private, you can no longer send API calls to the Kubernetes control plane unless you are on a known network which has been given access to the cluster via firewall/NSGs etc.
So it is much easier to create a VM inside the subscription alongside the AKS cluster to run DevOps jobs against.  

# Why are my self-hosted jobs failing?</H1>
If you find tasks inside a job/pipeline are failing and they are only on your self-hosted agents. You should check your self hosted agent pool.
You can do this by going to your Project Settings>Pipelines>Agent Pools and select your self-hosted agent pool. This will show you the status of each agent.  
It is likely that one or more (depending on your setup) is showing as offline.
  
# Why is my agent showing offline?
  
![](/public/agentpool2.png)
  
It can be a number of reasons.  To query the status of the agent you need to log on to the VM itself and query the service.  
We run our agent on Ubuntu, so after SSH-ing in you need to change directory to the home folder for the agent user, e.g.
/home/myDevopsUser/ADOAgent  
From here you need to query the service

```
sudo ./svs.ch status -l
```

This will show you the status and the tail end of the log so you can look for obvious issues.  
In our case it was complaining about not being able to reach the devops url ** "Attempt 1 of GET request to https://dev.azure.com failed" **  
This made us look at name resolution and found that DNS was failing to resolve names on the VM.  
Everything in the resolv.conf and other areas pointed to DNS working fine.  There were no NSGs or firewalls configured.
A simple reboot of the VM resolved the Name Resolution problem, and also auto-upgraded the agent to the latest release.
If you are having other issues, specific to the agent, you can try stopping and starting the agent with the same script

```
sudo ./svs.ch stop
sudo ./svs.ch start
```

You'll
# Fixed and/or upgrading agents in the portal
![image](/public/agentpool1.jpg) 
Once your agent is fixed you will see it showing as "Online" and the version will be the latest.  
You should now be able to run tasks/jobs utilising your Self-Hosted Agent again.
