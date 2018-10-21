---
layout: post
title: Building an Azure lab environment via ARM templates - Part3 Saving Money with Auto Shutdown
excerpt: Starting small and getting to enterprise redeployable lab blueprint
comments: true
date: 2018-10-22T10:00:00+00:00
image:
  feature: arm.jpg
tags: 
  - Azure
  - ARM Template
  - Template
  - Automation
  - Automated Deployment
  - Cost Saving
---

<img style="float: top;" src="https://msdnshared.blob.core.windows.net/media/2017/05/2017-05-06-Azure-IaC-How-to-get-started-with-ARM-Template.jpg">

Aim
------------
This post will show you how to take your existing template and add a special resource to it which is a "Schedules" resource which shuts down your VM at a specified time to save you money.


Pre-reqs
------------
You'll need:
* Read/do Part 1 (https://www.chrisneale.org/Azure-Lab-via-Templates-Part1/)
* Read/do Part 2 (https://www.chrisneale.org/Azure-Lab-via-Templates-Part2/)
* Be a really quick catcher upper

The Story so far and next
----------------
So you have an ARM template in your library.  It deployed a vNet a NIC and a VM (with a managed disk).  
Great...but please tell me you turned it off afterwards!  You didn't?  OK well hopefully you are following this series....

Next we're going to add another resource which creates a Schedule to turn off your VM.

The code is based on the schema here
https://docs.microsoft.com/en-us/azure/templates/microsoft.devtestlab/schedules
In fact all of the ARM resource template schemas and properties can be found here so you can create your own templates as complex as you like.
I've trimmed it down to only the properties we need.
```JSON
{
      "apiVersion": "[providers('Microsoft.DevTestLab','labs').apiVersions[0]]",
      "dependsOn": [
           "[concat('Microsoft.Network/virtualMachines/', 'DC01')]"
      ],
      "type": "Microsoft.DevTestLab/schedules",
      "name": "Shutdown-DC01",
      "location": "[resourceGroup().location]",
      "properties": {
        "status": "Enabled",
        "timeZoneId": "UTC",
        "taskType": "ComputeVmShutdownTask",
        "notificationSettings": {
          "status": "Disabled",
          "timeInMinutes": 15
        },
        "targetResourceId": "[resourceId('Microsoft.Compute/virtualMachines', 'DC01')]",
        "dailyRecurrence": {
          "time": "1700"
        }
      }
}
```

It's very standard, the only things I've set are the name of the schedule, the name of the VM (inside the targetresourceid property), the Time Zone and the Time to shutdown.
This is a resource so we just need to add this to the resource section of our template.  Oh and we've made sure it depends on the compute resource, so it only tries to create it once it's already there. Remember how?

1. Go to your dashboard or All Services, if you don't have it pinned, and choose Templates
2. Click on your template and click edit
3. Click on "ARM Template" and paste in our new code above into the Resource section.  
  
Don't forget to put a comma before or after, depending on where you insert it.
So we should have a template that looks like this 
```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "String",
            "metadata": {
                "description": "Default Admin username"
            }
        },
        "adminPassword": {
            "type": "SecureString",
            "metadata": {
                "description": "Default Admin password"
            }
        }
    },
    "variables": {
        "DC01VnetID": "[resourceId('Microsoft.Network/virtualNetworks', 'mylab-vnet')]",
        "DC01SubnetRef": "[concat(variables('DC01VnetID'), '/subnets/', variables('mylab-vnetSubnet1Name'))]",
        "MyLab-vNetPrefix": "10.0.0.0/16",
        "MyLab-vNetSubnet1Name": "Subnet-1",
        "MyLab-vNetSubnet1Prefix": "10.0.0.0/24",
        "MyLab-vNetSubnet2Name": "Subnet-2",
        "MyLab-vNetSubnet2Prefix": "10.0.1.0/24"
    },
    "resources": [
        {
            "type": "Microsoft.DevTestLab/schedules",
            "name": "shutdown-computevm-DC01",
            "apiVersion": "[providers('Microsoft.DevTestLab','labs').apiVersions[0]]",
            "location": "[resourceGroup().location]",
            "properties": {
                "status": "Enabled",
                "timeZoneId": "UTC",
                "taskType": "ComputeVmShutdownTask",
                "notificationSettings": {
                    "status": "Disabled",
                    "timeInMinutes": 15
                },
                "targetResourceId": "[resourceId('Microsoft.Compute/virtualMachines', 'DC01')]",
                "dailyRecurrence": {
                    "time": "1700"
                }
            },
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/','DC01')]"
            ]
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "name": "DC01",
            "apiVersion": "2017-03-30",
            "location": "[resourceGroup().location]",
            "properties": {
                "osProfile": {
                    "computerName": "DC01",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "hardwareProfile": {
                    "vmSize": "Standard_B1ms"
                },
                "storageProfile": {
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2016-Datacenter",
                        "version": "latest"
                    },
                    "osDisk": {
                        "createOption": "FromImage"
                    },
                    "dataDisks": []
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces','DC01-NIC')]"
                        }
                    ]
                }
            },
            "dependsOn": [
                "[concat('Microsoft.Network/networkInterfaces/', 'DC01-NIC')]"
            ]
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "name": "DC01-NIC",
            "apiVersion": "[providers('Microsoft.Network','networkInterfaces').apiVersions[0]]",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "DC01-NIC"
            },
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('DC01SubnetRef')]"
                            }
                        }
                    }
                ]
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', 'mylab-vnet')]"
            ]
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "MyLab-vNet",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "MyLab-vNet"
            },
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('MyLab-vNetPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('MyLab-vNetSubnet1Name')]",
                        "properties": {
                            "addressPrefix": "[variables('MyLab-vNetSubnet1Prefix')]"
                        }
                    },
                    {
                        "name": "[variables('MyLab-vNetSubnet2Name')]",
                        "properties": {
                            "addressPrefix": "[variables('MyLab-vNetSubnet2Prefix')]"
                        }
                    }
                ]
            },
            "dependsOn": []
        }
    ]
}
```
Now save and deploy.
Once it's done you should see it's deployed 4 resources.
<BR><img style="float: bottom;" src="/public/deployment.png">
But....the list in the resource group only shows three! 
<BR><img style="float: bottom;" src="/public/resourcelist.png">
That's because the shutdown only shows up as a blade on the vm.  
<BR><img style="float: bottom;" src="/public/scheduleview.png">

So there we have it, another quick addition, but an important money saving one.

