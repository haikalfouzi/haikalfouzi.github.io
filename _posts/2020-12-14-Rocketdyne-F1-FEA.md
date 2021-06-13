---
layout: post
title: FEA Analysis on Rocketdyne F-1 Rocket Engine - Flanged Nozzle Bolt
---

# Overview

{% include image.html file="/f1-engine/f1-rocket-engine.png" description="Rocketdyne F-1 Rocket Engine - First Stage" %}

{% include youtube.html id="ViNcBQ8cDA0" %}
<center>Rocketdyne F-1 rocket engine in action during Saturn V launch on July 16, 1969 for Apollo lunar mission.</center>

The goal of this research is to analyze the bolted flange joint that connect the mid and lower parts of the F1 engine nozzle. The image below shows the simplified corresponding nozzle model in ANSYS (upper nozzle is excluded in this analysis). Analysis will be done using a [non-linear finite element model](https://enterfea.com/difference-between-linear-and-nonlinear-fea/) in ANSYS to assess the margin of safety of the flange bolts and determine the gaps that develop between the jointed parts when the assembly is loaded.

{% include image.html file="/f1-engine/SaturnF1EngineDiagram.png" description="Saturn F1 Engine Diagram" %}
{% include image.html file="/f1-engine/model_expl.png" description="Overall Simplified ANSYS Model" %}
{% include image.html file="/f1-engine/spacecraft_assembly4.png" description="200 Bolts Securing Middle and Lower Part of the Nozzle" %}

Below are the material properties that are going to be used for this analysis.

Parts|Material|Young's Modulus (psi)|Poisson's Ratio|Coefficient of Thermal Expansion (per deg. F)
--------|--------|--------|--------|--------
Nozzle|300 Series Stainless Steel|29000000|0.27|0.00001
Bolt and Nut|A-286 Steel|29000000|0.31|0.0000095

The pressure due to the exhaust gas in the nozzle is calculated using 1D gas dynamics. It is assumed to vary linearly along the nozzle axis. The pressure at the exit (z=0)  is 12.17 psi and the pressure at the entrance to the mid-nozzle is 47.72 psi.

The regeneration channels are omitted in the model. In exchange, a free body diagram is used to deduce the equivalent forces on the mid nozzle and lower nozzle (the upper nozzle is not modeled here). This force pair is modeled as two separate forces, each of 1000 lbf. The gas temperature is 700 F which causes thermal strain. The bolt is assumed to be pre-loaded to 50% of its breaking strength.

## Essential Boundary Conditions & Governing Equations

Boundary conditions and assumptions:
1. Frictionless support at the top surface of mid nozzle
  * Normal displacement = 0
  * Tangential traction (Shear) = 0
2. Pressure due to propellant
  * Obey 1D gas dynamics
  * Varies in axial direction (z-axis)
3. Force from regeneration channels
  * Pulls apart the mid and lower nozzles

Governing equations are as follows:
{% include image.html file="/f1-engine/governingequations.png" description="Differential Equations of Equilibrium" %}

{% include image.html file="/f1-engine/Straindisplacement.png" description="Strain-Displacement Relations" %}

{% include image.html file="/f1-engine/matrixmodel.png" description="Constitutive Matrix Model" %}

## Initial Calculation/Prediction
### Axial Reaction of Thrust

{% include image.html file="/f1-engine/axial.png" %}

Based on the projected area of pressure in the mid & lower part of nozzle (shaded concentric circle), we can calculate the net force in the negative z-direction. That would be the net reaction force for the whole geometry, assuming that the top mid part of the nozzle is fixed to wall/support.

The calculated projected area in the mid & lower part of nozzle is to be 9,699 $$in^2$$, thus the net pressure force based on the average gas pressure at the entrance of mid nozzle and at the exit of lower nozzle is 290,970 $$lbf$$.

Our model is scale to 1:400, so the net pressure force of 1/400th of the model is 727 $$lbf$$.

### Hoop Stress on Nozzle

{% include image.html file="/f1-engine/hoop stress.png" %}

We can estimate the hoop stress or the stress in the circumferential direction of the nozzle. Based on the [pressure vessel theory](https://en.wikipedia.org/wiki/Cylinder_stress#Hoop_stress), we can find the hoop stress is given as follow:

$$\begin{equation}\sigma_{\theta} = \frac{Pr}{t}\end{equation}$$

where $$P$$ is the pressure, $$r$$ is the radius, and $$t$$ is the wall thickness. Plugging in all the value and we'll get estimated hoop stress for the nozzle.

$$\begin{equation}\sigma_{\theta} = \frac{12.17 psi * 69.5 in}{0.5 in} = 1692 in\end{equation}$$

### Thermal Strain of the Nozzle

{% include image.html file="/f1-engine/thermal strain.png" %}

The estimated deformation due to thermal stress can be calculated using the following thermal strain formula as shown below:

$$\begin{eqnarray}\frac{$\delta$l}{L} = $\alpha\Delta$T\\\frac{$\delta$l}{160 in} = (1e^{-5})(700 ℉ - 70 ℉) = 1.008 in\end{eqnarray}$$

### Bolt Preload Deformation

{% include image.html file="/f1-engine/bolt preload.png" %}

The bolt is pre-loaded with 50% of it's ultimate tensile strength which is 9280$$lb$$, so the load applied is 4640$$lb$$, and on each side is 2320$$lb$$.

$$\begin{eqnarray}$\Delta$l = \frac{FL}{EA}\\$\Delta$l = \frac{2320lbf * 0.5 in}{(2.9*10^{7}psi)*(3.8*10^{-2}in^2)} = 0.001 in\end{eqnarray}$$
