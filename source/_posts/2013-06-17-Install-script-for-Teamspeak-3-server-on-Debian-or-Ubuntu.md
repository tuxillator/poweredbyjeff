title: Install script for Teamspeak 3 server on Debian (or Ubuntu)
date: 2013-06-17 19:39:00
tags:
- teamspeak3
- serveradmin
- programming
---
The 'teamspeak-server' package built for Debian/Ubuntu at the time of writing this article is only for Teamspeak version 2, but everyone who is anyone these days uses TS3. After trolling around the Internet looking for a prebuilt package I could trust, I decided to roll my own install script. For the impatient **here are the steps to get a Teamspeak 3 server installed on Debian-based Linux quickly**:
1. Try this in a test environment first! It requires root privileges and can take your children. You have been warned.
2. Download and transfer installts3.sh to your server.
3. Download and transfer the correct server binary to your server.
4. Run `installts3.sh`:
```
chmod u+x installts3.sh
sudo ./installts3.sh serverbinary.tar.gz
```
Replace serverbinary.tar.gz with the name of the file you downloaded from the Teamspeak website. If there are no errors the script will tell you where to get your ServerAdmin privilege key and you are good to go!
<!-- more -->
For the curious, below is the detail on why I rolled my own and what features are added that aren't out of the box. First credit where credit is due - these 2 websites are the principle inspiration for this script:
- http://coding-journal.com/teamspeak-3-installation-script-for-debianubuntu/
- http://forum.teamspeak.com/showthread.php/74883-TUTORIAL-Teamspeak3-Server-w-MySQL-Databse-on-Debian-Ubuntu

**Run Teamspeak daemon as unprivileged user**: this one was important and lacking in most TS3 install scripts I saw. I have other services running on the same VPS that I run Teamspeak on and would be fairly unhappy should a bug in Teamspeak cause my whole server to be compromised. It is a security best practice to run things like this under their own account.

**Define install location**: most scripts I saw hardcoded the location, or installed in the home folder of the account created. This installs into /opt/teamspeak3 by default, which is a more generic location.

**Create an init.d script for convenient start/stop/start on boot**: this will also create an init.d script properly adjusted with the user and install path defined above. It will also conveniently set Teamspeak 3 to start on boot. For those that don't care for my script but want to roll their own init.d script quickly, a template script is located at /etc/init.d/skeleton in Debian systems. Start with that and customize so you have an LSB compliant init.d script.

The latest install script can be downloaded from [my Github](https://github.com/tuxillator/installts3)... if you spot any bugs and fix them send me a pull request or drop a note in the comments. Peace!