# [marynaggita](https://github.com/marynaggita/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/latlong_blog/status/1543726888908009472
- https://twitter.com/_jhamman/status/1579882167303168004
- https://twitter.com/ErikvanSebille/status/1578030942144188417



## Zarr Data Visualisation

Atmospheric Conditions from the Coupled Model Intercomparison Project Phase 6 (CMIP6)(https://www.wdc-climate.de/ui/cmip6?input=CMIP6.CMIP.AS-RCEC.TaiESM1.1pctCO2) 


## Code Snippets

- NASA Prediction of Worldwide Energy Resources (POWER) - https://registry.opendata.aws/nasa-power

Code 👇🏻

```
import zarr
from matplotlib import pyplot as plt
g = zarr.open_group(
    "s3://cmip6-pds/CMIP6/CMIP/AS-RCEC/TaiESM1/1pctCO2/r1i1p1f1/Amon/hfls/gn/v20200225/",
    storage_options={"anon": True}
)

print(list(g.keys()))
z = g["hfls"]
print(z.shape)
plt.imshow(z[900], origin="lower")
```

![image2](/_data/MSanKeys963/screenshots/zarr_data.jpg)