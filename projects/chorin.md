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
 - **Step 3: Correct the predicted velocity field:** By reorganizing the terms in the Poisson equation, we can now compute the divergent-free velocity field knowing 
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
This schemes read : <br/>
$$\frac{\partial u}{\partial x} = \frac{u(x+ \Delta x) - u(x- \Delta x)}{2 \Delta x} + O(\Delta x^2)$$ for the first derivative <br/>
$$\frac{\partial ^2 u}{\partial x^2} = \frac{u(x+ \Delta x) - 2u(x) + u(x- \Delta x)}{\Delta x^2} + O(\Delta x^2)$$ for the second derivative <br/>
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
<br/>
<br/>
<ins> Note </ins> : Only a part of the laplacian function is displayed. Particular attention was paid to the cells at the boundaries, which I won't explain here.

## Prediction of the velocity field (step 1)
For this first step, we are going to apply the convection-advection equation to each cell of the grid. To do so, a spatial loop is implemented as follows:

```
%% Initialisation of the predicated velocity field
    u_tilde = zeros(size(vel_u));
    v_tilde = zeros(size(vel_v));
	
%% Compute the x-component of the predicated velocity field : u_tilde
    for j = (2:Ncy+1)
        for i = (2:Ncx)	
			u_tilde(i,j) = (viski*lap_vel_u(i,j,Ncx,Ncy,dx,dy,bc_u,vel_u) - conv_vel_u(i,j,Ncx,Ncy,dx,dy,bc_u,bc_v,vel_u,vel_v))*tstep + vel_u(i,j);

        end
    end

```
As explained we predict the velocity field with the u_tilde(i,j) line. Each called function in this line represents a differential operator. Namely we have:
- lap_vel_u (resp. lap_vel_v) : the laplacian of the x- (resp. y-) component of the velocity 
- conv_vel u (resp. conv_vel_v) : the rotational of the x- (resp. y-) component of the velocity 




## Poisson solver (step 2)
As explained before, pressure can be a bit treacky to deal with in CFD. That comes from the fact that NSE don't give an explicit differntial equation for pressure. <br/>
However such an equation can be derived by combining NSE, that leads to the Poisson equation, which in an incompressible steady state reads: <br/>
$$\frac{\partial ^2 p}{\partial x^2}= \rho \frac{\partial}{\partial x_j}  \left( u_i \frac{\partial u_i}{\partial u_j} \right) $$ <br/>
<br/>
It is in fact a second-order linear differential equation that can be solved using a Gauss-Seidel method. Once both spatial differential operators are discretized
using the central difference scheme, the pressure at the center of one cell can be isolated as a function of the neighboring pressures. <br/>
<br/>
This is why we need to loop the whole process and stop it whenever the norm of the difference between the old and new velocities are small enough.

## Correct the velocity field (step 3)
After the first two steps, we do have the predicted velocity and pressure fields. At that point, we can't use the velocity as we found it since it does not verify 
the continuity equation i.e. it is not divergence-free. <br/>
To counter this, we can simply reorganise the terms in the Poisson equation. It reads:
$$u_{i,j}^{k+1}= \tilde u_{i,j}^{k+1} - \frac{\Delta t}{\rho} \frac{\delta p^{k+1}}{\delta x_i}$$ <br/>
As seen before $k$ is the time index, which shows that the velocity is now corrected only the terms from the actual time step. <br/>
In a code form it looks like that: <br/>
$$$$



## Spatial and time loops

## Verification









