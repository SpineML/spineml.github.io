---
layout: default
title: Prebuilt package installation
---

Download/Install
----------------

The latest binary release of the SpineML toolchain was made in January 2016. We provide packages for Mac, Debian and Ubuntu.

### Mac

The Mac installation package is available here: [spinecreator-0.9.6.dmg]

**Note:** *This is stored on a non-commercial web server which does not have a commercially signed SSL certificate; you will have to Okay some security warnings to access the file!*

### Debian

Debian packages are available for Jessie (Debian 8) and unstable (a.k.a. Debian Sid). Download the 4 .deb files for your architecture (amd64 for 64 bit; i386 for 32 bit).

**Debian Jessie debs** are here: <https://sebjames.zapto.org/owncloud/index.php/s/SKp02hTaoLy7rRh>

**Unstable/sid debs** are here: <https://sebjames.zapto.org/owncloud/index.php/s/grJuQkLUKZiUIWJ>

Now install the 4 .deb files:

`sudo dpkg -i brahms_0.8.0-1_amd64.deb spineml-preflight_0.1.0-1_amd64.deb \`
`             spineml-2-brahms_1.1.0-1_amd64.deb spinecreator_0.9.6-1_amd64.deb`

### Ubuntu

Ubuntu packages for Ubuntu versions 14.04 (trusty) and above are available from the following Personal Package Archive: <https://launchpad.net/~sebjames/+archive/ubuntu/spineml>

To install, open a terminal and do the following:

`sudo add-apt-repository ppa:sebjames/spineml`
`sudo apt-get update`
`sudo apt-get install spinecreator`

### A note about Windows

Unfortunately, we don't have the resources to prepare Windows builds of SpineCreator and SpineML-2-BRAHMS, although some work has been carried out towards this goal. If you're interested in making Windows builds, please contact Seb James.

  [spinecreator-0.9.6.dmg]: http://sebjames.zapto.org/owncloud/index.php/s/ScNR5109JQA6YzR
