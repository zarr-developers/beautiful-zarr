Weskeyz's Zarr collection

Tweet mentions:
https://twitter.com/pleiszenburg/status/1197567468845309952?s=20&t=pVprlZ-mbMk0cOkxOplfLw
https://twitter.com/miguelmahechag1/status/1561038886062751746?s=20&t=pVprlZ-mbMk0cOkxOplfLw

Zarr Data visualization
![Opera Snapshot_2022-10-21_101658_twitter com](https://user-images.githubusercontent.com/94924532/197161914-c01c3048-c015-4692-a1c5-a6c1c5f3064f.png)

Code snippets
Zarr API For Accessing HRRR Data (University of Utah) - https://mesowest.utah.edu/html/hrrr/zarr_documentation/html/zarr_api_multiple_hrrr_variables.html

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
image
