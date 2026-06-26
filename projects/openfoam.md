---
layout: page
title: openfoam
permalink: /projects/openfoam/
---

# Parameter study of the incidence angle on a NACA wing profile (OpenFOAM)

## Context
During my Erasmus study trip at the Karlsruher Insitut für Technology I attended a very qualitative course given by Dr.-Ing. Stroh on OpenFOAM CFD. This was my first ever contact with CFD and it went very smoothly since Mr X was not only a very good technician but also a remarquable teacher.\
Although the personnal work load necessary to attend the test was very high compared to other modules, I don't regret chosing this course and discovering a wide range of CFD applications by myself. The homework consisted indeed of parametrization, lauching and postprocessing of 12 cases throughout the semester. The whole process had then to be automated and delivered as a runnable Linux Script.\

## Project overview

One of the "mini-project" was the parameter study of incidence angle on a wing profile. The angle varies from 0 to 20 ° with a step of 1 °.
\
![Parent Directory Image](/Images/plane_scheme.png)
\
The objective was to determine numericaly the aerodynamic coefficients (lift and drag) based on the recorded pressure drop as a function of $\alpha$. We then end up
with the polar curve representing the variation of $C_l$ with resepct to $C_d$. The incidence angle varies implicitly along the grpah. &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
\
![Parent Directory Image](/Images/Cl_Cd.png)
\
The flow is incompressible since 

## Theoretical background
The aerodynamical forces acting on the wing profile are the lift force (directed perpendicular to the incident aorflow) and the drag force (directed in the incident airflow direction).
Note that the paradigm of inclining the wing profile of $\alpha$ radiant is exactly the same situation as keeping the wing horizontal and inclining the incident airflow.
Lift and drag forces can be expressed as : <br/>
$$F_l = \frac{\rho}{2} u_{\inf}^2.S_{ref}.C_l$$ <br/>
&nbsp; &nbsp; &nbsp;
$$F_d = \frac{\rho}{2} u_{\inf}^2.S_{ref}.C_d$$

As one can see, their are very similar. Only the aerodynamical coefficients differentiate them. This is why the knowledge of the above menionned polar curve
is a prerequisite for determining the performances of the aircraft.
+ calcul des coefficients

## Quick overview of NACA profiles

## Solver choice
In OpenFOAM, several solvers are available. It is an algorithm that solve the coupling between pressure and velocity in the Navier-Stokes system equations.
The choice of the solver is determining: one won't end up with the same results with different solvers. Its choice always depends on the configuration. <br/>
An important feature of our configuration is that, eventually, the fluid is going to reach a fully turbulent stationnary state with air flowing smoothly around the profile.
In other words the variation in the velocity and pressure profiles from one time step to another is going to be neglectable at some point. <br/>
In these situations one typcally uses a steady-state solver. The main advantage is that the modeller doesn't have to tell the software when to stop ("How much time steps should
the simulation last?") but when field variations are assumed to be neglictable ("Is a $10^{-4}$, $10^{-6}$ ... relative variation the sign that steady state is reached?"). <br/>
One can also spot a non-converging simulation more effectively instead of desperatly trying to run it with a higher number of time steps. <br/> 
In an incompressible case like this one, we can use the well-known simpleFOAM solver.

## Fluid characteristics determination
In CFD one typically uses undimensioned numbers to describe fluid characteristics. It allows scientists all around the world to test the configuration by taking
the same undimensioned numbers, regardless of the experiment or simulation scale. <br/>
This is why we only describe the fluid by the Reynolds number of the simulation. <br/>
$$Re_c = \frac{\rho.U_{inf}.c}{\nu}$$
with :
- \rho : the volumetric mass of the fluid (kg/$m^3$)
- \mu : the dynamic viscosity of of the fluid (kg/m/s)
- c : the chord of the wing profile (m)
- U_{inf} : the velocity of the incident flow (m/s)
Although we know the values $\rho_{air}$, $\mu_{air}, etc. it doesn't matter since others can reproduce my simulation with the same Reynolds number. <br/>
The chosen $Re$ has to be coherent with the physical parameters of air and high enough to allow a fully turbulent flow. $Re = 10^6$ meets those conditions.

## Turbulence model definition
In a RANS simulation, turbulence isn't resolved at any scale: it is modeled. This means we are looking at the averaged Navier Stokes equations, closing the problem using
another set of 2 equations and a priori knowledge about turbulence. The 2 conservation equations can be applied to k (turbulent kinetic energy) and $\epsilon$ (the turbulent
dissipation) or to k and $\omega$ (specific dissipation). In a nutshell the $k-\omega-SST$ turbulence model combines the strengths of the above-mentionned 
two-equations models and is widely use. <br/>
More information on the $k-\omega-SST$ model can be found [here](https://www.cfd-online.com/Wiki/SST_k-omega_model) <br/>.

## Initial and boundary conditions

## Postprocessing the simulation

## Parameter study automation
I detailled above the preprocessing and postprocessing for 1 specific simulation. Now, we need to automatically adapt this process to the 19 other simulations.
The good news is that nothing really changes in the pre- and postprocessing processes except for $\alpha$.

	# Preprocessing automation
	
	# Processing automation
	
	# Postprocessing automation
	
	
## Results







