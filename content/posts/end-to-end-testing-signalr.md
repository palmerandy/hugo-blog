---
title: "End-to-End Testing across ASP.Net MVC, Web API and SignalR Hubs"
date: 2020-01-11T18:02:11+11:00
draft: true
tags: ["ASP.Net", "MVC", "Web API", "SignalR", "Automated Testing", "Integration Testing", "End-to-End Testing"]
---

I recently needed to extend a collection of automated end-to-end integration tests for an ASP.Net website.  They were a great starting point, but only covered part a small part of the service layer.  I needed to extend them so they covered more end-to-end scenarios which meant changing their entry point from the service layer to the real end points of the system - that is the MVC controller, the API controller and the SignalR Hub action. This way an end-to-end test could start with interact with any combination of MVC controller, the API controller and the SignalR Hub actions - nice.

This series of blog post explains each step on the journey, and we start with the MVC Controller.

### SignalR

