# Learnings from the 2025 Hackathon

All in all, this was surprisingly successful. There were some technical issues though.

## Overall organization

* Andrew, Pier-Luigi, Sara and Yuting did a great job at holding things together
* Bjorn did a great job at shaking up structures and pushing things forward
* Tobi, Lukas, Mark M, and others made technical magic possibble.

## Schedule

## Inter-node communication

### Mattermost

* Works
* Invite more people earlier in the process
* China uses WeChat for most communication (in Chinese, but with auto-translation)

### Zoom calls during the preparation phase

* Double Telcos are tiring but necessary.
* Did anybody read the nodes / watch the recordings?

### Zoom calls during the hackathon

* Did not really line up with the schedules (at least at some nodes). Would need earlier planning to be incorporated into the local schedules.

### GitHub and Code sharing

* GitHub seems to provide a bit of a barrier for sharing code.
* Need a concept that scales to 500 people contributing at the same time without established teams or time for learning.

## Technical aspects

### Technical Team

* Actually have a real team with members from all nodes
* Meet with this team at EGU/AGU if possible at all
* Distribute tech team members more widely to foster connections?
* Might be worth hiring one person to oversee things / fully commit to running this
* Have a clear contact for each node who leads the preparations and actually is there during the hackathon

### Dataset preparation

* Provide default chunk settings for 2D/3D datasets
* Zarr with small (regional) Chunks works well.
    * The IFS GRIB datasets were very popular in China despite access via EERIE cloud being difficult.
* Prepare more uniform conversion scripts for better sharing among the teams
* Try to get datasets ready weeks before the hackathon (maybe practice with sample datesets to get the scripts right if teams are into last-minute simulations)
* Create scripts for consistency checks (e.g. do they have crs set?), and encourage their use
* A suite of common analysis could be applied to catch obvious errors - perhaps timeseries/zonal means.

### Technical instructions

* Prepare and share instructions on how to work with the data
* Pre-hackathon telcos on node-basis would be good (for most ppl. participating this is the 1st and not the 10th hackathon...)

### Node Set-up

* Set up proxies at all nodes and test them with all remote datasets (under fire) before the hackathon
* Spread users across different slurm-jobs

### Catalogs

* intake0.7 did the trick, but we need a new concept, that again includes loading the data
* intake0.7 is outdated and will increasingly collide with python environments
* Stac has no concept for loading the data
* The combined multi-node offline/online mix worked well
* Ideally, the catalog could do the cascading.

### Libraries and tools (e.g. easygems, uxarray)

* `ds = cat['ERA5'].to_dask().pipe(attach_crs, zoom=zoom).drop_vars(["lat", "lon"], errors="ignore")`
* CF Standard for HEALPix might improve things in the future.
* [XDGGS](https://xdggs.readthedocs.io/en/latest/tutorials/healpix.html) will help with hierarchies.
* Full uptake of python at Beijing node
* `egh.healpix_view` was very popular
* Much of the work started with regional remaps to lat/lon (for MCS tracking / ...). Would be good to provide tools for this.
