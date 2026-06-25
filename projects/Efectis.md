---
layout: page
title: Efectis
permalink: /projects/Efectis/
---

# Dev and validation of an AI tool to predict temperature evolution within a storage warehouse in a fire scenario

## Context
For my final-year project I chose to reinforce my CFD and data analysis skills. To do so, I collaborated with the company **Efectis France** which is in charge of carrying out fire and thermal sollicitations studies.<br/> 
Fire engineers have noticed that some of the studies had similar results or at least, results showing a similar time-evolution curve. Indeed temperature profiles of storage warehouses are typically the kind of data one can consistently predict with Machine Learning (ML). It could save a lot of time asking a verified AI tool to predict the temperature evolution of those simple cases, saving work forces for the more specific ones. &nbsp; &nbsp; &nbsp;


![Parent Directory Image](/Images/ICPE_incendie.png)

## Project overview

SCHEMA Pipeline du projet

The overall goal is to predict the evolution of temperature in both time and space in any rectengular-shaped warehouse containning any number of storage racks.<br/>
In order to train a predictive AI tool, one should first restrain the number of input parameters. To do so, I carried out numerous sensitivity study cases.<br/>
Once that done, I could jump into the simulations pre- and postprocessing. Since there were 107 of them, I had to automate the whole simulation process.<br/>
Finally the concatenated output data base was used to train different AI algorithmes and compare them based on their performances.

## Study cases

Before launching the simulations and for the sake of accuracy/performance compromise, one has to quantify the influence of certain parameters on output data. This way, we can keep only the most important onse in our model since adding some input parameters drastically increases the amount of required training data. <br/>
The parameters that present a high influence on the temperature (output data) must be treated :
- either by taking them as input parameters
- or by keeping them constant in the model, which would represent a limit of the predictive tool

With respect to an "initial" case, we tested both geometrical and numerical parameters. <br/> 
The results were compared according to the calculations of the international ISO 16730-1 norm relative to the verification and validation of numerical models in fire safety. <br/>

According to the results I kept the following input parameters associated with their variation ranges :

| Variable | Range of variations | Step |
|:--------|:---------:|:---------:|
| Surface area (m²) | [1000-1300] | 1500 |
| Length/width ratio (-) | [1-2.1] | 0.1 |
| Height (m) | [9-15] | 2 |
| Cinetic | [fast ; super-fast] | - |
| HRRPUA (kW/m²) | [250 ; 500] | - |

If you're not familiar with the quantitative and qualitative parameters used in fire modelling, here is a quick recap of their meaning : <br/>
- Cinetic : 
- Heat Realease Rate Per Unit Area :


## Preprocessing automation of the simulations
FDS is a CFD software dedicated to fire scenario simulation. It takes all the simulation parameters from a text file (.fds extension) known as the simulation file.
Given the variation range of the input parameters, we intended a total of 107 simulations for a reasonable budget. Therefore it was obviously not fiseable to modify "by hand" an initial case to adapt to each case. <br/>
 
 So I developped a comprehensive ready-to-use VBA Macro that produces the fds simulation script of any rectangular storage warehouse.
 The user was able to customize the following parameters or keeping the by default values for the sake of simplicity :
 
 ## Postprocessing of the simulations
 
 As fire engineers, we rarely use the output simulation data as they show up. Combustion is a very wide theme in which a very wide range of 
 physical phenomenon occur (smoke dynamics, heat conduction and convection, radiation, chemical processes ...). 
 To deal with that at a reasonable numerical cost, some implicit assumptions about fire propagation, material behaviors etc. are made in the model itself. <br/>
 <br/>
 In this context, postprocessing appears to be an opportunity for the modeler to deal with the idealization of the fire process.<br/>
To record the evolution of a variable (temperature, extinction coefficient ...) over time at one specific location in the simulation in FDS, 
we use the so-called devices. One can think of it as a numerical sensor that can be put anywhere in the simulation domain by giving its 3 spatial coordinates.
We then sprinkle such devices through the domain to have a spatial distribution of the studied variable.
Thanks to an Excel export, we then have access to all variable records in the entire warehouse. 
<br/>
IMAGE DE COURBE <br/>
<br/>
From this and according to the previously-developped paragraph, I applied 3 typically used in fire engineering data transformations :
- Temperature limitation up to 900°C : this is systematically performed by fire engineers and comes from the fact that 900°C is way enough to make classicly used construction material collapse.
- Gaussian filter : curves usually show a very fractured graph and those high frequences oscillations would make the predictive model far less accurate. Gaussian filter acts as a low-pass filter to get rid of those oscillations
- Curves extrapolation : certain sensor never get up to 900°C. Since we want to simulate the worst case fire scenario, we assume that everything in the

Those 3 processes had to be automated since it should apply to all the sensors of each simulation. With an approximate average of 40 sensors/simulatiuon,  this represented approximately $107 \times 40 = 4280$ curves.



## MPI optimization
Launching 107 LES simulations (SIZE OF SIMULATION DOMAINS) taking into account all sorts of heat tranfers is not feasible at the local scale in a reasonable amount of time. We had to deport the calculations on a University of Toulouse's cluster, using the Olympe supercalculator. <br/>
The classical compromise in MPI optimization is to choose the right amout of cores in which running the simulation case in parallel. In theory, running the simulation in 2, 3, 4 ... cores should divide the running time by 2, 3, 4 etc... That's not how it works since information has to be shared among the cores. The more cores we use, the more information has to be transfered and the efficiency per core decreases.
<br/>
[[ GRPAHIQUE DE LA PERTE D EFFICACITE PAR COEUR UTILISES ]]
<br/>
Although the efficiency goes down, we still run the calculations in parallel i.e. on multiple cores to shorten the run time. So I wrote a VBA-script that automatically divides the simulation domain in rectangular pieces. Those had to be as equal in size and close to a square as possible. <br/>
[IMAGE D UN DE MES DECOUPAGE]
<br/>




## AI training, optimization and verification
Once the postprocessing was done and after concatenating the results into a data base, I could start traning predictive models with my data. <br/>
Since the variable we're trying to predict is continuous (temperature), we're on a **supervised learning regression** problematic. The actual models (at least back in 2025) do so by meanns of a loss function by iteratively update their parameters. Those are going to change all the way through the learning unlike the so-called **hyperparameters** that one can see as the model "meta-" parameters. <br/>
For the sake of completeness, let us mention that according to a case study, the predictive models are way more accurate at predicting the time of reach of certain temperature thresholds. <br/>
Therefore the model output are the **TEMP** variables described with the scheme below : <br/>
[IMAGE DES TEMP] <br/> 
<br/>

















