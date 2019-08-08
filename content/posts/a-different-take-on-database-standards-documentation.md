---
title: "A different take on database standards documentation"
date: 2016-02-11T18:02:11+11:00
draft: false
tags: ["coding-standards", "sql"]
---

In my opinion the two biggest drawbacks of technical documentation for developers are:

1.  It is rare to find technical documentation that not up to date. Teams rarely treat documentation as a living document, and without a concerted effort by the team to keep it up to date goes out of date.
2.  The documentation is not in front of developers when they need it most. For instance a new starter might read the database standards documentation in their first week, and not when they create something in the database.

Rather than have a lengthy word document, our team has opted for an example SQL script file that lives in the codebase. This overcomes the above obstacles:

1.  It is a living document - if something is lacking from it, a developer can edit it with suggestions. As it is part of the codebase the change is appropriately versioned in source control.
2.  The example SQL file literally lives in the codebase in the parent folder where we commit our SQL changes. This means a developer can easily refer to it or copy and paste from it when creating a new table, stored procedure, index, etc..

Here is our example SQL script file (with a few changes to make it more generic):

{{< gist palmerandy 7f4d472bcb239727f495d3360f95e050 >}}