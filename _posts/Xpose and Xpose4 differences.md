# Xpose
xpose was designed as a ggplot2-based alternative to xpose4. xpose aims to reduce the post processing burden and improve diagnostics commonly associated the development of non-linear mixed effect models.
```r
library(xpose)
xpdb <- xpose_data(runno = '001')
```
## Properties of the xpdb fron Xpose
```r
str(xpdb)
```
It is a huge list with S3 data.frame
- $code this can output the list file 
- $summary this is the short version of the list file that can output shrinkage
- $data
- $files this gives you the .cor .cov .ext and .phi files

## Common functions from Xpose
- model summary
  -`summary(xpdb, problem = 1)`
- GOF
  - `dv_vs_ipred(xpdb)`
  - `ind_plots(xpdb, page = 1)`
  - `xpdb %>% vpc_data(stratify = 'SEX', opt = vpc_opt(n_bins = 7, lloq = 0.1)) %>% vpc()`
  - `eta_distrib(xpdb, labeller = 'label_value')`
  - `prm_vs_iteration(xpdb, labeller = 'label_value')`


# Xpose
Xpose 4 is a collection of functions to be used as a model building aid for nonlinear mixed-effects (population) analysis using NONMEM. It facilitates data set checkout, exploration and visualization, model diagnostics, candidate covariate identification and model comparison.
Xpose is implemented using the lattice graphics library
```r
library(xpose4)
xpdb <- xpose.data(runno = '001')
```
## Properties of xpdb is a S4 object
- xpdb@Data the output sdtab
- xpdb@SData the simulated sdtab
- xpdb@Data.firstonly the table with ID ETA and OBJ
- xpdb@SData.firstonly the simulated the table with ID ETA and OBJ
- xpdb@Runno run number
- xpdb@Nsim number of simulated file
- xpdb@Doc
- xpdb@Prefs

## How to make NONMEM generate input to Xpose
- sdtab* Standard table file, containing ID, IDV, DV, PRED, IPRED, WRES, IWRES, RES, IRES,
etc.
- patab* Parameter table, containing model parameters - THETAs, ETAs and EPSes
- catab* Categorical covariates, e.g. SEX, RACE
- cotab* Continuous covariates, e.g. WT, AGE
- extra*, mutab*, mytab*, xptab*, cwtab* Other variables you might need to have available to
Xpose
- run*.mod Model specification file
- run*.lst NONMEM output
Strictly, only one table file is needed for xpose (for example sdtab* or xptab*). However, using
patab*, cotab*, catab* will influence the way that Xpose interprets the data and are recommended
to get full benefit from Xpose.

## Common functions
- GOF
  - `basic.gof(object, force.wres = FALSE, main = "Default", use.log = FALSE, ...)`
  - `cwres.vs.cov(object,ylb = "CWRES",smooth = TRUE,type = "p",main = "Default",...)`
  - `cwres.vs.pred(object, abline = c(0, 0), smooth = TRUE, ...)`
  - `dOFV.vs.id(xpdb1,xpdb2,sig.drop = -3.84)`
  - `dv.vs.pred(object, abline = c(0, 1), smooth = TRUE, ...)`
  - `parm.vs.cov(object,onlyfirst = TRUE,smooth = TRUE,type = "p",mirror=3)` 

