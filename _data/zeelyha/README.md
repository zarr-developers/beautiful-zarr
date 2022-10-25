# [Zeelyha](https://github.com/zeelyha/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/azavea/status/1573036727169871872?s=20&t=QOQltG_i3uB5TEJX1j60WA

- https://twitter.com/IT4Innovations/status/1573183377812889600?s=20&t=QOQltG_i3uB5TEJX1j60WA

- https://twitter.com/al_merose/status/1582792311963934720?s=20&t=QOQltG_i3uB5TEJX1j60WA

- https://twitter.com/herrsaalfeld/status/1176682762616655872?s=20&t=oGV4_pUwjJ7anFv6o3hibQ

- https://twitter.com/VortexFdC/status/1434952297126170624?s=20&t=oGV4_pUwjJ7anFv6o3hibQ

- https://twitter.com/ngehlenborg/status/1369316956956995588?s=20&t=oGV4_pUwjJ7anFv6o3hibQ

## Zarr Use Cases

- Transfer Data from NetCDF on Hierarchical Storage to Zarr on Object Storage - https://presentations.copernicus.org/EGU21/EGU21-2442_presentation.pdf

- Using Cloud Computing to Analyze Model Output Archived in Zarr Format  - https://bit.ly/3TniVnZ

- Why You Should Save NUmpy Arrays With Zarr  - https://towardsdatascience.com/why-you-should-save-numpy-arrays-with-zarr-dabff4ae6c0c



## Code Snippets and Visualisations

- Zarr API For Accessing HRRR Data (University of Utah)  - https://mesowest.utah.edu/html/hrrr/zarr_documentation/html/zarr_api_multiple_hrrr_variables.html

Code 👇🏻

```
%%time
import s3fs
import zarr

url = "s3://hrrrzarr/sfc/20210601/20210601_00z_anl.zarr"
fs = s3fs.S3FileSystem(anon=True)
store = zarr.open(s3fs.S3Map(url, s3=fs))

thousand_mb_tmp = store["1000mb/TMP/1000mb/TMP"]
surface_tmp = store["surface/TMP/surface/TMP"]
hcdc = store["high_cloud_layer/HCDC/high_cloud_layer/HCDC"]
rh = store["2m_above_ground/RH/2m_above_ground/RH"]
spfh = store["2m_above_ground/SPFH/2m_above_ground/SPFH"]
dpt = store["2m_above_ground/DPT/2m_above_ground/DPT"]
pot = store["2m_above_ground/POT/2m_above_ground/POT"]
tmp = store["2m_above_ground/TMP/2m_above_ground/TMP"]

thousand_mb_tmp[...] # load into memory

import xarray as xr
chunk_index = xr.open_zarr(s3fs.S3Map("s3://hrrrzarr/grid/HRRR_chunk_index.zarr", s3=fs))
chunk_index

import matplotlib.pyplot as plt
import cartopy.crs as ccrs

ax = plt.axes(projection=ccrs.PlateCarree())
ax.contourf(chunk_index.longitude, chunk_index.latitude, thousand_mb_tmp)
ax.coastlines()

plt.show()
plt.close()
```

![image](/_data/zeelyha/screenshots/zarr_visual.png)



- Using Zarr Format on ERA5 Climate Girded Data (Digital earth Africa)  - https://docs.digitalearthafrica.org/en/latest/sandbox/notebooks/Datasets/Climate_Data_ERA5_AWS.html

Code 👇🏻
```
matplotlib inline
from matplotlib import pyplot as plt
import numpy as np
import datacube
import s3fs

from deafrica_tools.load_era5 import load_era5

# set AWS region to access ERA5 data
s3 = s3fs.S3FileSystem(anon=True, client_kwargs={'region_name':'us-east-1'})
# data is structred as era5-pds/zarr/<year>/<month>/data/
# available measurements should be consistent across time, so use 2021/04 to check
# to confirm for a different time period, replace the year and month values

s3.ls('era5-pds/zarr/2021/04/data/')

# Lake Turkana Wind Power Station, Kenya
lat = (2.45, 2.55)
lon = (36.75, 36.85)

# Define the time window
time = '2021-01', '2021-03'

var = 'precipitation_amount_1hour_Accumulation'

precip = load_era5(var, lat, lon, time, reduce_func=np.sum, resample='1D').compute()
# convert to Millimeters (mm), keeping other attributes
attrs = precip[var].attrs
attrs['units']='mm'
precip = precip*1000
precip[var].attrs = attrs
# plot daily total precipitation for this area
precip[var].sum(['lat','lon']).plot(figsize = (16,4), marker='o', markersize=4, linewidth=0);
plt.xlabel('Day');
plt.ylabel('%s (%s)'%('Total Precipitation', precip[var].attrs['units']));

```


![image](/_data/zeelyha/screenshots/zarr_era5.png)