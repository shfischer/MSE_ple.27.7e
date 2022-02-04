Comparison of ICES data-rich and data-limited methods with MSE
================

## Introduction

This repository contains the data and code for a comparison of the
data-rich ICES MSY rule to the ICES data-limited empirical methods (rfb
rule, hr rule).

Three case study stocks are included:

1.  Plaice (*Pleuronectes platessa*) in Division 7.e (western English
    Channel)

2.  Cod (Gadus morhua) in Subarea 4, Division 7.d, and Subdivision 20
    (North Sea, eastern English Channel, Skagerrak)

3.  Herring (Clupea harengus) in Subarea 4 and divisions 3.a and 7.d,
    autumn spawners (North Sea, Skagerrak and Kattegat, eastern English
    Channel)

The operating models (OMs) are created using the SAM
[`stockassessment`](https://github.com/fishfollower/SAM/) R package and
follows the principles developed during the ICES Workshop on North Sea
stocks management strategy evaluation
([WKNSMSE](https://doi.org/10.17895/ices.pub.5090)).

The simulation is based on the Fisheries Library in R
([FLR](http://www.flr-project.org/)) and its `mse` package.

The data-limited MSE framework is an adaptation of Fischer et
al. ([2021](https://dx.doi.org/10.1093/icesjms/fsab169)), see the
[`shfischer/GA_MSE_PA`](https://github.com/shfischer/GA_MSE_PA) GitHub
repository for the original source code and documentation.

For exact reproducibility, R packages versions are recorded with
[renv](https://rstudio.github.io/renv/) in a
[renv.lock](https://github.com/shfischer/MSE_risk_comparison/blob/master/renv.lock)
file.

## Repository structure

-   `funs_*`: Function libraries, defining the functions used in the
    other scripts

    -   `funs.R`: generic function library, including definition of
        data-limited management procedures (MPs)

    -   `funs_analysis.R`: for analysis of results

    -   `funs_GA.R`: functions used in the optimisation with the genetic
        algorithm (GA)

    -   `funs_OM.R`: functions for generating the operating models

    -   `funs_WKNSMSE.R`: functions required for the ICES MSY rule

-   `OM_*`: Scripts for operating models (OMs, including alternative
    OMs)

    -   `OM_ple.27.7e.R` for plaice

    -   `OM_cod.27.47d20.R` for cod

    -   `OM_her.27.3a47d.R` for herring

    -   `OM_MSY.R`: script for estimating MSY

    -   `OM_MSY.pbs`: job script, calling `OM_MSY.R`

-   `MP_*`: Script for running and analysing the MSE

    -   `MP_analysis.R`: script for analysing MSE results (summarising,
        exporting, visualisation, …)

    -   `MP_run.R`: script for running any MP in the MSE and optimising
        MPs

    -   `MP_*.pbs`: job submission scripts, used for running MP_run.R on
        a high-performance computing cluster, e.g. `MP_run_rfb_mult.pbs`
        for optimising the rfb rule with a multiplier

-   `OM*.R`: Scripts for creating the operating model (OM)

    -   `OM_SAM_fit.R` script for configuring and running SAM,
    -   `OM_Eqsim.R` runs EqSim on SAM model fit to create ICES style
        reference points,
    -   `OM_length.R` preparation of length data for the generation of
        the length index for the rfb rule,
    -   `OM.R` generate the operating model(s),
    -   `OM_MSY.R` calculation of real MSY reference points with the mse
        framework

`input/`: This directory contains the files required for generating the
OMs for the three stocks

-   `input/ple.27.7e/preparation/`: for plaice

-   `input/cod.27.47d20/preparation/`: for cod

-   `input/her.27.3a47d/preparation/`: for herring

-   `input/OM_refpts.csv`: summarised OM reference points

## R, R packages and version info

The MSE simulations were run with R:

``` r
> sessionInfo()
R version 4.1.0 (2021-05-18)
Platform: x86_64-w64-mingw32/x64 (64-bit)
Running under: Windows 10 x64 (build 19041)
```

The package versions and their dependencies are recorded with the R
package [renv](https://rstudio.github.io/renv/) and stored in the file
[renv.lock](https://github.com/shfischer/MSE_risk_comparison/blob/master/renv.lock).
The exact package version can be restored by cloning this repository,
navigating into this folder in R (or setting up a project), installing
the renv package

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
`FLBRP`, `FLAssess`, `FLXSA`, `ggplotFL`, `mse`, and `FLfse`. See
[renv.lock](https://github.com/shfischer/MSE_risk_comparison/blob/master/renv.lock)
for version details and sources.

Also, the R package
[`stockassessment`](https://github.com/fishfollower/SAM)is used.

For running the optimisations on a high-performance computing cluster, a
suitable MPI back-end and the R package `Rmpi` are required.
