---
title: Sniff iPhone traffic using Tomato router firmware
date: 2013-07-08 18:48:00
tags:
- tomato
- networking
---
I had a need recently to sniff traffic going to an iPhone at home. Nothing nefarious; just trying to figure out why iCloud backup was not working. Anyhow, a decent (and secure) wifi router would typically only send data to the destination hardware address, meaning I could not see the iPhone's traffic using tcpdump or Wireshark. In effect what I wanted to do is set up a SPAN port in the Tomoto wifi world. Here's how I did it...
<!-- more -->

**Enable the ipt_ROUTE module**:
```
/sbin/modprobe ipt_ROUTE
```
If this doesn't work upgrade your firmware to something more recent.
**Forward the traffic**:
```
iptables -A PREROUTING -t mangle -d iptomirror -j ROUTE --gw iptosendto --tee
iptables -A PREROUTING -t mangle -s iptomirror -j ROUTE --gw iptosendto --tee
iptables -A POSTROUTING -t mangle -d iptomirror -j ROUTE --gw iptosendto --tee
iptables -A POSTROUTING -t mangle -s iptomirror -j ROUTE --gw iptosendto --tee
```
Replace `iptomirror` with the IP of the iPhone/wifi device you wish to listen to. Replace `iptosendto` with the IP of the computer running tcpdump/Wireshark. The above 4 commands leverage the `--tee` option provided by the ipt_ROUTE kernel module and will:
1. Find any packet with source/destination IP of iptomirror, then
2. Duplicate both of these classes of packets packet and send a copy to iptosendto

These commands should do the trick to sniff traffic not just for iPhones, but for any device on connected to a Tomato router (and possibly DD-WRT, but I haven't tried). If you're having problems double check the ipt_ROUTE module is loaded:
```
lsmod | grep ipt
```
And check your rules made it into the mangle tables:
```
iptables -L -t mangle
```
You should see your entries in the PREROUTING and POSTROUTING chains.

To stop the mirroring, either reboot the router or  remove the entries from the chains using the same commands as creating, but replace -A (add) with -D (delete):
```
iptables -D PREROUTING -t mangle -d iptomirror -j ROUTE --gw iptosendto --tee
iptables -D PREROUTING -t mangle -s iptomirror -j ROUTE --gw iptosendto --tee
iptables -D POSTROUTING -t mangle -d iptomirror -j ROUTE --gw iptosendto --tee
iptables -D POSTROUTING -t mangle -s iptomirror -j ROUTE --gw iptosendto --tee
```
Feel free to comment below if you're having problems, otherwise happy spying... ahem... troubleshooting!