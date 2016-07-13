---
layout: api
title: Component Layer
---

# Component layer

The SpineML component layer syntax describes the functional entities, i.e. neurons and synapses, that are used as building blocks from a model. The syntax is derived from the NineML abstraction layer and LEMS common object model specification with some very minor changes requiring the specification of an initial regime and some minor syntax changes to the specification of ports.

A technical description of the NineML Abstraction layer is available [here](http://software.incf.org/software/nineml/wiki/nineml-specification/nineml-specifications#SECTION01030).

## Dimensionality

The units and dimensions of components parameters and analogue/impulse input ports are specified within a component description. A single ```@dimension``` attribute is used for this purpose and consists of a value derived from, a Si unit described by using a prefix and unit from the following tables. E.g. millivolts (mV) is prefix=m, unit=V.

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
