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
progress to describe how to build SpineCreator, SpineML_PreFlight and
SpineML_2_BRAHMS on Windows 10, providing a fully-functional SpineML
toolchain to develop and execute neural network models.

## Prerequisites

### Install Windows Subsystem for Linux

A new feature in Windows 10 is the ability to install a Linux
distribution to provide the bash shell and other Unixey utility
programs, such as ssh, git and so on. So, the first task is to install
it. Here are Microsoft's own instructions:

https://docs.microsoft.com/en-us/windows/wsl/install-win10

During this process, choose Ubuntu in the store (other Linux flavours
may well work, but I've tested Ubuntu).

Once you have Ubuntu installed (you should be able to run bash.exe),
you can install some pre-requisites:

sudo apt install build-essential cmake zlib1g-dev (etc)

### Install the Qt build environment

I installed Qt from https://www.qt.io choosing the open source
option. All the code in SpineCreator and friends is compatible with the
requirements for using Qt under the free LGPL licence. In the
Qt installer, I scrolled to Qt 5.8 and selected only the "MinGW 32 bit"
checkbox:

![qtsel](/spinecreator/sourcewin/qt_selection.png){: .center-image }

### Install graphviz

Go to http://graphviz.org/ and find "Download". Download and install
"Stable 2.38 Windows Install packages". Do the usual Windows install
process with this.

### Install python

Go to python.org and Find Downloads -> Windows.
Download Python 2.7.x in as a Windows x86-64 MSI installer. Again,
install in the normal way.

### Install cmake and the mingw compiler environment

This is useful if building BRAHMS and SpineML_PreFlight outside the
bash environment.

From http://cmake.org you can obtain the Windows build of cmake. CMake
is a build tool which is required to build BRAHMS and
SpineML_PreFlight.

MinGW stands for Minimal GNU Compiler for Windows, this avoids any
need to compile with Microsoft's Visual Studio. Of course, if you have
Visual Studio, you could use that instead of MinGW.

Assuming you need a compiler, then from
https://sourceforge.net/downloads/mingw download the MinGW installer,
which is called mingw-get-setup.exe

### Install git?

You can obtain git for windows from https://git-scm.com/download/win
but there's little to be gained over using the git inside the Ubuntu
bash shell.

## Building the software

I'm going to make use of the bash shell, which should now be
installed. To open a bash shell, you can open a command line terminal
and type bash.

## Compile BRAHMS on Windows

### Inside bash shell

Clone the official version of BRAHMS (which sports a
nice cmake compile and install scheme and supercedes any code
available on sourceforce):

Open a bash shell

```
mkdir scsrc
git clone https://github.com/BRAHMS-SystemML/brahms.git
```

Build brahms with cmake:

```
cd brahms
mkdir build
cd build
cmake -DSTANDALONE_INSTALL=OFF -DCOMPILE_WITH_X11=OFF -DCMAKE_INSTALL_PREFIX=/usr/local ..
make -j4
sudo make install
```

### Outside bash shell

Either clone brahms as above within the bash environment

## Compile SpineML_PreFlight on Linux

Clone a copy of SpineML_PreFlight:

```
cd ~/scsrc
git clone https://github.com/SpineML/SpineML_PreFlight.git
```

Build and install SpineML_PreFlight using cmake:

```
mkdir SpineML_PreFlight-build && cd SpineML_PreFlight-build
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
