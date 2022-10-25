# [INTeniola](https://github.com/INTeniola/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/pleiszenburg/status/1254786742126690306?s=20&t=ZySGFflwD_KVvETbXGriug
- https://twitter.com/higlass_io/status/1292947250935554049?s=20&t=ZySGFflwD_KVvETbXGriug
- https://twitter.com/BrockmannCon/status/1324293687812173824?s=20&t=ZySGFflwD_KVvETbXGriug

## Zarr Use Case
- The use of Zarr files to store and study 4D wind flow blocks - https://vortexfdc.com/zarr-the-new-format-to-store-and-study-4d-wind-flow-blocks/
- Continual extension of Zarr datasets - https://medium.com/pangeo/continuously-extending-zarr-datasets-c54fbad3967d
- Zarr architecture on climate data - https://dclimate.medium.com/introducing-zarrchitecture-on-dclimate-c12c0ad7e744
## Zarr Data Visualisation

[pleiszenburg.de](https://youtu.be/RLHM5MQ5kAs) showing Earthquake Zarr Data

GitHub repo: https://github.com/pleiszenburg/earthquakes_youtube01

![image1](/_data/INTeniola/screenshots/EarthQuake.PNG)

[dClimate](https://dclimate.medium.com/introducing-zarrchitecture-on-dclimate-c12c0ad7e744) showing how Zarr makes it possible to work with large datasets that do not comfortably fit into memory

![image2](/_data/INTeniola/screenshots/Africa.png)



## Code Snippets

- Skin Layers Dermis and Epidermis - https://webknossos.org/datasets/scalable_minds/skin


Code 👇🏻

```
z = zarr.open_array(
    "https://data-humerus.webknossos.org/data/zarr/scalable_minds/skin/color/4-4-1"
)
print(z.shape)
plt.imshow(z[:, :, :, 0].T)

```

![image3](/_data/INTeniola/screenshots/skin.png)
