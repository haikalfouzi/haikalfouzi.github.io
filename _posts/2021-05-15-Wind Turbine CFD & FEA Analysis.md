---
layout: post
title: CFD & FEA Analysis of a 1.5 MW General Electric Wind Turbine (1.5xle)
---

# Overview

This analysis covered the deformation due to aerodynamic loading of a wind turbine blade by performing a steady state one way fluid structure interaction (FSI). This analysis will use ANSYS Fluent to develop aerodynamic loading on the blade, the pressure loads on the blade, and the stresses and deformations that occurred on the blade.

{% include image.html file="/wind-blade-turbine/Screenshot 2021-05-30 at 3.23.16 PM.png" description="GE Wind Turbine Technical Data" %}

## Wind Turbine Blade Profile

Thte blade is 43.2 meters long and starts with a cylindrical shape at the root and then transitions to the airfoils S818, S825, and S826 for the root, body, and tip.

{% include image.html file="/wind-blade-turbine/Imported_pressure_2.PNG" description="Wind Turbine - Root View" %}
{% include image.html file="/wind-blade-turbine/blade render.png" description="Wind Turbine - ISO View" %}
{% include image.html file="/wind-blade-turbine/blade rende side viewr.png" description="Wind Turbine - Front View" %}

This blade also has pitch to vary as a function of radius, giving it a twist and the pitch angle at the blade tip is 4 degrees. The blade is made out of an orthotropic composite material, and it has varying thickness and spar inside of the blade for structural rigidity.

{% include image.html file="/wind-blade-turbine/homogonized orthotropic material.PNG" description="Material Properties" %}

## Case Setup

Operating conditions is using air at standard conditions:

Operating Conditions | Value
---------------------|-------
Air Temperature | 15 - ℃
Air Density | 1.225 kg/m^3
Viscosity | 1.7894e-05 kg/(ms)

Fluid Domains | Boundary Conditions
--------------|--------------------
Inlet | Velocity of 12 m/s with turbulent intensity of 5% and turbulent viscosity ratio of 10
Outlet | Pressure of 1 atm
Blade | No-slip
Side Boundaries | Periodic

{% include image.html file="/wind-blade-turbine/Boundary_Conditions3.png" description="Fluid Domain Boundary Conditions" %}
{% include image.html file="/wind-blade-turbine/fluid domain mesh CFD.PNG" description="Fluid Domain Mesh" %}
{% include image.html file="/wind-blade-turbine/fluid domain mesh CFD statistic.PNG" description="Fluid Domain Mesh Statistic" %}
{% include image.html file="/wind-blade-turbine/fluid domain mesh CFD metric.PNG" description="Fluid Domain Mesh Orthogonality Metric" %}
{% include image.html file="image/wind-blade-turbine/fluid domain mesh CFD metric skewness.PNG" %}

From the fluid domain mesh metrics, we have around 370,494 elements. So the total number of unknowns and the algebraic equations ANSYS Fluent needs to solve is around 2.22 million (u, v, w, p, k, 𝜔 - 6 unknowns from governing equations). The skewness and orthogonality quality of the mesh are also in acceptable range.

Skewness metrics:
Outstanding	| Very Good	| Good | Sufficient | Bad | Inappropriate
------------|-----------|------|------------|-----|---------------
0-0.25 | 0.25-0.50 | 0.50-0.8 | 0.80-0.95 | 0.95-0.98 | 0.98-1.00

Orthogonal quality:
Inappropriate | Bad | Sufficient | Good | Very Good | Outstanding
--------------|-----|------------|------|-----------|------------
0-0.001 | 0.001-0.15 | 0.15-0.20 | 0.20-0.70 | 0.70-0.95 | 0.95-1.00

## Computational Fluid Dynamics Result

 Mass Flow Rate               | (kg/s)
------------------------------|---------------------
                           inlet|            221216.84
                       inlet-top|            664992.78
                          outlet|           -886209.81
                             Net|           -0.1850242
