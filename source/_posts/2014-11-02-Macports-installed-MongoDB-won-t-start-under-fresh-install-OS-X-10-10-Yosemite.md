---
title: "Macports-installed MongoDB won't start under fresh install (OS X 10.10 Yosemite)"
date: 2014-11-02 11:02:00
tags:
- macports
- mongodb
---
I had problems getting MongoDB to start using launchd (my preferred method for portability) after a fresh Yosemite install. After initially suspecting the `_mongo` user was not being correctly created by the Macports installation script, it turned out to be a simple permissions issue. <!-- more --> After doing:
```
sudo port load mongodb
```
all looked well, no errors. In the console log however I was getting this:
```
... com.apple.xpc.launchd[1]: (org.macports.mongodb) Service only ran for 0 seconds. Pushing respawn out by 10 seconds.
```
And `ps aux | grep mongo` was not showing any running processes. Eventually OS X did not like my daemon:
```
... com.apple.xpc.launchd[1]: (org.macports.mongodb) This service is defined to be constantly running and is inherently inefficient.
```
So, after ensuring xCode was installed I checked how this service was starting:
```
open /opt/local/etc/LaunchDaemons/org.macports.mongodb/org.macports.mongodb.plist
```
This revealed the user account, and a variety of dependent files/folders that launchd was looking for to start this service. Of note:

`_mongo` - the user the server process will run under

/opt/local/bin/mongod - the folder where the server binary is located

/opt/local/var/db/mongodb - the default --dbpath option to mongod, where the data is stored

/opt/local/var/log/mongodb/mongodb.log  - the default --logpath option to mongod, where the log file is stored

First and most obvious, I checked if the system account existed:
```
dscl . -ls /Users | grep mongo
```
Looked fine, I saw `_mongo` as expected per the daemon plist. Turns out in my case something (another Macports package most likely) had changed the permissions for one of the parent folders of the log file. This resolved for me:
```
sudo chmod 755 /opt/local/var/log
```
It was (incorrectly) set to 744, while readable could not be descended into. A subsequent unload/reload of the port allowed me to connect to the mongo CLI successfully. Unfortunately through this issue there was very little error logging, and what I had was ambiguous... if you find yourself in a similar situation check each of these places (and the parent folders back to root).
