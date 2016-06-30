---
layout: api
title: Building SpineCreator from source on Linux
---

# Building on Linux

Because we are a small team, we do not have the resources to produce
regular packages for SpineCreator and the SpineML toolchain.  For this
reason, we recommend that users compile SpineCreator from source and
we have some easy to follow instructions here! Please tell us when you
have problems by submitting issues on the github project pages (for
example https://github.com/SpineML/SpineCreator/issues)

SpineCreator is built using the Qt toolkit (http://qt.io), a
cross-platform library of C++ code for building desktop apps. To run
models, SpineCreator uses a SpineML toolchain. On this page, we
describe how you can compile SpineML_PreFlight, SpineML_2_BRAHMS and
BRAHMS as your SpineML toolchain.

These build instructions have been verified on Ubuntu 14.04. Other
Linux flavours should work just fine, but there may be slight
differences in the names of some of the prerequisites. If your distro
does not provide Qt 5.x, then you will need to install that separately
from http://www.qt.io/download-open-source/. If your distro provides a
version of Graphviz older than 2.32, then you'll have to install a
more recent version of that (which is outside the scope of these
instructions). If I recall correctly, Ubuntu 13.04+ will provide an
out-of-the-box working platform for SpineCreator, as will Debian 7+.

If the build process for any of the four components fails, please let
us know by creating an issue on Github. For example, if you can't
build SpineML_PreFlight, then create an issue here:
[https://github.com/SpineML/SpineML_PreFlight/issues
https://github.com/SpineML/SpineML_PreFlight/issues]

## Linux Prerequisites

You will need the following programs installed on your system:

```
sudo apt-get install build-essential qtcreator libqt5svg5-dev libgvc6 python git gitk \
python-dev libgraphviz-dev libpopt-dev doxygen xsltproc cmake libxaw7-dev libxv-dev
```

Note that the Qt version needs to be 5.x; Qt 4.x is no longer supported.

Let's create a directory to keep the source code in one place:

```
cd ~
mkdir scsrc
```

### Menus in Qt

On current versions of Ubuntu, the window menus in a Qt program are
rendered along the top of the screen when using the default Unity
desktop. If you're not using Unity, these menus can become hidden. If
you have trouble accessing the window menus ("File", "Edit", "Help"
etc) in Qt Creator or SpineCreator, then you can apply this
workaround:

```
sudo apt-get install appmenu-qt
```

This configures all Qt programs to display their window menus in the
window, rather than at the top of the desktop screen.

## Compile BRAHMS on Linux

Clone the SpineML-group-maintained version of BRAHMS (which sports a
nice cmake compile and install scheme):

```
cd ~/scsrc
git clone https://github.com/sebjameswml/brahms.git
```

Build brahms with cmake:

```
cd brahms
mkdir build
cd build
cmake -DSTANDALONE_INSTALL=OFF -DCMAKE_INSTALL_PREFIX=/usr/local ..
make -j4
sudo make install
```

## Compile SpineML_PreFlight on Linux

Clone a copy of SpineML_PreFlight:

```
cd ~/scsrc git clone https://github.com/SpineML/SpineML_PreFlight.git
```

Build and install SpineML_PreFlight using cmake:

```
mkdir SpineML_PreFight-build && cd SpineML_PreFlight-build
cmake -DCMAKE_INSTALL_PREFIX=/usr/local ../SpineML_PreFlight
make -j4 sudo make install
```

## Clone SpineML_2_BRAHMS on Linux

Clone a copy of SpineML_2_BRAHMS into your home directory:

```
cd ~
git clone https://github.com/SpineML/SpineML_2_BRAHMS.git
```

There is no need to build SpineML_2_BRAHMS, which is a set of scripts.

## Build SpineCreator on Linux

Clone SpineCreator:

```
cd ~/scsrc
git clone https://github.com/SpineML/SpineCreator.git
```

Open QtCreator:

![Open_QtCreator](/public/images/Open_QtCreator.png "Open QtCreator"){: .center-image }

Now open the SpineCreator project. The file to open is called
spinecreator.pro (in older branches it was neuralNetworks.pro):

![Open_SpineCreator](/public/images/Open_spinecreator_pro.png "Open spinecreator.pro"){: .center-image }

On opening spinecreator.pro, you'll be asked to "configure" the
project. On Ubuntu you should be able to simply press the "Configure
Project" button:

![QtCreator_Configure_Project](/public/images/QtCreator_Configure_Project.png "Press Configure Project"){: .center-image }

Compiling should now be as simple as pressing the "run" or "build"
button in QtCreator:

![Run_Button](/public/images/Run_Button.png "Press the green play button"){: .center-image }

## Finishing up: Configuring SpineCreator on Linux

There's just one last job to do. We have to tell SpineCreator where to
find SpineML_2_BRAHMS.

Launch SpineCreator from the Qt Creator window, by pressing the "run"
button, then, in SpineCreator, go to the menu "Edit -> Settings". This
brings up a window with three tabs. Choose "Simulators".

Change the value for "Convert script" to
"/home/[you]/SpineML_2_BRAHMS/convert_script_s2b". Replace [you] with
your username, so that the path points to the SpineML_2_BRAHMS which
you cloned in your home directory.

Change the value for "Working directory" to
"/home/[you]/SpineML_2_BRAHMS"

I set the settings like this for my home directory (/home/seb):

![SC_Settings](/public/images/SC_Settings.png "SpineCreator Settings window"){: .center-image }

Click "Apply" then "Close".

Your installed version of SpineCreator can now find
SpineML_2_BRAHMS. SpineML_2_BRAHMS can find SpineML_PreFlight and
BRAHMS, because these were installed system-wide into
/usr/local/bin/spineml_preflight and /usr/local/bin/brahms.

You should now be able to test your installation by running the GPR
Basal Ganglia model.
