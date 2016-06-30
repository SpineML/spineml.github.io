---
layout: api
title: Testing your SpineCreator installation
---

# Tutorials

We have three tutorials to help new users get used to working with the
idea of creating components. **Components** are used to specify the
mathematical rules for the neural elements in a model. These
components are used as the basis for the populations which are
connected together in a **network**. Once you have a network, you can
then create an **experiment** which allows you to simulate the model
for a given amount of time and with a given set of inputs.

[Creating a component in the component editor](spinecreator/createcomponent)

[Creating a network in the network editor](spinecreator/createnetwork)

[Creating experiments and graphing](spinecreator/createexpt)

## Creating a new connectivity using a Python Script

Python scripts for generating connectivity are added to SpineCreator
through the Settings/Preferences dialog, and once added appear in the
list of connection types.

To allow non-programmers to use and configure extended connectivity
types a set of formatted comments is added to the top of the Python
script: these specify parameters to pass to the script, and define how
SpineCreator should provide a graphical interface for these
parameters. For example:

```python
#PARNAME=sigma #LOC=1,1
#PARNAME=minimum_weight #LOC=2,1
#HASWEIGHT

def connectionFunc(srclocs,dstlocs,sigma,min_w):
  import math
  normterm = 1/(sigma*math.sqrt(2*math.pi))
  i = 0
  out = []
  for srcloc in srclocs:
    j = 0
    for dstloc in dstlocs:
      dist = math.sqrt(math.pow((srcloc[0] - dstloc[0]),2) + \
        math.pow((srcloc[1] - dstloc[1]),2) + \
        math.pow((srcloc[2] - dstloc[2]),2))
      gauss = normterm*math.exp(-0.5*math.pow(dist/sigma,2))
      if gauss > min_w:
        conn = (i,j,0,gauss)
        out.append(conn)
      j = j + 1
    i = i + 1
  return out
```

Adding parameters to the SpineCreator GUI is performed using the
\#PARNAME comment, which gives a name used as a label for the
parameter, and a \#LOC which describes the row and column where the
parameter should be added in a grid for laying out the parameters. The
order of the parameters in the code denotes the order they have in the
corresponding Python function call, and allows the label to have a
more descriptive name than the variable used in the function. In
addition there are two more comments that are parsed; \#HASWEIGHT and
\#HASDELAY, which inform SpineCreator if the script needs to generate a
weight and/or a delay. If a weight is generated SpineCreator will
provide a drop-down list of the corresponding Properties in the
WeightUpdate Component, and the selected Property will have the weight
values inserted when the connectivity is generated.

The function itself has arguments srclocs and dstlocs, which are Lists
of Tuples, each Tuple containing the x, y, and z co-ordinates of the
neuron at that index in the List.
