
<!-- README.md is generated from README.Rmd. Please edit that file -->

# citesdb

Authors: *Noam Ross, Evan Eskew, and Carlos Zambrana-Torrelio*

[![License:
MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Project Status: WIP - Initial development is in progress, but there
has not yet been a stable, usable release suitable for the
public.](http://www.repostatus.org/badges/latest/wip.svg)](http://www.repostatus.org/#wip)
[![CircleCI](https://circleci.com/gh/ecohealthalliance/citesdb.svg)](https://circleci.com/gh/ecohealthalliance/citesdb)
[![AppVeyor build
status](https://ci.appveyor.com/api/projects/status/github/ecohealthalliance/citesdb?branch=master&svg=true)](https://ci.appveyor.com/project/ecohealthalliance/citesdb)
[![codecov](https://codecov.io/gh/ecohealthalliance/citesdb/branch/master/graph/badge.svg)](https://codecov.io/gh/ecohealthalliance/citesdb)
[![CRAN
status](https://www.r-pkg.org/badges/version/citesdb)](https://cran.r-project.org/package=citesdb)

**citesdb** provides access to the full history of CITES shipments for
analysis.

Features:

  - Local caching of database
  - RStudio connection pane with browsable database
  - HTMLwidget-enhanced help files for browsing metadata

While this repository is private you must have a GitHub Personal Access
Token (`GITHUB_PAT`) installed in order to access data. Instructions for
setting this up are found
[here](https://happygitwithr.com/github-pat.html).

## Installation

Install the **citesdb** package with this command:

``` r
source("https://install-github.me/ecohealthalliance/citesdb")
```

## Getting the data

When you first load the package you will see a message like this:

    library(citesdb)
    #> Local CITES database empty or corrupt. Download with cites_db_download()

Not to worry, just do as it says and run `cites_db_download()`. This
will fetch the most recent database from online. Currently this is test
data that is approximately 80MB. It will expand to over 200MB in the
database. During the downloaad and database building up to 1GB of disk
space may be used temporarily.

Once you fetch the data you can connect to the database with the
`cites_db()` command. If you have the **dplyr** package installed, you
can use the `cites_shipments()` command to load a remote `tibble` that
is backed by the database but not loaded into R. You can use this to
analyze CITES data without ever loading it into memory, then gather your
results with `collect()`:

``` r
library(citesdb)
library(dplyr)
start <- Sys.time()

cites_shipments() %>%
  group_by(year) %>%
  summarize(number = n()) %>%
  arrange(desc(year)) %>% 
  collect()
#> # A tibble: 42 x 2
#>     year number
#>    <int>  <dbl>
#>  1  2016    687
#>  2  2015 349952
#>  3  2014 301666
#>  4  2013 121023
#>  5  2012 251963
#>  6  2011 276767
#>  7  2010 266920
#>  8  2009  27563
#>  9  2007 264187
#> 10  2006 258962
#> # … with 32 more rows

stop <- Sys.time()
```

The back-end database, [MonetDB](https://monetdb.org) is very fast and
powerful, making such analyses quite snappy even on such large data and
normal desktops and laptops. Here’s the timing of the above query on an
ordinary laptop:

``` r
stop - start
#> Time difference of 1.664912 secs
```

If you are using RStudio interactively, loading the CITES package also
brings up a browable pane in the “Connections” tab that lets you explore
and preview the database, as well.

### Contributing

Want have feedback or want to contribute? Great\! Please take a look at
the [contributing
guidelines](https://github.com/ecohealthalliance/citesdb/blob/master/.github/CONTRIBUTING.md)
before filing an issue or pull request.

Please note that this project is released with a [Contributor Code of
Conduct](https://github.com/ecohealthalliance/citesdb/blob/master/.github/CODE_OF_CONDUCT.md).
By participating in this project you agree to abide by its terms.
