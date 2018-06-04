---
layout: default
title: Building SpineCreator from source on Windows
---
# Building on Windows

SpineCreator is built using the Qt toolkit
[https://www.qt.io](https://www.qt.io), a cross-platform library of C++ code for
building desktop apps. This means that SpineCreator can be compiled on
Windows just as well as on Mac or Linux. Previously, we only supported Mac
and Linux as "first class citizens", due to a lack of developer
resource.

SpineML_2_BRAHMS in particular would have required much work to
function on Windows, as it includes bash scripts. Luckily, Microsoft
have recently added bash for Windows 10. Now that it's possible to
install bash on Windows 10, it's possible to build and run
SpineCreator and SpineML_2_BRAHMS on Windows. Here is a work in
progress to describe how to build SpineCreator and SpineML_2_BRAHMS on
Windows 10.

### Install Windows Subsystem for Linux

A new feature in Windows 10 is the ability to install a Linux
distribution to provide the bash shell and other Unixey utility
programs, such as ssh, git and so on. So, the first task is to install
it. Here are Microsoft's own instructions:

https://docs.microsoft.com/en-us/windows/wsl/install-win10

During this process, choose Ubuntu in the store (other Linux flavours
may well work, but I've tested Ubuntu).

### Install the Qt build environment

I installed Qt from https://www.qt.io choosing the open source option
- all the code in SpineCreator and friends is compatible with the
requirements for using Qt under the free LGPL licence. In the
installer, I scrolled to Qt 5.8 and selected only the "MinGW..."
checkbox.

### Install graphviz

http://graphviz.org/

### Install python

Anaconda?
