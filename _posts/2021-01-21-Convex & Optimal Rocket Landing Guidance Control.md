---
layout: post
title: Convex & Optimal Rocket Landing Guidance Control
---

{% include image.html file="/landing/failed landing.gif" description="From this" %}
{% include image.html file="/landing/falcon b1052 & b1053 landing.gif" description="To this" %}

# Overview

The goal of this study is to solve complex planetary soft landing problem with a defined set of states and control constraint. The real-time generation of fuel-optimal paths to a prescribed location on a planet's surface is a challenging problem due to constraints on the fuel, control inputs and the states. This study also introduce a convexification of the control constraints that is proven to be lossless; this is required due to existence of nonconvex constraints on the control input magnitude at nonzero lower bound and a nonconvex constraint on its direction. Lossless convexification allows us to implement the algorithm on an autonomous real-time systems with powerful modern interior point method (IPM) solvers.

The trajectory optimization problems are nontrivial due to nonconvex control constraints and will be formulated as a finite-dimensional second-order cone program (SOCP) so it could be solve iteratively.

# Definition

$$r$$ = position
$$T$$ = Thrust vector (N)
$$g$$ = Gravity vector of planet ($$m/s^2$$)
