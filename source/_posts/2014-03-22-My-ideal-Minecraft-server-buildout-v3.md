title: My ideal Minecraft server buildout (v3)
date: 2014-03-22 18:00:00
tags:
- minecraft
- serveradmin
---
So after much consideration, I updated and tightened my private Minecraft server. TL;DR:

* Minecraft server hostname (copy/paste to join): minecraft.poweredbyjeff.com
* Teamspeak3: [ts3.poweredbyjeff.com](ts3server://ts3.poweredbyjeff.com)
* Contact any Arbiter to get promoted to build access.

My brief notes here only cover the software side of the setup; I'll be doing something interesting once I build out my new home hypervisor (currently waiting on 1GB/s internet connection to get installed).
<!-- more -->
A few enhancements/game philosophy changes:

* Rolled back to our old map, aka the "Sanctum" map. I found the regulars didn't play as much as the feeling of effort involved in the old map was lost. For the folks that built in the new world, I moved them to the old map via WorldEdit.
Moved to Spigot, latest version. If you are a Minecraft server admin (big or small) and are not using this, you haven't lived.
Set up BungeeCord in front of the setup. This is a simple but very powerful proxy for the Minecraft protocol. At the moment I only use this as a convenient way for me to run a testing server beside the main one transparently, and run some custom game modes/"resettable" servers. This will help if I choose to move to a local hypervisor.
* Created a "Lobby" world, both for aesthetics (it looks like heaven!) and a convenient way to mix and match Bungee portals with Multiverse portals transparently to the players.
* Set up a CNAME record of minecraft.poweredbyjeff.com to prepare the move of the server to my house.
* Rebuilt the server folder from the ground up. Cut out some rubbish plugins. Set the "world" folder in spigot.yml to prevent rubbish acculating in the root of each server.
* Moved to a tmux style server management model. All the scripts I looked at were either crippled/unmaintained since 1.7 was released or paid. I'm a tightass on principle here, so opted for run my own. Screen also sucks balls; tmux is another thing you had better try before you die from being throttle to legacy death by Screen.

The main outstanding issue is automated backup and crash recovery, which is not a small item. The backup process is manual at the moment and has a pretty big flaw (that being me :) ). Feel free to ask more about my setup in the comments.
