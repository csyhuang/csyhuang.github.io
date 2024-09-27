---
layout: post
title: IMPORTANT Bug fix release hn2016_falwa v0.7.2
comments: true
image_url: 'https://github.com/csyhuang/csyhuang.github.io/raw/master/assets/img/falwa_diagram.png'
tags: ['research', 'se']
---

We published an important bugfix release [hn2016_falwa v0.7.2](https://github.com/csyhuang/hn2016_falwa/releases/tag/v0.7.2), which requires recompilation of fortran modules. 

Two weeks ago, we discovered that there is a mistake in the derivation of expression of nonlinear zonal advective flux term, which leads to an *underestimation* of the nonlinear zonal advective flux component.

We will submit corrigendum for [Neal et al. (2022, GRL)](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2021GL097699) and [Nakamura and Huang (2018, Science)](https://www.science.org/doi/10.1126/science.aat0721) to update the numerical results. The correct derivation of the flux expression can be found in the corrected supplementary materials of NH18 (to be submitted soon).  There is no change in conclusions in any of the articles.

Please refer to [Issue #83](https://github.com/csyhuang/hn2016_falwa/issues/83) for the numerical details and preliminary updated figures in NHN22 and NH18:

Thank you for your attention and let us know if we can help.