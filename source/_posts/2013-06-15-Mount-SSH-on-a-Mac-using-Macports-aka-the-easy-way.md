---
title: Mount SSH on a Mac using Macports (aka the easy way)
date: 2013-06-15 02:32:00
tags:
- macports
- sshfs
- serveradmin
---
Often I would like to access files over SFTP but without the limitations of an FTP client. For example, it's handy to alter remote files directly in terminal as if they are local, or open them with my nice, fully fledged editor. Mucking around downloading/uploading files each time I modify them gets annoying quickly, especially with something fiddly like web development.

Fortunately SSHFS is now in [MacPorts](http://www.macports.org), and provides a quick solution for those on a Mac with SSH access. I could of course configure a file server but that is not always possible, particularly if you don't have the ablity to install software on the remote machine!

<!-- more -->

There are various ways I have seen to install SSHFS but this has been simplified with the port now available. To get this going quickly:

1. Install [MacPorts](http://www.macports.org).
2. Run the following to install SSHFS (Fuse will also automagically install):
```
sudo port install sshfs
```
The `sshfs` command is very simple and follows syntax similar to scp. **To mount an SFTP directory** customize the following and run in terminal ([sudo is not needed and not recommended](http://fuse.sourceforge.net/sshfs.html)):
```
mkdir [local_directory]
sshfs [username]@[hostname]:/[remote_directory] /[local_directory] -ocache=no
```


Breaking down the command:

* [username] is the username on the remote machine. If this matches your local machine, you do not need it.
* [hostname] is the hostname (or IP address) of the remote machine.
* [remote_directory] is the directory on the remote machine you want to mount. Leave it as just / if you want to mount the whole remote root.
* [local_directory] is a directory created on your local machine. I like using /tmp/sshfs.
* -ocache=no disables caching and is optional, but I highly recommend it if you are doing anything that involves a high number of * file changes (like the LAMP example above).

After running the command you will be challenged with the usual password for the remote account (or not if you have setup SSH keys). **To unmount, the standard `umount` does the trick**:
```
umount [local_directory]
```
If you're having problems, test connecting with SSH first to make sure the remote_server, username and password are all correct.