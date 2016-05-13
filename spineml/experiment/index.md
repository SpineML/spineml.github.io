---
layout: api
title: SpineML Language
---

# Experiment layer

## SpineML

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

The SpineML Experiment layer describes a simulation set-up for a single (or series) of experiments. The top level SpineML element can contain only Experiment elements.

|  Contains  |  Description  |
| ----- |
|  `Experiment [1..*]`  |  Set of one or more Experiment elements  |

## Experiment

An Experiment describes a complete single simulation configuration including any configurations, the simulation/simulator details and any inputs or outputs.

|  Contains  |  Description  |
| ----- |
|  `Model [1]`  |  A single Model element containing any model configurations  |
|  `Simulation [1]`  |  A single Simulation element containing simulation/simulator details  |
|  `Input [0..*]`  |  Zero or more inputs to the simulation  |
|  `Outout [0..*]`  |  Zero or more outputs to the simulation  |

## Model

The Model element contains any lesions to projections or configuration changes to parameters or state variables for any instances of components within the network layer.

|  Contains  |  Description  |
| ----- |
|  `Lesion [0..*]`  |  Set of zero or more Lesion elements  |
|  `Configuration [0..*]`  |  Set of zero or more Lesion elements  |

A Lesion has the effect of removing a projection from a network layer model. The lesion element names a source and destination population. If no projections exist from the source to the destination then the lesion will be ignored.

|  Contains  |  Description  |
| ----- |
|  `@src_population::string`  |  The name of a source population  |
|  `@dst_population::string`  |  The name of a destination population  |

A configuration allows any network layer property value to be overridden using by specifying a new network layer property element for a given target (Population, WeightUpdate, PostSynapse or Group). Properties using a partially complete explicit list of values will be applied after any properties specified in the network layer. This allows the network layer to set a default property and the experiment layer to overwrite specific instances of a component.

|  Contains  |  Description  |
| ----- |
|  `NL::Property`  |  A Network Layer property element  |
|  `@target::string`  |  The target name of a Population, WeightUpdate, PostSynapse (or Group)  |

## Simulation

The Simulation element names an integration method, simulation duration and a preferred simulation engine.

|  Contains  |  Description  |
| ----- |
|  `IntegrationMethod`  |  An IntegrationMethod element  |
|  `@duration::double`  |  The duration of the simulation  |
|  `@preffered_simulator::string`  |  Optional name of the preferred simulator  |

IntegrationMethod is an abstract element which is extended by either EulerIntegration or RungeKuttaIntegration with the following attributes.

|  Contains  |  Description  |
| ----- |
|  `@dt::double`  |  The integral time-step  |

|  Contains  |  Description  |
| ----- |
|  `@dt::double`  |  The integral time-step  |
|  `@dt::order`  |  The Runge-Kutta order  |

## Input

An Input is an experimental value or set of values which is directly applied to receive ports of a set of component model instances. Inputs must name a network layer Population, WeightUpdate, PostSynapse (or Group when using the Low Level Network Layer) as target (@target attribute), target indices (the @target_indices attribute) allows particular instances of a component to be targeted to receive the input value (i.e. a selection of neurons within a population). The @start_time and @end_time attributes allow the input to have a time window in which it is applied. If no start time or duration is specified then the input is assumed to be persistent for the length of the simulation. The @rate_based_distribution attribute is valid when the target port (@port) is an event receive or impulse receive port. It indicates whether the input value dictates a rate (in Hz) rather than value. Inputs to event or impulse receive ports which do not specify a @rate_based_distribution imply that the input is a single event or impulse occurring at a single time (specified by @value). Inputs to analogue ports are never rate based (and @rate_based_distribution is ignored). The Input element is abstract and can be extended by a number of different input prototypes as follows which give examples of usage;

  
Event inputs are analogous to the common concept of a spike source. When targeted at an event receive port of a WeightUpdate component a one to all connectivity between the spike source (input) and a WeighUpdate component is implied. The one to all connectivity can be manipulated in a limited way through the use of the @target_indices attribute which specifies a set of indices in which the input should be applied. For more complex connectivity of a spike source input to a network model, it is more pertinent to create a network layer population using a 'SpikeSource' component as the neuron body model (this component model simply relays input received events by sending out an event). Complex connectivity can be described using projections and the spike source can be driven by any Constant or TimeVarying input. Throughout this section this is the preferred method for describing spike sources used in the examples.

### ConstantInput

A ConstantInput indicates a input of a constant value.

|  Contains  |  Description  |
| ----- |
|  `@name::string`  |  Input name  |
|  `@target::string`  |  Population, Synapse, PostSynapse (or Group) name  |
|  `@target_indices::integer list` |  A comma separated list of targeted indices  |
|  `@port::string`  |  The port which the input will be applied to  |
|  `@start_time::double`  |  The time the input starts (ms)  |
|  `@duration::double`  |  The duration the input is applied (ms)  |
|  `@rate_based_distribution::distribution`  |  Optional String value of either regular or poisson  |
|  `@value::double`  |  The input value  |

For example a constant current based analogue input to indices 1, 2, 3 of an analogue receive port 'I_external' in a Population named 'Excitatory' could be specified as follows;

``` xml
<ConstantInput name="DC Current In" 
               target="Excitatory" 
               target_indices="0,2,3" 
               port="I_external" 
               value="0.5"/>
```

A spike source with a regular spiking frequency of 10Hz connected to a target population (using the 'SpikeSource' component and with a size of 1) can be specified as follows;

``` xml
  <ConstantInput name="Regular Spike Source" 
               target="SpikeSource" 
               port="spike_in" 
               value="10" 
               rate_based_distribution="regular"
```

The firing frequency can use a poisson distribution by changing the @rate_based_distribution. i.e.

``` xml
  <ConstantInput name="Poisson Spike Source" 
               target="SpikeSource" 
               port="spike_in" 
               value="10" 
               rate_based_distribution="poisson"
```
			   
Alternatively the spike source can be used to trigger a single spike event at a time 0.5 seconds by removing the @rate_based_distribution. i.e.
  
``` xml
  <ConstantInput name="Single Spike Source" 
               target="SpikeSource" 
               port="spike_in" 
               value="0.5"
```



### ConstantArrayInput

A ConstantArrayInput provides a set of constant inputs which are mapped to the target using a one to one connectivity. The number of array values (@array_value) must be equal to the array size (@array_size attribute) which must also be equal to the target population size (@size).

| Contains	| Description |
| ----- |
| `@name::string`	| Input name |
| `@target::string`	| Population, Synapse, PostSynapse (or Group) name |
| `@target_indices::integer list`	| A comma separated list of targeted indices |
| `@port::string`	| The port which the input will be applied to |
| `@start_time::double` | The time the input starts (ms) |
| `@duration::double`	| The duration the input is applied (ms) |
| `@rate_based_distribution::distribution` | A string value of either regular or poisson |
| `@array_size::integer`	| The array size of the input (must be equal to the target size) |
| `@array_value::double array`	| A comma separated list of constant values for each array item |

For example a set of current inputs with a different constant input value mapped to each neuron in the population target 'Excitatory' can be specified as follows;

```xml
  <ConstantArrayInput name="DC Current Array" 
                      target="Excitatory"  
                      port="I_external"  
                      array_size="4" 
                      array_value="0.5,0.2,0.3,0.35" />
```
					  
A spike source array with a regular spiking frequency of 10Hz, 11Hz, 10Hz and 12Hz connected to a target population (using the 'SpikeSource' component and of size 4) can be specified as follows;

```xml
  <ConstantArrayInput name="SS Array Regular" 
                      target="SpikeSourcePopulation"  
                      port="spike_in"  
                      rate_based_distribution="regular" 
                      array_size="4" 
                      array_value="10,11,10,12" />
```
					  
The firing frequency can use a poisson distribution by changing the @rate_based_distribution. Alternatively the spike source array can be used to trigger an array of single spike events at a time 0.1, 0.2, 0.3, 0.5 seconds by removing the @rate_based_distribution. i.e.

```xml
  <ConstantArrayInput name="SS Array Single Spike" 
                      target="SpikeSourcePopulation"  
                      port="spike_in"  
                      array_size="4" 
                      array_value="0.1,0.2,0.3,0.5" />
```
					  

### TimeVaryingInput

A TimeVaryingInput provides a constant input value which changes at fixed time points. If the target port is analogue then the input value will be applied each iteration. If the target port is event based and no @rate_based_distribution is set then time point values indicate spike times (and values are ignored). If the target port is event based and a @rate_based_distribution is set then the time point values indicate times in which the spike rate changes to the specified value. If time point values are specified outside of the start time and duration they will be ignored.

|  Contains  |  Description  |
| ----- |
|  `TimePointValue [1..*]`  |  One or more time point values specifying a time and value  |
|  `@name::string`  |  Input name  |
|  `@target::string`  |  Population, Synapse, PostSynapse (or Group) name  |
|  `@target_indices::integer list`  |  A comma separated list of targeted indices  |
|  `@port::string`  |  The port which the input will be applied to  |
|  `@start_time::double`  |  The time the input starts (ms)  |
|  `@duration::double` |  The duration the input is applied (ms)  |
|  `@rate_based_distribution::distribution`  |  A string value of either regular or poisson  |

For example a stepped current input to all neurons in a Population named 'Excitatory' could be specified as follows;

```xml
      <timevaryinginput name="Step Current Source" target="Excitatory" port="I_external">
          <timepointvalue time="100" value="0.1">
          <timepointvalue time="500" value="0.5">
          <timepointvalue time="600" value="0.0">
      </timepointvalue></timepointvalue></timepointvalue></timevaryinginput>
```
  
A spike source with with explicit spike times connected to a target population (using the 'SpikeSource' component and of size 1) can be specified as follows;

```xml
      <timevaryinginput name="SS Explicit" target="SpikeSource" port="spike_in">
          <timepointvalue time="0.1">
          <timepointvalue time="0.2">
          <timepointvalue time="0.7">
      </timepointvalue></timepointvalue></timepointvalue></timevaryinginput>
```

A regular spiking source with explicit rate change times can be specified as follows; Each time point value indicates a time in which the firing rate changes. i.e. at time 0.1 the spike rate should be 5Hz.

```xml
      <timevaryinginput name="SS Rate Explicit" target="SpikeSource" port="spike_in" rate_based_distribution="regular">
          <timepointvalue time="0.1" value="5">
          <timepointvalue time="0.2" value="5">
          <timepointvalue time="0.7" value="5">
      </timepointvalue></timepointvalue></timepointvalue></timevaryinginput>
```

### TimeVaryingArrayInput

A TimeVaryingArrayInput provides a set of constant input values (of size @array_size) which are mapped to the target using a one to one connectivity and which change at fixed time points. Time point array values speficy a single time and a set of values of the same size as the input array. If the target port is analogue then the input value will be applied each iteration. If the target port is event based and no @rate_based_distribution is set then time point values indicate spike times (and values are ignored). If the target port is event based and a @rate_based_distribution is set then the time point values indicate times in which the spike rate changes to the specified value. If time point values are specified outside of the start time and duration they will be ignored.

|  Contains  |  Description  |
| ----- |
|  `TimePointArrayValue [1..*]`  |  One or more time point values specifying a time and set of values  |
|  `@name::string`  |  Input name  |
|  `@target::string`  |  Population, Synapse, PostSynapse (or Group) name  |
|  `@target_indices::integer list`  |  A comma separated list of targeted indices  |
|  `@port::string`  |  The port which the input will be applied to  |
|  `@start_time::double`  |  The time the input starts (ms)  |
|  `@duration::double`  |  The duration the input is applied (ms)  |
|  `@rate_based_distribution::distribution`  |  A string value of either regular or poisson  |
|  `@array_size::int`  |  The size of the input array  |

For example a stepped current array input mapped to each neuron in the population target 'Excitatory' (of size 3) can be specified as follows. If no matching value is specified then a value of 0 is assumed.

```xml
      <timevaryingarrayinput name="Step Current Source Array" target="Excitatory" port="I_external" array_size="3">
          <timepointarrayvalue index="0" array_time="0.1,0.2,0.3" array_value="50,60,45/>
          <TimePointArrayValue index=" 1"="">
          <timepointarrayvalue index="2" array_time="1.1,2.1,3.1" array_value="40,50,40">
      </timepointarrayvalue></timepointarrayvalue></timevaryingarrayinput>
```
	   
An array of regular regular spiking sources with explicit spike times connected to a target population (using the 'SpikeSourcePopulation' component and of size 4) can be specified as follows; Each time point array value indicates the times that the spike sources (and index @index) will fire. i.e. the spike source at index 0 will fire at times 0.1, 0.5, 0.8 seconds.

```xml
      <timevaryingarrayinput name="SS Array Explicit" target="SpikeSourcePopulation" port="spike_in" array_size="4">
          <timepointarrayvalue index="0" array_time="0.1,0.5,0.8">
          <timepointarrayvalue index="1" array_time="0.2,0.6,0.7">
          <timepointarrayvalue index="1" array_time="0.5,0.6,0.7">
          <timepointarrayvalue index="1" array_time="0.3,0.7,0.9">
      </timepointarrayvalue></timepointarrayvalue></timepointarrayvalue></timepointarrayvalue></timevaryingarrayinput>
```

An array of regular spiking sources with explicit rate change times can be specified as follows; Each time point value indicates a time in which the firing rate changes. i.e. the spike source at index 0 will change its spike rate at times 0.1, 0.5, 0.8 seconds to 10Hz, 11Hz, 12Hz and 13Hz respectively.

```xml
      <timevaryingarrayinput name="SS Array Explicit" target="SpikeSourcePopulation" port="spike_in" array_size="4" rate_based_distribution="regular">
          <timepointarrayvalue index="0" array_time="0.1,0.5,0.8" array_value="10,11,12,13">
          <timepointarrayvalue index="1" array_time="0.2,0.6,0.7" array_value="12,13,14,15">
          <timepointarrayvalue index="1" array_time="0.5,0.6,0.7" array_value="14,15,16,17">
          <timepointarrayvalue index="1" array_time="0.3,0.7,0.9" array_value="16,17,18,19">
      </timepointarrayvalue></timepointarrayvalue></timepointarrayvalue></timepointarrayvalue></timevaryingarrayinput>
```

## Output

Outputs are used to log simulation data. Only a single output, a LogOutput, is supported. The LogOutput specifies a target and port to log. If the target port is analogue then the port value will be logged every simulation iteration, if the target port is event based then the events times will be logged. A start time and duration indicate the period in which the log should be recorded. The @indices attribute allows specific indices of the target Population, WeightUpdate, PostSynapse (or Group) to be logged (by default all indices are logged).

|  Contains  |  Description  |
| ----- |
|  `@name::string`  |  Output name  |
|  `@target::string`  |  Population, Synapse, PostSynapse (or Group) name  |
|  `@port::string`  |  The port which the input will be applied to  |
|  `@indices::integer list`  |  A comma separated list of targeted indices  |
|  `@start_time::double`  |  The time the input starts (ms)  |
|  `@duration::double`  |  The duration the input is applied (ms)  |

The following example will log the analogue port v from a population 'Excitatory' for the period of the simulation.

```xml
      <logoutput target="Excitatory" port="v" name="v_log_excitatory">  </logoutput></constantarrayinput></constantarrayinput></constantinput>
```