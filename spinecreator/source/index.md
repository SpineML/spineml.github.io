---
layout: api
title: Building SpineCreator from source on a Mac
---

# Building on a Mac

Because we are a small team, we do not have the resources to produce
regular packages for SpineCreator and the SpineML toolchain.  For this
reason, we recommend that users compile SpineCreator from source and
we have some easy to follow instructions here! Please tell us when you
have problems by submitting issues on the github project pages (for
example
[https://github.com/SpineML/SpineCreator/issues](https://github.com/SpineML/SpineCreator/issues))

SpineCreator is built using the Qt toolkit
[http://qt.io](http://qt.io), a cross-platform library of C++ code for
building desktop apps. To run models, SpineCreator uses a SpineML
toolchain. On this page, we describe how you can compile
SpineML_PreFlight, SpineML_2_BRAHMS and BRAHMS as your SpineML
toolchain.

This build process was last tested in June 2016.

## Mac prerequisites

To build the software, you'll need a compiler (Xcode) and some build
tools including CMake, the Qt development system and the headers for a
couple of necessary libraries.

### Xcode

You will need to install Xcode. This is used to compile popt,
graphviz-devel, as well as SpineCreator and its components.

Install Xcode from the App Store, assuming you have the latest Mac
OS. If you're using an older Mac OS, you'll have to find the matching
version of Xcode from:
[https://developer.apple.com/downloads/](https://developer.apple.com/downloads/)

### CMake

CMake is a build-coordinating system. We use it to build BRAHMS,
SpineML_PreFlight and the SpineML_2_BRAHMS tools.

Download and install from:
[https://cmake.org/download/](https://cmake.org/download/)

### Mac Ports

You will probably want to install Mac Ports. This is used to install
popt and graphviz-devel. It's not the only way; if you prefer an
alternative, use that.

Install Mac Ports from:
[https://www.macports.org/install.php](https://www.macports.org/install.php)

You can verify your installation by opening a terminal on your Mac and typing

```
 port
```

A program should run. Type "quit" to exit.

### Libraries

Once Xcode and Mac Ports is installed, you can install the
prerequisite libraries popt and graphviz like this (in a terminal):

```
 sudo port install popt
 sudo port install graphviz-devel
```

### Qt

One more prerequisite is Qt, which is required by SpineCreator. You
can install this with the Qt online installer from
[https://www.qt.io/download/](https://www.qt.io/download/)

## SpineML_PreFlight

SpineML_PreFlight is a program which prepares a SpineML model for a
simulator backend such as SpineML_2_BRAHMS. It's coded in C++ and
depends only on one library called popt (for the command-line
interface).

First make sure you installed popt, as described above in the
prerequisites section.

Clone a copy of SpineML_PreFlight:

```
 git clone https://github.com/SpineML/SpineML_PreFlight.git
```

Build and install SpineML_PreFlight using cmake:

```
 cd SpineML_PreFlight
 mkdir build
```

Now open CMake. In the CMake window, navigate to the SpineML_PreFlight
directory as the "source" and for "where to build" navigate to
SpineML_PreFlight/build. Press "configure" then "generate". Now go
back to your terminal:

```
 make -j4
 sudo make install
```

## BRAHMS

BRAHMS is the "execution middleware" used by SpineML_2_BRAHMS.

Clone the official version of BRAHMS (which sports a
nice cmake compile and install scheme and supercedes any code
available from sourceforce):

```
 git clone https://github.com/BRAHMS-SystemML/brahms.git
```

Build brahms in "standalone" mode and have it installed in your home
directory with cmake:

```
 cd brahms
 mkdir build
 cd build
 cmake -DSTANDALONE_INSTALL=ON -DCOMPILE_WITH_X11=OFF \
       -DLICENCE_INSTALL=OFF -DCMAKE_INSTALL_PREFIX=/Users/yourname ..
 make -j4
 make install
```

If you're using the GUI version of CMake (which is usual on a Mac)
then make sure to check "STANDALONE_INSTALL", uncheck the
LICENCE_INSTALL and COMPILE_WITH_X11 and set CMAKE_INSTALL_PREFIX to
/Users/yourname.

## SpineML_2_BRAHMS

SpineML_2_BRAHMS is the set of scripts which allows a SpineML model to
be built into C++ and executed by BRAHMS.

Clone a copy of SpineML_2_BRAHMS into your home directory:

```
 cd $HOME
 git clone https://github.com/SpineML/SpineML_2_BRAHMS.git
```

We'll build the tools in SpineML_2_BRAHMS in-place (they don't have to
be installed).

Open CMake. For both "source" and "where to build" select the
SpineML_2_BRAHMS directory. Press "configure" and "generate".

(Ignore any Policy CMP0042 error you see).

In the terminal:

```
 cd SpineML_2_BRAHMS
 make
```

And that's it for SpineML_2_BRAHMS.

## SpineCreator

SpineCreator is the desktop application which lets you graphically
edit your SpineML model.

### Python requisite

SpineCreator requires python. On a Mac, this is available by default,
so there's nothing to do to get it.

### Qt prerequisite

Qt is a prerequisite of SpineCreator - SpineCreator is built with the Qt toolkit.

Obtain the Qt online installer from
[https://www.qt.io/download/](https://www.qt.io/download/)

This will install both the library and the QtCreator build tool.

### Graphviz prerequisite

If you completed the initial prerequisites section and installed
graphviz-devel, then you're good to go.

We recommend using MacPorts to install Graphviz. Follow the guide
here: [https://guide.macports.org/](https://guide.macports.org/) to
install MacPorts. Once installed, you should only need the following
command to install Graphviz:

```
sudo port install graphviz-devel
```

This will install graphviz libraries to /opt/local/lib/graphviz and
header files to /opt/local/include.

The QtCreator project file which is part of SpineCreator should
contain these paths, so you can now go ahead and build SpineCreator.

### Obtain and compile SpineCreator

You can get a copy of the latest SpineCreator using this command in a
terminal window:

```
git clone https://github.com/SpineML/SpineCreator.git
```

Open QtCreator, and then open the SpineCreator project. The file to
open is called spinecreator.pro (on older branches it was
neuralNetworks.pro). You'll find the QtCreator application in the Qt
directory, wherever you installed it (and not necessarily directly in
Launchpad).

On opening spinecreator.pro, you'll be asked to select a "kit" to use
to build the project - that means a particular version of the Qt
library aimed at a particular target. You should be able to compile
with Qt version 5.x (Qt 4.x no longer supported). Choose the "clang"
kit which builds for desktop Mac OS (by default you'll also have
installed Android and iOS targetted kits).

### Compiling and running SpineCreator

Compiling should be as simple as pressing the "run" or "build" button
in QtCreator.

## Finishing up: Configuring SpineCreator on Mac

We have to tell SpineCreator where to find SpineML_2_BRAHMS, BRAHMS
etc.

Launch SpineCreator from the Qt Creator window, by pressing the "run"
button, then, in SpineCreator, go to the menu "Edit -> Settings". This
brings up a window with three tabs. Choose "Simulators".

Change the value for "Convert script" to
"/Users/[you]/SpineML_2_BRAHMS/convert_script_s2b". Replace [you] with
your username, so that the path points to the SpineML_2_BRAHMS which
you cloned in your home directory.

Change the value for "Working directory" to
"/Users/[you]/SpineML_2_BRAHMS"

On Linux, I set the settings like this for my home directory
(/home/seb):

![SC_Settings](/public/images/SC_Settings.png "SpineCreator Settings window"){: .center-image }

Set a value in SYSTEMML_INSTALL_PATH to be
"/Users/[you]/SystemML". That allows the component build scripts to
find the BRAHMS include files.

On Mac, you'll also need to add "/usr/local/bin" to the PATH
environment variable. Add it with a ':' to separate it. This allows
the spineml_preflight program to be found.

Last gotchas:

In SpineML_2_BRAHMS, you should:

```
 echo "OSX" > current_os
```

That will ensure that SpineCreator doesn't try (and fail) to
re-compile the tools that you compiled in the SpineML_2_BRAHMS section
earlier.

Click "Apply" then "Close".

Your installed version of SpineCreator can now find
SpineML_2_BRAHMS. SpineML_2_BRAHMS can find SpineML_PreFlight and
BRAHMS, because these were installed system-wide into
/usr/local/bin/spineml_preflight and /usr/local/bin/brahms.

You should now be able to test your installation by running the GPR
Basal Ganglia model.
