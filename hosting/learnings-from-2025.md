# Learnings from the 2025 Hackathon

All in all, this was surprisingly successful. There were some technical issues though.

## Overall organization

* Andrew, Pier-Luigi, Sara and Yuting did a great job at holding things together
* Bjorn did a great job at shaking up structures and pushing things forward
* Tobi, Lukas, Mark M, and others made technical magic possibble.

### Local node organization

* Important things to cover: Enough space for the group work (ideally all groups should be in one big room which lowers the physical barrier in interaction across groups).
* Eventually, a couple of small rooms for small group discussions or Zoom meetings.
* Very important: Snack and coffee/ tea breaks in-between for networking.

## Schedule

* General impression: The general [agenda](https://digital-earths-global-hackathon.github.io/hosting/logistics/agenda_template.html) from past hackathons was a good orientation point. Each node had their own additional programm with key notes or tutorials according to their needs.
* An idea came up to use some time before, for a "Mini-preparation workshop". I.e. if the hackathon starts in the afternoon, the morning of the same day could be used for example for data training or the science teams lead could meet to discuss how to organize themselves.
* Pan-node syncs were semi-successful. We should promote the syncs more and also identify the targeted audience (for every participant or just among the node organizers...) or the purpose of these syncs.

## Outreach

* LinkedIn was quite lively. We've created [a sub-page](https://www.linkedin.com/showcase/wcrp-global-km-scale-hackathon-2025/) and started this one month before the event started with some introdutory posts. During and after the event, we had a really active engagement rate, not only by the page itself but also from individual people and institutions reacting, reposting and mentioning hk25.

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
* Goals should be very clear.
* There were some difficulties in being and changing the host. This led to complications in recording or opening breakout rooms.

### GitHub and Code sharing

* GitHub seems to provide a bit of a barrier for sharing code.
* Need a concept that scales to 500 people contributing at the same time without established teams or time for learning.
* If we really want collaboration, we need to train people before the hackathon.
* Possible rules:
  * No Jupyter notebooks, just plain python scripts to avoid GB-size repo. `# %%` syntax is fine
  * Commit and merge frequently
  * Differentiate between libraries, examples, and dumpster

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
    * The IFS GRIB datasets were very popular in China despite access via EERIE cloud being difficult. This is due to IFS being popular, not b/c of GRIB being popular.
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
