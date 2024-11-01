---
layout: post
title: Download CMIP6 data from Google Cloud
comments: true
image_url: ''
tags: ['research']
---

This is just a post to locate resources I need. 🤔

As I am participating in the MDTF project, I need some sample climate data to test my POD. NOAA has a [Google Cloud repository](https://console.cloud.google.com/storage/browser/cmip6/) that stores CMIP6 data. To download the data, I need the [gsutil](https://cloud.google.com/storage/docs/gsutil_install#linux) installed on my linux machine. 

<!--more-->

I created the minimally necessary conda evironment by

```
conda create -n gcloud_cli python=3.11
```

I found a [slide deck about NOAA's public datasets](https://rammb.cira.colostate.edu/training/visit/links_and_tutorials/06_NOAA_Public_Datasets_on_Google_Cloud.pdf) online about how to read the data using Xarray:

```python
datasets = []
for file in lets_get:
    data_path = 'gs://' + bucket_name + '/' + file
    ds3 = xr.open_dataset(fs.open(data_path), engine='h5netcdf')
    datasets.append(ds3['TSkin'])
```

I downloaded two datasets

```
$ gsutil -m cp -r   "gs://cmip6/CMIP6/CMIP/CAMS/CAMS-CSM1-0/historical/r1i1p1f2/Amon/ta"   "gs://cmip6/CMIP6/CMIP/CAMS/CAMS-CSM1-0/historical/r1i1p1f2/Amon/ua"   "gs://cmip6/CMIP6/CMIP/CAMS/CAMS-CSM1-0/historical/r1i1p1f2/Amon/va"   .

$ gsutil -m cp -r   "gs://cmip6/CMIP6/C4MIP/E3SM-Project/E3SM-1-1/hist-bgc/r1i1p1f1/Amon/ta"   "gs://cmip6/CMIP6/C4MIP/E3SM-Project/E3SM-1-1/hist-bgc/r1i1p1f1/Amon/ua"   "gs://cmip6/CMIP6/C4MIP/E3SM-Project/E3SM-1-1/hist-bgc/r1i1p1f1/Amon/va"   .
```

Will give a try and report my findings here.

---

Additional information:

- [CMIP6 data request](https://clipc-services.ceda.ac.uk/dreq/mipVars.html)
- [IPCC Standard Output from Coupled Ocean-Atmosphere GCMs](https://pcmdi.llnl.gov/mips/cmip3/variableList.html#Table_A1a)
