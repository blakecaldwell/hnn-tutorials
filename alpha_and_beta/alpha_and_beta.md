<!-- pandoc -f markdown -t html -o alpha_and_beta.html alpha_and_beta.md -->

# Alpha (7-14 Hz) and Beta (15-29 Hz) Rhythms: The Mu-Complex #

## Getting Started ##

In order to understand the workflow and initial parameter sets provided with this tutorial, we must first briefly describe prior studies that led to the creation of the data you will aim to simulate. This tutorial is based on results from Jones et al. 2009 where, using MEG, we recorded spontaneous (pre-stimulus) alpha (7-14 Hz) and beta (15-20 Hz) rhythms that arise as part of the mu-complex from the primary somatosensory cortex (S1) [1]. (Figure 1. See also [2-4].)

<!-- start Figure 1 -->

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 1

<p><a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image3.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image3.png" alt="image3" width=90%/></a></p>

**Left:** Spectrogram of spontaneous activity from current dipole source in SI averaged across 100 trials, from an example subject, shows nearly continuous prestimulus alpha and beta oscillations. At time zero, a brief tap was given to the contralateral finger tip and the spontaneous oscillations briefly desynchronized.

**Right:** A closer look at the prestimulus waveform and spectrogram from spontaneous activity during example signal trials, shows that the alpha and beta oscillations occur intermittently and primarily non-overlapping.</span>

</div>

<!-- end Figure 1 -->

Our goal was to use our neocortical model to reproduce features of the waveform and spectrogram observed on single unaveraged trials (Figure 2 top panel, right) where the alpha and beta components emerge briefly and intermittently in time. On any individual trial (i.e., 1 second of spontaneous data), the presence of alpha and beta activity is not time locked and representative of so-called “induced” activity. Seemingly continuous bands of activity occur only when averaging the spectrograms across trials (Figure 2 top panel, left), and this is due to the fact that the spectrograms values are strictly positive and the alpha and beta events accumulate without cancellation [4].

<!-- start Figure 2 -->

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 2

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image29.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image29.png" alt="image29" width=90%/>
</a>
<p>Key features of the spontaneous non-average SI alpha/beta complex include, intermittent transient bouts of alpha/beta activity, a waveform that oscillates around 0nAm, PSD with peaks in the alpha and beta bands, primarily non-overlapping alpha and beta events, and a symmetric waveform oscillation. The model was able to reproduce each of these features.</p>

</div>

<!-- end Figure 2 -->

<!-- start Figure 3 -->

<div style="background-color:rgba(0, 0, 0, 0); margin-top:20px; text-align: center; vertical-align: middle; margin-bottom:40px;">

<h3>Figure 3</h3>

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image4.png">
<img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image4.png" alt="image4" width=80%/>
</a>
<p>Schematic illustration of exogenous 10 Hz burst drive through proximal and distal projection pathways.  “Population bursts”, consisting of a set number of “burst units” (10, 2-spike bursts shown) drive post-synaptic conductances in the local network with a set frequency (100 ms ISI) and mean delay between proximal and distal. </p>
</div>

<!-- end Figure 3 -->

We found that a sequence of exogenous subthreshold excitatory synaptic drive could activate the network in a manner that reproduced important features of the SI rhythms in the model (Figure 2). This drive consisted of two nearly-synchronous 10 Hz rhythmic drives that contacted the network through proximal and distal projection pathways (Figure 3). The drives were simulated as population “bursts” of action potentials that contacted the network every 100ms with the mean delay between the proximal and distal burst of 0ms. Specifically, as shown schematically in Figure 3, the population bursts consisted of 10, 2-spike bursts Gaussian distributed in time. We presumed that during such spontaneous activity, these drives may be provided by leminscial and non-lemniscal thalamic nuclei, which contact proximal and distal pyramidal neurons respectively, and they are know to burst fire at  ~10 Hz frequencies in spontaneous states [5,6].

We assumed that the macroscale rhythms generating the observed alpha and beta activity arose from subthreshold current flow in a large population of neurons, as opposed to being generated by local spiking interaction. As such, the effective strengths of the exogenous driving inputs were tuned so that the cells in the network remained subthreshold (all other parameters were tuned and fixed base on the morphology, physiology and connectivity within layered neocortical circuits, see Jones et al. 2009 [1] for details). The inputs drove subthreshold currents up and down the pyramidal neurons to reproduce accurate waveform and spectrogram features (see Figure 3). A scaling factor of 3000 was multiplied by the model waveform to reproduce nAm units comparable to the recorded data, suggesting on the order 200 x 3000 = 600,000 pyramidal neurons contributed to this signal.

We further found that decreasing the delay between the drives to ~50ms created a pure alpha oscillation, while applying an ~0ms delay caused beta events to emerge and increased the strength of the distal drive, creating stronger beta activity (data not shown; see parameter exploration below). This result led to the novel prediction that brief beta events emerge from a broad proximal drive disrupted by a simultaneous strong distal drive that lasted 50ms (i.e., one beta period). Support for this prediction was found invasively with laminar recordings in mice and monkeys [3].

In this tutorial, we will explore parameter changes that illustrate these results. We will walk you step-by-step through simulations with various combinations of rhythmic proximal and distal drives to describe how each contributes to the alpha and beta components of the SI alpha/beta complex rhythm. We will begin by simulating only rhythmic proximal alpha frequency inputs (Step 1), followed by simulating only distal alpha frequency inputs (Step 2), followed by various combinations of proximal and distal drive to generate alpha and beta rhythms. We’ll show you how HNN can plot waveforms, time-frequency spectrograms, and power spectral density plots of the simulated data, as well as for imported recorded data.

## Tutorial Table of Contents

1\. [Simulating Rhythmic Proximal Inputs: Alpha only](#toc_one)

2\. [Simulation Rhythmic Distal Inputs: Alpha only](#toc_two)

3\. [Simulating Combined Rhythmic Proximal and Distal Inputs: Alpha/Beta Complex](#toc_three)

4\. [Calculating and Viewing Power Spectral Density (PSD)](#toc_four)

5\. [Comparing model output and recorded data](#toc_five)

6\. [Adjusting parameters](#toc_six)

7\. [Have fun exploring your own data!](#toc_seven)

<!-- -->
<!-- -->
<!-- the above lines of text have been reviewed -->
<!-- -->
<!-- -->

<a id="toc_one"></a>

## 1. Simulating Rhythmic Proximal Inputs: Alpha Only

Note that before running/loading new simulations, we first remove the prior simulation(s) by pressing the “Remove Simulation” button at the bottom of the GUI. If we do not do this, both simulation dipoles are displayed (the old simulation is displayed with a dotted line, new simulation with a solid line; see “Tour of the GUI” for more details on simulation controls).

### 1.1 Load/view parameters to define the network structure & to “activate” the network.  

As described in the “Getting Started” section, low-frequency alpha and beta rhythms can be simulated by a combination of rhythmic subthreshold proximal and distal ~10Hz inputs. Here, we begin by describing the impact of proximal inputs only. An initial parameter set that will simulate the effect of ~10 Hz subthreshold proximal drive is provided in the file [OnlyRhythmicProx.param](https://www.google.com/url?q=https://hnn.brown.edu/wp-content/uploads/2018/05/AlphaAndBeta.zip&sa=D&ust=1552525363605000).



The template cortical column networks structure for this simulation is described in the [Overview](https://www.google.com/url?q=https://hnn.brown.edu/index.php/overview-uniqueness/&sa=D&ust=1552525363605000) and [Under the Hood](https://www.google.com/url?q=https://hnn.brown.edu/index.php/under-the-hood/&sa=D&ust=1552525363605000) sections. Several of the network parameter can be adjusted via the HNN GUI (e.g. local excitatory and inhibitory connection strengths), but we will leave them fixed for this tutorial and only adjust the inputs the “activate” the network.



To load the initial parameter set, navigate to the HNN GUI and click:

```
Set Parameters From File
```

Then select the file [OnlyRhythmicProx.param](https://www.google.com/url?q=https://hnn.brown.edu/wp-content/uploads/2018/05/AlphaAndBeta.zip&sa=D&ust=1552525363606000) from HNN’s param subfolder or from your local machine. 



To view the parameters that “activate” the network via rhythmic proximal input, click:

```
Set Parameters > Rhythmic Proximal Inputs
```

You should see the values of adjustable parameters displayed as in the dialog boxes below. There are 3 tabs, one regulating the timing statistics of the driving input, one regulating the post-synaptic conductances onto the Layer 2/3neurons, and one regulating the post-synaptic conductances onto the Layer 5 neurons. We describe adjustable parameters in each dialog box separately.

<!-- start Figure 4 -->

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

<table style="border-collapse: collapse; border: none;">

<tr style="border: none;">
<h3>Figure 4</h3>
</tr>

<tr style="border: none;">

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image11.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image11.png" alt="image11" width=100%/>
</a>
<p>  </p>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image18.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image18.png" alt="image18" width=100%/>
</a>
<p>  </p>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image32.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image32.png" alt="image32" width=100%/>
</a>
<p>  </p>
</td>

</tr>

</table>

</div>

<!-- end Figure 4 -->

Timing tab: The rhythmic proximal inputs drive excitatory synapses in the neocortical network in a proximal projection pattern, as shown at the bottom of the dialog box. For further details on the connectivity structure of the network, see the Under the Hoodsection of the HNN website. Rhythmic proximal input occurs through stochastic, presynaptic bursts of action potentials from a population of bursting cells (set with “Number bursts”; see Figure 3) onto postsynaptic neurons of the modelled network. Stochasticity is introduced in two places: the spike train start time for each bursting cell is sampled from a normal distribution with mean “Start time mean (ms)” and standard deviation “Start time stdev (ms)” and the inter-burst intervals for each bursting cell are sampled from a normal distribution of mean ![](https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image1.png)(e.g., a 100 ms inter-burst interval corresponds to a “Burst frequency” of 10 Hz) and standard deviation “Burst stdev (ms)” (see Figure 3). Also note that the number of spikes per burst unit is set with “Spikes/burst” (currently, only values of 1 and 2 with a fixed 10ms delay can be used) and the final stop time for the entire population of rhythmic proximal inputs is set with “Stop time (ms)”.

Layer 2/3, and Layer 5 tabs: This dialog boxallows you to set the postsynaptic conductance of each of the excitatory synapses in the networks. There are AMPA and NMDA receptors on each cell type (pyramidal and basket cells). There is also a delay parameter to control the arrival time of each spike to the network. In this example, the delay to the layer 2/3 cells is 0.1 ms, with a slightly longer delay to the layer 5 cells of 1 ms. For further details on the connectivity structure of the network, see Under the Hood.  

### 1.2 Run the simulation and visualize net current dipole

To run this simulation, navigate to the main GUI window and click:
```
Start Simulation
```
This simulation runs for 700 ms of simulation time, so will take a little longer to run than the ERP simulations. Once completed, you will see output similar to that shown below.

<!-- start Figure 5 -->

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:20px">

### Figure 5

<p align="center"><a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image29.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image25.png" alt="image25" width=90%/></a></p>
</div>

<!-- end Figure 5 -->

As shown in the red histogram in the top panel of Figure 5 above, with this parameter set, a burst of proximal input spikes is provided to the network ~10 Hz (i.e., every 100 ms). Due to the stochastic nature of the inputs (controlled by the start time stdev and Burst stdev parameters, there is some variability in the histogram of proximal input times. Note that a decrease in the Burst stdev would create shorter duration bursts (i.e., more synchronous bursts); this will be explored further in step 6.1 below.

The ~10 Hz bursts of proximal drive induces current flow up the pyramidal neuron dendrites increasing the signal above the 0 nAm baseline, which then relaxes back to zero, approximately every 100 ms. This is observed in the black current dipole waveform in the GUI window. The middle panel shows the corresponding time-frequency spectrogram for this waveform that exhibits a high-power continuous 10 Hz signal. Importantly, in this example the strength of the proximal input was titrated to be subthreshold (i.e., cells do not spike) under the assumption that macroscale oscillations are generated primarily by subthreshold current flow across large populations of synchronous pyramidal neurons. In step 6.2 below, we explore differences in the signal when the cells are driven to spike (see also ERP tutorial).

While this exploration with proximal drive is only useful in understanding how subthreshold rhythmic inputs impact the current dipole produced by the circuit, several features of the waveform and spectrogram of the signal do not match the recorded data shown in Figures 1and 2. Next, we explore the impact of rhythmic distal inputs only (step 2), and then a combination of the two (step 3).

<a id="toc_two"></a>

## 2. Simulating Rhythmic Distal Inputs: Alpha Only

Having seen that proximal inputs alone push the current flow only in a single direction (positive), we can confirm that the same occurs if we provide only rhythmic distal inputs, which drive current flow in the pyramidal neuron dendrites, and hence current dipole signal, in the opposite direction (negative).

### 2.1 Load/view parameters to define the network structure & to “activate” the network

We will use a param file that generates bursts of distal inputs provided at the alpha frequency (10 Hz; [OnlyRhythmicDist.param](https://www.google.com/url?q=https://hnn.brown.edu/wp-content/uploads/2018/05/AlphaAndBeta.zip&sa=D&ust=1552525363614000)).

The template cortical column networks structure for this simulation is described in the Overview and What’s Under the Hood sections. Several of the network parameter can be adjusted via the HNN GUI (e.g. local excitatory and inhibitory connection strengths), but we will leave them fixed for this tutorial and only adjust the inputs the “activate” the network.

To load the initial parameter set, navigate to the HNN GUI and click:
```
Set Parameters From File
```
Then select the file [OnlyRhythmicDist.param](https://www.google.com/url?q=https://hnn.brown.edu/wp-content/uploads/2018/05/AlphaAndBeta.zip&sa=D&ust=1552525363615000) from HNN’s param subfolder or from your local machine. To view the new parameters that “activate” the network via rhythmic distal input, click:
```
set parameters > Rhythmic Distal Inputs
```
You should see the values of adjustable parameters displayed as  in the dialog boxes below. Notice that these parameters are the same as those regulating the proximal drive in step (1). However, in this case the parameters define bursts of synaptic inputs that drive the network in a distal project pattern, shown schematically at the bottom of the dialog box.

<!-- start Figure 6 -->

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

<table style="border-collapse: collapse; border: none;">

<tr style="border: none;">
<h3>Figure 6 </h3>
</tr>

<tr style="border: none;">

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image6.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image6.png" alt="image6" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image22.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image22.png" alt="image22" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image5.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image5.png" alt="image5" width=100%/>
</a>
</td>

</tr>

</table>

</div>

<!-- end Figure 6 -->

### 2.2 Run the simulation and visualize net current dipole

To run this simulation, navigate to the main GUI window and click:
```
Start Simulation
```
Once completed, you will see output in the GUI window similar to that shown below.

<!-- start Figure 7 -->

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 7

<p align="center"><a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image7.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image7.png" alt="image7" width=90%/></a>

</div>

<!-- end Figure 7 -->

As shown in the green histogram in the top panel of the HNN GUI above, with this parameter set, a burst of distal input spikes is provided to the network ~10 Hz (i.e., every 100 ms). Due to the stochastic nature of the inputs (controlled by the start time stdev, and Burst stdev parameters), there is some variability in the histogram of proximal input times. The ~10 Hz bursts of distal input induces current flow down the pyramidal neuron dendrites decreasing the signal below the 0 nAm baseline, which then relaxes back to zero, approximately every 100 ms. This is observed in the black current dipole waveform in the GUI window. The middle panel shows the corresponding time-frequency spectrogram for this waveform that exhibits a high power continuous 10 Hz signal. Importantly, in this example the strength of the distal input was also titrated to be subthreshold (i.e., cells do not spike) under the assumption that macroscale oscillations are generated primarily by subthreshold current flow across large populations of synchronous pyramidal neurons.  

While instructional, this simulation also does not produce waveform and spectral features that match the experimental data in Figures 1 and 2. In the next step (step 3), we describe how combining both the 10 Hz proximal and distal drives can produce an oscillation with many characteristic features of the spontaneous SI signal (Jones et al 2009).

<a id="toc_three"></a>

## 3. Simulating Combined Rhythmic Proximal and Distal Inputs: Alpha/Beta Complex

### 3.1 Load/view parameters to define the network structure & to “activate” the network.  

In this example, we provide a parameter set ([AlphaAndBeta.param](https://www.google.com/url?q=https://hnn.brown.edu/wp-content/uploads/2018/05/AlphaAndBeta.zip&sa=D&ust=1552525363619000)) that produces many of the waveform and spectral features observed in our SI data (Figure 2).

The template cortical column networks structure for this simulation is described in the Overview and What’s Under the Hood sections. Several of the network parameter can be adjusted via the HNN GUI (e.g. local excitatory and inhibitory connection strengths), but we will leave them fixed for this tutorial and only adjust the inputs the “activate” the network.

To load the initial parameter set, navigate to the HNN GUI and click:
```
Set Parameters From File
```
Then select the file [AlphaAndBeta.param](https://www.google.com/url?q=https://hnn.brown.edu/wp-content/uploads/2018/05/AlphaAndBeta.zip&sa=D&ust=1552525363620000) from HNN’s param subfolder or from your local machine.

To view the new parameters that “activate” the network via both rhythmic proximal and rhythmic distal input, click:
```
Set Parameters > Rhythmic Proximal Inputs

Set Parameters > Rhythmic Distal Inputs
```
You should see the values displayed in the dialogue boxes below.

<!-- start Figure 8 -->

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

<table style="border-collapse: collapse; border: none;">

<tr style="border: none;">
<h3>Figure 8 </h3>
</tr>

<tr style="border: none;">

<td style="border: none;" width="50%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image11.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image11.png" alt="image11" width=100%/>
</a>
</td>

<td style="border: none;" width="50%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image6.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image6.png" alt="image6" width=100%/>
</a>
</td>

</tr>

</table>

</div>

<!-- end Figure 8 -->

In this simulation, the Start time mean (ms) values for both proximal and distal inputs are set to 50.0 ms, and all other parameters are the same. Note that the synaptic weights are the same as used in the previous two simulations (not shown in dialog boxes above, click on Layer 2/3 and Layer 5 to see them). The equal start time implies that the proximal and distal input bursts will arrive nearly synchronously to the network on each cycle of the 10 Hz input. Due to the stochasticity in the parameters (start time stdev, and Burst stdev) sometimes the bursts will arrive together and sometimes there will be a slight delay. As will be described further below, this stochasticity creates intermittent alpha and beta events.  

### 3.2 Run the simulation and visualize net current dipole

To run this simulation, navigate to the main GUI window and click:
```
Start Simulation
```
Once completed, you will see output similar to that shown below.

<!-- start Figure 9 -->

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 9

<p align="center"><a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image28.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image28.png" alt="image28" width=90%/></a>

</div>

<!-- end Figure 9 -->

As shown in the green and red histogram in the top panel of the HNN GUI above, with this parameter set, bursts of both proximal and distal input spikes are provided to the network ~10 Hz (i.e., every 100 ms). Due to the stochastic nature of the inputs, there is some variability in the timing and duration of the input bursts such that sometimes they arrive at the same time and sometimes there is a slight offset between them. As a result, intermittent transient alpha and beta  events emerge in the time-frequency spectrogram. Alpha events are produced when the inputs occur slightly out of phase and current flow is pushed alternately up and down the dendrites for ~50 ms duration each (set by the length of the bursts inputs). Beta events occur when the burst inputs arrive more synchronously and the upward current flow is disrupted by downward current flow for ~50 ms to effectively cut the oscillation period in half. As such, the relative alpha to beta expression can be controlled by the delay between the inputs and their relative burst strengths. We will detail this further below (see step 6 below).

In contrast to the results from only proximal or distal input, since the current in the pyramidal neurons is pushed both upward and downward in this simulation, the current dipole signal oscillates above and below 0 nAm, which qualitatively matches the experimental data (see Figures 1 and 2 in “Getting Started”).   Additionally, this simulation reproduces the transient nature of the alpha and beta activity and several other features of the waveform and spectrogram can be quantified to show close agreement between model and experimental results (see Figure 2 above, and Jones et al. 2009[1], for further details). 

We note that here we do not directly compare the spontaneous current dipole waveform to recorded data, as was done in the ERP tutorial with a root mean squared error. This is due to the fact that the spontaneous SI signal we are simulating is not time locked to  alpha or beta events on any given trial, and the stochastic nature of the driving inputs causes variability in the timing of the alpha or beta activity, making it difficult to align recorded data and simulated results. However, a direct comparison can be made between time averaged recorded and simulated signals by comparing power spectral density waveforms. An example of comparison is shown in step 5 below.

### 3.3 Simulating and averaging multiple trials with jittered start times creates the impression of continuous oscillations

As described in the “Getting Started” section above, our simulation goal was to study the mechanisms that reproduce features of spontaneous alpha and beta rhythms observed in un-averaged data, where the alpha and beta components are transient and intermittent (Figure 1, right panel). Each tutorial step up to this point was based on simulating un-averaged data. Here, we describe how to run and average multiple “trials” (700 ms epochs of spontaneous activity). We show that, due to the stochastic nature of the proximal and distal rhythmic input, controlled by the standard deviation (stdev) of the start times, and the stdev of the input bursts), when running multiple trials, the precise timing of the input bursts on each trial is jittered, and hence the alpha and beta activity in the spectrograms on each trial is jittered. This is akin to simulating induced rhythms rather than time-locked evoked rhythms. In the averaged spectrogram across trials, the alpha and beta events accumulate without cancellation (due to the fact that spectrogram value are purely positive) creating the impression of a continuous oscillation (Figure 1, left panel).

Below we illustrate the effects of “jitter” in the proximal and distal rhythmic inputs across trials in two ways. First, we examine the effects of “jitter” due to the “Burst stdev”, and second due to the “Start time stdev”).

To first test the effects of jittering due to “Burst stdev” and averaging across trials,  we will use a param file ([AlphaAndBetaJitter0.param](https://www.google.com/url?q=https://hnn.brown.edu/wp-content/uploads/2018/05/AlphaAndBeta.zip&sa=D&ust=1552525363626000)) with rhythmic proximal and distal inputs provided at 10 Hz, with proximal and distal inputs in phase. These are the same parameters as in the AlphaAndBeta.param file (Step 3.2 above), but now with 10 trials instead of 1.

To load the parameter set, navigate to the HNN GUI and click:
```
Set Parameters From File
```
Then select the file from HNN’s param subfolder or from your local machine. To view the new parameters, click:
```
set parameters > Run Parameters

set parameters > Rhythmic Proximal Inputs

set parameters > Rhythmic Distal Inputs
```
You should see the values displayed in the three dialog boxes below.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

<table style="border-collapse: collapse; border: none;">

<tr style="border: none;">
<h3>Figure 10</h3>
</tr>

<tr style="border: none;">

<td style="border: none; vertical-align:top" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image19.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image19.png" alt="image19" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image9.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image9.png" alt="image9" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image14.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image14.png" alt="image14" width=100%/>
</a>
</td>
</tr>
</table>
</div>

Notice that there are 10 trials in the Run Parameters (in previous tutorial simulation the number of trails was 1) and the Start time stdev (ms) is set to 0.0 for both proximal and distal inputs, while the Burst stdev (ms) is 20.



To run this simulation, click Start Simulation from the main GUI window. This simulation will take longer to run because it uses 10 trials. Once completed, you will see output similar to that shown below.


<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 11

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image17.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image17.png" alt="image17" width=90%/>
</a>
</div>

Notice that the input histograms for distal (green) and proximal (red) input accumulated across the 10 trials, now have higher values than before (up to ~20 compared to 5 in Step 3.2) and the burst inputs are slightly broader on each cycle, since these histograms represent the accumulated activity from 10 simulations, where the standard deviation in the Burst duration across trials is 20 ms. Approximately 10 Hz rhythmicity in the timing of the distal and proximal inputs can be clearly visualized (note also the symmetric profile of the histograms). However, on any individual trial, the coincidence of inputs leading to alpha or beta events displays some variability due to the stochastic parameter value (Burst stdev=20 ms). This is observed in the dipole waveforms shown for each trial (example shown below). The spectrogram shown in the GUI window is now created by calculating the spectrogram from each of the 10 trials separately, then averaging the 10 spectrograms. Importantly, this is not the spectrogram of the average of the dipole waveforms. The averaged spectrogram above shows more continuous bands of alpha and beta activity than for a single trial (compare to spectrogram in Step 3). Running more trials will increase the appearance of continuous rhythms.

Note that you can also view the spectrogram from an individual trial using HNN’s spectrogram viewer, which allows viewing simulation or experimental dipoles and resultant spectrograms.

To access the spectrogram viewer, navigate to the main GUI and click:
```
View > View Spectrograms
```

You will then see a window similar to the one shown below, allowing you to select dipoles and spectrograms from individual trials, or the average spectrogram, the same way the main HNN GUI provides. In the image below, we have selected trial #5 from the drop-down menu at the bottom.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 12

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image31.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image31.png" alt="image31" width=90%/>
</a>
</div>

In the next simulation, we will jitter the start times of rhythmic inputs across trials with the Start time stdev, in addition to a non-zero Burst stdev. This will add additional variability to the timing of the transient alpha and beta events on each trial, and hence produce even more continuous bands of activity in the averaged spectrogram.

To run this simulation, open the Rhythmic Proximal Inputsand Rhythmic Distal Inputs dialog boxes, and change the start time stdev from 0 ms to 50 ms in the timing tabs. The dialog boxes should now look as follows.



<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

<table style="border-collapse: collapse; border: none;">

<tr style="border: none;">
<h3>Figure 13</h3>
</tr>

<tr style="border: none;">

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image8.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image8.png" alt="image8" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image13.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image13.png" alt="image13" width=100%/>
</a>
</td>

</tr>
</table>
</div>

To run this simulation, click Start Simulation from the main GUI window. Once completed, you will see output similar to that shown below.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 14

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image27.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image27.png" alt="image27" width=90%/>
</a>
</div>

Notice that the input histograms for distal (green) and proximal (red) input accumulated across the 10 trials now show little rhythmicity due to the jitter in the rhythmic input start times across trials (Start time stdv (ms) = 50), in addition to jitter due to the Burst stdev (ms) = 20\. However, if we were to visualize histograms on each individual trial (using the View spectrograms tab), they would show the ~10 Hz and 20 Hz (alpha and beta) rhythmicity. It is also difficult to visualize rhythmicity in any of the overlaid dipole waveforms. However, on each trial, alpha and beta rhythmicity is present, and even more continuous bands of alpha and beta activity are observed (compare to averaged data in Figure 1 left panel; n=100 trials) when the spectrograms from individual trials are averaged. Running more trials will further increase the continuous nature of alpha and beta activity across time.

### 3.4 Viewing network spiking activity

Importantly, as stated above in this example the strength of the proximal and distal input were titrated to be subthreshold (cells do not spike) under the assumption that macroscale oscillations are generated primarily by subthreshold current flow across large populations of synchronous pyramidal neurons. We can verify the subthreshold nature of the inputs by viewing the spiking activity in the network.

Return to the single-trial parameter set as in Steps 3.1 and 3.2, by loading the AlphaAndBeta.param file. Then navigate to the main GUI window, and click:
```
View > View Simulation Spiking Activity
```

You should see the following window.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 15

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image21.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image21.png" alt="image21" width=90%/>
</a>
</div>

In this window, the rhythmic distal (green/top) and proximal (red/middle) inputs bursts histograms are shown along with the spiking activity in each population of cells (bottom panel). In this case, the alpha and beta events are indeed produced through subthreshold processes and there is no spiking produced in any cell in the network (no dots present in the bottom raster plot).

### 3.5 Exercises for further exploration

Try decreasing or increasing the number of trials in the above simulations to see how these changes impact the continuity of alpha/beta power over time. View some of the individual spectrograms to see that alpha/beta are maintained on individual trials.


<a id="toc_four"></a>

## 4. Calculating and Viewing Power Spectral Density (PSD)

HNN provides a feature to calculate and view the power spectral density (PSD) of the simulated signal and imported data (Note: the PSD is calculated as the time average of the spectrogram, in the simulation examples).

To calculate and view the PSD, navigate to the main GUI window and click:
View> View PSD 

You should see something similar to the following window.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 16

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image33.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image33.png" alt="image33" width=90%/>
</a>
</div>

The PSD Viewer window shows the net current dipole (bottom panel) and contribution from each layer in the network separately (top panels). This example was run using the parameter set described in Step 3\. PSD from the simulation shows a strong peak in the alpha (~10 Hz) band,  with a lower peak power in beta band (~20 Hz).

<a id="toc_five"></a>

## 5. Comparing model output and recorded data

We can also use HNN to compare model-generated to empirical PSD, which represents the averaged activity across time. As discussed above, HNN does not provide means to directly compare spontaneous time domain rhythmic waveforms to data, due to the fact that spontaneous rhythms are not time linked to specific events making it difficult to align recorded data and simulated results. However, a direct comparison can be made between time averaged recorded and simulated signals by comparing PSD plots. To do so, you will need time-series of MEG data in a format that HNN can read (more details on this are provided in the Viewing Datatutorial). The [S1_ongoing](https://www.google.com/url?q=http://hnn.brown.edu/wp-content/uploads/2018/03/S1.zip&sa=D&ust=1552525363636000) file will be used in the following example. This file contains raw data source localized to SI from the 1 second prestimulus period before a tactile stimulus, during the tactile detection experiment described in the “Getting Started” section above[1]. You will need to extract the contents of the .zip file to access the text file within. The data was collected at 600 Hz. (Note, when loading your own data, if it was not collected at 600 Hz, you must first downsample to 600 Hz to run a frequency analysis and view it in the HNN GUI.)

Once you have downloaded the example data, you can load it into HNN by first starting HNN’s PSD Viewer from main GUI window (View> View PSD). From HNN’s PSD Viewer, click:
```
File > Load Data File
```

Then select the data file from your local machine. HNN will calculate the PSD from the time-series data and overlay it on the simulation PSD for comparison.

Below is an example output using the S1_ongoing file provided above. The viewer will display the average PSD across trials and also the standard error (displayed as dotted lines).

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 17

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image15.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image15.png" alt="image 15" width=90%/>
</a>
</div>

In this example, there is strong similarity in the shape and amplitude of the PSD generated by the model (bottom white traces) and the PSD from experimental MEG data (bottom violet traces).

You can also load data directly into the “HNN Spectrogram Viewer”. To load a single trial example of spontaneous SI activity from data provided, first start the HNN Spectrogram Viewer by clicking View Spectrogramsfrom HNN’s View menu in then main GUI. Then, click:
```
File > Load Data File 
```

And load the file S1_ongoing.txt. Next, select Trial 32 (for example) from the drop-down menu at the bottom of the viewer. You will see the following display.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 18

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image10.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image10.png" alt="image10" width=90%/>
</a>
</div>

Notice that as shown in the “Getting Started” section above (Figure 1), this single trial example of spontaneous SI data exhibits non-continuous brief alpha and beta events. Since these data are spontaneous and non-time locked, it would be difficult to directly compare to simulated data. Instead, we compare qualitative features between data and simulation results, as in Figure 2 above. See also Step 6.4 below, where multiple trials of spontaneous SI data are simulated and averaged, producing more continuous bands of alpha and beta activity.

<a id="toc_six"></a>

## 6. Adjusting parameters

Parameter adjustments will be key to developing and testing hypotheses on the circuit origin of your own low-frequency rhythmic data. HNN is designed so that many of the parameters in the model can be adjusted from the GUI (see the Tour of the GUI tutorial).

Here, we’ll walk through examples of how to adjust several “Rhythmic Proximal/Distal Input” parameters to investigate how they impact the alpha and beta rhythms described above. We end with some suggested exercises for further exploration.

### 6.1 Changing the strength (post-synaptic conductance) and synchrony of the distal drive increases beta activity

We described above (Step 3) that the timing of proximal and distal inputs can lead to either alpha events (when the bursts arrive to the local network out of phase) or beta events (when the bursts arrive in phase).

We have also found that other factors that contribute to the prevalence of beta activity are the strengthand synchrony of the distal inputs; beta activity is increased with stronger and more synchronous subthreshold drive, where the beta frequency is set by the duration of the driving bursts (~50ms) (Jones et al. 2009; Sherman et al. 2016). The strength is controlled by the postsynaptic conductance, and the synchrony is controlled by the Burst stdev in the “Rhythmic Distal Inputs” dialog box. We will demonstrate this here.

To change the Rhythmic Distal Input parameters, first change the simulation name to, e.g.,AlphaAndMoreBeta in the main Set Parameters window. This will save the simulation data to a file with this new name. If you don’t still have the “Rhythmic Distal Inputs” dialog window open, click:
```
Set Parameters > Rhythmic Distal Inputs
```


Under the timing tab, reduce the Burst stdev (Hz) value from 20 ms to 10 ms.  This will create higher synchrony in the timing of the distal input bursts. Under both the Layer 2/3 and Layer 5 tabs, increase the postsynaptic condances weights of the AMPA synapses onto the Layer 2/3 and Layer 5 pyramidal neurons from 5.4e-5 ![](https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image2.png)to 6e-5 ![](https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image2.png). Both of these changes will cause the distal input burst to push a greater amount of current flow down the pyramidal neuron dendrites. The “Rhythmic Distal Input” dialog windows should look as shown below.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

<table style="border-collapse: collapse; border: none;">

<tr style="border: none;">
<h3>Figure 19</h3>
</tr>

<tr style="border: none;">

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image26.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image26.png" alt="image26" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image16.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image16.png" alt="image 16" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image30.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image30.png" alt="image30" width=100%/>
</a>
</td>

</tr>
</table>
</div>

Next, we will test how these parameter changes affect the simulation. Click on Start Simulation from the main GUI window. This simulation runs for ~700 ms of simulation time. Once completed, you will see output in the GUI similar to that shown below.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 20

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image34.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image34.png" alt="image34" width=90%/>
</a>
</div>

First, notice that the histogram profile of the distal input bursts (green) are narrower corresponding to more synchronous input than in the prior simulation (Step 3). Second, notice that the waveform of the oscillation is different with a sharper downward deflecting signal, due to to the stronger distal input. These deflections increased ~20 Hz beta activity, as seen in the corresponding spectrogram (compare to spectrogram in Step 3). The 20 Hz frequency is set by the duration of the downward current flow, which with this parameter set is approximately 50 ms (see Sherman et al. 2016[3] for further details).

We can verify the increase in beta activity more directly by viewing the simulation’s power spectral density (PSD). To do so, navigate to the main GUI window and click:
```
View > View PSD
```

You should see the following window:

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 21

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image36.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image36.png" alt="image36" width=90%/>
</a>
</div>

Notice that the power spectral density from the simulation shows a larger beta than alpha amplitude in net current dipole (bottom panel), with a stronger contribution from Layer 5 (middle panel).

### 6.1.1 Exercise for further exploration

Try changing the frequency of the rhythmic distal drive from 10 Hz to 20 Hz. Try other frequencies for the proximal and distal rhythmic drive. How do the rhythms change? See how changes in the Burst stdev effects the rhythms expressed.

### 6.2 Increasing the strength (post-synaptic conductance) of the distal drive further creates high frequency responses due to induced spiking activity

Recall that in the above simulations, the strength of the rhythmic proximal and distal inputs were chosen so that the cells remained subthreshold (no spiking). We will now demonstrate what happens if we increase the strength of the inputs far enough to induce spikes. Instead of simulating subthreshold alpha/beta events, we will see that the dipole signals are dominated by higher-frequency events created by spiking activity. We note that the produced waveforms of activity are, to our knowledge, not typically observed in MEG or EEG data, supporting the notion that alpha/beta rhythms are created through subthreshold processes.

To test this, change the parameters in the “Rhythmic Distal Inputs” dialog box as follows. First, change the simulation name to, e.g., AlphaAndBetaSpike in the main Set Parameterswindow. This will save the simulation data to a file with this new name. If you don’t still have the “Rhythmic Distal Inputs” dialog window open, click set parameters > Rhythmic Distal Inputs. Under the timing tab, change the Burst stdev value back to 20 ms. Under both the Layer 2/3 and Layer 5 tabs, increase the postsynaptic conductance weights of the AMPA synapses onto the Layer 2/3 and Layer 5 pyramidal neurons from 6e-5 ![](https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image2.png)to 40e-5 ![](https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image2.png) (a big change that will provide enough current to cause the cells to spike). The “Rhythmic Distal Input” dialog windows should look as shown below.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

<table style="border-collapse: collapse; border: none;">

<tr style="border: none;">
<h3>Figure 22</h3>
</tr>

<tr style="border: none;">

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image6.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image6.png" alt="image6" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image12.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image12.png" alt="image12" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image20.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image20.png" alt="image20" width=100%/>
</a>
</td>

</tr>
</table>
</div>

Next, run the simulation. Click on Start Simulation from the main GUI window. Once completed, you will see output similar to that shown below.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 23

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image37.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image37.png" alt="image37" width=90%/>
</a>
</div>

Notice that the histogram profile of the distal input bursts (green) are once again wider corresponding to less synchronous input and comparable to those shown in the example in Step 3\. However, in this case the postsynaptic conductance of these driving spike is significantly larger (40e-5 ![](https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image2.png)). This strong input induces spiking activity in the pyramidal neuron on several cycles of the drive (2.5 shown here) resulting in a sharp and rapidly oscillating dipole waveform. The corresponding dipole spectrogram shows broadband spiking from ~60-120 Hz. This type of activity is not typically seen in EEG or MEG data, and hence unlikely to underlie macroscale recordings.

We can verify the increase in high-frequency activity more directly by viewing the simulation’s power spectral density (PSD). To do so, click on the View> View PSD from the main GUI window. You should see the following window.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 24

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image23.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image23.png" alt="image23" width=90%/>
</a>
</div>

The PSD from the simulation shows broadband 60-120Hz high frequency activity caused by neuronal spiking.



We can verify that the neurons are spiking by looking at the spiking raster plots. To do so, click View > View Simulation Spiking Activity from the main GUI window. You should see something similar to the following.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 25

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image38.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image38.png" alt="image38" width=90%/>
</a>
</div>

Notice that highly synchronous neuronal spiking in each population coincides with the high-frequency events seen in the waveform and spectrogram. The waveform response is induced by the pyramidal neuron spiking which creates rapid back-propagating action potentials and repolarization of the dendrites.

Hypothesis testing:This simulation demonstrates that HNN can be used to test the limits of physiological variables and to see how, as parameters are varied, simulations results can be similar or dissimilar to experimental data.



### 6.2.1 Exercise for further exploration

View the contribution of Layer 2/3 and Layer 5 to the net current dipole waveform and compare with the spiking activity in each population. How do each contribute? Try also to change the proximal input parameters instead of the distal input parameters.

Adjust one of the parameter regulating the local network connections. What happens?



### 6.3 Increasing the delay between the proximal and distal inputs to anti-phase (50 ms delay) creates continuous alpha oscillations without beta activity


We mentioned above that, in addition to parameters controlling the strength and synchrony of the distal (or proximal) drive, the relative timing of proximal and distal inputs is an important factor in determining relative alpha and beta expression in the model. Here we will demonstrate that out-of-phase, 10 Hz burst inputs can produce continuous alpha activity without any beta events. For this simulation, load the [AlphaAndBeta.param](https://www.google.com/url?q=https://hnn.brown.edu/wp-content/uploads/2018/05/AlphaAndBeta.zip&sa=D&ust=1552525363653000) parameter file as described in Step 3 by clicking Set Parameters From File and selecting the file from HNN’s param subfolder. To view the new parameters, click on Set Parameters, and then Rhythmic Proximal Inputs and Rhythmic Distal Inputs. Next, in the timing tab for the Rhythmic Distal Inputs, change the start time mean from 50 to 100 ms. The timing tabs in the Rhythmic Proximal and Distal Input dialog boxes should look as follows:  

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

<table style="border-collapse: collapse; border: none;">

<tr style="border: none;">
<h3>Figure 26</h3>
</tr>

<tr style="border: none;">

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image9.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image9.png" alt="image9" width=100%/>
</a>
</td>

<td style="border: none;" width="30%">
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image35.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image35.png" alt="image35" width=100%/>
</a>
</td>

</tr>
</table>
</div>

Note that both the proximal and distal input frequency are set to 10 Hz (bursts of activity every ~100 ms). Since the proximal input Start time mean is 50.0 ms and the the distal input Start time mean is 100.0 ms, the input will, on average, arrive to the network a 1/2 cycle out of phase (i.e., in antiphase, every 50 ms).

Next, we will run the simulation to investigate the impact of this parameter change. Click on Start Simulation from the main GUI window. Once completed, you will see output similar to that shown below.

<div style="background-color:rgba(0, 0, 0, 0); text-align:center; vertical-align: middle; margin-top:20px; margin-bottom:40px">

### Figure 27

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image24.png"><img src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/alpha_and_beta/images/image24.png" alt="image24" width=90%/>
</a>
</div>

Notice that the histogram profile of the proximal (red) and distal (green) input bursts are generally ½ cycle out-of-phase (antiphase). This rhythmic alteration of proximal followed by distal drive induces alternating subthreshold current flow up and down the pyramidal neuron dendrites to create a continuous alpha oscillation in the current dipole waveform that oscillates around 0 nAm. The period of the oscillation is set by the duration of each burst (~50 ms, controlled in part by Burst stdev) and the 50 ms delay between the inputs on each cycle (due to different start times). The corresponding spectrogram shows continuous nearly pure alpha activity. This type of strong alpha activity is similar to what might be observed over occipital cortex during eyes closed conditions.



### 6.3.1 Exercise for further exploration

Try changing the delay between the proximal and distal drive by varying amounts. What happens to the rhythm expressed?

Can you create a simulation where other frequencies are expressed? How is it created? Are the cells spiking or subthreshold?


<a id="toc_seven"></a>

## 7. Have fun exploring your own data!

Follow steps 1-6 above using your data and parameter adjustments based on your own hypotheses.  



## References

1. [Jones, S. R.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/NxR0&sa=D&ust=1552525363656000) [et al.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/NxR0&sa=D&ust=1552525363656000)[Quantitative analysis and biophysically realistic neural modeling of the MEG mu rhythm: rhythmogenesis and modulation of sensory-evoked responses.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/NxR0&sa=D&ust=1552525363656000) [J. Neurophysiol.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/NxR0&sa=D&ust=1552525363657000)[ ](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/NxR0&sa=D&ust=1552525363657000)[102,](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/NxR0&sa=D&ust=1552525363657000)[ 3554–3572 (2009).](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/NxR0&sa=D&ust=1552525363657000)

2. [Ziegler, D. A.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/WrO3&sa=D&ust=1552525363657000) [et al.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/WrO3&sa=D&ust=1552525363658000)[Transformations in oscillatory activity and evoked responses in primary somatosensory cortex in middle age: a combined computational neural modeling and MEG study.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/WrO3&sa=D&ust=1552525363658000) [Neuroimage](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/WrO3&sa=D&ust=1552525363658000)[ ](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/WrO3&sa=D&ust=1552525363658000)[52,](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/WrO3&sa=D&ust=1552525363658000)[ 897–912 (2010).](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/WrO3&sa=D&ust=1552525363659000)

3. [Sherman, M. A.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/ciWL&sa=D&ust=1552525363659000) [et al.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/ciWL&sa=D&ust=1552525363659000)[Neural mechanisms of transient neocortical beta rhythms: Converging evidence from humans, computational modeling, monkeys, and mice.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/ciWL&sa=D&ust=1552525363659000) [Proc. Natl. Acad. Sci. U. S. A.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/ciWL&sa=D&ust=1552525363659000)[ ](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/ciWL&sa=D&ust=1552525363660000)[113,](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/ciWL&sa=D&ust=1552525363660000)[ E4885–94 (2016).](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/ciWL&sa=D&ust=1552525363660000)

4. [Jones, S. R. When brain rhythms aren’t ‘rhythmic’: implication for their mechanisms and meaning.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/4jg3&sa=D&ust=1552525363660000) [Curr. Opin. Neurobiol.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/4jg3&sa=D&ust=1552525363660000)[ ](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/4jg3&sa=D&ust=1552525363661000)[40,](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/4jg3&sa=D&ust=1552525363661000)[ 72–80 (2016).](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/4jg3&sa=D&ust=1552525363661000)

5. [Jones, E. G. The thalamic matrix and thalamocortical synchrony.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/CgPQ&sa=D&ust=1552525363661000) [Trends Neurosci.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/CgPQ&sa=D&ust=1552525363662000)[ ](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/CgPQ&sa=D&ust=1552525363662000)[24,](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/CgPQ&sa=D&ust=1552525363662000)[ 595–601 (2001).](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/CgPQ&sa=D&ust=1552525363662000)

6. [Hughes, S. W. & Crunelli, V. Thalamic mechanisms of EEG alpha rhythms and their pathological implications.](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/oQZB&sa=D&ust=1552525363663000) [Neuroscientist](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/oQZB&sa=D&ust=1552525363663000)[ ](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/oQZB&sa=D&ust=1552525363663000)[11,](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/oQZB&sa=D&ust=1552525363663000)[ 357–372 (2005).](https://www.google.com/url?q=http://paperpile.com/b/XBRvEX/oQZB&sa=D&ust=1552525363663000)
