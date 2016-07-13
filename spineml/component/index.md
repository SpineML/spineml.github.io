---
layout: api
title: Component Layer
---

# Component layer

The SpineML component layer syntax describes the functional entities, i.e. neurons and synapses, that are used as building blocks from a model. The syntax is derived from the NineML abstraction layer and LEMS common object model specification with some very minor changes requiring the specification of an initial regime and some minor syntax changes to the specification of ports.

A technical description of the NineML Abstraction layer is available [here](http://software.incf.org/software/nineml/wiki/nineml-specification/nineml-specifications#SECTION01030).

# Differences from NineML Abstraction Layer

We now describe the differences between the NineML Abstraction layer and the SpineML Component layer explicitly.

## Ports

In SpineML we use W3C shemas for validation. This means that the port tags require seperation into several types.

### AnalogSendPort

|  Contains  |  Description  |
| ----- |
|  `@name`  |  state variable or alias for output  |
|  `@is_per_connection`  |  for weightupdates sends an output for each connection, instead of each destination neuron  |

### AnalogReceivePort

|  Contains  |  Description  |
| ----- |
|  `@name`  |  port identifying name  |
|  `@dimension`  |  port dimensionality  |
|  `@post`  |  for weightupdates remaps the input into connections as though it were a postsynaptic input  |

### AnalogReducePort

|  Contains  |  Description  |
| ----- |
|  `@name`  |  port identifting name  |
|  `@dimension`  |  port dimensionality  |
|  `@reduce_op`  |  operation used to combine multiple inputs  |
|  `@post`  |  for weightupdates remaps the input into connections as though it were a postsynaptic input  |

### EventSendPort

|  Contains  |  Description  |
| ----- |
|  `@name`  |  port identifying name  |
|  `@is_per_connection`  |  for weightupdates sends an output for each connection, instead of each destination neuron  |

### EventReceivePort

|  Contains  |  Description  |
| ----- |
|  `@name`  |  port identifying name  |
|  `@dimension`  |  port dimensionality  |
|  `@post`  |  for weightupdates remaps the input into connections as though it were a postsynaptic input  |

### ImpulseSendPort

|  Contains  |  Description  |
| ----- |
|  `@name`  |  statevariable, parameter or alias for output  |
|  `@is_per_connection`  |  for weightupdates sends an output for each connection, instead of each destination neuron  |

### ImpulseReceivePort

|  Contains  |  Description  |
| ----- |
|  `@name`  |  port identifying name  |
|  `@dimension`  |  port dimensionality  |
|  `@post`  |  for weightupdates remaps the input into connections as though it were a postsynaptic input  |

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
