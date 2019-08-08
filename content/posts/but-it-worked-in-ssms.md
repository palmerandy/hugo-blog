---
title: "But it worked in SSMS?"
date: 2017-06-03T18:02:11+11:00
draft: false
tags: ["ssms", "sql"]
---

Every year, towards the end of May, hundreds of thousands of people across the world start their virtual journey around the [Global Challenge](https://globalchallenge.virginpulse.com). This creates my team’s peak load on our website and has happened every year for well over a decade. Each year we spend significant resources to ensure we have a performant system when under peak load. In May 2017 we were somewhat surprised when we encountered some performance bottlenecks that we had previously not seen before.

Application insights, our Application Performance Management service, identified two queries were utilising 100% CPU of our Azure SQL database. This was surprising as our Azure SQL database was part of the premium service tier. Even more surprising was that when you copied the SQL that was being executed, via [Dapper](https://github.com/StackExchange/Dapper), into Sql Server Management Studio (SSMS):

*   the query executed successfully in under two seconds
*   after turning on execution plan no recommendations of missing indexes were suggested Despite this our Database CPU was running at 100% usage and users were starting to be unable to use the web site.

After mitigating the issue, we delved deeper into the root cause. We stumbled across several websites suggesting similar sorts of fixes:

*   https://blogs.msdn.microsoft.com/sqlprogrammability/2008/11/26/optimize-for-unknown-a-little-known-sql-server-2008-feature/
*   http://www.sommarskog.se/query-plan-mysteries.html Both articles are great and were very useful. Particularly considering we do not have any Database Administers (DBAs) in my team. [The first link by Peter Scharlock](https://blogs.msdn.microsoft.com/sqlprogrammability/2008/11/26/optimize-for-unknown-a-little-known-sql-server-2008-feature/) is a bit friendlier to the .NET developer who is not a qualified DBA. Whilst the second [article by Erland Sommarskog](http://www.sommarskog.se/query-plan-mysteries.html) goes into great detail about many things I didn't know about SQL Server and was more relevant in explaining our specific problem. In our case the two queries had been running happily for some time and SQL Server had cached the optimum query plan. In the last few years my team added support for multiple Global Challenge programs to be run in a year, for instance a secondary challenge took place in September. This year was the first year where a Global Challenge was still running when the main May Global Challenge started in May. This meant SQL server had cached the execution plan for a smaller Global Challenge i.e. in the tens of thousands or people rather than hundreds of thousands. Consequently, when the query was executed it used the wrong execution plan and performed poorly. We went with two changes to fix our issue:
*   We wrapped the troublesome queries in stored procedures. This way we could fail fast and deploy potential fixes quickly.
*   We changed our query to use the ‘optimize for unknown’ optimizer hint.

> This hint directs the query optimizer to use the standard algorithms it has always used if no parameters values had been passed to the query at all. In this case the optimizer will look at all available statistical data to reach a determination of what the values of the local variables used to generate the query plan should be, instead of looking at the specific parameter values that were passed to the query by the application. 

[Read more at Peter Peter Scharlock's excellent blog post](https://blogs.msdn.microsoft.com/sqlprogrammability/2008/11/26/optimize-for-unknown-a-little-known-sql-server-2008-feature/)