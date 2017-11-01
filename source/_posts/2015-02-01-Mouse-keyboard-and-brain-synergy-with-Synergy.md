title: Mouse keyboard and brain synergy with Synergy
date: 2015-02-01 19:01:49
tags:
---
I'm a big fan of [Teleport](http://www.abyssoft.com/software/teleport/) on Mac OS X. If you've never used it before, you probably haven't had to work with more than 1 Mac at a time. I'm not talking about remote access (SSH is my tool of choice for remote access). This is for an extended desktop experience with physically distinct machines. My mouse is connected to the Windows machine in the middle, Mac OS X on the laptop to the left and the Ubuntu guest VM console on the right all feel like one "extended desktop".

The problem with the tool is it's platform specific... no workie with Windows or Linux. After some google-fu the currently accepted alternative is [Synergy](http://synergy-project.org/). It's basically a cross-platform tool that does the same thing - allow a computer to share keyboard and mouse to hosts on the LAN. It's very nice, but here's some gotchas along with solutions I found along the way...
<!-- more -->

## Windows DPI scaling breaks mouse movement
After installing, configuring and enabling I was able to move the mouse once it went to the remote machine. This included moving it back... in essence the mouse and keyboard were "stuck". To resolve this:
- I stopped Synergy on the remote machine. This caused mouse control to return to the main computer.
- It turned out to be [this issue](https://github.com/synergy/synergy/issues/4041), so I disabled Windows DPI scaling on the synergys.exe (note the extra "s")
Restarting on both ends it now worked. 
- As a learning, I added a hotkey to send the keyboard back to the main computer in case I got stuck remote again.

## Long wireless beacon causes intermittent lag
I also had a problem where input took time to "restart" if I stopped doing things in the client. For example, I move my mouse over to the remote machine. At this point moving the mouse around and typing is flawless. If I stop for about 5 seconds, then start again there is a very obvious delay before input is recognised again.

Turns out this was to do with wireless settings in my router. I'm running Tomato firwmware on my router, and at some point in an effort to reduce noise had increased the Beacon Interval to 1000. Reducing this back to the 100ms resolved my issue. Thanks to [this thread](http://superuser.com/questions/553125/synergy-choppiness-is-my-wireless-router-delaying-packets-after-silent-period) for putting me on the right path to resolve.

All up the tool is great - I've only just began using it, but already it's getting out of the way and letting me get things done.