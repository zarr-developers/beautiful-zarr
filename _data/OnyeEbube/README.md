# OnyeEbube's Zarr Collection

## Zarr Use Cases

- There was a study to provide guidance on how to transfer existing NetCDF data from a hierarchical storage system into Zarr to an object storage system by the German Climate Computing Center (DKRZ) in the year 2021. [This is a link to the study](https://presentations.copernicus.org/EGU21/EGU21-2442_presentation.pdf)
- In April 2022, a research was made by using cloud computing to analyze model output archived in Zarr format. This research was possible by harnessing the flexibility of zarr because Zarr is designed to be accessed with open-source software either using cloud or local computing resources.[A link to the study can be found here](https://journals.ametsoc.org/view/journals/atot/39/4/JTECH-D-21-0106.1.xml) 
- It is possible to use Zarr to plot data from the created array The code snippet and visualization is shown below
## Code Snippets
This code was gotten from https://mesowest.utah.edu/html/hrrr/zarr_documentation/html/zarr_HowToDownload.html and is used as an example to show zarr visualization properties

~~~
import s3fs
import zarr
url = "s3://hrrrzarr"
fs = s3fs.S3FileSystem(anon=True)
store = zarr.open(s3fs.S3Map(url, s3=fs))
~~~

~~~
temperature = store["sfc/20200801/20200801_00z_anl.zarr/1000mb/TMP/1000mb/TMP"]
index = store["grid/HRRR_chunk_index.zarr"]
~~~

~~~
import cartopy.crs as ccrs
import matplotlib.pyplot as plt

ax = plt.axes(projection=ccrs.Mercator())
ax.contourf(index["longitude"], index["latitude"], temperature, transform=ccrs.PlateCarree())
ax.coastlines()
plt.show()
~~~

Tech bloggers are making their audience aware of ways to maximize Zarr. One of them is Richard Pelgrim who wrote a blog with the topic "Why You Should Save NumPy Arrays with Zarr". He shared with his audience the following code snippets
### Creating a dummy numpy array

~~~
import numpy as np
array_XS = np.random.rand(3,2)</code><br>
array_l = np.random.rand(1000, 1000, 100)
~~~
### Storing the 3-dimensional array_L as .txt or .csv will throw a ValueError

~~~
np.savetxt('array_L.txt', array_L, delimiter=" ")
np.savetxt('array_L.csv', array_L, delimiter=",")
ValueError: Expected 1D or 2D array, got 3D array instead
~~~
A link to the full blog [is here](https://towardsdatascience.com/why-you-should-save-numpy-arrays-with-zarr-dabff4ae6c0c)
## Zarr Visualizations

![image](https://user-images.githubusercontent.com/101593852/197403790-96adf693-0530-4430-bb1d-26374c6d2b3d.png)

The data represented above is the result of the code snippet [here](https://github.com/OnyeEbube/beautiful-zarr/blob/main/_data/OnyeEbube/README.md#code-snippets), referenced from https://mesowest.utah.edu/html/hrrr/zarr_documentation/html/zarr_HowToDownload.html

## Twitter Mentions
Mini-bloggers on twitter are sharing the story of Zarr to their audiences<br>
- This tweet thread by **Terence Tuhinanshu** is an example of Zarr's chunking capacity with large volumes of multiple dimensions stored at once - https://twitter.com/rajadain/status/1574097638747537408?s=20&t=WpcGsL7fcTZ3eWtlRJl_gw
  
- **LatxLong** endorses Zarr 2.0 specifications as community standard - https://twitter.com/latlong_blog/status/1543726888908009472?s=20&t=WpcGsL7fcTZ3eWtlRJl_gw
  
- Openmicroscopy defining a joint specification with OMENGFF and anndata which can be stored in a single Zarr fileset - https://twitter.com/openmicroscopy/status/1582413612433346571?s=20&t=WpcGsL7fcTZ3eWtlRJl_gw
