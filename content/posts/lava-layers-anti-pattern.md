---
title: "Lava Layers Anti-pattern"
date: 2015-10-17T18:02:11+11:00
draft: false
tags: ["anti-pattern", "asp .net mvc", "asp .net webforms", "Code maintainability"]
---

Whenever introducing a new technology to an older codebase it's important to have a plan on how and when to replace the existing technology. Case in point, consider an existing codebase with over one hundred ASP .Net WebForms that are used every day. My team was forced to get creative when trying to introduce the shiny-ness that is ASP .Net MVC. [This great piece by Matt Hawley](http://www.eworldui.net/blog/post/2011/01/07/Using-Razor-Pages-with-WebForms-Master-Pages.aspx) was the creativity we needed.  It enabled the team to slowly integrate MVC one Model View Controller at a time at our own pace. Cut forward well over a year later, and the team removed the last ASP .Net WebForm and successfully migrated to ASP .Net MVC.

Our team was successful as we avoided the lava layer anti-pattern which is discussed in [Jimmy Bogard's blog](https://lostechies.com/jimmybogard/2015/01/15/combating-the-lava-layer-anti-pattern-with-rolling-refactoring/). The take home is that often it can be better to live with an existing technology rather than partly introduce yet another technology, even if it is shiny and new. Our team avoided this as we made a concerted effort to replace all instances of the older technology even though it took over a year to do this.  During this time, we deliberately avoided introducing any new presentation frameworks.