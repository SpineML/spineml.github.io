---
layout: api
title: Component Layer
---

# Component layer

The SpineML component layer syntax describes the functional entities, i.e. neurons and synapses, that are used as building blocks from a model. The syntax is derived from the NineML abstraction layer and LEMS common object model specification with some very minor changes requiring the specification of an initial regime and some minor syntax changes to the specification of ports.

A technical description of the NineML Abstraction layer is available [here](http://incf.github.io/nineml-spec/specification/).

# Differences from NineML Abstraction Layer

We now describe the differences between the NineML Abstraction layer and the SpineML Component layer explicitly.

## Ports

In SpineML we use W3C shemas for validation. This means that the port tags require seperation into several types.

### AnalogSendPort

|  Contains  |  Description  |
| ----- |
|  `@name::String`  |  state variable or alias for output  |
|  `@is_per_connection::bool`  |  for weightupdates sends an output for each connection, instead of each destination neuron  |

### AnalogReceivePort

|  Contains  |  Description  |
| ----- |
|  `@name::String`  |  port identifying name  |
|  `@dimension::String`  |  port dimensionality  |
|  `@post::bool [optional]`  |  for weightupdates remaps the input into connections as though it were a postsynaptic input  |

### AnalogReducePort

|  Contains  |  Description  |
| ----- |
|  `@name::String`  |  port identifting name  |
|  `@dimension::String`  |  port dimensionality  |
|  `@reduce_op::String`  |  operation used to combine multiple inputs  |
|  `@post::bool [optional]`  |  for weightupdates remaps the input into connections as though it were a postsynaptic input  |

### EventSendPort

|  Contains  |  Description  |
| ----- |
|  `@name::String`  |  port identifying name  |
|  `@is_per_connection::bool [optional]`  |  for weightupdates sends an output for each connection, instead of each destination neuron  |

### EventReceivePort

|  Contains  |  Description  |
| ----- |
|  `@name::String`  |  port identifying name  |
|  `@dimension::String`  |  port dimensionality  |
|  `@post::bool [optional]`  |  for weightupdates remaps the input into connections as though it were a postsynaptic input  |

## Impulses

We have added a new communication port type: Impulse. Impulses transfer an event with a value. The tags associated with these ports align with the event port tags, with the following form.

### ImpulseSendPort

|  Contains  |  Description  |
| ----- |
|  `@name::String`  |  statevariable, parameter or alias for output  |
|  `@is_per_connection::bool [optional]`  |  for weightupdates sends an output for each connection, instead of each destination neuron  |

### ImpulseReceivePort

|  Contains  |  Description  |
| ----- |
|  `@name::String`  |  port identifying name  |
|  `@dimension::Stirng`  |  port dimensionality  |
|  `@post::bool [optional]`  |  for weightupdates remaps the input into connections as though it were a postsynaptic input  |

### ImpulseOut

As EventOut

### OnImpulse

As OnEvent, however the ImpulseRecievePort name can be used in the contained MathinLine tags as a variable.

## Small differences

`ComponentClass` contains an attribute `type`, which can take the values `neuron_body`,`postsynapse` or `weight_update`.

`Dynamics` contains an attribute `initial_regime` which denotes the starting Regime.

`NineML` is replaced as the root tag by `SpineML`.

## Dimensionality

The units and dimensions of components parameters and analog/impulse input ports are specified within a component description. A single ```@dimension``` attribute is used for this purpose and consists of a value derived from, a Si unit described by using a prefix and unit from the following tables. E.g. millivolts (mV) is prefix=m, unit=V.

| Prefix | Description |
| --- | --- |
| G | giga (10<sup>9</sup>) |
| M | mega (10<sup>6</sup>) |
| k | kilo (10<sup>3</sup>) |
| c | centi (10<sup>-2</sup>) |
| m | milli (10<sup>-3</sup>) |
| u | micro (10<sup>-6</sup>) |
| n | nano (10<sup>-9</sup>) |
| p | pico (10<sup>-12</sup>) |
| f | femto (10<sup>-15</sup>) |

| Unit | Description |
| --- | --- |
| V | volt (voltage) |
| Ohm | Ohm (resistance) |
| g | gram (weight) |
| m | meter (distance) |
| S | siemens (conductance) |
| A | ampere (current) |
| cd | candela (luminosity) |
| mol | mole (substance) |
| degC | degree Celsius (temperature) |
| s | seconds (time) |
| F | farad (capacitance) |
| Hz | hertz (frequency) |
