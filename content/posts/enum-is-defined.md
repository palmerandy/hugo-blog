---
title: "Enum.IsDefined()"
date: 2018-02-06T14:07:11+11:00
draft: false
tags: ["did you know?", "enum", "dotNET"]
---

Did you know that you can assign any integral type value to an enum even if it is not part of the enum values?  

Consider the following int based enum.
``` 
public enum FunRunDistances
{
    FiveKilometers = 5,
    TenKilometers = 10,
    HalfMarathon = 21,
    Marathon = 42
}
```
Although 5, 10, 21 and 42 have been explicitly added as values of the ```FunRunDistances``` enum, the following code does not produce an error:

```
FunRunDistances funRun = (FunRunDistances)100;
```

Clearly you should not do this in your code as most developers will expect all enum based variables to bet set from a value that is defined in the enum.  This is all good and well, except sometimes you aren't the in control of how the value of enum is set.  For instance a Web API, or reading from a database value, etc.

#### Enter Enum.IsDefined(Type, Object) Method 

As per the [.NET docs](https://docs.microsoft.com/en-us/dotnet/api/system.enum.isdefined?view=netframework-4.7.1), this method:

> Returns an indication whether a constant with a specified value exists in a specified enumeration.

So we can use this method to determine whether the enum value falls within the defined values of the ```FunRunDistances``` enum:
```
var isDefined = Enum.IsDefined(typeof(FunRunDistances), funRun);
```