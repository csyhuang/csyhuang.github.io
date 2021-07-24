---
layout: page
title: Climate Analysis Tools
permalink: /climate_tools/
comments: true
---

Here are packages I have written for processing climate data, together with tools I have found 
online for climate data analysis/visualization.

## My projects on GitHub

- **Python package hn2016_falwa**: 
[hn2016_falwa](https://github.com/csyhuang/hn2016_falwa) is a python package for 
implementing the local finite-amplitude Rossby wave activity diagnostic on climate data. 
This is an analysis technique I proposed in my Ph.D. dissertation. The package is under 
active maintenance and constantly being updated.

- **python-climate-data-processing**: A collection of 
[jupyter notebook tutorials](https://github.com/csyhuang/python-climate-data-processing/tree/master/tutorials) 
(in python 3.6) on Fourier transform, spectral analysis on climate data and visualization using 
the [Cartopy library](http://scitools.org.uk/cartopy/).

## Other blog posts/tutorials

{% for post in site.tags.climate_tools_python %}
 - [{{ post.title }}]({{ post.url }})
{% endfor %}

## Free online tools for climate data visualization/manipulation
- [Panoply by NASA](https://www.giss.nasa.gov/tools/panoply/): data viewer (on maps) of 
netCDF, HDF and GRIB files; I use it on Windows/macOS.
- [NCO toolkit](http://nco.sourceforge.net/): command-line program to manipulate netCDF files; 
I often use that on Linux.
