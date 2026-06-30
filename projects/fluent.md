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

A structured mesh has been chosen for this relatively simple geometry. Special attention was paid to the near-wall regions where both velocity and temperature gradients
are high. If the first cell of the mesh is too large, complex phenomena won't be resolved leading to inacurate predictions. However a too thin first cell would lead 
to very expensive numerical ressources. Again, the whole point is to make a compromise between accuracy and performance. <br/>
Knowing the flow parameters and the desired refinement one can compute the length of the first cell using the friction velocity and the shear stress at the wall. The 
calculations won't be detailled here but the interested reader can throw an eye to this [very well done calculator](https://github.com/monikamadasamy/CFD-yplus-Near-Wall-Mesh-Calculator) 
which explains near-wall turbulence basics. <br/>
<br/>
Afterwards we took $y+=30$ and then a first cell length of $4.5e-5 m$. We then used the so-called Wall functions in both Fluent and OpenFOAM to inflate the mesh properly at the 
edges of the domain.

## Boundary conditions
Boundary conditions are mathematical conditions that the velocity, pressure and temperature fields have to meet at the edges of the domain for any time step. They originate
from physical arguments that can be turned into an equation valid anywhen. <br/>
For exemple, considering an internal flow like this one, the velocity has to be equal to zero at the wall. It is called the no-slip condition and it is a sepcial
form of Dirichlet condition. We also use them for temperature as we set a fixed value for the temperatures at top and bottom of the pipe. <br/>
At the in- and outlet, we want a straight velocity profile flowing into the pipe. To ensure that, we set the velocity gradient at zeroat the beginning and end of it : 
$\frac{\partial U_x}{\partial x} =0$. This is a type of Neumann condition. We also use them for pressure. <br/>
<br>
These are pretty basic but considering our simple geometry, a more fancy one is going to significantly reduce the numerical cost of the simultion.

	# A numerical trick
	The pipe admits a vertial axis of symmetry. Because of that, we can simply simulate half of our domain and set a ** symmetry boundary condition ** at the middle 
	of the pipe. This way, the software knows that velocity, pressure and temperature profiles have to be mirrored along that surface. <br/>
	Ultimatly, it only sets the fluxes across the symmetry surface and the normal components of the variables to zero. The domain can basically be halved.
	
	https://www.simscale.com/docs/simulation-setup/boundary-conditions/symmetry/


## Results
With both softwares we obtained usable results to be compared with theoretical values. <br/>
First, we were seeking the establishment length of the flow. Indeed the time-averaged velocity eventually converges to a well-defined velocity profile. 
The flow is then called ** fully-developped **. <br/>






