---
title: "C# SQL Database pagination using Dapper ORM"
date: 2019-11-11T20:08:07+22:00
draft: false
tags: ["Dapper", "sql", "pagination"]
section: "blog"
---

Database Pagination is something I have coded at every employer I have worked at.  Here is a look at how I would go about it in 2020.

The requirements for this incarnation:

* SQL server database
* Support for querying ordering columns of more than one SQL type
* One database query should go across the wire, and we should both the Total Row Count as well as the Rows of the current page

{{< gist palmerandy f41f85b91274feb88be627e1c30d7347 >}}

Things that will be improved shortly:

* support for resilency
* support for isolation level / read uncommitted
* making more generic as more use cases are used
* validation of invalid requests

If you have any feedback please leave it on this blog or the GitHub Gist.