---
layout: post
title: Python Library and scripts for downloading ERA-Interim Data
tags: ['climate_tools_python']
comments: true
---

### Update: ECMWF API Clients on pip and conda

The ECMWF API Python Client is now available on pypi and anaconda.  
The Climate Corporation has distributed the ECMWF API Python Client on 
pypi. Now it can be installed via:

> pip install ecmwf-api-client

Anaconda users on OS X/linux system can install the package via:

> conda install -c bioconda ecmwfapi 

<!-- 
### Old steps (1-3)

```
(1) Installing the package requires the python Setuptools. You can set it up locally with the command:

> wget https://bootstrap.pypa.io/ez_setup.py -O - | python - --user

(2) Download the Python library package and unzip it (You can do it in any directory)
> wget https://software.ecmwf.int/wiki/download/attachments/56664858/ecmwf-api-client-python.tgz
> tar zxf ecmwf-api-client-python.tgz

You shall see four items extracted:
- example.py
- ecmwfapi/__init__.py
- ecmwfapi/api.py
- setup.py

(3) In the directory with these four items, install the package with:
> python setup.py install --user
```
 -->
<!--more-->
To use the sample script, you need an API key ( .ecmwfapirc ) placed in your home directory. You can retrieve that by logging in: https://api.ecmwf.int/v1/key/
Create a file named ".ecmwfapirc" in your home directory and put in the content shown on the page:

```
{
    "url"   : "https://api.ecmwf.int/v1",
    "key"   : "(...)",
    "email" : "(...)"
}
```

After doing that, in the directory with the sample script example.py, you can test the package by running it:
> python example.py  

You should see it successfully retrieves a .grib file if the package has been set up properly.

There are [sample scripts](https://software.ecmwf.int/wiki/display/WEBAPI/Python+ERA-interim+examples) 
available on the ECMWF website (look under "Same request NetCDF format"). Below is a example of python 
script I wrote to retrieves zonal wind, meridional wind and temperature data at all pressure levels 
during the time period 2017-07-01 to 2017-07-31 in 6-hour intervals:

```python
#!/usr/bin/env python
from ecmwfapi import ECMWFDataServer
server = ECMWFDataServer()

param_u, param_v, param_t = "131.128", "132.128", "130.128"

for param_string, param in zip(["_u", "_v", "_t"],
                               [param_u, param_v, param_t]):

    server.retrieve({
        "class": "ei",
        "dataset": "interim",
        "date": "2017-07-01/to/2017-07-31",
        "expver": "1",
        "grid": "1.5/1.5",
        "levelist": "1/2/3/5/7/10/20/30/50/70/100/125/150/175/200/225/250/300/350/400/450/500/550/600/650/700/750/775/800/825/850/875/900/925/950/975/1000",
        "levtype": "pl",
        "param": param,
        "step": "0",
        "stream": "oper",
        "format": "netcdf",
        "time": "00:00:00/06:00:00/12:00:00/18:00:00",
        "type": "an",
        "target": "2017-07-01/to/2017-07-31" + param_string + ".nc",
    })
```

I learnt the above steps on these pages:
- [ECMWF python library](https://software.ecmwf.int/wiki/display/WEBAPI/Access+ECMWF+Public+Datasets#AccessECMWFPublicDatasets-python)
- [Python setuptool](https://pypi.python.org/pypi/setuptools)
