---
title: "End-to-End Testing across ASP.Net MVC, Web API and SignalR Hubs"
date: 2020-01-11T18:02:11+11:00
draft: false
tags: ["ASP.Net", "MVC", "Web API", "SignalR", "Automated Testing", "Integration Testing", "End-to-End Testing"]
---

I recently needed to extend a collection of automated end-to-end integration tests for an ASP.Net website.  They were a great starting point, but only covered part a small part of the service layer.  I needed to extend them so they covered more end-to-end scenarios which meant changing their entry point from the service layer to the real end points of the system - that is the MVC controller, the API controller and the SignalR Hub action. This way an end-to-end test could start with interact with any combination of MVC controller, the API controller and the SignalR Hub actions - nice.

This series of blog post explains each step on the journey, and we start with the MVC Controller.

### MVC Controller

When writing an integration test for and MVC controller we need to mimic the browser.  

In MVC you should be using [anti-forgery tokens](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) to prevent against Cross-Site Request Forgery (CSRF).  By trying to mimic the browser I eventually found an [excellent blog article](https://geeklearning.io/asp-net-core-mvc-testing-and-the-synchronizer-token-pattern/) written by Arnuad.  

In short we need to perform a get request, retrieve the value of the anti-forgery token from the response, and use it when performing the subsequent post request.  Additionally the cookies are copied from the previous response to the next request. By mimicing the browser we don't introduce an insecure backdoor by doing something silly like disabling or weaking anti-forgery tokens.

The following Gist are the MVC helpers needed. I suggest starting at LoginHttpRequest() of MvcHttpClientHelper.

{{< gist palmerandy 2da6b7fc0e9bfa0b4a8e39888677468b>}}