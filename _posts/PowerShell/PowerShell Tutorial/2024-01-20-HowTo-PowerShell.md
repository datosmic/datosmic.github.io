---
title: PowerShell Overview
date: 2024-01-20 19:30 +0530
categories: [PowerShell, PowerShell Tutorial]
tags: [PowerShell, scripting, shell, Windows PowerShell, PowerShell Core, PowerShell Tutorial]
---

## Introduction

There are 2 types of PowerShell versions which can be present in a system. `version 5.x` - Example: `5.1.22621.2506` which we call it as `Windows PowerShell` or `PowerShell Desktop Edition` and other is `version 7.x and above` and we call it as `PowerShell Core` - Example: `7.4.0` which can be run on Windows/Linux/Mac OS machines (platform independent).

## How to Open PowerShell

#### Windows PowerShell

1. To Open, Search for `PowerShell` in Windows Search   
    In Windows 10:

    ![Windows-PowerShell-Search-Win10][Windows-PowerShell-Search-Win10]
    
    In Windows 11:

    ![Windows-PowerShell-Search-Win11][Windows-PowerShell-Search-Win11]

2. To Open, Select `Windows PowerShell` and Open

    ![Windows PowerShell][Windows PowerShell]

#### PowerShell Core

PowerShell Core is .NET Core version and platform independent. To run PowerShell Core Edition, we need to first [install PowerShell Core](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.4) in our system. Once its installed in the system then 

1. To Open, Search for `PowerShell` in Windows Search   

    ![PowerShell-Core-Search-Win11][PowerShell-Core-Search-Win11]

2. Select `PowerShell` and Open

    ![PowerShell][PowerShell]

#### Windows Terminal

If you have already installed [Windows Terminal](https://github.com/microsoft/terminal) in Windows 10 or you are on Windows 11, you can open PowerShell from Windows Terminal as well.

1. To Open, Search for `Terminal` or `Windows Terminal` in Windows Search

    ![Windows-Terminal-Search-Win11][Windows-Terminal-Search-Win11]

2. We can select the PowerShell Edition directly we want to open (if both Windows/Desktop and Core Editions exist) or Open Terminal and then select the Profile we want to open by clicking on the expand button (down arrow)

    ![Windows-Terminal-PowerShell][Windows-Terminal-PowerShell]


#### Windows PowerShell ISE

[Windows PowerShell ISE or Windows PowerShell Integrated Scripting Environment (ISE)](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/ise/introducing-the-windows-powershell-ise?view=powershell-7.4) is a scripting environment for PowerShell Desktop Edition. We can have an interactive session with script pane (editor) and Commands Pane.

1. To Open, Search for `PowerShell ISE` in Windows Search   

    ![Windows-PowerShell-ISE-Search-Win11]Windows-PowerShell-ISE-Search-Win11]

2. Select `PowerShell ISE` and Open

    ![PowerShell-ISE][PowerShell-ISE]


## Retrieve PowerShell Version

To retrieve PowerShell version
1. [Open PowerShell](#how-to-open-powershell)
3. Type the command `$PSVersionTable` and hit enter

We will see output something similar to this below output for `Windows Powershell` and also we can see the PSEdition in the list is shown as `Desktop`.

``` powershell
 $PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.22621.2506
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.22621.2506
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

We will see output something similar to this below output for `Powershell Core` and also we can see the PSEdition in the list is shown as `Core`.

```powershell
 $PSVersionTable

Name                           Value
----                           -----
PSVersion                      7.4.1
PSEdition                      Core
GitCommitId                    7.4.1
OS                             Microsoft Windows 10.0.22631
Platform                       Win32NT
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```


<!-- Reference Images -->
[Windows PowerShell]: /assets/img/2024-01-20-HowTo-PowerShell/Windows-PowerShell.png
[Windows-PowerShell-Search-Win10]: /assets/img/2024-01-20-HowTo-PowerShell/Windows-PowerShell-Search-Win10.png
[Windows-PowerShell-Search-Win11]: /assets/img/2024-01-20-HowTo-PowerShell/Windows-PowerShell-Search-Win11.png
[PowerShell-Core-Search-Win11]: /assets/img/2024-01-20-HowTo-PowerShell/PowerShell-Core-Search-Win11.png
[PowerShell]: /assets/img/2024-01-20-HowTo-PowerShell/PowerShell-Core.png
[Windows-Terminal-Search-Win11]: /assets/img/2024-01-20-HowTo-PowerShell/Windows-Terminal-Search-Win11.png
[Windows-Terminal-PowerShell]: /assets/img/2024-01-20-HowTo-PowerShell/Windows-Terminal-PowerShell.png
[Windows-PowerShell-ISE-Search-Win11]: /assets/img/2024-01-20-HowTo-PowerShell/Windows-PowerShell-ISE-Search-Win11.png
[PowerShell-ISE]: /assets/img/2024-01-20-HowTo-PowerShell/PowerShell-ISE.png
