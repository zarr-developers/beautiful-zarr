# [Zobbs-git](https://github.com/Zobbs-git/)'s Zarr Collection

## Tweet Mention

- https://twitter.com/googleai/status/1573064656914190337?s=46&t=4l8zNcgTfDZmjuo83Vm7Tg

## Zarr Data Visualization

![image1](/_data/zobbs-git/screenshots/visual.gif)
          
## Zarr Use Case

- TensorStore for High-Performance, Scalable Array Storage - with the aid of Zarr - https://ai.googleblog.com/2022/09/tensorstore-for-high-performance.html?m=1


## Code Snippets

- TensorStore for High-Performance, Scalable Array Storage - with the aid of Zarr - https://ai.googleblog.com/2022/09/tensorstore-for-high-performance.html?m=1

Code 👇

```
>>> import tensorstore as ts
>>> import numpy as np
>>> # Create a zarr array on the local filesystem
>>> dataset = ts.open({
...     'driver': 'zarr',
...     'kvstore': 'file:///tmp/my_dataset/',
... },
... dtype=ts.uint32,
... chunk_layout=ts.ChunkLayout(chunk_shape=[256, 256, 1]),
... create=True,
... shape=[5000, 6000, 7000]).result()
>>> # Create two numpy arrays with example data to write.
>>> a = np.arange(100*200*300, dtype=np.uint32).reshape((100, 200, 300))
>>> b = np.arange(200*300*400, dtype=np.uint32).reshape((200, 300, 400))
>>> # Initiate two asynchronous writes, to be performed concurrently.
>>> future_a = dataset[1000:1100, 2000:2200, 3000:3300].write(a)
>>> future_b = dataset[3000:3200, 4000:4300, 5000:5400].write(b)
>>> # Wait for the asynchronous writes to complete
>>> future_a.result()
>>> future_b.result()
```

![image2](/_data/zobbs-git/screenshots/result.jpg)