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
The projection method was developped simultaneously by Chorin and Temam in 1968. Its principle is the following:

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
From this one can change every differential operators in the Navier-Stokes equations to switch from continuous physical equations to dicretized computer-understandable ones.

## Poisson solver

## Spatial and time loops

## Verification









