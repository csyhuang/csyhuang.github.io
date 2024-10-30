---
layout: page
title: Climate Research
permalink: /climate_research/
comments: true
---

## Blog posts related to research/publications

{% for post in site.tags.research %}
 - {{post.date | date: "%Y-%m-%d"}} - [{{ post.title }}]({{ post.url }})
{% endfor %}

--- 

## Climate Research Tools

### My projects on GitHub

Here are packages I have written for processing climate data.

- **Python package hn2016_falwa**: 
[hn2016_falwa](https://github.com/csyhuang/hn2016_falwa) is a python package for 
implementing the local finite-amplitude Rossby wave activity diagnostic on climate data. 
This is an analysis technique I proposed in my Ph.D. dissertation. The package is under 
active maintenance and constantly being updated.

- **python-climate-data-processing**: A collection of 
[jupyter notebook tutorials](https://github.com/csyhuang/python-climate-data-processing/tree/master/tutorials) 
(in python 3.6) on Fourier transform, spectral analysis on climate data and visualization using 
the [Cartopy library](http://scitools.org.uk/cartopy/).

### Other blog posts/tutorials

{% for post in site.tags.climate_tools_python %}
 - [{{ post.title }}]({{ post.url }})
{% endfor %}

### Free online tools for climate data visualization/manipulation

Here are tools I have found online for climate data analysis/visualization.

- [Panoply by NASA](https://www.giss.nasa.gov/tools/panoply/): data viewer (on maps) of 
netCDF, HDF and GRIB files; I use it on Windows/macOS.
- [NCO toolkit](http://nco.sourceforge.net/): command-line program to manipulate netCDF files; 
I often use that on Linux.

---

## Academic presentations

* **[Atmospheric Blocking as a Traffic Jam in the Jet Stream](https://www.dropbox.com/s/e2oxup0p7esv5nz/clare-uw-colloquium-20190909.pdf?dl=1)**
Colloquium, Department of the Atmospheric and Oceanic Sciences, University of Wisconsin-Madison,
Wisconsin. (Sep 2019)

* **[Budgets of finite-amplitude Local Wave Activity in Northern Winter](https://www.dropbox.com/s/0ocvvwfb44e551b/AMS2017_poster_final.pdf?dl=1)**  
Poster presentation at [AMS 21th AOFD Meeting 2017](https://ams.confex.com/ams/21Fluid19Middle/webprogram/21FLUID.html), Portland, OR. (Jun 2017, substitute presenter: Dr. Sandro Lubis)  

* **[Trends and Climatologies of Northern Hemispheric Local Wave Activity and Fluxes in a Warming Climate (Dec 2016)](https://agu.confex.com/data/handout/agu/fm16/Paper_197912_handout_9528_0.pdf)**  
Poster presentation at [AGU 2016 Fall Meeting](http://fallmeeting.agu.org/2016/), San Francisco, CA.

* **[Characterizing Blocking Episodes with Local Finite-amplitude Wave Activity: Lifecycle and Climatology (Apr 2016)](http://www.met.reading.ac.uk/~ben/blocking2016/talks/huang.pdf)**  
Oral presentation at [Workshop on Atmospheric Blocking](http://http//www.sparc-climate.org/meetings/wwwsparc-climateorgmeetingssparc-blocking-workshop-april2016/), Reading, United Kingdom.

* **Characterization of Blocking Episodes in the North Atlantic and Pacific with the Finite-amplitude Local Wave Activity Formalism (Dec 2015)**  
Oral presentation at [AGU 2015 Fall Meeting](http://fallmeeting.agu.org/2015/), San Francisco, CA.

* **[Local Finite-amplitude Rossby Wave Activity as a Diagnostic of Anomalous Weather Regimes (June 2015)](https://ams.confex.com/ams/20Fluid/videogateway.cgi/id/30468?recordingid=30468)**  
Oral presentation at [AMS 20th AOFD Meeting 2015](http://www.ametsoc.org/meet/fainst/201520fluid.html), Minneapolis, MN.

* **[Local Finite-Amplitude Rossby Wave Activity as a Diagnostics for Wave-Breaking (Dec 2014)](http://home.uchicago.edu/~csyhuang/Presentations/AGU2014_upload.pdf)**  
Poster presentation at [AGU 2014 Fall Meeting](http://fallmeeting.agu.org/2014/), San Francisco, CA.

* **[Generalize Critical Line and Rossby Wave Breaking (Dec 2013)](http://home.uchicago.edu/~csyhuang/Presentations/AGU2013_upload.pdf)**  
Poster presentation at [AGU 2013 Fall Meeting](http://fallmeeting.agu.org/2013/), San Francisco, CA.

* **[A Study of Barotropic Decay with Finite-Amplitude Wave Theory(June 2013)](http://home.uchicago.edu/~csyhuang/Presentations/AMS2013_final.pdf)**  
Poster presentation at [AMS 19th AOFD Meeting 2013](https://ams.confex.com/ams/19Fluid17Middle/webprogram/start.html), Newport, RI.
