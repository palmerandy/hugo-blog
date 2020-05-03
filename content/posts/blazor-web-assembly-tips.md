---
title: "Blazor Web Assembly Tips and gotchas"
date: 2020-04-30T14:10:55+11:00
draft: false
tags: ["blazor", "wasm", "wasm"]
section: "blog"
---

I have been experimenting with Blazor Web Assembly (WASM) which at the time of writing is in preview for ASP.NET Core 3.1.  Here are some quick tips and things to watch out for.

### Debugging WASM doesn't work (in firefox)
The team recently shipped Blazor WASM debugging support.  I followed the [excellent documentation](https://docs.microsoft.com/en-us/aspnet/core/blazor/debug?view=aspnetcore-3.1#enable-debugging-for-visual-studio-and-visual-studio-code) but couldn't get it to work.  The solution was to pay more attention to the documentation: debugging does not work in Firefox (version 75), as per the [documentation](https://docs.microsoft.com/en-us/aspnet/core/blazor/debug?view=aspnetcore-3.1#prerequisites)  
Debugging is only > supported in Chrome based browsers.
> Debugging requires either of the following browsers:
> - Microsoft Edge (version 80 or later)
> - Google Chrome (version 70 or later)

### Data Annotations not validating all properties
Again the [documentation](https://docs.microsoft.com/en-us/aspnet/core/blazor/forms-validation?view=aspnetcore-3.1#nested-models-collection-types-and-complex-types) is excellent and this specifically calls it out:
> Blazor provides support for validating form input using data annotations with the built-in DataAnnotationsValidator. However, the DataAnnotationsValidator only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.
> 
> To validate the bound model's entire object graph, including collection- and complex-type properties, use the ObjectGraphDataAnnotationsValidator provided by the experimental Microsoft.AspNetCore.Components.DataAnnotations.Validation package:

### Hosting statically on GitHub Pages fails with "Failed to find a valid digest in the 'integrity' attribute"
I followed Andrea Cirioni's blog on [Hosting Blazor WebAssembly app on GitHub Pages](https://dev.to/cirio/hosting-blazor-webassembly-app-on-github-pages-137k).  This worked great except when I was done, I was seeing the following error:
> Failed to find a valid digest in the 'integrity' attribute for resource 'https://palmerandy.github.io/Running-Pace-Calculator/_framework/wasm/dotnet.3.2.0-preview3.20168.1.js' with computed SHA-256 integrity 'xxx/yyyy='. The resource has been blocked.

After researching a while, I stumbled across this [github issue and solution from Steve Sanderson](https://github.com/dotnet/aspnetcore/issues/19796#issuecomment-598286345).  TLDR I copied [.GitAttributes file](
https://github.com/SteveSandersonMS/TestGithubPages/blob/master/.gitattributes), and added it to my Git repository with the following command:
{{< highlight csharp "linenos=table">}}
git add --renormalize .
{{< /highlight >}}

This meant my simply Blazor Web Assembly app would run succesfully on [GitHub Pages](https://palmerandy.github.io/Running-Pace-Calculator/).  Source code can be found in my [Pace Calculator Github Repository](https://github.com/palmerandy/Pace-Calculator)).