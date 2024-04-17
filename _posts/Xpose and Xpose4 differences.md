# Xpose
xpose was designed as a ggplot2-based alternative to xpose4. xpose aims to reduce the post processing burden and improve diagnostics commonly associated the development of non-linear mixed effect models.
```r
library(xpose)
xpdb <- xpose_data(runno = '001')
```
Properties of the xpdb
```r
str(xpdb)
```
It is a huge list with S3 data.frame
- $code this can output the list file 
- $summary this is the short version of the list file that can output shrinkage
- $data
- $files this gives you the .cor .cov .ext and .phi files

Common functions from Xpose
- model summary `summary(xpdb, problem = 1)`
- GOF
  - `dv_vs_ipred(xpdb)`
  - `ind_plots(xpdb, page = 1)`
  - `xpdb %>% vpc_data(stratify = 'SEX', opt = vpc_opt(n_bins = 7, lloq = 0.1)) %>% vpc()`
  - `eta_distrib(xpdb, labeller = 'label_value')`
  - `prm_vs_iteration(xpdb, labeller = 'label_value')`


# Xpose
