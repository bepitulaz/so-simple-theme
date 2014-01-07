---
title: What To Do If MacPorts Fail To Do Self Update After Mavericks Upgrade
author: bepitulaz
layout: post
permalink: /2013/11/what-to-do-if-macports-fail-to-do-self-update-after-mavericks-upgrade/
categories:
  - Blog
  - Programming
tags:
  - mac
  - programming
  - tools
comments: true
---
My MacPorts was outdated this morning. So, I did sudo port -v selfupdate. Tadaaa&#8230;.I got this error.  
<!--more-->

[<img class="alignnone size-large wp-image-59" alt="MacPorts Error" src="http://asep.co/wp-content/uploads/2013/11/Screen-Shot-2013-11-18-at-10.35.14-AM-1024x410.png" width="696" height="278" />][1]

After stack-overflowing, I got the answer that the location of the Tcl framework has changed, it broke the existing MacPorts infrastructure. So, the solution is very simple, just download the binary installation from <a href="https://distfiles.macports.org/MacPorts/" target="_blank">https://distfiles.macports.org/MacPorts/</a> and install the latest version. I installed the MacPorts-2.2.1-10.9-Mavericks.pkg and MacPorts was running well again.

 [1]: http://asep.co/wp-content/uploads/2013/11/Screen-Shot-2013-11-18-at-10.35.14-AM.png