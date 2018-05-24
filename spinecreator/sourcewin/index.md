---
layout: default
title: Building SpineCreator from source on Windows
---
# Building on Windows

SpineCreator is built using the Qt toolkit
[https://www.qt.io](https://www.qt.io), a cross-platform library of C++ code for
building desktop apps. This means that SpineCreator can be compiled on
Windows just as well as on Mac or Linux. However, we only support Mac
and Linux at present as "first class citizens", due to a lack of
developer resource.

SpineML_2_BRAHMS in particular would require some work to function on
Windows, as it includes bash scripts. Microsoft have recently added
bash for Windows 10 developers, so in the near future it may be easier
to bring the SpineML toolchain to Windows.

For now, if you would like to use SpineCreator on Windows as an editor
(without the use of the simulator back-end), then please go ahead and
install Qt, graphviz and python, then compile SpineCreator in
QtCreator.
