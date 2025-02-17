Simulations for ICES categories 4-6
================

This repository
([GA_MSE_cat456](https://github.com/shfischer/GA_MSE_cat456)) is a
mirror of [GA_MSE_HR](https://github.com/shfischer/GA_MSE_HR),
[GA_MSE](https://github.com/shfischer/GA_MSE), and
[GA_MSE_PA](https://github.com/shfischer/GA_MSE_PA) with the `cat456`
branch displayed as default branch.

## Introduction

This repository contains the code for an exploration of the ICES
approach for categories 4-6. It includes the typical ICES harvest
control rule (constant catch with PA buffer) and a version with a
conditional PA buffer based on length data (including optimisation with
a genetic algorithm). The simulation is based on the Fisheries Library
in R ([FLR](http://www.flr-project.org/)) and the Assessment for All
(a4a) standard MSE framework ([`FLR/mse`](github.com/FLR/mse)) developed
during the Workshop on development of MSE algorithms with R/FLR/a4a
([Jardim et al.,
2017](https://ec.europa.eu/jrc/en/publication/assessment-all-initiativea4a-workshop-development-mse-algorithms-rflra4a)).

The simulation framework is based on the
([GA_MSE_HR](https://github.com/shfischer/GA_MSE_HR)) repository which
explores the use of harvest rates and contains the code for the
publication:

> Fischer, S. H., De Oliveira, J. A. A., Mumford, J. D., and Kell, L. T.
> (2022). Exploring a relative harvest rate strategy for moderately
> data-limited fisheries management. ICES Journal of Marine Science. 79:
> 1730-1741. <https://doi.org/10.1093/icesjms/fsac103>.

The operating models provided as an input are those from the repository
[shfischer/wklifeVII](https://github.com/shfischer/wklifeVII) as
described in:

> Fischer, S. H., De Oliveira, J. A. A., and Laurence T. Kell (2020).
> Linking the performance of a data-limited empirical catch rule to
> life-history traits. ICES Journal of Marine Science, 77: 1914-1926.
> <https://doi.org/10.1093/icesjms/fsaa054>.

## Repository structure

The root folder contains the following R scripts:

- `OM.R`: This script creates the operating models (OMs),
- `funs_OM.R` contains functions and methods used for the creation of
  the operating models
- `funs.R` contains functions for running the MSE such as the OM and MP
  modules,
- `funs_GA.R` contains the function used in the optimisation procedure,
- `run_ms_cat456.R` is an R script for running MSE projections and is
  called from a job submission script
- `run*.pbs` are job submission scripts which are used on a high
  performance computing cluster and call `run_ms.R`
- `analysis_cat456.R` is for analysing the results

The following input files are provided:

- `input/stocks.csv` contains the stock definitions and life-history
  parameters
- `input/brps.rds` contains the FLBRP objects which are the basis for
  the OMs

Summarised outputs are provided in:

- `output/`

## R, R packages and version info

This is branch `cat456` in which R and R packages have been updated to R
4.2 (4.3 should also work):

``` r
> sessionInfo()
R version 4.2.3 (2023-03-15 ucrt)
Platform: x86_64-w64-mingw32/x64 (64-bit)
Running under: Windows 10 x64 (build 19043)
```

The package versions and their dependencies are recorded with the R
package [renv](https://rstudio.github.io/renv/) and stored in the file
[renv.lock](https://github.com/shfischer/GA_MSE_cat456/blob/cat456/renv.lock).
The exact package version can be restored by cloning this repository,
checking out the current branch (`cat456`), navigating into this folder
in R (or setting up a project), installing the renv package

``` r
install.packages("renv")
```

and calling

``` r
renv::restore()
```

See [renv](https://rstudio.github.io/renv/) and the package
documentation for details.

The framework is based on the Fisheries Library in R (FLR) framework and
uses the [FLR packages](https://flr-project.org/) `FLCore`, `FLasher`,
`FLBRP`, `ggplotFL`, `mse`. See
[renv.lock](https://github.com/shfischer/GA_MSE_cat456/blob/cat456/renv.lock)
for version details and sources.

The FLR package versions can also be installed manually with `remotes`
(requires suitable tools to compile R packages):

``` r
remotes::install_github(repo = "flr/FLCore", ref = "9ba6652000574cbaea278ad4b0428b6fb98b4607")
remotes::install_github(repo = "flr/FLasher", ref = "84268b3bc21941bfde20dc9cbb0cfc3367f570a7", INSTALL_opts = "--no-multiarch")
# INSTALL_opts = "--no-multiarch" to avoid issues in Windows
remotes::install_github(repo = "flr/FLBRP", ref = "a76a06a5cf6afbf3cadefe3ed8ce7617a8dfd57c", INSTALL_opts = "--no-multiarch")
remotes::install_github(repo = "shfischer/mse", ref = "9daf60000fb3a0b7343ff602d0bb479d609f0fe2", INSTALL_opts = "--no-multiarch")
# and the GA package for running the genetic algorithm
remotes::install_github(repo = "shfischer/GA", ref = "48c1092437629b86a5310fa2873621621ff0b0e0")
```

For using MPI parallelisation, an MPI backend such as OpenMPI and the R
packages `Rmpi` and `doMPI` are required.
