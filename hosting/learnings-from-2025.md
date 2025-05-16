# Learnings from the 2025 Hackathon

I'll start this as a random collections of thoughts, and hope we'll be able to evolve it into something more structured and issues

All in all, this was surprisingly successful. There were some technical issues though.

## Overall organization

* Andrew, Pier-Luigi, Sara and Yuting did a great job at holding things together
* Bjorn did a great job at shaking up structures and moving pushing things forward
* Tobi, Lukas, Mark M, and others made technical magic possibble.

## Schedule

## Inter-node communication

### Telcos

* Double Telcos are tiring but necessary.
* Did anybody read the nodes / watch the recordings?

### Mattermost
* Works
* Invite more people earlier in the process
* China uses WeChat for most communication (in Chinese, but with auto-translation)

## Technical aspects

### Technical Team
* Actually have a real team with members from all nodes
* Meet with this team at EGU/AGU if possible at all
* Distribute tech team members more widely to foster connections?
* Might be worth hiring one person to oversee things / fully commit to running this
* Have a clear contact for each node who leads the preparations and actually is there during the hackathon

### Dataset preparation
* Zarr with small (regional) Chunks works well.
    * The IFS GRIB datasets were very popular in China despite access via EERIE cloud being difficult.
* Prepare more uniform conversion scripts for better sharing among the teams

### Technical instructions

* Prepare and share instructions on how to work with the data
* Pre-hackathon telcos on node-basis would be good (for most ppl. participating this is the 1st and not the 10th hackathon...)

### Node Set-up
* Set up proxies at all nodes and test them with all remote datasets (under fire) before the hackathon
* Spread users across different slurm-jobs
* 

### Catalogs
* intake0.7 did the trick, but we need a new concept, that again includes loading the data
* intake0.7 is outdated and will increasingly collide with python environments
* Stac has no concept for loading the data
* The combined multi-node offline/online mix worked well

### Libraries (e.g. easygems, uxarray)
* `ds = cat['ERA5'].to_dask().pipe(attach_crs, zoom=zoom).drop_vars(["lat", "lon"], errors="ignore")`

