---
title: "Living under a rock - Powershell Core"
date: 2020-09-27T19:11:45+10:00
draft: true
tags: ["Powershell", "Powershell Core", "Pester", "Unit Testing", "did-you-know","vs-code"]
---

Recently I learnt that there have been many great advances in Powershell in recent years.  This post is to highlight some of them to developers who may have also been turning a blind eye.

### Powershell CORE exists, and it's cross platform
I'm a bit late to the party with this one - it was announced in 2016.  If you didn't know, now you do, you can get more information on it [https://github.com/PowerShell/PowerShell](https://github.com/PowerShell/PowerShell) or you can install it with chocolately:

```
    choco install powershell-core
```

### Requires statements:
Powershell has an excellent ```#requires``` statement

> The #Requires statement prevents a script from running unless the PowerShell version, modules (and version), or snap-ins (and version), and edition prerequisites are met. If the prerequisites aren't met, PowerShell doesn't run the script.

More information can be found [on docs.microsoft.com](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_requires?view=powershell-7.1).

Specifically I made heavy use of two statements, this gem specifies that the PowerShell session in which you're running the script must be started with elevated user rights:
```
#requires -RunAsAdministrator
```
The following ensures the developer running your script is using Powershell Core 
```
#requires -PSEdition Core 
```

### How to create powershell modules

You can create your own powershell modules.  Here is some example code showing:
- Powershell module, 
- Powershell module manifest 
- Unit test (with mocks) using Pester.

{{< gist palmerandy 15f3542112237ba4f0521db6e4bf2896 >}}

Some useful links:
https://docs.microsoft.com/en-us/powershell/scripting/developer/module/how-to-write-a-powershell-script-module?view=powershell-7.1

https://docs.microsoft.com/en-us/powershell/scripting/developer/module/how-to-write-a-powershell-module-manifest?view=powershell-7.1

https://github.com/pester/Pester

### VS Code:
VS Code has excellent support for Powershell Core.

You can [select this from the version number in the lower right corner](https://code.visualstudio.com/docs/languages/powershell#_multiversion-support).