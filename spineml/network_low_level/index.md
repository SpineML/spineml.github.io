---
layout: api
title: SpineML Language
---


# Low level network layer

## SpineML

The low level network layer extends the existing network layer to allow the connection or arbitrary groups of components via generic inputs (without using a projection construct). This allows biologically constrained model entities such as gap junctions to be described. The top level element in SpineML. It extends the network layer element and can contain either network layer populations or Groups.

|  Contains  |  Description  |
| ----- |
|  `NL::Population [1..*]`  |  Set of one or more Network Layer Population elements  |
|  `Group [1..*]`  |  Set of one or more Groups  |

## Group

The Group element describes a set of instances of component model (referenced by the @url attribute). The @size and @name attributes indicate the size and name of the group respectively. Zero or more property attributes can be specified to set the initial values of StateVaraibles and Parameters of the component model. If a component model contains Parameters or StateVariables which do not have valid properties within the Group element then the values are assumed to be zero. A Group can contain zero or more inputs.

|  Contains  |  Description  |
| ----- |
|  `NL::Property [0..*]`  |  Set or zero or more Property elements  |
|  `Input [0..*]`  |  Set or zero or more Input elements  |
|  `@name::String`  |  The population name  |
|  `@url::String`  |  URL to the neuron body component model file  |
|  `@size::int`  |  The population size  |

## Input

An input connects either a group or population ( named as a source via the @src attribute) using a network layer connectivity pattern. Components are connected via named ports (i.e. @src_port and @dst_port attributes).

|  Contains  |  Description  |
| ----- |
|  `NL::Connectivity [1]`  |  A Network Layer connectivity type  |
|  `@src::String`  |  The input source group or population name  |
|  `@src_port::String`  |  Port on the source used to connect components  |
|  `@dst_port::String`  |  Port on the destination used to connect components  |

## Neuron, WeightUpdate and PostSynapse

Within the low level network layer the Neuron, WeightUpdate and PostSynapses elements are all extended to add the ability to have generic inputs. The Population, Synapse and Projection elements are extended to reference the extended definitions of these elements.  