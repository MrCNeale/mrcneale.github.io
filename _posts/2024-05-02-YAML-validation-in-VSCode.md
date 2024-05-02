---
layout: post
title: How to add YAML syntax validation to VSCode
excerpt: Adding YAML validation to VSCode using the Redhat extension 
comments: true
date: 2024-05-02T09:00:00+00:00
image:
  feature: vscode.jpg
tags:
  - vsCode
  - extensions
  - yaml
  - Code
  - Time saving
---

<img src="/public/vscode.jpeg" alt="vscode logo" width="100" height="100"/>

# VSCode checks so much out of the box...just not YAML

Anyone who has ever had to write YAML code, for pipelines/devops/terraform etc. then you will know how fickle it can be with spaces, tabs and indentation.  
We recently had an issue where someone had edited directly in github (something we 'discourage' heavily unless there is some real exception justified) and in doing so had messed up the indentation.
It wasn't until someone tried to run the pipeline that the issue was discovered as it gets checked against the YML schema prior to running.  

I was sure that VSCode would have warned them, had they used it instead of editing on the web.  However I was a little surprised to see that YML isn't part of the built-in intellisense/validation checks.  

The top result in extensions for YAML is the Redhat extension.  After installing that, *BOOM* instant YAML errors shown.  

<img src="/public/ymlerror.png" alt="yaml validation error">  
  
It's free and quick to install.  
The URL is [Redhat YAML validation extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml "Redhat YAML Validation Extension")  

  
<img src="/public/ymlextension.png" alt="yaml validation extension">
