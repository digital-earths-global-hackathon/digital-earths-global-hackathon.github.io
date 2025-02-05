---
title: Scientific Scope
order: 10
aliases: 
  - /planning/science/index.html
---

# Core Simulations 

The common scientific agenda of the Digital Earths Global Hackathon (DEGH) will be simulations of an annual cycle using global atmospheric general circulation models with a horizontal discretization of 5 km or less.  Every participating node will host one or more such simulations, which are performed and for which output is provided following a common protocool  These are the DEGH core simulations.

##  Experimental Protocol

The protocol follows Takasuka et al., *Prog. Earth and Planet. Sci.* (2024) [(preprint)](https://www.researchsquare.com/article/rs-4458164/v1), and contains the following elements:

 * Atmosphere-only model with specified sea-ice and sea-surface tmeperature and ideally no convective parameterization.
 * Simulation grid: horizontal tiling of 5 km or less in linear dimension, vertical grid unspecified.
 * Atmospheric initializaiton interpolated from IFS on 2020-01-20.
 * Land initialization up to individual groups, but must be documented.
 * Analysis period: From 2020-03-01 to 2021-02-28.
 * Output grid: HEALPix zoom level 9 or higher (effective cell size of 13 km) at 6 hr interval (3D variables) and 1 hr interval for 2D fields.

##  Output Protocol

See the [Data request under development](../hosting/technical/data_request.md) for details. 

