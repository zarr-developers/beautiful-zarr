# [Steph237](https://github.com/steph237/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/tswetnam/status/1565708621303885824?s=20&t=CVDZBpbgEyEnUm9BLXboxg
- https://twitter.com/latlong_blog/status/1543726888908009472?s=20&t=CVDZBpbgEyEnUm9BLXboxg
- https://twitter.com/GdalOrg/status/1417318103491416064?s=20&t=CVDZBpbgEyEnUm9BLXboxg

## Zarr Use Case

[Streaming with Zarr](https://medium.com/pangeo/streaming-zarr-ccf0d518b1c0) Exploring the use of Zarr to support streaming models to analysis environments



## Code Snippets

- Here we demonstrate how Zarr and Redis can be used together. - https://mybinder.org/v2/gh/jhamman/streaming-zarr/master?urlpath=lab

Code 👇🏻

```
store = zarr.RedisStore(port=args.port)
root = zarr.group(store=store, overwrite=True)
t = 0
while True:
    arr = root.zeros(f"{t}", shape=grid.shape, chunks=(25, 25))  # create a new array for this timestep
    arr[…] = grid # write data to zarr array
    
    t += 1  # increment the time counter
    time.sleep(update_interval)
    grid = update(grid, N)  # evolve the model one time step
```

![image2](/_data/steph237/screenshots/zarr_streaming.gif)