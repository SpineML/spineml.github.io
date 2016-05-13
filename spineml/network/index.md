---
layout: api
title: SpineML Network Layer
---

# Network layer

## SpineML

The SpineML Network layer describes the connectivity of components using the high level concepts of populations and projections. A population contains a set of neurons which are instances of a single component model. The top level Network layer element is SpineML. It can contain only Population elements.


|  Contains  |  Description  |
| ----- |
|  `Population [1..*]`  |  Set of one or more Population elements  |

## Population

A Population describes a collection of neurons and a set of projections to other populations. At least one projection is required.

|  Contains  |  Description  |
| ----- |
|  `Neuron [1]`  |  Neuron body definition  |
|  `Projection [1..*]`  |  Set of one or more Projection elements  |

## Neuron

The Neuron element describes a set of instances of a neuron body component model (referenced by the @url attribute). The @size and @name attributes indicate the size and name of the population respectively. Zero or more property attributes can be specified to set the initial values of StateVaraibles and Parameters of the component model. If a component model contains Parameters or StateVariables which do not have valid properties within the Neuron element then the values are assumed to be zero.

|  Contains  |  Description  |
| ----- |
|  `Property [0..*]`  |  Set or zero or more Property Elements  |
|  `@name::String`  |  The population name  |
|  `@url::String`  |  URL to the neuron body component model file  |
|  `@size::int`  |  The population size  |

## Properties

Properties are used to set the value of Parameters or StateVariables for instances of a component model (this could be within a Neuron, WeightUpdate or PostSynapse). The @name attribute must reference a Parameter of StateVariable from the parents named component model. Dimensionality is inherited from the component.

A Property element references a single abstract Value element. A number of implementations of value types can be substituted wherever value is referenced.

|  Contains  |  Description  |
| ----- |
|  `Delay [1]`  |  A delay element  |
|  `@name::string`  |  The name of a Parameter of StateVariable from the parents named component model  |

### Fixed Value

A fixed value indicates that the named StateVariable or Parameter for all instances of a component (within a Population, WeightUpdate or PostSynapse) have a fixed value.

|  Contains  |  Description  |
| ----- |
|  `@value::double`  |  Fixed value  |

### Value List

A value list type indicates that the named StateVariable or Parameter values are set explicitly using a set of Value elements. There should be a value for each component instance (Neuron, WeightUpdate or PostSynapse). Note that value lists cannot be used where the number of weight update component instances is not fixed (i.e. for fixed probability connections).

|  Contains  |  Description  |
| ----- |
|  `Value [1..*]`  |  Set of one or more Value elements  |

A Value element defines a property value for a single instance of a component. The @index attribute indicates the index position of the component instance within the set. WeightUpdate indices are interpreted depth first. i.e. For an all to all connectivity of two populations of 10 neurons, index 0 implies src neuron 0 to dst neuron 0 and index 1 implies src neuron 0 to dst neuron 1.

|  Contains  |  Description  |
| ----- |
|  `@index::int`  |  Index of the component within a population, weight update or post-synapse set  |
|  `@index::value`  |  Value of a property for a single instance of a component  |

### Distribution Value

Distribution values indicates that the named StateVariable or Parameter values are set using a distribution of values. Value distributions may be either Uniform, Normal or Poisson. Each distribution may have an optional @seed attribute used to initialise the random number generation.

|  Contains  |  Description  |
| ----- |
|  `@seed::integer`  |  Optional seed value to be used for random number generation  |
|  `@minimum::double`  |  Minimum value within the range of the distribution  |
|  `@maximum::double`  |  Maximum value within the range of the distribution  |

|  Contains  |  Description  |
| ----- |
|  `@seed::integer`  |  Optional seed value to be used for random number generation  |
|  `@mean::double`  |  Mean value of the distribution  |
|  `@variance::double`  |  Variance of the distribution  |

|  Contains  |  Description  |
| ----- |
|  `@seed::integer`  |  Optional seed value to be used for random number generation  |
|  `@mean::double`  |  Mean value of the distribution  |

## Projection

A projection links the neuron models of two populations. Connectivity is described within a projection by a synapses (each projection may have multiple synapses). The @dst_population defines the destination population name (specified within a Neuron @name element).

|  Contains  |  Description  |
| ----- |
|  `Synapse [1..*]`  |  Optional seed value to be used for random number generation  |
|  `@dst_population::double`  |  Population name of the projection synapse  |

## Synapse

A Synapse describes a specific connectivity pattern between two populations in a projection. Each synapse also contains a single weight update and post-synapse model instance which describe how neuron, synapse weight update and post-synapse model components are linked by ports.

|  Contains  |  Description  |
| ----- |
|  `Connectivity [1]`  |  A connectivity type  |
|  `WeightUpdate [1]`  |  WeightUpdate model  |
|  `PostSynapse [1]`  |  PostSynapse model  |

## Connectivity

Connectivity is used within a Synapse to describes how two populations of neurons are connected. The connectivity element is abstract, a number of implementations of connectivity types can be substituted wherever connectivity is referenced. Each connectivity type has an optional delay element. A Delay element is a special form of property which can contain only a fixed or distribution value. If no delay is specified then all delays are assumed to be 0.

### OneToOneConnection

A one to one connection implies that two populations (of equal size) have a synaptic connection between source a destination neuron with the same index value. Where the projection source and destination of the projection are the same self connections are created.

|  Contains  |  Description  |
| ----- |
|  `Delay [1]`  |  An optional synaptic delay  |

### AllToAllConnection

An all to all connection implies that two populations (of equal size) have a synaptic connection between every source a destination neuron. Where the projection source and destination of the projection are the same self connections are created.

|  Contains  |  Description  |
| ----- |
|  `Delay [1]`  |  An optional synaptic delay  |

### FixedProbabilityConnection

A fixed probability connection implies that two populations (of equal size) have a synaptic connection between every source a destination neuron with a probability specified by the @probability attribute. Where the projection source and destination of the projection are the same self connections are created.

|  Contains  |  Description  |
| ----- |
|  `Delay [1]`  |  An optional synaptic delay  |
|  `@probability::double [1]`  |  Probability of connectivity  |
|  `@seed::integer [1]`  |  Optional seed value to use for random number generator  |

### ConnectionList

A connection list explicitly lists synaptic connections between source a destination neurons.

|  Contains  |  Description  |
| ----- |
|  `Delay [1]`  |  An optional synaptic delay  |
|  `Connection [1..*]`  |  Set of one or more single connection instances  |

A connection element describes a single connection between a source neuron (@src_neuron) and a destination neuron (@dst_neuron). An optional delay value can be used to set a unique delay value for each synaptic connection. Where a delay value is set it will overwrite any delay value specified in the parent element.

|  Contains  |  Description  |
| ----- |
|  `@src_neuron::integer`  |  Index of the source neuron  |
|  `@dst_neuron::integer`  |  Index of the destination neuron  |
|  `@delay::double`  |  An optional single synaptic delay value  |

## WeightUpdate

The WeightUpdate element describes a set of instances of a weight update component model (referenced by the @url attribute). A WeightUpdate is connect to a set of pre synaptic source neurons (from the containing projections parent population) by specifying a neuron component port (@input_src_port attribute) and weight update component port (@input_dst_port attribute) to link the components. Feedback from a post synaptic neuron to the weight update (for implmenting learning mechanisms) is possible by using the @feedback_src_port attribute to specify the post synaptic neuron component port and @feedback_dst_port attribute to specify the synaptic component port. Zero or more property attributes can be specified to set the initial values of StateVaraibles and Parameters of the weight update component model. If a component model contains Parameters or StateVariables which do not have valid properties within the Neuron element then the values are assumed to be zero. No size is specified for a weight update component. Size is determined by the source and destination populations sizes and the connectivity pattern used to connect them.

|  Contains  |  Description  |
| ----- |
|  `Property [0..*]`  |  Set or zero or more Property elements  |
|  `@name::String`  |  The population name  |
|  `@url::String`  |  URL to the neuron body component model file  |
|  `@input_src_port::string`  |  Source port of pre synaptic neuron component  |
|  `@input_dst_port::string`  |  Destination port of weight update component for pre-synaptic input  |
|  `@feedback_src_port::string`  |  Optional destination port of post-synapse component to communicate weight update data for learning  |
|  `@feedback_dst_port::string`  |  Optional source port of weight update component to communicate feedback to post-synaptic neuron for learning  |

## PostSynapse

The PostSynapse element describes a set of instances of a post-synapse component model (referenced by the @url attribute). A PostSynapse is connect to a set of synapes by specifying a weight update component port (@input_src_port attribute) and the post-synapse component port (@input_dst_port attribute) to link the components. The post-synapse connects to a set of post synaptic neurons by using the @output_src_port attribute to specify the post-synapse component port and @output_dst_port attribute to specify the post synaptic neuron component port. Zero or more property attributes can be specified to set the initial values of StateVaraibles and Parameters of the post-synapse component model. If a component model contains Parameters or StateVariables which do not have valid properties within the Neuron element then the values are assumed to be zero. No size is specified for a post-synapse component. Size is determined by the destination populations size.

|  Contains  |  Description  |
| ----- |
|  `Property [0..*]`  |  Set or zero or more Property elements  |
|  `@name::String`  |  The population name  |
|  `@url::String`  |  URL to the neuron body component model file  |
|  `@input_src_port::string`  |  Source port of weight update component  |
|  `@input_dst_port::string`  |  Destination port of post-synapse component for synaptic input  |
|  `@output_src_port::string`  |  Source port of post-synpase component  |
|  `@output_dst_port::string`  |  Destination port of post-synaptic neuron for post synaptic output  |  