# [Mubaraq](https://github.com/Mubarraqqq/)'s Zarr Collection


## Zarr Use Case
- Demonstration of how we may use Zarr to support streaming data from simulation models to analysis environment - [Click here](https://medium.com/pangeo/streaming-zarr-ccf0d518b1c0)
- Using Zarr files to store and study 4D wind flow BLOCKS - [Click here](https://vortexfdc.com/zarr-the-new-format-to-store-and-study-4d-wind-flow-blocks/)
- Preparing versions of NWM Reanalysis dataset in Zarr format - [Click here](https://github.com/azavea/noaa-hydro-data/blob/master/src/esip-2022-presentation/save_zarr_data.ipynb)
- Using Zarr to access cloud storage just like your local file system - [Click here](https://measurespace.medium.com/use-zarr-to-access-clound-storage-just-like-your-local-file-system-d67607cb128b)


## Tweet Mentions

- https://twitter.com/rajadain/status/1574097641469841410
- https://twitter.com/notjustmoore/status/1579536653852766208
- https://twitter.com/azavea/status/1573036727169871872
- https://twitter.com/notjustmoore/status/1256232842755014656


## Zarr Data Visualisation

#### 30GB [microscopic image](https://twitter.com/notjustmoore/status/1256232842755014656) of covid19 newly published and converted to zarr stored in Embassy S3 and viewed with an [openmicroscopy plugin](https://www.openmicroscopy.org/bio-formats/) :

![image1](/_data/Mubarraqqq/images/Microscopy1.jpg)

![image2](/_data/Mubarraqqq/images/Microscopy2.jpg)


#### Timing streamflow queries using Zarr :

![image3](/_data/Mubarraqqq/images/TimingZarr.png)


#### Comparing Zarr and Parquet for streamflow queries :

![image4](/_data/Mubarraqqq/images/ComparingPandZ.png)

## Code Snippets

- Using [Vizarr](https://hms-dbmi.github.io/vizarr), a minimal purely client-side program to view Zarr-based images on Jupyter notebook.

![image5](/_data/Mubarraqqq/images/vizarr.png)


- Storing images in chunks using Zarr

Code 👇🏻

```
import zarr
import dask.array as da
import numpy as np
from skimage.io import imread
import pyclesperanto_prototype as cle
from pyclesperanto_prototype import imshow
from numcodecs import Blosc
```
For demonstration purposes, we use a dataset that is provided by Theresa Suckert, OncoRay, University Hospital Carl Gustav Carus, TU Dresden.
We are using a cropped version here that was resaved a 8-bit image to be able to provide it with the notebook. 
You find the full size 16-bit image in CZI file format [online](https://zenodo.org/record/4276076#.YX1F-55BxaQ).

```
image = imread('../../data/P1_H_C3H_M004_17-cropped.tif')[1]

# for testing purposes, we crop the image even more.
# comment out the following line to run on the whole 5000x2000 pixels
image = image[1000:1500, 1000:1500]
```
Now we will resave our image to the zarr format.

```
#compress AND change the numpy array into a zarr array
compressor = Blosc(cname='zstd', clevel=3, shuffle=Blosc.BITSHUFFLE)

chunk_size = (100, 100)

zarray = zarr.array(image, chunks=chunk_size, compressor=compressor)
```


```
zarr_filename = '../../data/P1_H_C3H_M004_17-cropped.zarr'
zarr.convenience.save(zarr_filename, zarray)
```
Visualizing the image

```
zarr_result = da.from_zarr(zarr_filename)

result = zarr_result.compute()

cle.imshow(result)
```
![image6](/_data/Mubarraqqq/images/bioimaging.png)


