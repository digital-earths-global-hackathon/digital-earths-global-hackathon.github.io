---
title: "Processing needs"
subtitle: "Lessons learned during previous hackathons"
author: "Lukas Kluft, Florian Ziemen"
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
  - /talks/processing_needs/index.html
---

## Hardware

* a couple of dedicated analysis nodes in a SLURM partition (~32 at DKRZ, 10 above normal)
* reservations have proven less useful than simply expanding the *interactive* partition
* limit of **2 nodes per participant** (larger jobs are usually user error)

## Services

* interactive access to the computing resources
* [JupyterHub](https://jupyter.org/hub) (or similar) is usually an appreciated entry point

## Python environments

* it's hard (if not impossible) to provide a single environment for all users
* _Compromise:_ provide a lean python environment with "common" scientific packages
* users can install their exotic dependencies on top of that

## Our recommendation

* use [micromamba](https://mamba.readthedocs.io/)^[Faster than conda and circumvents [license issues](https://prefix.dev/blog/towards_a_vendor_lock_in_free_conda_experience)] to manage a basic python environment
* we should maintain a single `environment.yaml` to provide **common** packages
* users may install more "exotic" packages on their own

## Data

* _state-of-the-art_ compression can help to reduce **both** disk usage and access time ([`zstd`](https://github.com/facebook/zstd), [`lz4`](https://github.com/lz4/lz4))
* test access to data **using the hackathon environment**
