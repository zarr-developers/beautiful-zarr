# [MSanKeys](https://github.com/MSanKeys963/)'s Zarr Use Cases

## Tweets

- https://twitter.com/LLC4320Bot/status/1274862822778982402
- https://twitter.com/trevmanz/status/1265377097981329423

## Zarr Data

![image1](/assets/images/zarr_data_2.jpg)

## Code Snippets

- NASA Prediction of Worldwide Energy Resources (POWER) - https://registry.opendata.aws/nasa-power

Code 👇🏻

```
import zarr
import matplotlib as plt

g = zarr.open_group(
    "s3://power-analysis-ready-datastore/power_901_monthly_meteorology_utc.zarr",
    storage_options={"anon": True}
)
print(list(g.keys()))
z = g["EVLAND"]
print(z.shape)
plt.imshow(z[200], origin="lower", cmap="hot")
```

![image2](/_data/MSanKeys963/screenshots/data.png)