# [Azaya](https://github.com/Azaya89)'s Zarr collection.

## Twitter Mentions

- https://twitter.com/rajadain/status/1574097645806682121

- https://twitter.com/latlong_blog/status/1543726888908009472?s=20&t=-ydMJPjiK47szWDjB7EIZw

- https://twitter.com/ruth_mottram/status/1580130892692918272?s=20&t=-ydMJPjiK47szWDjB7EIZw

-----

## Youtube mentions

- https://www.youtube.com/watch?v=RLHM5MQ5kAs

- https://www.youtube.com/watch?v=0bqpxX3Nn_A

- https://www.youtube.com/watch?v=oWltJsPlIaQ&t=173s

----

## Screenshots

- ![Whitehole Package](beautiful-zarr\_data\azaya89\zarr_screenshot.jpeg)


## Code Snippets

- https://github.com/valeriupredoi/PyActiveStorage/blob/96c2305678dcdf0ffff7ae84f8c358a42ec15f46/old_code/PCI_wrapper.py

        import os
        import zarr

        from kerchunk.hdf import SingleHdf5ToZarr


        ### example valid netCDF4 file
        ### can be loaded through h5py
        cmip6_test_file = r"/home/valeriu/climate_data/CMIP6/CMIP/MPI-M/MPI-ESM1-2-HR/historical/r1i1p1f1/Omon/sos/gn/v20190710/sos_Omon_MPI-ESM1-2-HR_historical_r1i1p1f1_gn_198001-198412.nc"

        def build_zarr_dataset():
            """Create a zarr array and save it."""
            dsize = (60, 404, 802)
            dchunks = (12, 80, 160)
            dvalue = 42.
            store = zarr.DirectoryStore('data/array.zarr')
            z = zarr.zeros(dsize, chunks=dchunks, store=store, overwrite=True)
            z[...] = dvalue
            zarr.save_array("example.zarr", z, compressor=None)


        def load_zarr_native():
            """Load the Zarr file we have built, actual/native format."""
            zarr_dir = "./example.zarr"
            if not os.path.isdir(zarr_dir):
                build_zarr_dataset()
            ds = zarr.open("./example.zarr")

            return ds


- https://github.com/NHPatterson/napari-imsmicrolink/blob/1c582fc131f12d1b0efa2726247e625f87b735cd/napari_imsmicrolink/utils/image.py#L1

        from typing import Tuple, List
        from pathlib import Path
        import numpy as np
        import zarr
        import dask.array as da
        import SimpleITK as sitk
        from tifffile import TiffFile, imread, xml2dict


        def tifffile_to_dask(im_fp: [str, Path], largest_series: int):
            imdata = zarr.open(imread(im_fp, aszarr=True, series=largest_series))
            if isinstance(imdata, zarr.hierarchy.Group):
                imdata = [da.from_zarr(imdata[z]) for z in imdata.array_keys()]
            else:
                imdata = da.from_zarr(imdata)
            return imdata


## Zarr Data Visualisation

- https://nbviewer.org/gist/rsignell-usgs/4edfff890a7a18f97eaef42d647ec534

![Zarr visualisation](beautiful-zarr\_data\azaya89\zarr_visua.png)