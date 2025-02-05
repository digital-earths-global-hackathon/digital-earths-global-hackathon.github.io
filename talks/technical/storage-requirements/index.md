---
title: "Storage requirements"
author: "Florian Ziemen"
subtitle: ""
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
  - /talks/storage-requirements/index.html
---


# What goes where

* HEALPix data for variables requested is duplicated/cached among those centers that have enough space.
* Native grid data stays at the originating center.
* Bonus variables stay at the originating center.

# Requirements for Healpix data

## Ballpark estimate for level 9 (13km)

| type   | cells / snap-shot | MB / snap-shot[^1] | snapshots | GB / var | vars  | GB total |
| ------ | ---------------- | ----------- | --------- | ------------- | -------------- | -------- |
| 2D     | 3 M              |   6         | 365*24    | 52            | 33             | 1650     |
| 2D[^2] | 3 M              |   6         | 365*4     | 13            | 33             | 275      |
| 3D     | 75 M             |   150       | 365*4     | 220           | 12             | 2600     |

[^1]: Assuming 4-byte floats and 50% compression
[^2]: For consistent datasets

## Making things useful

* Adding the full HEALPix hierarchy adds 30%, and makes the dataset useful.
* Add daily means (+25% w.r.t the 6-hourly data)
* Total size per dataset at 13 km: 6.6TB
* Doubling the resolution multiplies the storage need by 4.
* 3-hourly 3D fields increase the size to 10.6 TB
  
## Totals

* 10 models at level 9 (13 km): 68 TB
* 10 models at level 10 (6 km): 272 TB
* 10 models at level 11 (3 km): 1.1 PB
