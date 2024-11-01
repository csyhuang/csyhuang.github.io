---
layout: post
title: Download CMIP6 data from Google Cloud
comments: true
image_url: ''
tags: ['research']
---

This is just a post to locate resources I need. 🤔

As I am participating in the MDTF project, I need some sample climate data to test my POD. NOAA has a [Google Cloud repository](https://console.cloud.google.com/storage/browser/cmip6/) that stores CMIP6 data. To download the data, I need the [gsutil](https://cloud.google.com/storage/docs/gsutil_install#linux) installed on my linux machine.

I found a [slide deck about NOAA's public datasets](https://rammb.cira.colostate.edu/training/visit/links_and_tutorials/06_NOAA_Public_Datasets_on_Google_Cloud.pdf) online about how to read the data using Xarray:

```python
datasets = []
for file in lets_get:
	data_path = 'gs://' + bucket_name + '/' + file
	ds3 = xr.open_dataset(fs.open(data_path), engine='h5netcdf')
	# Reduce data set to Skin tempera
```
