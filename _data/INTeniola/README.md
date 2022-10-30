# [INTeniola](https://github.com/INTeniola/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/pleiszenburg/status/1254786742126690306?s=20&t=ZySGFflwD_KVvETbXGriug
- https://twitter.com/higlass_io/status/1292947250935554049?s=20&t=ZySGFflwD_KVvETbXGriug
- https://twitter.com/BrockmannCon/status/1324293687812173824?s=20&t=ZySGFflwD_KVvETbXGriug
- https://twitter.com/_jhamman/status/1233416097388326912?s=20&t=QU2L5D7V8ShmZDoyiN-C-A
- https://twitter.com/Underfox3/status/1559312865269530626?s=20&t=7SqXNz15-lpcbDUOk4me8w
- https://twitter.com/carbonplanorg/status/1442559371775397889?s=20&t=7SqXNz15-lpcbDUOk4me8w

## Zarr Use Case
- The use of Zarr files to store and study 4D wind flow blocks - https://vortexfdc.com/zarr-the-new-format-to-store-and-study-4d-wind-flow-blocks/
- Continual extension of Zarr datasets - https://medium.com/pangeo/continuously-extending-zarr-datasets-c54fbad3967d
- Zarr architecture on climate data - https://dclimate.medium.com/introducing-zarrchitecture-on-dclimate-c12c0ad7e744
- Cloud-Performant NetCDF4/HDF5 Reading with the Zarr Library - https://medium.com/pangeo/cloud-performant-reading-of-netcdf4-hdf5-data-using-the-zarr-library-1a95c5c92314
- Building open source downscaling pipelines for the cloud - https://carbonplan.org/blog/cloud-downscaling-pipelines
## Zarr Data Visualisation

[pleiszenburg.de](https://youtu.be/RLHM5MQ5kAs) showing Earthquake Zarr Data

GitHub repo: https://github.com/pleiszenburg/earthquakes_youtube01

![image1](/_data/INTeniola/screenshots/EarthQuake.PNG)

[dClimate](https://dclimate.medium.com/introducing-zarrchitecture-on-dclimate-c12c0ad7e744) showing how Zarr makes it possible to work with large datasets that do not comfortably fit into memory

![image2](/_data/INTeniola/screenshots/Africa.png)



## Code Snippets

- Skin Layers Dermis and Epidermis - https://webknossos.org/datasets/scalable_minds/skin


Code 👇🏻

```
z = zarr.open_array(
    "https://data-humerus.webknossos.org/data/zarr/scalable_minds/skin/color/4-4-1"
)
print(z.shape)
plt.imshow(z[:, :, :, 0].T)

```

![image3](/_data/INTeniola/screenshots/skin.png)

```
src <- 'https://ncsa.osn.xsede.org/Pangeo/pangeo-forge/gcp-feedstock/gpcp.zarr'
reticulate::source_python("read_zarr.py")
x <- read_zarr(src, "precip", slice = 1L)[1L, , , drop = TRUE]
xs <- read_zarr_raw(src, "lon_bounds")
ys <- read_zarr_raw(src, "lat_bounds")
ex <- c(range(xs), range(ys))
ximage::ximage(x[nrow(x):1, ], extent = ex, col = hcl.colors(256))
maps::map("world2", add = TRUE)
```

![image4](/_data/INTeniola/screenshots/worldmap.PNG)

```
import { Map, Raster, Line } from '@carbonplan/maps'
import { useColormap } from '@carbonplan/colormaps'

const bucket = 'https://storage.googleapis.com/carbonplan-maps/'

const colormap = useColormap('warm')

<Map>
  <Line
    color={'white'}
    source={bucket + 'basemaps/land'}
    variable={'land'}
  />
  <Raster
    colormap={colormap}
    clim={[-20,30]}
    source={bucket + 'v1/demo/2d/tavg'}
    variable={'​​tavg'}
  />
</Map>
#The source data is a Zarr group with temperature data from WorldClim at multiple zoom levels. The file layout (with some files hidden for clarity) is as follows — note how the number of chunks quadruples as the zoom level increases. We automatically load metadata from files in this directory.

/
 ├── .zmetadata
 ├── 0
 │   ├── tavg
 │       └── 0.0
 ├── 1
 │   ├── tavg
 │       └── 0.0
 │       └── 0.1
 │       └── 1.0
 │       └── 1.1
 ├── 2
...
```

![image5](/_data/INTeniola/screenshots/carbon.PNG)
