---
layout: default
title: Home
---

![dam]

DAMSON is a functional emulator for SpiNNaker architecture which aims to achieve 1 million low power interconnected ARM cores. DAMSON emulates both the fixed point precision and event driven operation of SpiNNaker using an event driven from of C.

Simulating SpineML with DAMSON
------------------------------

As a multi-core simulator DAMSON requires that models can be mapped to processing cores with fixed or limited processing requirements. This requires that large populations (and any associated projections) are split into smaller computational units via an external *splitter* application. Within the examples section split versions of all models are provided for use with the DAMSON XSLT templates. The DAMSON XSLT templates uses both the original *un-split* and *split* version of the model to generate the program and data parts of DAMSON respectively.

A SpineML model can be translated to a DAMSON source file (\*.d) by using the DAMSON xsl templates to translate a SpineML experimental layer model file. For example, the Brett Benchmark Model can be simulated using damson by placing the model files in the ./model directory (of the DAMSON xsl download) and by executing the following commands.

``` bash
xsltproc -o ./model/Brette_Benchmark.xml ./xsl/HtoL.xsl ./model/Brette_Benchmark.xml
xsltproc -o ./model/split_Brette_Benchmark.xml ./xsl/HtoL.xsl ./model/split_Brette_Benchmark.xml
```

The above commands converts any high level network layer models (split and un-split) into corresponding low level network layer models to avoid providing duplicate translation templates for both the high and low level. The DAMSON program and data can then be generated separately using the below commands respectively. Note that the damson data file must be output to a file 'data.d' which is included by the program file during execution. The maxdepth flag must also be set due to deep recursion used to translate dense connection lists.

``` bash
xsltproc -o program.d ./xsl/DAMSON_program.xsl ./model/experiment.xml
xsltproc --maxdepth 20000 -o data.d ./xsl/DAMSON_data.xsl ./model/experiment.xml
```

The simulation can then be executed in DAMSON by calling the DAMSON executable with the damson program source file as an argument (and the damson data file data.d in the same directory) i.e.

``` bash
./damson program.d
```

Any log files will be saved in the local directory.

Downloads
---------

[**DAMSON program executable for debian based Linux 32 bit only**](/public/files/damson)

[**XSLT scripts and directory structure for SpineML to DAMSON code generation**](/public/files/spineml_2_damson.tar)

Usage and Limitations
---------------------

### Fixed Point Arithmetic

DAMSONS internal fixed point arithmetic format is able to robustly match the numerical performance for most component models. Due to the limited range (−32768 to +32767) and resolution (1.526 x 10−5) it is possible to produce overflow and underflow errors within component dynamics. In order to detect this DAMSON has underflow and overflow detection which reports errors at runtime.

### WeightUpdate dynamics

Currently DAMSON is not able to support WeightUpdate components which contain any dynamics or analogue inputs/output ports (i.e. only event based components are supported). This is not a limitation of DAMSON but a limitation of the SpiNNaker hardware system which DAMSON emulates.

### PostSynapse dynamics

Currently DAMSON is not able to support relay impulse ports from a weight update to the post synaptic neuron. This is again a limitation of the event passing within the SpiNNaker hardware system. A work around for some cases will be implemented shortly.

### Experiment Layer Inputs

DAMSON currently has no support for experiment layer inputs. Any inputs must be modelled instead as components and connected to the model via the network layer.


  [dam]: /public/images/Damson.png "fig:damson.png"
  [1]: /public/images/Xml_icon.png "fig:xml_icon.png"
