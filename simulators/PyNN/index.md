---
layout: default
title: damson
---


!["PyNN logo"](http://neuralensemble.org/static/photos/pynn_logo.png)

[PyNN] is described as a simulator independant language for building neuronal network models. It is based on the Python programming language and has simulator support for r range of simulation engines including NEST, NEURON, PCSIM and Brian.

Simulating SpineML with PyNN
----------------------------

The PyNN website describes the [PyNN installation process]. SpineML can be translated to PyNN simulation code using a single XSLT template which can be applied to an experimental layer model file. For example, the Brett Benchmark Model (using PyNN neurons) from the examples section can be used to generate *pynn\_simulation.py* by using the following syntax.

``` bash
xsltproc -o pynn_simulation.py PyNN.xsl experiment.xml
```

The simulation can then be executed in PyNN by executing the Python script with a PyNN simulator as an argument. i.e.

``` bash
python pynn_simulation.py brian
```

Any log files will be saved in the local directory.

Downloads
---------

[**SpineML to PyNN XSLT Template**](http://bimpa.group.shef.ac.uk/SpineML/simulators/PyNN/PyNN.xsl)

Usage and Limitations
---------------------

### Neuron Body Components and Network Layer Connectivity

The major limitation of PyNN is that only the High Level Network layer is supported. PyNN does not have the flexibility to allow the connection of arbitrary components. PyNN is also not able to support the simulation of arbitrary neuron or synapse models described in SpineML. Instead a set fixed set of neuron component models can be used in any Network Layer specification which requires PyNN simulation support. This set of component names matches the names of the PyNN standardised neuron model types. i.e.;

-   [IF\_curr\_exp.xml]
-   IF\_curr\_alpha.xml
-   IF\_cond\_alpha.xml
-   IF\_cond\_exp.xml
-   HH\_cond\_exp.xml
-   EIF\_cond\_alpha\_isfa\_ista.xml
-   EIF\_cond\_exp\_isfa\_ista.xml

Implementations of these these components are matched to the PyNN standard model names which each have internal implementation within all of the PyNN simulation back-ends. WeightUpdate and PostSynapse models must use the *PyNN\_WeightUpdate.xml* and *PyNN\_PostSynapse.xml* components model respectively. Connectivity between these components within the network layer must be via the following port configurations;

-   [PyNN\_WeightUpdate.xml]
    -   input\_src\_port=“spike”
    -   input\_dst\_port=“spike”
-   [PyNN\_PostSynapse.xml]
    -   input\_src\_port=“w”
    -   input\_dst\_port=“w\_in”
    -   output\_src\_port=“w\_out”
    -   output\_dst\_port=“w\_I” or “w\_E” (for Inhibitory or Excitatory respectively)

In future the PyNN.xslt template will be updated to support learning synapses. A PyNN module will also be created to allow PyNN model descriptions to be saved in SpineML format.

### Spike Source Inputs

Spike source inputs must be described within the network as populations (using the component model [PyNNSpikeSource.xml] for the neuron body). This allows spike sources to be connected via projections using the *PyNN\_WeightUpdate.xml* and *PyNN\_PostSynapse.xml* components for synapse behaviour.

Details of specifying spike sources are listed within the [experimental layer section]. The only limitation is that time varying rates are not supported in PyNN.

  [PyNNSpikeSource.xml]: /simulators/PyNN/spineml/PyNNSpikeSource.xml
  [experimental layer section]: /spineml/experiment/
  [PyNN]: http://neuralensemble.org/PyNN/
  [PyNN installation process]: https://pypi.org/project/PyNN/
  [IF\_curr\_exp.xml]: /simulators/PyNN/spineml/IF_curr_exp.xml
  [PyNN\_WeightUpdate.xml]: /simulators/PyNN/spineml/PyNN_WeightUpdate.xml
  [PyNN\_PostSynapse.xml]: /simulators/PyNN/spineml/PyNN_PostSynapse.xml
