---
layout: default
title: Testing your SpineCreator installation
---
# Testing your SpineCreator installation

Whether you installed on Mac or Linux, you should be able to test your
installation by running the GPR Basal Ganglia model. This model is
available from the ABRG-Models github account. You can either fork the
model, or simply clone it:

```
cd ~
git clone https://github.com/ABRG-Models/GPR-BasalGanglia.git
```

Open SpineCreator, select "File -> Load project" and navigate to
GPR-BasalGanglia/SpineML/GPR_BG.proj

You should see the Network for the model looking like this:

![TestInst_Network](/public/images/TestInst_Network.png "Network diagram for GPR Basal Ganglia model"){: .center-image }

Click on the "Expts" button to get to the Experiment interface. Choose
the "Step Input" experiment and press the "Run experiment" button:

![TestInst_Expt](/public/images/TestInst_Expt.png "Experiment layer"){: .center-image }

The system should first compile the components, then run the
model. Compiling the components will take much longer than running
this particular model. If you get the "Simulator Complete" message,
then you can nagivate to the "Graphs" interface. The "Loaded Logs"
list should be populated with several logs. Click on
"SNr_out_log.bin". In "Select log indices to plot" highlight all six -
hold the Ctrl key down and drag across them. Finally, click on "Line
plot" for "Plot type" and then press the "Add plot" button. Your
output should look like this:

![TestInst_Output](/public/images/TestInst_Output.png "Output of the model's Substantia Nigra pars Reticulata population"){: .center-image }

If your output matches the above, then congratulations, you have a
working SpineCreator!
