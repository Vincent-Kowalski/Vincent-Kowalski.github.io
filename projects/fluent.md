---
layout: page
title: fluent
permalink: /projects/fluent/
---



# Heat transfer correlations in a trapezoidal-shaped channel (Ansys Fluent & OpenFOAM)

## Context
During my last semester of study I attended this Numerical Fluid mechanics and thermics module. It was a heavy one concentrating on the actual ways to simulate
flows and heat exchanges. We alternatively focused on both theoretical and practical aspects of it. One of the projects was intended to make us dive 
further into the Ansys software and more precisely on its Fluent extension. A list of subjects has been proposed to us and we were able to pick anyone 
we wanted. <br/>
The particularity of our project is that we decided to run simulations on both Fluent and OpenFOAM to validate the results twice.

## Project overview
Trapezoidal pipes are often used in HVAC facilities, notably to use them as vent ducts. Heat transfers happening within them has to be studied, especially
forced convection. <br/>
First we gonna find the establishment length of the trapezoidal configuration, i.e. the length needed for the pipe to go from a zero-gradient uniform velocity
profile to a fully turbulent state.<br/>
Secondly, we gonna find the correlation between the 3 determining undimensionned numbers in this configuration: $Re$, $Pr$ and $Nu$ . In their numerical study
and in the same configuration, Rokni and Sunden found that $$Nu=0.03Re^{0.8}Pr^{0.3}$$ featuring the following numbers: <br/>

| Number | Name | Formula | Representation |
|:--------|:---------:|:---------:|:---------:|
| Re | Reynolds number | $\frac{\rho.U.D}{\mu}$ | the ratio of inertial to viscosity forces|
| Nu | Nusselt number | $\frac{h.D}{\lambda}$ | the ratio of convection to conduction|
| Pr | Prandtl number | $\frac{C_p \mu}{\lambda}$ | the ratio of momentum diffusivity to thermal diffusivity| <br/>|

Among them only $Pr$ is constant in our study since it only takes thermophysical properties of the fluid as parameters. <br/>
<br/>
To find this correlation again, we set up and automated a simulations campaign featuring different $Nu$ and $Re$.
We then perform a logarithmic regression to find the coefficients of the study.<br/>
<br/>
To sum up, our numerical investigation as two main goals:
- Finding the establishment length
- Find the Rokni and Sunden's correlation back

## Hypothesis
We took water as the working fluid in our pipe. We assume that its density $\rho$ is constant, putting us an the incompressible case. Thermal conductivity $\lambda$
and viscosity $\mu$ are considered constant as well. <br/>
We can reasonably assume that the flow shows a study state after the fully turbulent regime has been reached. <br/>
Moreover we neglected the radiative heat trasnfer since temperatures are relatively low and natural convection is weak
enough to be neglected.

## Turbulence model definition
In a RANS simulation, turbulence isn't resolved at any scale: it is modeled. This means we are looking at the averaged Navier Stokes equations, closing the problem using
another set of 2 equations and a priori knowledge about turbulence. The 2 conservation equations can be applied to k (turbulent kinetic energy) and $\epsilon$ (the turbulent
dissipation) or to k and $\omega$ (specific dissipation). In a nutshell the $k-\omega-SST$ turbulence model combines the strengths of the above-mentionned 
two-equations models and is widely use. <br/>
<br/>
More information on the $k-\omega-SST$ model can be found [here](https://www.cfd-online.com/Wiki/SST_k-omega_model) <br/>.

## Numerical mesh

A structured mesh has been chosen for this relatively simple geometry. A ... attention has been ... for the near-eadges zones. There, both velocity and temperature gradients
are higher and the zones habe to be resolved finner.




