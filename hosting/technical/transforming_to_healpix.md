# Transforming data to healpix

## Interpolation methods

Healpix has kind-of-strange curved cell boundaries, so conservative remaps are not trivial. A more trivial approach is nearest-neighbor remapping.

If you put emphasis on conservation properties, you can do a nearest-neighbor remap to a higher-resolution healpix grid, and then coarsen again. Possibly all in the same processing pipeline.

There needs to be some attention paid to missing values when coarsening (averaging 4 cells together) from a fine to a coarser grid.

## Tools

### CDO

The simplest way to convert a file to a healpix level 9 netCDF
 probably is using a recent cdo version and calling

```bash
cdo -f nc4 -k auto -z zstd -remapnn,hpz9 INFILE OUTFILE
```

To create zarr, first create an [.ncrc](https://github.com/Unidata/netcdf-c/blob/main/docs/quickstart_env.md) file specifying that you want the dimension separator to be a `/` instead of `.`.

```bash
echo >> ~/.ncrc
echo ZARR.DIMENSION_SEPARATOR=/ >> ~/.ncrc
```

Then use

```bash
cdo -f nczarr -k auto -z zstd -remapnn,hpz9 INFILE \
    file://OUTFILE.zarr#mode=zarr,file
```

However, cdo does not really produce the optimal chunking, so going via python might be better for production. For testing, this should actually work pretty well.

