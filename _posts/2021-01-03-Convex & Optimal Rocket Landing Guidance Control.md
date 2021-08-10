---
layout: post
title: Convex & Optimal Rocket Landing Guidance Control
---

{% include image.html file="/landing/failed landing.gif" description="From this" %}
{% include image.html file="/landing/falcon b1052 & b1053 landing.gif" description="To this" %}

# Overview

The goal of this study is to solve complex planetary soft landing problem with a defined set of states and control constraint. The real-time generation of fuel-optimal paths to a prescribed location on a planet's surface is a challenging problem due to constraints on the fuel, control inputs and the states. This study also introduce a convexification of the control constraints that is proven to be lossless; this is required due to existence of nonconvex constraints on the control input magnitude at nonzero lower bound and a nonconvex constraint on its direction. Lossless convexification allows us to implement the algorithm on an autonomous real-time systems with powerful modern interior point method (IPM) solvers.

The trajectory optimization problems are nontrivial due to nonconvex control constraints and will be formulated as a finite-dimensional second-order cone program (SOCP) where it could be solve iteratively.

### Partial List of Notations

$r$ = position

$T$ = Thrust vector (N)

$g$ = Gravity vector of planet ($$m/s^{2}$$)

$m(t)$ = Mass of vehicle

$\alpha$ = Constant describing mass consumption rate

$T_1$ = Lower Thrust Bound

$T_2$ = Upper Thrust Bound

$\rho_1$ = Lower Thrust Bound

$\rho_2$ = Upper Thrust Bound

$m_{wet}$ = Total mass of vehicle including propellant

$t_f$ = Length of finite time

$x$ = Augmented state vector defined as $[r, \dot r]^T$

$r_z(t)$ = Vertical position

$r_x(t)$ = Downrange position

$\Theta_{alt}$ = Glideslope constraint

$S_j, v_j, a_j$ = Matrices, vectors, and scalars defining convex state constraints

$I_{sp}$ = Specific impulse of the rocket engine

$u$ = Control acceleration put on by the thrusters

$\Gamma$ = Slack variable required for convexification

$log$ = Natural logarithm

$\vert \vert x \vert \vert$ = Euclidian 2-norm of vector x

## Introduction

Soft landing is the final phase of a planetary EDL (entry, descent, and landing) and it's also referred to as the powered descent landing stage. In the powered descent stage, the vehicle is guided as close as possible to a prescribed location on the planet's surface by using thrusters that provide the control authority. This phase concludes when the vehicle lands with zero velocity relative to the surface.

With the atmospheric qualities of the planet being stochastic, the position in which the descent phase must begin is uncertain. Thus, we would focus on the algorithm which maximize the divert capability of the craft by minimizing fuel consumption from initial condition to the terminal state.

The convex optimization is required because it allows real-time onboard implementation and has guaranteed convergence properties with deterministic criteria. The convex optimization framework will be posed as a finite-dimensional second-order cone program (SOCP) problem, as SOCP have low complexity and can be solved in polynomial time.

## Planetary Landing with Thrust Pointing Constraints

The planetary soft landing problem searches for the thrust (control) profile $T_c$  and an accompanying translational state trajectory $x$ which guide a lander from an initial position $r_0$ and velocity $\dot r_0$ to a state of rest at prescribed target location on the planet while minimizing the fuel consumption.

When the target point is unreachable from a given initial state, a precision landing problem (or minimum landing error problem) is considered instead, with the objective to first find the closest reachable surface location to the target and second to obtain the minimum fuel state trajectory to that closest point.

We will solve our system in two dimensions (x,z), and the position and velocity vectors will be the size of $\Bbb R^2$. We assumed everything is in the surface fixed frame, thus the translational dynamics of the system are given as:

$$\begin{eqnarray}\ddot r(t) = g + \frac{T_c(t)}{m(t)}\\\dot m(t) = -\alpha \vert \vert T_c(t) \vert \vert \end{eqnarray}$$

where $\ddot r(t) \in \Bbb R^2$ is acceleration of the vehicle, $g$ is the gravitional acceleration vector of the planet, $T_c(t)$ is the thrust vector, $m(t)$ is the mass of the vehicle, and $\alpha$ is a positive constant describing the fuel consumption rate.

For $nth$ identical thrusters with nominal thrust level $T$ and specific impulse $I_{sp}$, we can denote the thrust vector and fuel consumption rate as follow:

$$\begin{eqnarray}T_c = (nT\cos \phi)e\\\alpha = (I_{sp}g\cos \phi)^{-1}\end{eqnarray}$$

But for simplicity, we'll assume vehicle with one engine and cantilever angle of 0 degrees.

{% include image.html file="/landing/glideslope constraint.png" description="Glideslope Constraint" %}

The main state constraints are the glide slope constraint on the position vector and an upper bound constraint on the velocity vector magnitude. The glide slope constraint is described in figure above and is imposed to ensure that the lander stays at a safe distance from the ground until it reaches its target. The upper bound on velocity is needed to avoid supersonic velocities for planets with atmosphere, where the control thrusters can become unreliable. Both of these constraints are convex and fit well to the convex optimization framework.
