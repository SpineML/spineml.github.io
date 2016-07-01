---
layout: default
title: Home
---

![b] 

BRAHMS
------

BRAHMS is a Modular Execution Framework (MEF) for executing integrated systems built from component software processes (a SystemML-ready execution client). It allow the connection of processes together into systems, by linking the outputs of some processes into the inputs of others. For more details see [here](http://brahms.sourceforge.net/home/).

Introduction to SpineML with BRAHMS
-----------------------------------

Due to the flexibility of BRAHMS it is able to support the full SpineML low-level network layer, and is therefore used to create the SpineML reference simulator. BRAHMS requires the creation of C++ code implementations of each of the SpineML components as well as the generation of XML documents for the System (equivalent to the SpineML network layer) and the Execution (equivalent to the SpineML experiment layer). This is done using a set of XSLT scripts linked by a shell script on Unix or batch script on Windows. Use of user created components with the BRAHMS simulator requires a working compile environment.

Installation on Linux
---------------------

The best way to install BRAHMS and SpineML\_2\_BRAHMS on Linux is to compile the SpineML-maintained version of BRAHMS along with SpineML\_2\_BRAHMS and SpineML\_PreFlight

### Linux Prerequisites

You will need the following programs installed on your system:

``` bash
sudo apt-get install build-essential python git gitk \
  python-dev libpopt-dev doxygen xsltproc cmake libxaw7-dev libxv-dev
```

Let's create a directory to keep the source code in one place:

``` bash
cd ~
mkdir scsrc
```

### Compile BRAHMS on Linux

Clone the SpineML-group-maintained version of BRAHMS (which sports a nice cmake compile and install scheme):

``` bash
cd ~/scsrc
git clone [`https://github.com/sebjameswml/brahms.git`]
```

Build brahms with cmake:

``` bash
cd brahms
mkdir build
cd build
cmake -DSTANDALONE_INSTALL=OFF -DCMAKE_INSTALL_PREFIX=/usr/local ..
make -j4
sudo make install
```

### Compile SpineML\_PreFlight on Linux

Clone a copy of SpineML\_PreFlight:

``` bash
cd ~/scsrc
git clone [`https://github.com/SpineML/SpineML_PreFlight.git`]
```

Build and install SpineML\_PreFlight using cmake:

``` bash
mkdir SpineML_PreFight-build && cd SpineML_PreFlight-build
cmake -DCMAKE_INSTALL_PREFIX=/usr/local ../SpineML_PreFlight
make -j4
sudo make install
```

### Clone SpineML\_2\_BRAHMS on Linux

Clone a copy of SpineML\_2\_BRAHMS into your home directory:

``` bash
cd ~
git clone [`https://github.com/SpineML/SpineML_2_BRAHMS.git`]
```

There is no need to build SpineML\_2\_BRAHMS, which is a set of scripts.

Installation on OSX
-------------------

BRAHMS as a simulator is only supported as part of the SpineCreator toolchain currently. To install you should install [SpineCreator] and the SpineML\_2\_BRAHMS package for OSX.

Simulating SpineML with BRAHMS
------------------------------

To run a SpineML model from the command line, chandirectory into your SpineML\_2\_BRAHMS directory:

``` bash
cd /path/to/SpineML_2_BRAHMS
```

Then explore the commandline options for convert\_script\_s2b:

``` bash
./convert_script_s2b -?
```

As a quick start, you can run experiment 0 of the GPR Basal Ganglia model with the following command:

``` bash
./convert_script_s2b -m /home/seb/GPR-BasalGanglia/SpineML -e 0
```

The results of the model can be found in the 'SpineML\_2\_BRAHMS/temp' directory. Each log data file is accompanied by a simple XML file containing a description of the structure of the log and the type of data contained within. There are scripts in SpineCreator to help you extract the data from these files. See for example SpineCreator/analysis\_utils/matlab/load\_sc\_data.m and SpineCreator/analysis\_utils/matlab/load\_sc\_data.py.

Downloads
---------

Releases for Debian based Linux and Mac OSX can be found on the [release page](https://github.com/SpineML/SpineML_2_BRAHMS/releases/tag/1.0.0-1), although these are currently not up to date and we recommend using the Git versions

Usage and Limitations
---------------------

BRAHMS is the SpineML reference simulator and there are currently no limitations.

  [b]: /public/images/Brahmslogo-128.png "fig:Brahmslogo-128.png"

