# [Anelawrence](https://github.com/Anelawrence/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/geosolutions_it/status/1516167576581255168
- https://twitter.com/PyDataDelhi/status/1528047388627456000
- https://twitter.com/opengeospatial/status/1415692393068867595
- https://twitter.com/Ulrike_Boehm/status/1526977063067983872
- https://twitter.com/Julien_Lau/status/1207352060053741569
- https://twitter.com/edhartnett/status/1204841202459144197
- https://twitter.com/NumFOCUS/status/1195039103441616896
- https://twitter.com/NumFOCUS/status/1195031794623664129
- https://twitter.com/loicaroyer/status/1194742247079546881
- https://twitter.com/_jhamman/status/1182352773813362688
- https://twitter.com/Kimberl64788230/status/1174706061879603201
- https://twitter.com/TobiasAdeJong/status/1154746138869846016
- https://twitter.com/eramirem/status/1149039195740131329
- https://twitter.com/shoyer/status/1145861508460498944
- https://twitter.com/_jhamman/status/1140704257961652224
- https://twitter.com/JuliusBusecke/status/1129204320518451200
- https://twitter.com/_jhamman/status/1072984431814684673
- https://twitter.com/andreazonca/status/979282994816012288
- https://twitter.com/ijstokes/status/900809689025437697
- https://twitter.com/FrancescAlted/status/775765627826950144
- https://twitter.com/FrancescAlted/status/720993747039830016
- https://twitter.com/cschrader/status/1574417198495449092
- https://twitter.com/azavea/status/1574409561024380930
- https://twitter.com/rajadain/status/1574097650047229954
- https://twitter.com/PhilippBauman15/status/1572981355180433408
- https://twitter.com/INCForg/status/1570064709306679298
- https://twitter.com/INCForg/status/1570060307044929536
- https://twitter.com/ironArray/status/1567577711006056455
- https://twitter.com/MiguelDMahecha/status/1561038886062751746
- https://twitter.com/MiguelDMahecha/status/1560745724010065921
- https://twitter.com/EuroBioImaging/status/1556973266320982017
- https://twitter.com/ironArray/status/1523998598375030785
- https://twitter.com/giswqs/status/1516419028004265998
- https://twitter.com/GlencoeSoftware/status/1514543991475240963
- https://twitter.com/clintcabanero/status/1511350429271085062
- https://twitter.com/GlencoeSoftware/status/1507385839898218500
- https://twitter.com/alexanderrey42/status/1506242447629762565
- https://twitter.com/OurRadiantEarth/status/1498354591670411268
- https://twitter.com/mdsumner/status/1496974485614137344
- https://twitter.com/OurRadiantEarth/status/1496518731467476992
- https://twitter.com/GlencoeSoftware/status/1491391571786350594
- https://twitter.com/jimmyc42/status/1483828050152157185

## Zarr Use Cases

- Cloud-Performant NetCDF4/HDF5 Reading with the Zarr Library - https://medium.com/pangeo/cloud-performant-reading-of-netcdf4-hdf5-data-using-the-zarr-library-1a95c5c92314

- Benchmarking Zarr and Parquet Data Retrieval using the National Water Model (NWM) in a Cloud-native environment - https://www.azavea.com/blog/2022/09/22/benchmarking-zarr-and-parquet-data-retrieval-using-the-national-water-model-nwm-in-a-cloud-native-environment/

- Publishing Xarray Datasets via a Zarr compatible REST API - https://medium.com/pangeo/xpublish-ff788f900bbf

## Zarr Data Visualization

[HRRR as Zarr on AWS #2 ](https://github.com/blaylockbk/Herbie/issues/2)

![image1](/_data/Anelawrence/screenshots/image2.png)

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


![image2](/_data/Anelawrence/screenshots/image1.png)
