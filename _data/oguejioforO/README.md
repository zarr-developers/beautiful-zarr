# [oguejioforO](https://github.com/oguejioforO/beautiful-zarr.git) beautiful-Zarr collection

## Tweet Mentions

•   https://twitter.com/opencholmes/status/1583130100756643845/photo/1

•	https://twitter.com/notjustmoore/status/1256232842755014656

•	https://twitter.com/LLC4320Bot/status/1274862822778982402

•	https://twitter.com/azavea/status/1573036727169871872

•	https://twitter.com/rabernat/status/1573103573373943810

•	https://twitter.com/maximlamare/status/1430805616273108995


## Zarr use cases

## Continuously extending Zarr datasets:https://medium.com/pangeo/continuously-extending-zarr-datasets-c54fbad3967d


## code snippets

Code 👇🏻


```python
dt = datetime(2000, 3, 1, 12)
filenames, datetimes = download_files(dt, 40)
ds = create_dataset(filenames, datetimes)
print(ds)

<xarray.Dataset>
Dimensions:        (lat: 480, lon: 1440, time: 40)
Coordinates:
  * lat            (lat) float64 59.88 59.62 59.38 ... -59.38 -59.62 -59.88
  * lon            (lon) float64 0.125 0.375 0.625 0.875 ... 359.4 359.6 359.9
  * time           (time) datetime64[ns] 2000-03-01T12:00:00 ... 2000-03-06T09:00:00
Data variables:
    precipitation  (time, lat, lon) float32 0.0 0.0 0.0 0.0 ... 0.0 0.0 0.0 0.0
    
    import matplotlib.pyplot as plt

ds.precipitation.sum(['time']).plot(vmax=50)
plt.title('Accumulated precipitation between\n'
          '2000-03-01T12 and 2000-03-06T09 (mm)')
plt.show()
```

![image 1](https://user-images.githubusercontent.com/115434947/197654092-4345ce20-15cd-4b6a-9028-b4375890f6db.png)







