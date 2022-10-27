# [Anelawrence](https://github.com/Anelawrence/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/geosolutions_it/status/1516167576581255168

- https://twitter.com/PyDataDelhi/status/1528047388627456000

- https://twitter.com/opengeospatial/status/1415692393068867595

- https://twitter.com/Ulrike_Boehm/status/1526977063067983872

## Zarr Use Cases

- Cloud-Performant NetCDF4/HDF5 Reading with the Zarr Library - https://medium.com/pangeo/cloud-performant-reading-of-netcdf4-hdf5-data-using-the-zarr-library-1a95c5c92314

- Benchmarking Zarr and Parquet Data Retrieval using the National Water Model (NWM) in a Cloud-native environment - https://www.azavea.com/blog/2022/09/22/benchmarking-zarr-and-parquet-data-retrieval-using-the-national-water-model-nwm-in-a-cloud-native-environment/

- Publishing Xarray Datasets via a Zarr compatible REST API - https://medium.com/pangeo/xpublish-ff788f900bbf

## Zarr Data Visualization

[HRRR as Zarr on AWS #2 ](https://github.com/blaylockbk/Herbie/issues/2)

![image1](\_data\Anelawrence\screenshots\image2.png)

## Code Snippets

- Bio-image Analysis Notebook - https://haesleinhuepf.github.io/BioImageAnalysisNotebooks/32_tiled_image_processing/tiled_area_mapping.html

Code 👇🏻
```
import zarr
import dask.array as da
import numpy as np
from skimage.io import imread
import pyclesperanto_prototype as cle
from pyclesperanto_prototype import imshow
from numcodecs import Blosc

image = imread('../../data/P1_H_C3H_M004_17-cropped.tif')[1]

# for testing purposes, we crop the image even more.
# comment out the following line to run on the whole 5000x2000 pixels
image = image[1000:1500, 1000:1500]

#compress AND change the numpy array into a zarr array
compressor = Blosc(cname='zstd', clevel=3, shuffle=Blosc.BITSHUFFLE)

# Convert image into zarr array
chunk_size = (100, 100)
zarray = zarr.array(image, chunks=chunk_size, compressor=compressor)

# save zarr to disk
zarr_filename = '../../data/P1_H_C3H_M004_17-cropped.zarr'
zarr.convenience.save(zarr_filename, zarray)

def area_map(image):
    """
    Label objects in a binary image and produce a pixel-count-map image.
    """
    print("Processing image of size", image.shape)
    
    labels = cle.voronoi_otsu_labeling(image, spot_sigma=3.5)
    result = cle.pixel_count_map(labels)
    
    print(result.shape)
    
    return np.asarray(result)

test_image = image[100:200,100:200]

imshow(test_image)

test_result = area_map(test_image)

imshow(test_result, colorbar=True)

```

![image2](\_data\Anelawrence\screenshots\image1.png)
