---

title: "Hosting free blog using Wyam"
date: 2017-12-29T18:02:11+11:00
draft: false
tags: ["blog", "wyam", "vsts", "azure", "app service web app"]
---

I am trying to create a blog to solve the following problems:

1. Host a blog cheaply as possible - ideally for free.
2. Simplicity is king - particularly when it comes to authoring and publishing.  

This article explains my first approach, my failures and learning before moving to Netlify which is covered in [Part 2 - Netlify](/posts/hosting-a-free-blog-part-two).

### <a name="the-approach"></a>The approach

#### Wyam Static content generator

Being a .NET developer I tried out https://wyam.io which is 

> ... a static content toolkit and can be used to generate web sites, produce documentation, create e-books, and much more. Since everything is configured by chaining together flexible modules (that you can even write yourself), the only limits to what it can create are your imagination.

I had heard about it from [Scott Hanselman's blog post](https://www.hanselman.com/blog/ExploringWyamANETStaticSiteContentGenerator.aspx), and knew my colleague [authored his blog using Wyam](http://philgeorge.info/). 

As per what I I had heard about Wyam, it was very easy to use. In early 2017 I started work creating the blog on my desktop PC and had it running locally.  
 
#### Visual Studio Team Services (VSTS)

We use [VSTS](https://www.visualstudio.com/team-services) at my employer.  If you are familar with Team Foundation Server (TFS) then it is basically the online version of that.  If not you can use it for:

- agile tools: issue tracking in Kanban or Scrum boards
- GIT repository hosting: code visualisation, Pull requests, etc.
- Continuous Integration and Delivery (CI/CD): deploy to anywhere from anywhere. 

I stored all my code in a free private GIT repository hosted by VSTS. Being the only developer who would work on the blog I qualified under the small teams pricing model (up to 5 users) which is both free and doesn't require a credit card.  

#### Hosting the website

I chose [Azure App Service Web Apps](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview) to host the website.

> Azure Web Apps enables you to build and host web applications in the programming language of your choice without managing infrastructure. It offers auto-scaling and high availability, supports both Windows and Linux, and enables automated deployments from GitHub, Visual Studio Team Services, or any Git repo
 
 I followed the advice in the [Wyam documentation about deploying to Azure](https://wyam.io/docs/deployment/azure), which lead me to Dave Glick's article [Easy deployment to Azure using FTP or FTPS](https://daveaglick.com/posts/synchronizing-files-with-azure-web-apps-over-ftp).

#### Failures, drawbacks and learnings

I will group the failures into the headings defined in [The Approach](#the-approach)

#### Wyam Static content generator

Wyam is indeed easy to use. At the time of writing it does not yet support .NET Core. This became a deal breaker for me as over the holidays I wanted to work on some of the issues with the blog using my MacBook Pro. I learnt that above all, I should be able to write articles quickly and publish them without thinking or a complicated process.  

.NET Core support is underway and being tracked in [this GitHub issue](https://github.com/Wyamio/Wyam/issues/300). 

The deployment of a static site to Azure is more tedious than that of Netlify which I cover in [Part 2](/hosting-a-free-blog-part-two).  This probably could be solved using VSTS for Continuous Integration and Delivery but that requires a similar level of complexity - which is too much for me.

#### Visual Studio Team Services (VSTS)

No deal breakers with VSTS as performs as advertised.  

The biggest drawback occurs when Microsoft gets a bit confused trying to sign in with my personal account rather than my work account.  This can be overcome using multiple browsers but that violates the second objective of simplicity is king.  

#### Hosting the website

I think Azure generally is fantastic. I can achieve most things I want quickly and with great documentation - or at least documentation that is improving.  

The biggest drawback is the pricing structure. For instance the [pricing of Azure App Services](https://azure.microsoft.com/en-au/pricing/details/app-service/) does not include custom domains in the Free plan.  Instead you have to move to the Shared plan which costs ~$A12.13 a month.  This also ups the number of CPU minutes you can use per day from 60 to 240.  Additionally you can host 100 instances rather than 10.  

The custom domain limitation eventually became a deal breaker as every month I was reminded how much I was spending just to have a custom domain.  If I even wanted to host other websites this might be more justifiable, but for now ~$A145 is too much.
