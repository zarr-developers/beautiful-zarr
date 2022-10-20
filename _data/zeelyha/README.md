# [Zeelyha](https://github.com/zeelyha/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/azavea/status/1573036727169871872?s=20&t=QOQltG_i3uB5TEJX1j60WA

- https://twitter.com/IT4Innovations/status/1573183377812889600?s=20&t=QOQltG_i3uB5TEJX1j60WA

- https://twitter.com/al_merose/status/1582792311963934720?s=20&t=QOQltG_i3uB5TEJX1j60WA

## Zarr Use Case

- Transfer Data from NetCDF on Hierarchical Storage to Zarr on Object Storage - https://presentations.copernicus.org/EGU21/EGU21-2442_presentation.pdf



## Code Snippets and Visualisation

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