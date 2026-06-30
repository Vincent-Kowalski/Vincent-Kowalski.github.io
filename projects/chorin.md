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

## Spatial and time discretization

  ### Time
  From a simple Newton's first order explicit scheme, we formulate the $k^{th}$ time step with: $$t^k = (k-1) \Delta t$$

  ### Space
  We discretized our square simulation domain with a staggered grid instead of a ... grid. It is a very commun way to compute precisely pressure gradients.
  [IMAGE DE LA GRILLE]

## Differential operators discretization
Simulating a flow implies resolving the approximated Navier Stokes equations on the discretized space.

## Poisson solver

## Spatial and time loops

## Verification









