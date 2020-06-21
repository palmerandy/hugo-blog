---
title: "C# SQL Database pagination using Dapper ORM"
date: 2019-11-11T20:08:07+22:00
draft: false
tags: ["Dapper", "sql", "pagination", ".NET"]
section: "blog"
---

Database Pagination is something I have coded at every employer I have worked at.  My approach has changed over the years, here is a look at how I would go about it now.

The requirements for this incarnation:

* One database query should go across the wire, and we should both the Total Row Count as well as the Rows of the current page.  Only the rows on a page should be returned from the database.
* Support for querying ordering columns of more than one SQL type
* SQL server database

{{< gist palmerandy f41f85b91274feb88be627e1c30d7347 >}}

Note the query uses [offset and fetch clauses](https://docs.microsoft.com/en-us/sql/t-sql/queries/select-order-by-clause-transact-sql?view=sql-server-ver15#using-offset-and-fetch-to-limit-the-rows-returned) to limit the number of rows sent over the wire.

Things that will be improved shortly:

* support for resilency
* support for isolation level / read uncommitted (https://www.carlrippon.com/scalable-and-performant-asp-net-core-web-apis-sql-server-isolation-level/)
* making more generic as more use cases are used
* validation of invalid requests

If you have any feedback please leave it on this blog or the GitHub Gist.