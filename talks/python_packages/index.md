---
title: "Sharing is caring"
author: "Lukas Kluft"
subtitle: "Python packages used or driven by the GEMS community"
format:
  revealjs:
    transition: slide
    slide-number: true
    code-background: true
    logo: /static/logos.png
    footer: "[Talks page](/talks)"
    theme: [default, ../custom.scss]
    chalkboard:
      buttons: true
    navigation-mode: linear
    auto-stretch: true
    center: true
    mermaid:
      theme: default
---

## The default stack

This talk will not cover the "usual suspects"^[I expect those to be installed]

NumPy, SciPy, matplotlib, cartopy, netcdf4, xarray (+flox), ...

## [healpix](https://github.com/ntessore/healpix)

* implements a lean set of routines for working with HEALPix
  ```py
  healpix.nside2npix(9)
  healpix.ang2pix(nside=9, 52, 10, nest=True, lonlat=True)
  ```
* requires only numpy, and can be installed using pip:
  ```
  python -m pip install healpix
  ```

## [easygems](https://github.com/mpimet/easygems)

* provides a (small) set of convenience functions to work with model output
  ```py
  easygems.healpix.healpix_show(ds_daily.tas.sel(time="2024-09-10"))
  ```
* started as a "bin" for functions used on easy.gems
* can be installed using pip
  ```
  python -m pip install easygems
  ```

## [gribscan](https://github.com/gribscan/gribscan)

* scan GRIB files and create zarr-compatible indices
  ```sh
  gribscan-index *.grib2
  gribscan-build --magician ifs --prefix $PWD *.index
  ```
* provides the command-line tools `gribscan-index` and `gribscan-build`
* requires an ecCodes version matching the output data
* can be installed using pip
  ```
  python -m pip install gribscan
  ```
