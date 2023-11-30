---
layout: post
title: GetLoggedOn - Remote Registry Enumeration
---

# Remote Registry to enumerate the logged on users?
---

During red team operations we, more time than not, are targeting the Active Directory. We have all been in the situation that we quickly want to check wether or not users are connected/logged in to certain devices, for let's say dumping their credentials if they are. Some tools already help us with that, for example Bloodhound/Sharphound does session enumeration using:

- NetWkstaUserEnum
- NetSessionEnum
- Remote Registry

And however it is quite easy to just run Sharphound or Bloodhound.py, it might be the case that we cannot open a socks proxy or just run a .NET assembly in memory without being detected. So how to fix this? Well I tried writing a quick a dirty Cobalt Strike BOF to help us with just that.

# Original Idea

So a while back I saw this tweet: 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Following <a href="https://twitter.com/splinter_code?ref_src=twsrc%5Etfw">@splinter_code</a> idea, you can also start RemoteRegistry remotely. This way you can check on which server DAs are connected, in case you want dump their creds. This script could help: <a href="https://t.co/xmAzF2k81c">https://t.co/xmAzF2k81c</a><br>It works from low privileged user 😉 <a href="https://t.co/KXgPsCGNXP">pic.twitter.com/KXgPsCGNXP</a></p>&mdash; Geiseric (@Geiseric4) <a href="https://twitter.com/Geiseric4/status/1719764121111908510?ref_src=twsrc%5Etfw">November 1, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

He tweeted about a python script that does just what we want to accomplish with our BOF:

- Start the Remote Registry (if not running)
- Session enumeration using Remote Registry
- SID to accountname conversion





# Project

WIP - BOF almost ready
[Cobalt Strike - GetLoggedOn BOF](https://github.com/0xSH4RKS/getloggedonBOF)

# Sources

- [Bloodhound Inner Workings - Part 3](https://blog.compass-security.com/2022/05/bloodhound-inner-workings-part-3/)