---
layout: page
title: chorin
permalink: /projects/chorin/
---


# Implementation & Validation of the Chorin algorithm in a driven square cavity (Matlab)

## Context
During my last semester of study I attended this Numerical Fluid mechanics and thermics module. It was a heavy one concentrating on the actual ways to simulate flows and heat exchanges. We alternatively focused on both theoretical and practical aspects of it. One of the projects was intended to make us dive further into the Matlab scientific coding and understand how it helps us to solve technical problems.

## Project overview
The goal of the project was to implement by ourselves the Chorin projection method in a simple case. Developped in 1967 it is the first algorithm capable of solving the pressure-velocity coupling in a incompressible flow. 
Well-known in the CFD communnity, it has been validated more than once and represents the first step into incremental pressure-correcting algorithms. <br/>
<br/>
Starting from almost a blank page, we had to apply the algorithm to the simple case of the driven square cavity.
[SCHEMA]
After the postprocessing step we had to validate our results with the numerical data from Ghia and al.

## Algorithm principle
The projection method was developped simultaneously by Chorin and Temam in 1968.
To solve a flow at each time step, one should find the velocity and pressure fields that satisfy the so-called Navier Stokes equation (NSE) :
[[EQUATIONS]]
The projection methed develop following principle :
 - **Step 1: Solve the convection-diffusion equation :** It is derived from the 2nd Navier-Stokes equation and is basically the conservation of momentum equation
 without the pressure gradient. The solution is written $\tilde{u}$ since this predicted velocity field does not satisfy the continuity equation yet. 
 - **Step 2: Solve the Poisson equation :** By using both continuity and momentum equation one can end up with a specific differential equation for pressure called
 the Poisson equation. We solve it using the predicted velocity from step 1.
 - **Step 3: Correct the predicted velocity field: ** By reorganizing the terms in the Poisson equation, we can now compute the divergent-free velocity field knowing 
 the pressure from step 2.

## Spatial and time discretization

  ### Time
  We simply formulate the $k^{th}$ time step with: $$t^k = (k-1) \Delta t$$

  ### Space
  We discretized our square simulation domain with a staggered grid instead of a ... grid. It is a very commun way to compute precisely pressure gradients.
  [IMAGE DE LA GRILLE]

## Differential operators discretization
Simulating a flow implies resolving the approximated Navier Stokes equations on the discretized space. This set of equations features continuous differential operators that we need to discretized.
<br/> A classical way to do that is to use centered finite difference schemes. It is communly admitted that a second order scheme is quit accurate for simple cases. <br/>
This schemes read :
$$ \frac{\partial u}{partial x} = \frac{u(x+ \Delta x) - u(x- \Delta x)}{2 \Delta x} + O(\Delta x^2) $$ for the first derivative <br/>
$$  \frac{\partial ^2 u}{partial x^2} = \frac{u(x+ \Delta x) - 2u(x) + u(x- \Delta x)}{\Delta x^2} + O(\Delta x^2) $$ for the second derivative <br/>
<br/>
From this one can replace every differential operators in the Navier-Stokes equations to switch from continuous physical equations to dicretized computer-understandable ones. 
Below are 2 examples of discretized differential operators : velocity divergence and laplacian. The first is only featuring first derivative while the other utilizes 
the second derative. <br/>
<br/>
For the divergence:

```
function [ div ] = div_vel(i, j, Ncx, Ncy, dx, dy, bc_u, bc_v, vel_u, vel_v )
% Computes the divergence of the velocity field
% div to be calculated at the cell center (i,j)

    % Interior points: Use central differences
    du_dx = (vel_u(i+1, j) - vel_u(i-1, j)) / (2 * dx);
    dv_dy = (vel_v(i, j+1) - vel_v(i, j-1)) / (2 * dy);

    % Compute divergence
    div = du_dx + dv_dy;

end
```
For the laplacian:

```
function [ lap ] = lap_vel_u( i, j, Ncx, Ncy, dx, dy, bc_u, vel_u  )
%Computes the Laplacian of the velocity component u

	% extract the relevant components of the u velocity
	u_i = vel_u(i,j);
		   
	% apply the second order finite difference scheme at the point i+1/2, j
	d_square_u_dx_square = (vel_u(i-1,j) - 2*u_i + vel_u(i+1,j)) / dx^2;
	d_square_u_dy_square = (vel_u(i,j-1) - 2*u_i + vel_u(i,j+1)) / dy^2;

	lap = d_square_u_dx_square + d_square_u_dy_square;
```

## Prediction of the velocity field (step 1)


## Poisson solver (step 2)

## Correct the velocity field (step 3)

## Spatial and time loops

## Verification









