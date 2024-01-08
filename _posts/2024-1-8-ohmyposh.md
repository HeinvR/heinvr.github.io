---
categories: [posh]
tags: [posh, terminal, environment, Oh My Posh]
title: Setting up PowerShell with Oh My Posh and Nerd Fonts
---

Nowadays, I spend a lot more time in shells than the passed couple of years, which involved a lot of GUI work. It kind of reminds me of the start of my career in IT, when I used to do a lot of networking. Now it's mostly working with PowerShell, Git and IaC. The default PowerShell layout started to bother me and I knew there were fancy configurations out there. So I decided to spice up my shell interface!

# Oh My Posh
The first thing we need is a theme engine, which i found in [Oy My Posh](https://ohmyposh.dev/). This engine enables you to use the full color set of your shell by using colors to define and render the prompt. Oh My Posh has a lot of built-in themes and you can also create your own.

## Installing Oh My Posh
To install Oh My Posh on Windows client systems we can simply use Winget:

```
winget install JanDeDobbeleer.OhMyPosh
```

## Browse themes
After installing the package we need to set a theme. The easiest way to get started is to use a built-in theme. To browse built-in the themes you can use the following command:

```
Get-PoshThemes
```

## Selecting themes
To try out a theme, the following commmand can be used. Notice that i used the "Tokyo" theme for this example.
```
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\tokyo.omp.json" | Invoke-Expression
```

After running the command above, my shell looks like this:

![](../../_posts/img/TokyoThemeNoFont.png)

## Setting a default theme
When you found a theme which you want to setup as default, edit your PowerShell profile.

First open your PowerShell profile in notepad:
```
notepad $profile
```

Next paste the following code and save your file:

```
(@(& 'C:/Users/Hein/AppData/Local/Programs/oh-my-posh/bin/oh-my-posh.exe' init pwsh --config='C:\Users\Hein\AppData\Local\Programs\oh-my-posh\themes\tokyo.omp.json' --print) -join "`n") | Invoke-Expression
```
Now, your selected theme is being loaded when your shell is being started.

# Nerd Fonts
Perhaps you noticed there are some unrecognizable icons in the example theme we set previously. This is because the default installed fonts don't recognize the icons. 

![](../../_posts/img/TokyoThemeErrorFont.png)

To fix this we need to install a custom font. I used one of the Nerd Fonts to fix this issue. These fonts are available on [Github](https://github.com/ryanoasis/nerd-fonts). Just pick a font you like and download the font from the [release page]https://github.com/ryanoasis/nerd-fonts/releases(). After downloading, extract the archive and install the font.

## Setting the font in Windows Terminal

To set the installed font in Windows terminal go to Settings -> PowerShell -> appearance. Edit the "Font face" property by selecting your installed font. In this example i used the "FiraCode Nerd Font".

![](../../_posts/img/WindowsTerminalFont.png)

## Setting the font in VScode

To set the font in VSCode go to File -> Preferences -> Settings. In the Text Editor section you can specify your font.

![](../../_posts/img/VScodefont.png)

After setting your prompt, the icons in the the should be recognized and your prompt should look like this:

![](../../_posts/img/TokyoThemeFont.png)