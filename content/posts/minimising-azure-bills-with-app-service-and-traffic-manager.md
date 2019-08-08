---
title: "Minimising Azure Bills with App Service and Traffic Manager"
date: 2016-03-06T11:12:11+11:00
draft: false
tags: ["azure", "app service web app", "app service environment", "cloud service", "traffic manager"]
---

Our team works on high scale website available at [globalchallenge.virginpulse.com](https://globalchallenge.virginpulse.com/). Historically we have used [Cloud Services](https://azure.microsoft.com/services/cloud-services/) and over the last year we have migrated to [Azure App Service specifically Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview). We choose to migrate to Web Apps for a variety of reasons such making deployments and operational overheads more efficient.

A quick note on Azure terminology:

*   A Web App is a type of app intended for hosting websites and web applications.
*   Web Apps run on the collection of physical resources that is referred to as an App Service plan.
*   A more detailed overview can be found [here](https://docs.microsoft.com/en-us/azure/app-service/app-service-value-prop-what-is).

Well before we made the decision to move to App Services we had to get management approval which meant we had to choose what sort of plan our App Service we thought we would need. App Service plans [range in price](https://azure.microsoft.com/en-au/pricing/details/app-service/), features and functionality from free, shared, basic, standard and premium. The team mistakenly thought we would be able to run using the Standard App Service plan, specifically using the S3 instance pricing. We went through the relevant management and budget approval processes based on this information. We then updated our codebase to allow us to deploy the web site as a Web App running in an Azure App Service. It was only at this point where we realised that the standard plan restricted the maximum number of instances to 10 and we knew we would need more than 10 instances.

This left the team with four distinct options.

### Premium Service plan

Upgrading to the [Azure Premium App Service plan](https://azure.microsoft.com/en-au/pricing/details/app-service/) facilitates increasing the maximum number of instances to 50, and even allowed for more instances on request. This was the simplest option. The catch was that each instance would be significantly more expensive to operate which would mean we needed to go through the budget approval process again - far from ideal.

### App Service Environment

The team even explored the option of using an [App Service Environment](https://docs.microsoft.com/en-us/azure/app-service/environment/intro) in which your hosting environment is dedicated and fully isolated. This comes with all the bells and whistles including the Azure’s most powerful and expensive P4 instance. This was quickly ruled out as it was both the costliest option, and came with a surprising amount of complexity.

### Cloud Services

Despite the team's effort to get the codebase into a position where we could utilise Web Apps, we could fall back to deploying the website as a Cloud Service. Despite being an option this would mean we couldn’t benefit from the any of the operational efficiencies gained from using Web Apps, and was deemed our last resort.

### Traffic Manager

The team’s last option was using a Traffic Manager such as [Azure’s offering](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview) or [Cloudflare’s offering](https://blog.cloudflare.com/cloudflare-traffic-manager-the-details/). We were initially sceptical and thought that Azure would block our attempts to use traffic manager. Our team has a business requirement that our data and all servers reside in a specific geographic region (i.e. all Azure services need to belong to one data centre with DR in another data centre). This goes against the typical use case of any Traffic Manager which is to geographically distribute load based on the servers closest to the end user. Given these requirements let’s know go with a theoretically example, where 40 server instances are required:

*   Given an App Service plan running on the standard tier support a maximum of 10 instances, spin up 4 app server instances on the standard tier.
*   The 4 App Service plans are then connected via traffic manager
*   Generally speaking, the standard tier is a third of the price of the premium tier for the same specifications (except for disk storage which increases on the premium tier) In this instance, you are saving a third of the price of the 40 servers. However, you trade off that money saved for operational overhead of maintaining and deploying to 4 App Service plans.