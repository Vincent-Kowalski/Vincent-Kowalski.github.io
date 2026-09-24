---
layout: page
title: openfoam
permalink: /projects/openfoam/
---

# Parameter study of the incidence angle on a NACA wing profile (OpenFOAM)

## Context
During my Erasmus study trip at the Karlsruher Insitut für Technology in Germany I attended a very qualitative course given by Dr.-Ing. Stroh on OpenFOAM CFD. This was my first ever contact with CFD and it went very smoothly since Dr Stroh was not only a very good technician but also a remarquable teacher.\
Although the personnal work load necessary to attend the test was very high compared to other modules, I don't regret chosing this course and discovering a wide range of CFD applications by myself. The homework consisted in parametrization, lauching and postprocessing of 12 cases throughout the semester. The whole process had then to be automated and delivered as a runnable Linux Script. <br/>

## Project overview

One of the "mini-project" was the parameter study of incidence angle $\alpha$ on a wing profile. <br/>
<br/>
![Parent Directory Image](/Images/plane_scheme.png)
<br/>
<br/>
For the purpose of flying an airplane, a wing profile developps both a vertical and a horizontal force component, respectively called lift and drag forces. The intricated phenomena that give rise to these forces are compliated but they can be determined by the flow characteristics.
The objective was to determine numerically the aerodynamic coefficients (lift and drag) based on the recorded pressure drop as a function of $\alpha$. We then end up with the polar curve representing the variation of $C_l$ with resepct to $C_d$. The incidence angle varies implicitly along the graph. <br/>
<br/>
![Parent Directory Image](/Images/Cl_Cd.png) <br/>
<br/>
The flow is considered to be incompressible since we are interested in the regions of the flow that are relatively close to the wing edge.

## Theoretical background
The aerodynamical forces acting on the wing profile are the lift force (directed perpendicular to the incident aorflow) and the drag force (directed in the incident airflow direction).
Note that the paradigm of inclining the wing profile of $\alpha$ radiant is exactly the same situation as keeping the wing horizontal and inclining the incident airflow.
Lift and drag forces can be expressed as : <br/>
<br/>
$$F_l = \frac{\rho}{2} u_{\inf}^2.S_{ref}.C_l$$ <br/>
<br/>
$$F_d = \frac{\rho}{2} u_{\inf}^2.S_{ref}.C_d$$ <br/>
<br/>
with 
- $C_L$ the lift coefficient (-)
- $C_d$ the drag coefficient (-)
- $\rho$ the density of the fluid (kg/$m^3$)
- $U_{inf}$ the speed of the incoming fluid (m/s)
- $S_{ref}$ the surface area of the wing (m²)
<br/>
As one can see, their are very similar. Only the aerodynamical coefficients differentiate them. This is why the knowledge of the above menionned polar curve
is a prerequisite for determining the performances of the aircraft.
+ calcul des coefficients

## Quick overview of NACA profiles
In this study, we observe the performances of the 0010 NACA airfoil. <br/>
The NACA (National Advisory Committee for Aeronautics) wing profiles have been developped by the NASA since the late 1920s'. With a range of different series reaching a very wide range of performances, spacecraft engineers have thrown the basics of industrial and scalable aerodynamics. <br/>
<br/> The digits of a NACA airfoil correspond to the developped series and more importantly to their features. In the four-digits series, the first 2 give the curvature of the profile and its location on the chord. A 4-digits code of the type 00xx then means that the profile has no curvature ans therefore is symmetrical. One can show that giving a curvature to a profile strictly betters its aerodynamic performances. The last 2 digits describe the maximal thickness of the profile, based on the pourcentage of the chord. <br/>
<br/> NACA series is a fascinating research theme and represent one of the most important milestone in aerodynamics. More information about it can be found [here](https://nasa.fandom.com/wiki/NACA_airfoil#1-series)

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
- $\rho$ : the volumetric mass of the fluid (kg/$m^3$)
- $\mu$ : the dynamic viscosity of of the fluid (kg/m/s)
- c : the chord of the wing profile (m)
- $U_{inf}$ : the velocity of the incident flow (m/s) <br/>
<br/>
Although we know the values $\rho_{air}$, $\mu_{air}, etc. it doesn't matter since others can reproduce my simulation with the same Reynolds number. <br/>
The chosen $Re$ has to be coherent with the physical parameters of air and high enough to allow a fully turbulent flow. $Re = 10^6$ meets those conditions.

## Turbulence model definition
In a RANS simulation, turbulence isn't resolved at any scale: it is modeled. This means we are looking at the averaged Navier Stokes equations, closing the problem using
another set of 2 equations and a priori knowledge about turbulence. The 2 conservation equations can be applied to k (turbulent kinetic energy) and $\epsilon$ (the turbulent
dissipation) or to k and $\omega$ (specific dissipation). In a nutshell the $k-\omega-SST$ turbulence model combines the strengths of the above-mentionned 
two-equations models and is widely use. <br/>
<br/>
More information on the $k-\omega-SST$ model can be found [here](https://www.cfd-online.com/Wiki/SST_k-omega_model) <br/>.

## Initial and boundary conditions
To reduce the resolution of each time step to a algebraic system of equations, one necessarly needs the following values of the researched fields (v and p):
- on the entire domain at the first time step
- on the edge of the domain at any time step <br/>
<br/>
These are repectively called initial and boundary conditions. <br/>
At time 0, the pressure and velocity fields can be taken as homogeneous in the whole domain. In particular, the chosen values shouldn't be too far from the calculated ones, otherwise
it's going to be harder (or even impossible) to converge to the solution. <br/>
For the boundaries, one has to adapt the conditions depending on the edge. Note that the wing profile is a solid boundary and has to be considered as a domain edge. <br/>
For the inlet and outlet, we can set a so-called "free stream" condition for both pressure and temperature. It is especially appropriate ti the fluid flowing in and out
at a constant $U_{inf}$ velocity. <br/>
For the profile surface, we use classical boundary conditions for wall-bounded flows. By definiton, velocity has to present a no-slip condition on a solid surface,
i.e. should equal 0. It is obviously not the case of pressure which cannot fall to 0 at the boundary. Instead, a zero-gradient condition ... the continuity of pressure.
Remark: in a RANS turbulence model we must give boundary conditions for the turbulent variables ($k$, $\omega$, $\nu_T$). <br/>
More information about RANS modelling and the turbulent variables can be found here.

## Postprocessing the simulation
Once the steady-state of our simulations would be reached, the $C_L$ and $C_D$ coefficients would be calculable, only based on the pressure distribution around the wing profile.
To compute them, one first need the so-called pressure coefficient distribution: <br>
<br/>
$$C_P = \frac{p(x, y) - p_{\infty}}{\frac{1}{2} \rho U_{\infty}^2} $$ <br/>
<br/>
With:
- $p$ the dynamic pressure (Pa)
- $p_{\infty}$ the atmospheric pressure, far from the airfoil (Pa)
- $\rho$ the density of the surrounding fluid (kg/$m^3$)
- $U_{\infty}$ the velocity of the incoming flow (m/s)
which then results in the expressions of $C_L$ and $C_D$ by integrating around the edge of the airfoil <br/>
<br/>
$$C_L = \int_{\partial L} C_P d(\frac{x}{l}) $$
$$C_D =- \int_{\partial L} C_P d(\frac{z}{l}) $$ <br/>
<br/>
Since we know the exact curve that follows a NACA wing profile, one can complete the calculations. Note that we could restrain the physical domain to only one variable $\theta$ since each point has to lie on the airfoil, simplifying the integrals. <br/>
<br/>
OpenFOAM offers pre-defined functions that compute the aerodynamic coefficients for us. In order not to waste computation ressources, OpenFOAM will only compute the data that the user required. In the ControlDict directory, one simply needs to indicate the variables whose values have to be saved at each simulation time step.

```
functions
{
#include "forceCoeffs_object"; # Compute the coefficients
#include "residual_object"; # Residuals
#includeFunc "yPlus"; # Built-in functions
}
```

<br/>
The 2 other #include lines incorporate important simulations output values that will be stored at each time step.

## Testing the single simulation
Although our final goal is to launch a certain number of these simulations, we can run single simulation to see what they look like. <br/>
<br/> After reaching the steady-state, I used ParaView to plot the velocity magnitude over the entire domain for different angle of incidence.

<img src="/Images/alpha5.png" alt="drawing" width="400" caption="alpha = 1"/>  <img src="/Images/alpha10.png" alt="drawing" width="400"/>  <img src="/Images/alpha15.png" alt="drawing" width="400"/>  <img src="/Images/alpha20.png" alt="drawing" width="400"/> 


## Parameter study automation
I detailled above the preprocessing and postprocessing for 1 specific simulation. Now, we need to automatically adapt this process to the 19 other simulations.
The good news is that nothing really changes in the pre- and postprocessing processes except for $\alpha$. <br/>
<br/> The 3 above Python files manage respectively the preprocessing, the processing and the postprocessing process of the study. By simply lauching them successively in the working directory, they autonomously solve this study case resulting in ploting the $C_L$ / $C_d$ polar curve.

### Preprocessing automation
The first Python file manages the preprocessing. From the 'raw' OpenFOAM directory template for single simulations, the script copies it in the main function, pastes it in a new directory using the Create_folder and Copy_Template functions and then changes the value of $\alpha$ in the OpenFOAM directory. <br/>
<br/>
```
def main():
    
    # go to the parent directory which is the project directory
    parent_directory = os.path.abspath('..')
    os.chdir(parent_directory)
    
    
    # repeat for each alpha angle
    for i in range(0,20):
        
        name = 'alpha'+str(i)
        
        alpha = i
        
        # create the alpha## folder
        Create_folder(name)
        
        # copy the Template simulation in the folder
        Copy_Template(name)
        
        # change the value of alpha 
        Change_Template(name, alpha)
        
    os.chdir(os.getcwd()+"/Code/")
      
    
## ------ Call the main function ------ ##
#
if __name__ == "__main__":
    main()
```
<br>
At the end of this first script, we are left with 21 identical directories, except for the value of the incidence angle. 
	
### Processing automation
In CFD, the processing part is the easiest one when we are not talking about MPI and code parallelization. Here, the simulations are not computally challenging, even launched on one single CPU core. <br/>
<br/> This is why the processing Python file is kind of straightforward, only lauching the ./Allrun script in each simulation directory. This script then calls the known OpenFOAM functions that generate the mesh and basically all the simulation algorithm that we set up in the template.
	
### Postprocessing automation
Finally, with the now available data from all the simulations, we are able to draw the polar graph and print it automatically. As discussed earlier, the "forceCoeffs_object" function computes the aerodynamic forces and coefficients using the pressure field as input.<br/>
<br/> The postprocessing Python script then simply extract those values from the generated data and plot them in a gnuplot window that is printed as a pop-up.

```
def main():
    
    # Create the processed folder and the CdCl_values file
    create_Processed_folder()
    
    # Repeat for each angle
    for i in range(0,21):

        pwd = os.getcwd()
        
        # Extract the Cd and Cl coeffs of the simulation from the correct folder
        os.chdir(os.getcwd()+"/alpha"+str(i)+"/Template/postProcessing/forceCoeffs_object/0")
        cd, cl = extract_ForceCoeffs()

        # Go back in the tree
        os.chdir(str(pwd))
        parent_directory = os.path.abspath('..')

        # Write the extracted values in the CdCl_values file
        os.chdir(parent_directory+"/Data/raw/processed")
        with open('CdCl_values', 'a') as file:
        
            file.write("\n"+str(cd)+"   "+str(cl))

        os.chdir(str(pwd))
        
    # Create a gnuplot graph   
    Create_Graph()



if __name__ == "__main__":
    main()
```
	
## Results


## Sources 
https://www1.grc.nasa.gov/beginners-guide-to-aeronautics/lift-equation/






