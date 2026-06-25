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
|:--------|---------:|---------:|
| Surface area (m²) | [1000-1300] | 1500 |
| Length/width ratio (-) | [1-2.1] | 0.1 |
| Height (m) | [9-15] | 2 |
| Cinetic | [fast ; super-fast] | - |
| HRRPUA (kW/m²) | [250 ; 500] | - |

If you're not familiar with the quantitative and qualitative parameters used in fire modelling, here is a quick recap of their meaning : <br/>
- Cinetic : 
- Heat Realease Rate Per Unit Area :


## Pre- postprocessing automation of the simulations
FDS is a CFD software dedicated to fire scenario simulation. It takes all the simulation parameters from a text file (.fds extension) known as the simulation file.
Given the variation range of the input parameters, we intended a total of 107 simulations for a reasonable budget. Therefore it was obviously not fiseable to modify "by hand" an initial case to adapt to each case. <br/>
 

## MPI optimization


## AI training, optimization and verification



