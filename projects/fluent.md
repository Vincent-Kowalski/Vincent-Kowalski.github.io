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
forced convection.
In their numerical study, Rokni and Sunden found that the correlation between the 3 determining undimensionned numbers in this configuration:
$$Nu=0.03Re^{0.8}Pr^{0.3}$$ <br/>
futuring the following numbers: <br/>

| Number | Name | Formula | Representation |
|:--------|:---------:|:---------:|:---------:|
| Re | Reynolds number | $\frac{\rho.U.D}{\mu}$ | the ratio of inertial to viscosity forces|
| Nu | Nusselt number | $\frac{h.D}{\lambda}$ | the ratio of convection to conduction|
| Pr | Prandtl number | $\frac{C_p \mu}{\lambda} | the ratio of momentum diffusivity to thermal diffusivity| <br/>
Among them only $Pr$ is constant in our study since it only takes thermophysical properties of the fluids as parameters. <br/>
<br/>
To find this correlation again, we set up and automated a simulations campaign featuring different $Nu$ and $Re$.
We then perform a logarithmic regression to find the coefficients of the study.<br/>
<br/>
To sum up, our numerical investigation as two main goals:
- Finding the establishment length
- Find the Rokni and Sunden's correlation back




