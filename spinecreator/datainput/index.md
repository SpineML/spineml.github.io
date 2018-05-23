---
layout: default
title: SpineCreator data input
---

More about getting data into your model. If you need either to: a) dynamically
generate data or b) generate more complex static data input than is possible from
within the SpineCreator experiments interface, you'll need to use one
of the methods described here to get the data into your running
SpineML model.

# SpineMLNet: TCP/IP data input

You can write a program to send data via a network connection to your
SpineML model. This is implemented both for the SpineML_2_BRAHMS
simulator backend and for SpineML_2_GeNN.

The network protocol is described in the SpineCreator source code; see
the file networkserver/protocol.txt.


```
SpineMLNet protocol
-------------------

Here is the network protocol for communicating data with
SpineCreator-generated SpineML models.

These notes were developed from an email from Alex Cope, and
subsequent experience implementing the matlab/mex functions which
allow you to communicate data from your matlab script to your SpineML
model. One extra feature was added to the protocol (a connection name
is transferred during the handshake).

Seb James, June 2014.

----------- Alex Cope's description -----------------

Note: Alex's suggestion was to use matlab's in-built tcp/ip
communication methods. I found these to be aimed at line-by-line
network communications, rather than the byte-by-byte style protocol
defined by SpineML/SpineCreator.

Note: The C++ code referred to at the end is
spinemlnetworkserver.cpp/h, which you can find in the SpineCreator
source tree.

Note: I've added some notes to clarify some of the steps and also
2g-2i, which add a connection name to the protocol.

-----------------------------------------------------
So here is the process to connect:

1) Listen for a connection (using fopen(t), where t is the tcp server)
- this will block until a connection is received

2) Handshake:
2a) Read a byte - this is the data direction.
    The value will be AM_SOURCE or AM_TARGET depending on whether the
    connection is an input or output.
2b) Send RESP_HELLO
2c) Read a byte - this is the data type.
    Value should be RESP_DATA_NUMS (i.e. Analog data).
2d) Send RESP_RECVD
2e) Read an int (4 bytes) - this is data size.
    Size is given in "number of doubles to transfer per timestep".
2f) Send RESP_RECVD
2g) Read an int (4 bytes) - this number is the connection name length.
2h) Read name length bytes - this is the connection name.
2i) Send RESP_RECVD

3) Data sending:
(Ok, now comes the trickier bit)
3a) Enter data sending loop
3b) For each connection
3c) if is an input to the model:
Check if there is a SendData reply, or we haven't sent un-replied data
    - IF TRUE: Send data
    - IF FALSE: Continue
3d) If there is an output from the model
See if there is data available
     - IF TRUE: Read data
     - IF FALSE: Continue
3e) If we have neither read nor written, increase a counter, if the
    counter is > 10 abort
3f) Pause for a few milliseconds
3f) Loop


Here are the values of the codes that are sent:

#define RESP_DATA_NUMS      31
#define RESP_DATA_SPIKES    32
#define RESP_DATA_IMPULSES  33
#define RESP_HELLO          41
#define RESP_RECVD          42
#define RESP_ABORT          43
#define RESP_FINISHED       44
#define AM_SOURCE           45
#define AM_TARGET           46
#define NOT_SET             99

--------------------------------------------------

When the client connection is complete, it will simply hang up. This
is seen at the server side by a fail to read any further data or
responses.
```

## c++ implementation of SpineMLNet

There's an example of how to write a c++ program as a "data server"
process that you can launch with your SpineML simulation. See
networkserver/cpp/spinemlnetworkserver.cpp/h and modify to suit your
model.

## Matlab implementation of SpineMLNet

There's also a matlab/octave option, for which you can build a set of
mex/oct files that allow your to write your data server in matlab
code. See networkserver/matlab/readme.spinemlnet, reproduced here:

```
SpineMLNet on MATLAB and Octave
-------------------------------

This is a communication server which can be launched from your matlab
(or octave) script. It can send data to and receive data from a
SpineML model (which is probably running on Brahms).

It's a set of Matlab mex functions (also compilable as octfiles) which
implement the network server to which the SpineML model connects as a
network client.

The functions are:

spinemlnetStart() - this launches the server

spinemlnetAddData() - write some data into the spinemlnet server's data buffer.
                      These data will then be sent to the SpineML model.

spinemlnetGetData() - retrieve data from the spinemlnet server which the
                      SpineML model sent over.

spinemlnetQuery() - Check up on the progress of the simulation; has the
                    SpineML model finished?

spinemlnetStop() - Stop the spinemlnet server.

Consult spinemlnet_run.m and spinemlnet_test.m to see how to use these
mex functions.

You can run the example:

1) Build the mex/oct files, using either ./mexbuild or ./octbuild

2) Run matlab or octave and start the server:

   spinemlnet_test

3) Open the SpineCreator project spinemlnet_test and run the experiment
   "Simple1"
```

# Data input with a BRAHMS component using external.xsl

If you place an external.xsl file into your SpineCreator project, and
you're using SpineML_2_BRAHMS as the simulator backend, and you've
included external.xsl in the &lt;AdditionalFiles&gt; section of the
[SpineCreator project file](/spinecreator/annotations#spf)
SpineML_2_BRAHMS will read external.xsl. That means you can write a
BRAHMS component to generate input data, and specify in external.xsl
how to connect that data to inputs in your SpineML model. Contact Seb
James for help setting this up. See external_default.xsl in
SpineML_2_BRAHMS for some more info in the comments.

# A future SpineMLData plugin

I (Seb) would like to add a simulator backend-independent plugin API
to make it possible to access data from a C++ program without using
the SpineMLNet protocol.
