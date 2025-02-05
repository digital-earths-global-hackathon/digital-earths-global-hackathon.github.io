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
    theme: [default, ../../custom.scss]
    chalkboard:
      buttons: true
    navigation-mode: linear
    auto-stretch: true
    center: true
    mermaid:
      theme: default
aliases: 
  - /talks/python_packages/index.html
---

## The basics

* Python functions and classes can be organised in [modules](https://docs.python.org/3/tutorial/modules.html)
  ```py
  import my_module

  my_module.my_function(42)
  ```
* [Packages](https://docs.python.org/3/tutorial/modules.html#packages) allow for a hierarchical structuring of modules
* The Python Package Index ([PyPI](https://pypi.org)) helps you find and install software^[authors need to actively publish their code]
  ```
  pip install <package_name>
  ```

## The default stack

This talk will not cover the "usual suspects"

    NumPy, SciPy, matplotlib, cartopy, netcdf4, xarray (+flox), ...

## [healpix](https://github.com/ntessore/healpix) <span style="color: #555">_(not healpy)_</span>

* implements a lean set of routines for working with HEALPix
  ```py
  healpix.nside2npix(9)
  healpix.ang2pix(nside=9, 52, 10, nest=True, lonlat=True)
  ```
* requires only NumPy, and can be installed with pip:
  ```
  python -m pip install healpix
  ```

## [easygems](https://github.com/mpimet/easygems)

* provides a (small) set of convenience functions to work with model output
  ```py
  easygems.healpix.healpix_show(ds_daily.tas.sel(time="2024-09-10"))
  ```
* started as a "bin" for functions used on [easy.gems](http://easy.gems.dkrz.de)
* can be installed with pip
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
* requires an ecCodes version that matches the output data
* can be installed with pip _(apart from ecCodes)_
  ```
  python -m pip install gribscan
  ```
