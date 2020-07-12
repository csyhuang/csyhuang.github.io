---
layout: page
title: Handy Tools
permalink: /tools/
comments: true
---

Here are some of the tools I have been using for research and presentation. I look forward to learning more and sharing resources and tips with the interested community.

## KLatexFormula for generating images of equations from LaTeX

<img style="float: right;" src="https://klatexformula.sourceforge.io/assets/klfmainwin.png" width="120">

[KLatexFormula](https://klatexformula.sourceforge.io/), developed by [Philippe Faist](https://people.phys.ethz.ch/~pfaist/), is a handy application that generates images (pdf, eps, png, etc.) that can be flexibly resized. Operations are as simple as drag and drop, copy and paste. This is my favorite tool to use for preparing graphics of equations to include in presentation slides.

## BA-poster: a LaTeX Poster template

The [BA-poster](http://www.brian-amberg.de/uni/poster/) latex package, designed by [Brian Amberg](https://scholar.google.com/citations?user=iqUqxgIAAAAJ&hl=zh-TW), is a flexible latex poster template that *efficently design pretty posters for scientific conferences*. By specifying the number of blocks per column, the package does a great job in positioning the text, equations and graphs that you stuff in with little twist and tweak. It has saved a lot of my time preparing for poster presentations.

## Python Interface for downloading ECMWF Public Datasets

ECMWF has a nice [web interface](http://apps.ecmwf.int/datasets/) for downloading the Reanalysis datasets (e.g. ERA-Interim). However, to download massive chunk of data into different files naming according to dates onto a server, using their [python interface](https://software.ecmwf.int/wiki/display/WEBAPI/Access+ECMWF+Public+Datasets) would save a lot of time.

Here is a [python code](http://home.uchicago.edu/~csyhuang/Tools/ERAInterim_Retrieve_T2m.py) that I used to download ERA-Interim Surface Temperature (T2m) data from 1979-2016 into netCDF files year by year for the use of hackathon in the Rossbypalooza workshop. To run the code, you have to install the [ecmwfapi python library](http://pypi.python.org/pypi/ecmwf-api-client/) first (see my [previous post](/2016/06/13/ECMWF-download/) for instructions). You also need the [ECMWF key](https://software.ecmwf.int/wiki/display/WEBAPI/Access+ECMWF+Public+Datasets#AccessECMWFPublicDatasets-key).

## Emacs OrgMode as Research Notebook

<img style="float: right;" src="http://home.uchicago.edu/~csyhuang/images/OrgMode-Preview.png" width="300">

[OrgMode](http://orgmode.org/) is an editing mode in Emacs created by [Carsten Dominik](http://staff.science.uva.nl/~dominik/) that is capable of organizing notes, schedules, images and even codes all at one place (with plain text typing). Here is a [Youtube video of the GoogleTech Talk](https://www.youtube.com/watch?v=oJTwQvgfgMM) in which Carsten Dominik was introducing it. Its official webpage has a [useful tutorial for beginners](http://orgmode.org/worg/org-tutorials/org4beginners.html).

I introduced the OrgMode and how to configure EMacs for it in a one of our graduate student lunch talk. Here is [a link to my demo](http://home.uchicago.edu/~csyhuang/Tools/May4_Presentation.zip) together with the required configuration file (.emacs). Instructions of what keys to press are all included in "(...)" in the .org file itself. You can open the .org file with Emacs and play with it.

