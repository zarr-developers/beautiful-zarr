# Weddy Gikunda's Zarr Collection

## Tweet Mentions

- https://twitter.com/ngehlenborg/status/1369316956956995588?t=K17ZS89IgKxzUYzLdJixBQ&s=19
- https://twitter.com/Ulrike_Boehm/status/1526977063067983872?t=3-8y0hcb7YaRB7mP3FNZbQ&s=19
- https://twitter.com/pleiszenburg/status/1197567468845309952?t=WOb9XvM9QhS6enJaORPYrg&s=19

## Zarr use cases

- [Overview of Zarr support in netCDF-C](https://www.unidata.ucar.edu/blogs/developer/en/entry/overview-of-zarr-support-in)
- [Creating dask array from Zarr files](https://www.coiled.io/blog/creating-dask-array-from-zarr)
- [The use of Zarr files to store and study 4D wind flow BLOCKS](https://vortexfdc.com/zarr-the-new-format-to-store-and-study-4d-wind-flow-blocks/)
- [Compress Zarr meteo data for cheap blob storage](https://the-fonz.gitlab.io/posts/compress-zarr-meteo/)
- [HDF in the cloud](https://matthewrocklin.com/blog/work/2018/02/06/hdf-in-the-cloud)

## Zarr Data Visualisation

[Here's an interactive visualisatin of 728 GB of ERA5 temperature data curated
by Pangeo stored in Zarr using
Neuroglancer](http://neuroglancer-demo.appspot.com/#!%7B%22dimensions%22:%7B%22d0%22:%5B1%2C%22%22%5D%2C%22d1%22:%5B1%2C%22%22%5D%2C%22d2%22:%5B1%2C%22%22%5D%7D%2C%22position%22:%5B175320.5%2C360.5%2C720.5%5D%2C%22crossSectionOrientation%22:%5B-0.5%2C-0.5%2C-0.5%2C0.5%5D%2C%22crossSectionScale%22:1%2C%22projectionScale%22:524288%2C%22layers%22:%5B%7B%22type%22:%22image%22%2C%22source%22:%7B%22url%22:%22zarr://gs://pangeo-era5/reanalysis/spatial-analysis/t2m%22%2C%22subsources%22:%7B%22default%22:true%2C%22bounds%22:true%7D%2C%22enableDefaultSubsources%22:false%7D%2C%22tab%22:%22source%22%2C%22shader%22:%22#uicontrol%20float%20minValue%20slider%28min=200.0%2C%20max=350.0%2C%20default=200.0%29%5Cn#uicontrol%20float%20maxValue%20slider%28min=200.0%2C%20max=350.0%2C%20default=320.0%29%5Cn%5Cnvec3%20colormapTurbo%28float%20x%29%20%7B%5Cn%20%20vec3%20result%3B%5Cn%20%20x%20=%20clamp%28x%2C%200.0%2C%201.0%29%3B%5Cn%20%20result.r%20=%200.24+x%2A%28-3.45+x%2A%2899.45+x%2A%28-877.16+x%2A%283444.49+x%2A%28-6981.22+x%2A%287681.98+x%2A%28-4381.31+x%2A1017.48%29%29%29%29%29%29%29%3B%5Cn%20%20result.g%20=%200.06+x%2A%283.34+x%2A%28-10.53+x%2A%2879.3+x%2A%28-311.43+x%2A%28637.65+x%2A%28-752.42+x%2A%28482.1+x%2A-128.07%29%29%29%29%29%29%29%3B%5Cn%20%20result.b%20=%200.21+x%2A%288.3+x%2A%28-34.61+x%2A%28173.05+x%2A%28-970.18+x%2A%282775.48+x%2A%28-3940.03+x%2A%282719.41+x%2A-731.65%29%29%29%29%29%29%29%3B%5Cn%20%20return%20clamp%28result%2C%200.0%2C%201.0%29%3B%5Cn%7D%5Cn%5Cnvoid%20main%28%29%20%7B%5Cn%20%20float%20scale_factor%20=%200.0016946343655403098%3B%5Cn%20%20float%20add_offset%20=%20265.9707255587938%3B%5Cn%20%20float%20x%20=%20scale_factor%20%2A%20float%28toRaw%28getDataValue%28%29%29%29%20+%20add_offset%3B%5Cn%20%20float%20v%20=%20%28x%20-%20minValue%29%20/%20%28maxValue%20-%20minValue%29%3B%5Cn%20%20emitRGB%28colormapTurbo%28v%29%29%3B%5Cn%7D%5Cn%22%2C%22shaderControls%22:%7B%22minValue%22:237.5%2C%22maxValue%22:308%7D%2C%22name%22:%22t2m%22%7D%5D%2C%22selectedLayer%22:%7B%22size%22:290%2C%22visible%22:true%2C%22layer%22:%22t2m%22%7D%2C%22layout%22:%224panel%22%7D)
![screenshot](/_data/caviere/images/screenshot_1.png)

## Code Snippets

### CMIP6

Code snippet from
[CMIP6 data](https://www.r-bloggers.com/2022/09/reading-zarr-files-with-r-package-stars/):

```python
import fsspec
import xarray
import zarr
m = fsspec.get_mapper("https://storage.googleapis.com/cmip6/CMIP6/HighResMIP/CMCC/CMCC-CM2-HR4/highresSST-present/r1i1p1f1/6hrPlev/psl/gn/v20170706")
ds = xarray.open_zarr(m)
ds.sel(time=slice("1948-01-01","1955-12-31")).to_zarr("./psl.zarr")
```

![screenshot](/_data/caviere/images/screenshot_2.png)

### Carbonplan

Code snippet from
[Carbonplan](https://carbonplan.org/blog/maps-library-release):

```python
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
<Raster
  colormap={colormap}
  clim={clim}
  source={bucket + 'v1/demo/4d/tavg-prec-month'}
  variable={'climate'}
  selector={{ band, month }}
/>
```

![screenshot](/_data/caviere/images/screenshot_3.png)

### Cubed

[Cubed](https://github.com/tomwhite/cubed) is a distributed N-dimensional array
library implemented in Python using bounded-memory serverless processing and
Zarr for storage. Code snippet from the Cubed project:

```python
import numpy as np
import zarr
from dask.array.core import common_blockdim, normalize_chunks
from dask.array.utils import validate_axis
from dask.blockwise import broadcast_dimensions
from dask.utils import has_keyword
from tlz import concat, partition
from toolz import accumulate, map
from zarr.indexing import BasicIndexer, IntDimIndexer, is_slice, replace_ellipsis

from cubed.core.array import CoreArray, check_array_specs, compute, gensym
from cubed.core.plan import Plan, new_temp_store
from cubed.primitive.blockwise import blockwise as primitive_blockwise
from cubed.primitive.rechunk import rechunk as primitive_rechunk
from cubed.utils import chunk_memory, get_item, to_chunksize

if TYPE_CHECKING:
    from cubed.array_api.array_object import Array


def from_array(x, chunks="auto", asarray=None, spec=None) -> "Array":
    """Create a Cubed array from an array-like object."""

    if isinstance(x, CoreArray):
        raise ValueError(
            "Array is already a Cubed array. Use 'asarray' or 'rechunk' instead."
        )

    previous_chunks = getattr(x, "chunks", None)
    outchunks = normalize_chunks(
        chunks, x.shape, dtype=x.dtype, previous_chunks=previous_chunks
    )

    if isinstance(x, zarr.Array):  # zarr fast path
        from cubed.array_api import Array

        name = gensym()
        target = x
        plan = Plan._new(name, "from_array", target)
        arr = Array(name, target, spec, plan)

        chunksize = to_chunksize(outchunks)
        if chunks != "auto" and previous_chunks != chunksize:
            arr = rechunk(arr, chunksize)
        return arr
```
