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

[Creating a component in the component editor](/spinecreator/createcomponent)

[Creating a network in the network editor](/spinecreator/createnetwork)

[Creating experiments and graphing](/spinecreator/createexpt)

[More about data input to a model](/spinecreator/datainput)

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

I usually develop my connectionFunc externally, using matplotlib to
look at the connection patterns. Here's an example, called gabor.py:

```python
#
# 1) Define the connectionFunc - the code that will be pasted into the
# python script settings window in SpineCreator.
#

#PARNAME=sigma_g  #LOC=1,1
#PARNAME=gain_g   #LOC=1,2
#PARNAME=lambda_s #LOC=2,1
#PARNAME=gain_s   #LOC=2,2
#PARNAME=dir_s    #LOC=3,1
#PARNAME=wco      #LOC=3,2
#PARNAME=roi      #LOC=4,1
#HASWEIGHT

# This implements a Gabor connection; a 2-D Gaussian multiplied by a
# 1-D sine. Arguments are:
#
# sigma_g - sigma of the gaussian
# gain_g - a gain for the gaussian
# lambda_s - wavelength of sine
# gain_s - gain of sine
# dir_s - direction of (1-D) sine in degrees
def connectionFunc(srclocs,dstlocs,sigma_g,gain_g,lambda_s,gain_s,dir_s,wco,roi):

    import math

    twopi = 6.283185307;

    i = 0 # i is 'src iterator'
    j = 0 # j is 'dst iterator'
    out = [] # To return result

    for srcloc in srclocs:
        j = 0
        for dstloc in dstlocs:

            # Avoid many tanh, sin, cos, pow and exp computations for well-separated neurons:
            xdist = srcloc[0] - dstloc[0]
            ydist = srcloc[1] - dstloc[1]
            # Ignore zdist
            if abs(xdist) > roi or abs(ydist) > roi:
                j = j + 1
                continue

            zdist = srcloc[2] - dstloc[2]
            dist = math.sqrt(math.pow(xdist,2) + math.pow(ydist,2) + math.pow(zdist,2))

            # Direction from source to dest
            ##print ('dstloc[1]: ', dstloc[1], ' srcloc[1]:', srcloc[1])
            ##print ('dstloc[0]: ', dstloc[0], ' srcloc[0]:', srcloc[0])
            top = dstloc[1]-srcloc[1]
            bot = dstloc[0]-srcloc[0]
            ##print ('top:',top,'bot:',bot)
            dir_d = math.atan2(top, bot);
            ##print ('dir_d: ', dir_d, 'dir_s:', dir_s)

            # Find the projection of the source->dest direction onto the sine wave direction. Call this distance dprime.
            dprime = dist*math.cos(dir_d + twopi - ((dir_s*twopi)/360));
            ##print ('dprime: ', dprime, ' dist: ', dist)

            # Use dprime to figure out what the sine weight is.
            sine_weight = gain_s*math.sin(dprime*twopi/lambda_s);
            ##print ('sine_weight:', sine_weight)

            gauss_weight = gain_g*math.exp(-0.5*math.pow(dist/sigma_g,2))
            ##print ('gauss_weight:', gauss_weight)

            combined_weight = sine_weight * gauss_weight;
            ##print ('combined_weight:', combined_weight)

            if abs(combined_weight) > wco:
                #sys.stdout.write('gauss>0.0001: i={0} gauss={1}'.format( i, gauss))
                conn = (i,j,0,combined_weight)
                out.append(conn)

            j = j + 1
        i = i + 1
    #sys.stdout.write('out length: %d' % len(out))
    return out

#
# 2) Compute weights for the map
#

# Set up some parameters
rowlen = 50
sigma_g = 2
gain_g = 1
lambda_s = 10
gain_s = 1
dir_s = 45
wco = 0.001
roi = 8; # 'Region of interest' For index n, consider distances +-roi for weights, outside this region, don't add a weight.

# Containers for source/destination locations
srclocs = []
# Make srclocs - this makes rowlen x rowlen grids:
for i in range(0, rowlen):
    for j in range(0, rowlen):
        srcloc=[j,i,0] # [x,y,z] locations
        srclocs.append(srcloc)

# Call the connectionFunc to generate result
result = connectionFunc (srclocs, srclocs, sigma_g, gain_g, lambda_s, gain_s, dir_s, wco, roi)
print "Done computing"


#
# 3) Show weight results for one particular source neuron projecting
# out to destination neurons.
#

import math

# The source neuron index to look at the connection pattern
src_index = 1275

# Extract xs, ys and weights for source-to-destination connection into
# these lists:
xs = []
ys = []
ws = []

for res in result:
    if (res[0] == src_index):
        # In my example, the position x and y come from the
        # destination index, which is found in res[1]. res[2] contains
        # the delay (unused here). res[3] contains the weight.
        xs.append(res[1]%rowlen)
        ys.append(math.floor(res[1]/rowlen))
        ws.append(res[3])
        #print ('Appended ', res[1]%rowlen, math.floor(res[1]/rowlen), res[3])

# Now do a scatter plot of the weights
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.scatter(xs,ys,ws)
plt.show()
```
