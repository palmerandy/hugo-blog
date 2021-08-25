---
title: "Partially inflated objects"
date: 2021-08-12T20:23:06+11:00
draft: false
tags: ["Latent Bugs", "Code maintainability", "dotNET"]
---

One thing that I, and many developers I talk to, find annoying is when you receive an object from any API that is partially inflated.  A partially inflated object is any object that you receive that has one or more properties not set.  

For instance consider an API that returns the following fictitious ```FunRun``` class and in particular the ```AthleteIds``` property.  
 
```
public class FunRun
{
    /// The unique identifier of a fun run
    [Required]
    public Guid Id { get; set; }

    [Required]
    public string Name { get; set; }

    public Guid[] AthleteIds { get; set; }
}
```

The ```FunRun``` class represents a particular fun run race, and the Identifiers of the Athletes who are participating in it, e.g. here is an example of the Melbourne Marathon with 3 athletes.

```
var melbourneMarathon = new FunRun { 
    Id = Guid.Parse("761DA327-C6DA-4B72-9EA3-8B3297620AB4"),
    Name = "Melbourne Marathon",
    AtheteIds = new [] {
        Guid.Parse("8F0F13C9-CFBA-462F-A95E-02F7BB4A4001"),
        Guid.Parse("8F0F13C9-CFBA-462F-A95E-02F7BB4A4002"),
        Guid.Parse("8F0F13C9-CFBA-462F-A95E-02F7BB4A4003")
    }
 }
```

Any developer consuming this class (either over the HTTP wire or internally in API), expects `FunRun.AthleteIds` to always contain the list of `AthleteIds`.  However if the object is partially inflated (i.e. not all fields are set) the developer may be surprised to find if the `FunRun.AthleteIds` set to null, e.g. consider: 

```
var emptyMarathon = new FunRun { 
    Id = Guid.Parse("761DA327-C6DA-4B72-9EA3-8B3297620AB4"),
    Name = "A Marathon with no participants",
    // Note: we aren't setting AthleteIds
 }
```

We had this exact scenario happen to two different developers at my current employer, which resulted in data loss as the partially inflated object was sent back to the server.  

### A latent bug

In my opinion a partially inflated object is a latent bug, latent being defined by:

> existing but not yet developed or manifest; hidden or concealed.

When you work in a small team or even by yourself it is easier for developers to know that some APIs return objects that are partially inflated and how to use these APIs and objects.

The problem occurs when the team scales and other developers aren't aware objects may be partially inflated.  This is compounded by the fact that depending on which properties aren't inflated it may not be immediately obvious the object isn't fully inflated.


### The take home

Always inflate all properties of a object to avoid for latent problems down the track.  If you have good reasons to not inflate a property, such as your client doesn't need it or it's costly, please create a object for that purpose (with the properties you do not need to inflate).  