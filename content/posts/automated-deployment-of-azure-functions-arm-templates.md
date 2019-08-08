---

title: "Automated Deployment of Azure Functions - ARM Templates"
date: 2018-05-27T13:05:11+11:00
tags: ["azure", "functions", "vsts", "serverless", "ARM", "Azure DevOps", "Continuous Integration & Continuous Delivery", "Infrastructure as Code"]
---

Whilst working with Azure Functions I found it difficult to find good documentation on best practices with Continuous Integration & Continuous Delivery. This two part article aims to summarise my learnings in the hope it helps someone in a similar position. 

My requirements were straight forward:

1. Utilise an 'Infrastructure as Code' approach: all azure resources required are created via versionable definition files rather than using manually clicking through the Azure Portal.
2. The Function App should be created and maintained by the release pipeline.  
3. The Function app must stay online whilst code is being deployed. 

We will address the first requirement in this article.

### Azure Resource Manager

If you aren't familar with Azure Resource Manager (ARM), [Micrsoft defines it as](https://azure.microsoft.com/en-au/resources/templates/):

> Azure Resource Manager allows you to provision your applications using a declarative template. In a single template, you can deploy multiple services along with their dependencies. You use the same template to repeatedly deploy your application during every stage of the application life cycle.

### ARM Template with Production and Staging Slots

First you will need an ARM Template.  Here is a desensitised version of the one our team uses, note it uses 1.x of the Azure Functions Runtime.

{{< gist palmerandy 22909c79de6d1f483a727116e5a1bfd4 >}}

Take note that a production slot (lines 127 to 161 onwards) and a staging slot are configured (line 162 onwards). I'll show you how to use the staging slot below so that you can deploy to it whilst keeping the production slot online.

Pay particular attention to `DisableAllFunctions` slot values, these are used to ensure the functions are always disabled on the Staging slot and always enabled on the Production Slot.  Line 139 ensures `DisableAllFunctions` is used as a slot setting, that is when slots are swapped the setting sticks to the slot - thanks to [Anthony Chu for the tip](https://anthonychu.ca/post/azure-app-service-resource-templates-tips-tricks/)

Side note, we do not use the Consumption plan as we already have an existing Azure App Service plan that we use to host the function app.  If you want more information on that see the [Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-functions/functions-infrastructure-as-code#deploy-a-function-app-on-the-consumption-plan)

### Deployment from Visual Studio

The [Microsoft Docs site](https://docs.microsoft.com/en-us/azure/azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy#deploy-the-resource-group-project-to-azure) do a wonderful job explaining this.

### Deployment from Azure DevOps

In the next [article we discuss deploying from Azure DevOps (formely VSTS)](/posts/automated-deployment-of-azure-functions-azure-devops).