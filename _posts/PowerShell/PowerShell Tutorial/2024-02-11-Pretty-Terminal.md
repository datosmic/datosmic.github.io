---
title: Style your PowerShell/Terminal with Custom Prompt
image:
  path: /assets/img/2024-02-11-Pretty-Terminal/Pretty-Prompt1.png
date: 2024-02-11 17:30 +0530
categories: [PowerShell, Terminal]
tags: [PowerShell, scripting, shell, Windows PowerShell, PowerShell Core, Terminal, Oh-my-Posh]
---

## Let's get started

To make a pretty prompt for your terminal (PowerShell/bash/zsh). [Get the PowerShell][Get the PowerShell] from Windows Store or download the [latest version from Github][PS-GitHub].

## Get Windows Terminal

If you are using Windows 11 OS then Windows Terminal (Terminal) will be already installed else [Get the Windows Terminal from Microsoft Store or GitHub][Terminal] and set powershell as a default shell. Open settings from Windows Terminal -> Startup and set PowerShell as default shell. 

![Set-Default-Shell][Set-Default-Shell]

## Nerd Fonts 

Customized command prompts often use glyphs (a graphic symbol) in order to style the prompt. If your font does not include the appropriate glyphs, you may see several Unicode replacement characters '▯' throughout your prompt. In order to see all of the glyphs in your terminal, we recommend installing a [Nerd Font][Nerd-Font].

After downloading, you will need to unzip and install the font on your system. See [How to add a new font to Windows][How to add a new font to Windows].

![NerdFont][NerdFont]

## Install Oh My Posh 

[Oh My Posh][Oh My Posh] is a custom prompt engine for any shell that has the ability to adjust the prompt string with a function or variable.

To customize your PowerShell prompt, you can install Oh My Posh using [winget][winget]. winget installer is an executable installer like apt-get in linux. 

```powershell
winget install JanDeDobbeleer.OhMyPosh
```

Once Oh my Posh is installed then you can customize themes and segments as per your wish.

## PowerShell Profile

If you are opening your PowerShell profile for the first time then you can get to know where the profile is located by running `$PROFILE` in PowerShell terminal.

```powershell
notepad $PROFILE
```

If there is no default profile present then you can create one under Documents\PowerShell by running above command and save it.

Then edit `$PROFILE` and add the following line, remembering at this point that oh-my-posh is an executable on the PATH.

```powershell
oh-my-posh --init --shell pwsh --config ~/jandedobbeleer.omp.json | Invoke-Expression
```

## Custom Themes/Segments

You can customize the prompts as per wish by modifying the existing json files or creating a one from scratch. Here is my [customized version][ravikiran.omp.json] of prompt.

![Pretty-Prompt][Pretty-Prompt]


<!-- Reference Images -->
[Pretty-Prompt]: /assets/img/2024-02-11-Pretty-Terminal/Pretty-Prompt.png
[Set-Default-Shell]: /assets/img/2024-02-11-Pretty-Terminal/Set-Default-Shell.png
[NerdFont]: /assets/img/2024-02-11-Pretty-Terminal/NerdFont.png

<!-- Reference Links -->
[Get the PowerShell]: /posts/HowTo-PowerShell/
[PS-GitHub]: https://github.com/PowerShell/PowerShell
[Terminal]: https://learn.microsoft.com/en-us/windows/terminal/
[Nerd-Font]: https://www.nerdfonts.com/font-downloads
[How to add a new font to Windows]: https://support.microsoft.com/en-us/office/add-a-font-b7c5f17c-4426-4b53-967f-455339c564c1
[Oh My Posh]: https://ohmyposh.dev/
[winget]: https://learn.microsoft.com/en-us/windows/package-manager/winget
[ravikiran.omp.json]: /assets/files/ravikiran.omp.json