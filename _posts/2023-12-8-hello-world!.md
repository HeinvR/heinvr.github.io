---
categories: [blog, Jekyll]
tags: [helloWorld, blog, jekyll, ruby]
---

Hello and welcome to my documentation blog! 

Lately i've been reinventing the wheel a bit to often when trying to find a solution to a problem i fixed before. It's gotten to a point where it annoys me enough to actually do something about it. I decided to centralize my notes in an attempt to reduce the time i spend searching through random text files, OneNotes, bookmarks, e-mails etc.. You get the picture. 

So i figured.. Why just not start a blog and write about it? Putting my notes into a blog will (hopefully) help me in multiple ways:
- Centralizing my notes, just one place to look!
- Putting it online will hopefully motivate me to improve quality, so my future self will actually understand what i wrote down.
- Making it easy to share.

The question is, what is the easiest way to accomplish this? I'm not really into blogging and unfamiliar with blogging solutions. So i just made a list of what i prefered and started searching online.
- To keep things simple i prefer to write in markdown.
- To keep my heartrate down and keep my environment happy, it would be wise to avoid frontend coding as much as possible.
- I would like it to be low maintenance and secure

After searching a while i came across [Jekyll](https://jekyllrb.com/) and this was exactly what i needed. Just a static website with no updates to install. And with GitHub pages i can host it for free! I also found a suitable [theme](https://github.com/cotes2020/chirpy-starter) to get me started and decided to give this one a go.

## Setup Jekyll on Windows
To install Jekyll i took the following steps.

- Copied the [template](https://github.com/cotes2020/chirpy-starter) repository to my own GitHub account.
- Cloned the repo to vscode on my local machine to make modications.
- Used winget to install Ruby (with devkit) on Windows.
```powershell
PS C:\Users\Hein> winget search rubyinstaller
Name                Id                                   Version  Source
-------------------------------------------------------------------------
Ruby 3.2 with MSYS2 RubyInstallerTeam.RubyWithDevKit.3.2 3.2.2-1  winget
Ruby 3.1 with MSYS2 RubyInstallerTeam.RubyWithDevKit.3.1 3.1.2-1  winget
Ruby 3.0 with MSYS2 RubyInstallerTeam.RubyWithDevKit.3.0 3.0.4-1  winget
Ruby 2.7 with MSYS2 RubyInstallerTeam.RubyWithDevKit.2.7 2.7.6-1  winget
Ruby 2.6 with MSYS2 RubyInstallerTeam.RubyWithDevKit.2.6 2.6.10-1 winget
Ruby 3.2            RubyInstallerTeam.Ruby.3.2           3.2.1-1  winget
Ruby 3.1            RubyInstallerTeam.Ruby.3.1           3.1.2-1  winget
Ruby 3.0            RubyInstallerTeam.Ruby.3.0           3.0.4-1  winget
Ruby 2.7            RubyInstallerTeam.Ruby.2.7           2.7.6-1  winget
Ruby 2.6            RubyInstallerTeam.Ruby.2.6           2.6.10-1 winget
```
```powershell
winget install RubyInstallerTeam.RubyWithDevKit.3.2
```

- Ran the bundle command to install Ruby dependencies

```ruby
PS C:\Users\Hein\blog\heinvr.github.io> bundle

Bundle complete! 6 Gemfile dependencies, 49 gems now installed.
```
- Started the local webserver to check things out

```ruby
PS C:\Users\Hein\blog\heinvr.github.io> bundle exec jekyll s
Configuration file: C:/Users/Hein/blog/heinvr.github.io/_config.yml
            Source: C:/Users/Hein/blog/heinvr.github.io
       Destination: C:/Users/Hein/blog/heinvr.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.73 seconds.
 Auto-regeneration: enabled for 'C:/Users/Hein/blog/heinvr.github.io'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```
- I just needed a few more modifications to the _config.yml to personalize the blog a bit more and i was ready to start writing.

## Setting up Github pages
After writing this initial post i wanted to publish the site online, this was way easier then expected. All it took was a modification to the GitHub repo and commiting the modifications to the main branch.

And we are live!