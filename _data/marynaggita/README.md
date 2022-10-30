# [marynaggita](https://github.com/marynaggita/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/latlong_blog/status/1543726888908009472
- https://twitter.com/_jhamman/status/1579882167303168004
- https://twitter.com/ErikvanSebille/status/1578030942144188417

## Zarr Use Cases
- Day's worth of analysis files for a single variable and combine them using high-level APIs.(https://mesowest.utah.edu/html/hrrr/zarr_documentation/html/xarray_one_day_analysis_example.html)

- Analyzing MOM6 with a python/xarray/dask/zarr software stack (https://www.cesm.ucar.edu/events/2020/MOM6/files/2020-MOM6-Dussin.pdf)

- Test ZARR performance for CAB-LAB data cubes (https://notebook.community/CAB-LAB/cablab-cubeio/notebooks/zarr_cube)

- Beyond netCDF: Cloud Native Climate Data with Zarr and XArray(https://ui.adsabs.harvard.edu/abs/2018AGUFMIN33A..06A/abstract)

## Zarr Data Visualisation
- Boundaries of active fires (red outlines) estimated using VIIRS 375-m thermal anomalies, and smoke from wildfires in the Pacific Northwest on 9 Sep 2020 (https://worldview.earthdata.nasa.gov/)

<img src="https://github.com/marynaggita/beautiful-zarr/blob/marynaggita/_data/marynaggita/Screenshots/data4.jpg" width="400" height="400" display="block" margin= "0 auto">


- Use Case: Study Amazon Estuaries with Data from the EOSDIS Cloud(https://github.com/podaac/tutorials/blob/master/notebooks/SWOT-EA-2021/Estuary_explore_inCloud_zarr.ipynb)

<img src="https://github.com/marynaggita/beautiful-zarr/blob/marynaggita/_data/marynaggita/Screenshots/data1.PNG" width="400" height="400" display="block" margin= "0 auto">


## Code Snippets



Code 👇🏻

```
import s3fs
import xarray as xr

fs = s3fs.S3FileSystem(anon=True)
mapper = fs.get_mapper("s3://cmip6-pds/CMIP6/CMIP/AS-RCEC/TaiESM1/1pctCO2/r1i1p1f1/Amon/hfls/gn/v20200225/")
ds = xr.open_zarr(mapper, consolidated=True, decode_times=False)
print(list(ds.keys()))
z = ds["hfls"]
print(z.shape)
plt.imshow(z[200], origin="lower", cmap="hot")
```
![image3](https://github.com/marynaggita/beautiful-zarr/blob/marynaggita/_data/marynaggita/Screenshots/data6.PNG)

```
plt.imshow(z[10])
```
![image3](https://github.com/marynaggita/beautiful-zarr/blob/marynaggita/_data/marynaggita/Screenshots/data7.PNG)



