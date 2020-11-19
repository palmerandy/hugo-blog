---
title: "ASPNET Core appsettings are stored in a dictionary"
date: 2020-11-18T14:05:20+53:00
draft: false
tags: ["ASPNET Core", "appsettings.json", "did you know", "learning in the open"]
---

Today I learned the ASPNET Core reads appsettings into a key-value pair dictionary.  This is outlined in the [Hierarchical configuration data section of the Configuration documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1#hierarchical-configuration-data-1):

> The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.

This can be problematic if this isn't known.  For instance consider an array in your `appsettings.json`:

```
{
  "myproperty": {
    "myarray": [
      {
        "name": "one"
      },
      {
        "name": "two"
      }
    ]
  }
}
```
This would flatten into:

```
"myproperty:myarray:0:name" = "one"
"myproperty:myarray:1:name" = "two"
```

The problem occurs when you go to override the array in another environment `appsettings.production.json`:

```
{
  "myproperty": {
    "myarray": [
      {
        "name": "override all elements of the myarray"
      }
    ]
  }
}
```

Before knowing this I would have expected one element of `myarray` containing the value "override all elements of the myarray".  I now know this wouldn't be the case, and that it would actually would be:

```
"myproperty:myarray:0:name" = "override all elements of the myarray"
"myproperty:myarray:1:name" = "two"
```

That extra appsetting value might be fine, or it might be a dragon - hence this blog post.

The learning for me is to only put values into `appsettings.json` that you want to have in each environment. For me I'll now use `appsettings.local.json` as a local environment.

Hope this helps you out!