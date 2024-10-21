# Creating Intake catalogs

There are two versions of [intake](https://intake.readthedocs.io/en/latest/). Here, we focus on intake version 1. Version 2 has advanced features, but a [(partially) incompatible syntax](https://intake.readthedocs.io/en/latest/scope2.html#relationship-to-v1).

## Simple entry

As a minimal catalog, this yaml file would work:
```yaml
sources:
  ngc3028:
    args:
      chunks: null
      consolidated: true
      urlpath: https://swift.dkrz.de/v1/dkrz_b381d76e-63d7-4aeb-96f0-dfd91e102d40/ngc3028_hp/ngc3028_P1D_5.zarr
    driver: zarr
```

This can then be loaded and plotted via 
```python
import intake
import healpy
cat = intake.open_catalog("/path/to/catalog.yaml")
ds = cat['ngc3028'].to_dask()
tas_day0 = ds.tas.isel(time=0)
healpy.mollview(tas_day0, nest=True, flip='geo')
```

## Working with netCDF files

Examples for catalog entries with netCDF files can be found in [the nextGEMS catalog FESOM files](https://github.com/nextGEMS/catalog/blob/main/FESOM/IFS_4.4-FESOM_5-cycle3.yaml).

The metadata provided there is optional, but it helps the user to figure out what to load before loading.

## Stacking catalogs

Catalogs can form tree-like hierarchies, as can be seen in [the root of the nextGEMS catalog](https://github.com/nextGEMS/catalog/blob/main/catalog.yaml). The sub-catalogs then simply are files like the ones we looked at above, or further references to catalogs, or a mix of those.

## Working with catalogs

See [easy.gems](https://easy.gems.dkrz.de/Processing/Intake/index.html) for some examples of working with catalogs in nextGEMS and EERIE.

