---

title: "Automated Deployment of Azure Functions - Azure DevOps Release Pipelines"
date: 2018-06-15T13:05:11+11:00
tags: ["azure", "functions", "vsts", "serverless", "ARM", "Azure DevOps", "Continuous Integration & Continuous Delivery"]
---

This is the second article around automating deployments of Azure Functions.  The first article can be found [here](/posts/automated-deployment-of-azure-functions-arm-templates).

This article focuses on deploying from Azure DevOps (formerly VSTS). It will address the second and third requirements discussed in the previous post, to recap:

- The Function App should be created and maintained by the release pipeline.  
- The Function app must stay online whilst code is being deployed. Strangely this requirement took a considerable amount of research to resolve, maybe this was because Azure Functions were still in preview as I was researching.

### Create your Azure DevOps Release pipeline

In Azure Develops (formerly VSTS) go to your release pipeline. I assume you are already familiar with the basics of Azure DevOps, but if not, you will need an artifact to release which should be created from a build pipeline.  For the sake of brevity, I have only included the most important properties, if in doubt assume the default is fine or consult the documentation.

#### Incrementally deploy the ARM template 

 Add an [Azure Resource Group Deployment](https://www.hanselman.com/blog/ExploringWyamANETStaticSiteContentGenerator.aspx) task with the following settings:

- Version: `2.0*`
- Display Name: `ARM Template Deployment`
- Azure Subscription: `Azure Resource Manager`
- Action: `Create or update resource group`
- Resource Group: `$(ResourceGroup)`
- Location: `$(Location)`
- Template Location: `Linked artifact`
- Template: `$(System.DefaultWorkingDirectory)/$(Release.PrimaryArtifactSourceAlias)/FunctionApp.json`.  Your template json file will need to match this path.
- Template Parameters: `$(System.DefaultWorkingDirectory)/$(Release.PrimaryArtifactSourceAlias)/$(ReleaseEnvironment).parameters.json` Your template json file will need to match this path.
- Override template parameters: 
- Deployment Mode: `Incremental`
- Enable prerequisites: `None`

This task will incrementally create or update the Function app for each release based on your supplied ARM template and parameter.

#### Stop the staging slot
Add an [Azure App Service Manage](https://github.com/Microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureAppServiceManageV0/README.md) task with the following settings:
 
- Version: `0.*`
- Display Name: `Stop Staging slot`
- Azure Subscription: `Azure Resource Manager`
- Action: `Stop App Service`
- App Service name: `$(AppName)`
- Specify Slot or App Service Environment: `true`
- Resource group: `$(ResourceGroup)`
- Slot name: `$(SlotName)`

This step is optional, but in my experience, it helps to have the app service in a known state before publishing code.

#### Deploy Function App code
Add an [AAzure App Service Deploy](https://github.com/Microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureRmWebAppDeploymentV4/README.md) task with the following settings:
 
- Version: `3.*`
- Display Name: `Code Deployment`
- Azure Subscription: `Azure Resource Manager`
- App type: `Function App`
- App Service name: `$(AppName)`
- Deplot to slot: `true`
- Resource group: `$(ResourceGroup)`
- Slot Name: `$(SlotName)`.  Suggested name 'staging'.
- Package or folder: `$(System.DefaultWorkingDirectory)/`

This step takes the package or folder and deploys it to the staging slot.

#### Start the staging slot
Add an [Azure App Service Manage](https://github.com/Microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureAppServiceManageV0/README.md) task with the following settings:
 
- Version: `0.*`
- Display Name: `Start Staging slot`
- Azure Subscription: `Azure Resource Manager`
- Action: `Start App Service`
- App Service name: `$(AppName)`
- Specify Slot or App Service Environment: `true`
- Resource group: `$(ResourceGroup)`
- Slot name: `$(SlotName)`

This step ensures the staging slot is started.  

#### Swap slots
Add an [Azure App Service Manage](https://github.com/Microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureAppServiceManageV0/README.md) task with the following settings:
 
- Version: `0.*`
- Display Name: `Swap slots`
- Azure Subscription: `Azure Resource Manager`
- Action: `Swap Slots`
- App Service name: `$(AppName)`
- Specify Slot or App Service Environment: `true`
- Resource group: `$(ResourceGroup)`
- Slot name: `$(SlotName)`

This task will swap the contents of the Staging slot with that of Production.  More information on this and what specifically gets swapped can be found on [docs.microsoft.com](https://docs.microsoft.com/en-us/azure/app-service/web-sites-staged-publishing#which-settings-are-swapped).