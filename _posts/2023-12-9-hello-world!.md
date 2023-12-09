---
categories: [blog, Jekyll]
tags: [helloWorld, blog, jekyll, ruby, github]
---

Lately, I've been reinventing the wheel a bit to often when trying to find a solution to a problem I fixed before. It's gotten to a point where it annoys me enough to actually do something about it. I decided to centralize my notes in an attempt to reduce the time I spend searching through random text files, OneNotes, bookmarks, e-mails etc.. You get the picture. 

So I figured.. Why just not start a blog and write about it? Putting my notes into a blog will (hopefully) help me in multiple ways:
- Centralizing my notes, just one place to look!
- Putting it online will hopefully motivate me to improve quality, so my future self will actually understand what I wrote down.
- Making it easy to share.

The question is, what is the easiest way to accomplish this? I'm not really into blogging and unfamiliar with blogging solutions. So I just made a list of what i preferred and started searching online.
- To keep things simple i prefer to write in markdown.
- To keep my heartrate down and keep my environment happy, it would be wise to avoid frontend coding as much as possible.
- I would like it to be low maintenance and secure.

After searching a while I came across [Jekyll](https://jekyllrb.com/) and this was exactly what I needed. Just a static website with no updates to install. And by using [GitHub Pages](https://pages.github.com/) I can host it for free! I also found a suitable [theme](https://github.com/cotes2020/chirpy-starter) to get me started and decided to give this one a go.

## Setup Jekyll on Windows
To install Jekyll I took the following steps.

- Copied the [template](https://github.com/cotes2020/chirpy-starter) repository to my own GitHub account.
- Cloned the repository to vscode on my local machine to make modications.
- Used winget to install Ruby (with devkit) on Windows.
```powershell
winget search rubyinstaller
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

- Ran the bundle command to install Ruby dependencies.

```ruby
PS C:\Users\Hein\blog\heinvr.github.io> bundle

Bundle complete! 6 Gemfile dependencies, 49 gems now installed.
```
- Started the local webserver to check things out locally.

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
- I just needed a few modifications to the _config.yml to personalize the blog a bit more and I was ready to start writing.

## Github pages
After writing this initial post i wanted to publish the blog online, this was way easier than expected. Github pages was allready setup for me. All it took was a commit to main and [we are live!](https://heinvr.github.io/)