---
title: "End-to-end Testing in C# ASP.Net MVC, Web API and SignalR"
date: 2020-01-11T18:02:11+11:00
draft: false
tags: ["ASP.Net", "MVC", "Web API", "SignalR", "Automated Testing", "Integration Testing", "End-to-End Testing", ".NET"]
---

I'm a big fan of automated testing and a strong advocate of the development team owning automated tests.  I find C# end-to-end integration tests an attractive offering as development teams are already familar with the language and how to write tests and so ver little upskilling is required.  

I recently extended a collection of automated end-to-end integration tests for an ASP.Net .NET framework website.  They were a great starting point, but only covered a small part of the service layer.  The needed to be extended so they covered more end-to-end scenarios which meant changing their entry point from the service layer to the real end points of the system - that is the MVC controller, the API controller and the SignalR Hub action. This way an end-to-end test could start with interact with any combination of MVC controller, the API controller and the SignalR Hub actions - nice.  This allowed for real end-to-end world scenarios, like starting with a MVC action, followed by a SignalR action, followed by a Web API action.

### MVC Controller

Getting started with MVC end-to-end testsing isn't as obvious as it should be.  When writing an integration test for MVC we need to mimic the browser to have success.  

In MVC you should be using [anti-forgery tokens](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) to prevent against Cross-Site Request Forgery (CSRF).  By trying to mimic the browser I eventually found an [excellent blog article](https://geeklearning.io/asp-net-core-mvc-testing-and-the-synchronizer-token-pattern/) written by Arnuad.  

In short we need to perform a get request, retrieve the value of the anti-forgery token from the response, and use it when performing the subsequent post request.  Additionally the cookies are copied from the previous response to the next request. By mimicing the browser we don't introduce an insecure backdoor by doing something silly like disabling or weaking anti-forgery tokens.

The following Gist are the MVC helpers needed. I suggest starting at ```LoginHttpRequest() of MvcHttpClientHelper```.

{{< gist palmerandy 2da6b7fc0e9bfa0b4a8e39888677468b>}}

### SignalR

The SignalR Hub action I need to test does not allow anonymous access.  Because it uses same authentication as the hosting MVC application I can simple use the above ```MvcHttpClientHelper(MvcHttpClient).LoginHttpRequest()```.  Then follow the [documentation on how to call SignalR server methods from the client](https://docs.microsoft.com/en-us/aspnet/signalr/overview/guide-to-the-api/hubs-api-guide-net-client#how-to-call-server-methods-from-the-client).  

{{< gist palmerandy 17dcf71567005f2f2e810aa75de41bba>}}

### Web API

If you have read this far, you should already know what to do for Web API.  Here is the code.
{{< gist palmerandy 0f6c396fcd51fb89b4b98aa04ba120ae>}}