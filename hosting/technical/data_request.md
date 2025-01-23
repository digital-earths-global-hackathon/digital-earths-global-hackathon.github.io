---
aliases: 
  - /planning/hosting/technical/data_request.html
---

# Draft Data request

This data request still is in a draft stadium. To improve it, please [open an issue](https://github.com/digital-earths-global-hackathon/planning/issues) (and a matching pull request). It is based on the [Dyamond phase 3 request](https://www.researchsquare.com/article/rs-4458164/v1).

::: {.callout-note}
## The main differences to the DYAMOND 3 data request

* We request data on a hierarchy of HEALPix grids instead of 0.25 degree resolution.
* We added snowfall_flux, liquid_water_content_of_surface_snow, snow_area_fraction_viewable_from_above, soil_liquid_water_content to the request.
* We specify variable names.
* We request most 2D fields only as 3-hourly means.
* We specify that hourly 2D data should be instantaneous.
:::

## Data grid and vertical levels

 The data from global models should be provided on the [HEALPix](https://easy.gems.dkrz.de/Processing/healpix/index.html) grid on zoom level 9 or higher (effective cell size of 13 km). To ease analysis, please provide all HEALPix levels up to this level.

 For regional models, we will need further discussion with teams that have a strong experience in intercomparisons of regional models.

**3D Output Levels:**

The output should be interpolated to the following 25 pressure levels:

```python
import numpy as np
tr = np.arange(100,900,100)
lt = np.arange(850,1025,25)
ua = np.arange(10,90,20)
levels = sorted({1,5,20,150,250,750}.union(tr,lt,ua))
```

## Data volume

HEALPix grids have `12*4**level` cells, so a level 9 HEALPix grid consists of roughly 3 million cells. It has proven very beneficial for the analysis to also store all lower grid resolutions. This adds approximately 30% to the output volume, and allows prototyping analyses at lower resolution, or generating maps from an amount of data that actually matches the pixels in a plot.
For level 9 and all lower levels together, about 4 million floats are needed per 2D slice.
Furthermore, we want the 2D fields on 6-hourly interval as well, and all fields also daily.
The totals for storing this data (assuming 4 bytes/float, and 50% compression) are

```raw
3D: 2.8TB
2D: 1.7TB 
total: 4.5TB
```

See [below](#code-for-computing-the-volume) for the code.

Without the hierarchy, the requirements are:

| type   | cells / snapshot | MB / snap-shot[^1] | snapshots | GB / var | vars  | GB total |
| ------ | ---------------- | ----------- | --------- | ------------- | -------------- | -------- |
| 2D     | 3 M              |   6         | 365*24    | 52            | 33             | 1650     |
| 3D     | 75 M             |   150       | 365*4     | 220           | 12             | 2600     |

[^1]: Assuming 4-byte floats and 50% compression

Note that for any additional healpix level, the requirements grow by a factor of 4, so a ~6km resolution dataset (HEALPix level 10) already consumes about 20 TB.

## File formats

In principle any file format that is compatible with standard software could be used. However, zarr has proven very advantageous, as it allows to

* build large datasets covering anything up to an entire simulation output
* chunk data in all dimensions

If plain zarr 2 is used, data can be read in many programming languages. For C-based software, a recent libnetcdf will do the trick. The downside of this approach is a lot of small files, which can be problematic on HPC systems, especially with inode quota.

Other possible approaches include the use of [kerchunk](https://fsspec.github.io/kerchunk/) in python for grouping data chunks in (netCDF/HDF5) files into unified datasets that look like zarr to python. Other programming languages / codes can then still make use of the underlying netCDF files.

## Variables

For some models, the hydrometeor categories may not map directly onto the specified output.  In these cases hydrometeor habits can be left out (for instance if snow and cloud ice are not distinguished), or additional information can be added, e.g., for models with hail. In such cases, please try to follow [the CF conventions](https://cfconventions.org/Data/cf-standard-names/current/build/cf-standard-name-table.html), and [open an issue](https://github.com/digital-earths-global-hackathon/planning/issues), so we can amend the table and keep the naming consistent among all teams.

### 3D Output Variables, write instantaneous values at 6hr interval

| standard name              | short name |   units |        comment |
| :------------------------- | ---------: | ------: | -------------: |
| geopotential height        |         zg |       m |                |
| eastward_wind              |         ua |     m/s |                |
| northward_wind             |         va |     m/s |                |
| upward_air_velocity        |         wa |     m/s |                |
| temperature                |         ta |       K |                |
| relative_humidity          |        hur |       - |                |
| specific_humidity          |        hur | kg kg-1 |                |
| mass_fraction_hydrometeors |       qall | kg kg-1 | names invented |


### 2D Output Variables, write averages at 3hr interval

| CF standard name                                    | short name |      units |                                                comment |
| :-------------------------------------------------- | ---------: | ---------: | -----------------------------------------------------: |
| atmosphere_mass_content_of_cloud_condensed_water    |      clwvi |     kg m-2 |                                                        |
| atmosphere_mass_content_of_cloud_ice                |      clivi |     kg m-2 |                                                        |
| surface_downward_latent_heat_flux                   |      hflsd |      W m-2 |                       direction included in short name |
| surface_downward_sensible_heat_flux                 |      hfssd |      W m-2 |                       direction included in short name |
| toa_outgoing_longwave_flux                          |       rlut |      W m-2 |                                                        |
| toa_outgoing_longwave_flux_clear_sky                |     rlutcs |      W m-2 |                                                        |
| toa_incoming_longwave_flux                          |       rldt |      W m-2 |                                                        |
| surface_upwelling_longwave_flux_in_air              |       rlus |      W m-2 |                                                        |
| surface_upwelling_longwave_flux_in_air_clear_sky    |     rluscs |      W m-2 |                                                        |
| surface_downwelling_longwave_flux_in_air            |       rlds |      W m-2 |                                                        |
| surface_downwelling_longwave_flux_in_air_clear_sky  |     rldscs |      W m-2 |                                                        |
| toa_outgoing_shortwave_flux                         |       rsut |      W m-2 |                                                        |
| toa_outgoing_shortwave_flux_clear_sky               |     rsutcs |      W m-2 |                                                        |
| toa_incoming_shortwave_flux                         |       rsdt |      W m-2 |                                                        |
| surface_upwelling_shortwave_flux_in_air             |       rsus |      W m-2 |                                                        |
| surface_upwelling_shortwave_flux_in_air_clear_sky   |     rsuscs |      W m-2 |                                                        |
| surface_downwelling_shortwave_flux_in_air           |       rsds |      W m-2 |                                                        |
| surface_downwelling_shortwave_flux_in_air_clear_sky |     rsdscs |      W m-2 |                                                        |
| precipitation_flux                                  |         pr | kg m-2 s-1 |                                                        |
| solid_precipitation_flux                            |        prs | kg m-2 s-1 |              includes all forms of solid precipitation |
| atmosphere_mass_content_of_water_vapor              |        prw | kg m-2 s-1 |                                                        |
| surface_air_pressure                                |         ps |         Pa |                                                        |
| air_pressure_at_mean_sea_level                      |        psl |         Pa |                                                        |
| specific_humidity                                   |       huss |    kg kg-1 |                                        2m above ground |
| air_temperature                                     |        tas |          K |                                        2m above ground |
| eastward_wind                                       |        uas |      m s-1 |                                       10m above ground |
| northward_wind                                      |        vas |      m s-1 |                                       10m above ground |
| surface_temperature                                 |         ts |          K |                                                        |
| surface_downward_eastward_stress                    |       tauu |      N m-2 |                                                        |
| surface_downward_northward_stress                   |       tauv |      N m-2 |                                                        |
| cloud_area_fraction                                 |        clt |          1 |                                                        |
| liquid_water_content_of_surface_snow                |        swe |     kg m-2 |                                    short name invented |
| snow_area_fraction_viewable_from_above              |     sncvfa |          1 | short name based on snc for surface_snow_area_fraction |
| soil_liquid_water_content                           |       mrso |     kg m-2 |                                    short name invented |
| sea_ice_area_fraction                               |     siconc |          1 |                                                        |

### 2D Output Variables, write at 1hr interval as instantaneous

This list is designed to include key outputs like accumulated precipitation, surface temperature and surface wind speed, as well as other features desired for trackers of convective storms and MCS. Other features requiring vertical information are suggested to be done 6 hourly.

| CF standard name               | short name |      units |          comment |
| :----------------------------- | ---------: | ---------: | ---------------: |
| toa_outgoing_longwave_flux     |       rlut |      W m-2 |                  |
| toa_outgoing_shortwave_flux    |       rsut |      W m-2 |                  |
| precipitation_flux             |         pr | kg m-2 s-1 |      accumulated |
| air_pressure_at_mean_sea_level |        psl |         Pa |                  |
| eastward_wind                  |        uas |      m s-1 | 10m above ground |
| northward_wind                 |        vas |      m s-1 | 10m above ground |
| surface_temperature            |         ts |          K |                  |

### 2D time-constant Variables

| CF standard name       | short name | units | comment |
| :--------------------- | ---------: | ----: | ------: |
| land_area_fraction     |      sftlf |     1 |         |
| land_ice_area_fraction |     sftgif |     1 |         |
| surface_altitude       |       orog |     m |         |

## Code for computing the data volume

```python
vars_3d = 8
vars_2d_3h = 35
vars_2d_1h = 7
interval_3d = 6/24.
interval_2d_3h = 1/8.
interval_2d_1h = 1/24.
interval_daily = 1.
levels_3d = 25

params = dict ( 
    max_healpix_level = 9,
    duration = 365,
    float_precision = 4,
    float_compression = .5,
)
def compute_volume(var_count, levels, interval, max_healpix_level, duration, float_precision, float_compression):
    cells = sum (12 * 4** level for level in range (max_healpix_level + 1))
    return cells * var_count * levels * duration / interval * float_precision * float_compression

volume_3d = ( compute_volume(var_count=vars_3d, levels=levels_3d, interval=interval_3d, **params) +
compute_volume(var_count=vars_3d, levels=levels_3d, interval=interval_daily, **params))
volume_2d_3h = (compute_volume(var_count=vars_2d_3h, levels=1, interval = interval_2d_3h, **params) +
            compute_volume(var_count=vars_2d_3h, levels=1, interval = interval_3d, **params) +
            compute_volume(var_count=vars_2d_3h, levels=1, interval = interval_daily, **params))
volume_2d_1h = (compute_volume(var_count=vars_2d_1h, levels=1, interval = interval_2d_1h, **params))
print (f'3D: {volume_3d/1024**4:.1f}TB\n2D: {(volume_2d_3h+volume_2d_1h)/1024**4:.1f}TB \ntotal: {(volume_3d+(volume_2d_3h+volume_2d_1h))/1024**4:.1f}TB')
```
