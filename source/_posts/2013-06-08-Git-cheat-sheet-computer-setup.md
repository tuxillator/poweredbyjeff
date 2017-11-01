title: Git cheat sheet - computer setup
date: 2013-06-08 00:01:00
updated: 2015-01-26 11:55:00
tags:
- git
- programming
---
*_January 2015 update:_ these days I generally use the powerful [SourceTree tool from Atlassian](http://www.sourcetreeapp.com). While it's great to be a purist, sometimes I just need to get the "job done". The below still applies when I'm working exclusively in a shell (generally on a remote box).*

These are some post-installation commands I use to get Git quickly into a working, friendly to use state - cherry-picked from http://git-scm.com/book/en/Customizing-Git-Git-Configuration:
```
# Required. Tell git who you are... appears in commit logs.
git config --global user.email "youremail@example.com"
git config --global user.name "Your Name"

# Optional! Some preferences to ease the eye and editing
git config --global core.editor nano
git config --global color.ui true
```

This list may grow/update over time, but this I find gets me out of the gate and over the minor annoyances quickly when using git on a fresh machine.
<!-- more -->
