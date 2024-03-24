# Update Nordic Microalgae taxonomic backbone

The R Markdown document update-nua-taxonomy.Rmd calls a number of R and Python scripts to interact with APIs and webpages from [WoRMS](https://www.marinespecies.org/), [Dyntaxa](https://namnochslaktskap.artfakta.se/), [AlgaeBase](https://www.algaebase.org/), [GBIF](https://www.gbif.org/), [NORCCA](https://norcca.scrol.net/) in order to update the species content of Nordic Microalgae.

You need a Python interpreter to be used in the ```reticulate``` package. See [rstudio.github.io/reticulate](https://rstudio.github.io/reticulate/) for details.

The R package ```SHARK4R``` is required for API queries towards Dyntaxa. See installation details on [github.com/sharksmhi/SHARK4R/](https://github.com/sharksmhi/SHARK4R/). API query functions towards AlgaeBase have been modified (stored in /src/R/fun) from the ```algaeClassify``` package to include AlgaeBase IDs (Patil et al. 2023).

Store your API keys to Dyntaxa and AlgaeBase in .Renviron. The easiest way to edit this file is by running:
```
install.packages("usethis")
usethis::edit_r_environ("project")
```
Edit your .Renviron to look like this (fake checksums provided below):
```
ALGAEBASE_APIKEY = "e1482dc9abfe073d56db08c0b604e333"
DYNTAXA_APIKEY = "89ad0b9cac6ce53184cc942147e1f06f"
```
### References
Patil, V.P., Seltmann, T., Salmaso, N., Anneville, O., Lajeunesse, M., Straile, D., 2023. algaeClassify (ver 2.0.1, October 2023): U.S. Geological Survey software release, https://doi.org/10.5066/F7S46Q3F

## To update the content:
* Clone this repo
* Download the latest NOMP biovolume list (in .xlsx) and store in /data_in/. The file can be accessed from the [Nordic Microalgae webpage](http://nordicmicroalgae.org/tools)
* Download the latest complete IOC HAB list in .txt from the [IOC-UNESCO Taxonomic Reference List of HAB](https://www.marinespecies.org/hab/aphia.php?p=download&what=taxlist) and store in /data_in/
* Additional taxa that exist in WoRMS can be manually added to the database in /data_in/additions_to_old_nua.txt
* Run (knit) update-nua-taxonomy.Rmd. Please note that all the API calls will take 9-10 hours to run if lists are not loaded from cache
* Check output for potential duplicated taxa or errors, they are listed in the .html report in /update_history/
  * Taxa may be filtered using /data_in/blacklist.txt and /data_in/whitelist.txt, if needed
* Push updated lists from /data_out/content to [nordicmicroalgae/content/](https://github.com/nordicmicroalgae/content/) and verify GitHub CI checks
* Run the syncdb app as a superuser from the admin pages, see logs for potential problems
* Check if any images become assigned as taxon = 'none' after the import, and assign them to their current names.
* Verify updated Quick-View filters in /data_out/backend/taxa/config and push to [nordicmicroalgae/backend](https://github.com/nordicmicroalgae/backend), if needed
  * Corrections can be made in /data_in/plankton_groups.txt, where major groups can be defined for Kingdom and Phylum. 'Other microalgae' are defined as everything else except groups specified under exclude_from_others
* Upload a new version of the checklist to data.smhi.se (optional)
