# Initialise a log file

Choose a directory in which to save the log file. If this is not called,
no log file is created.

## Usage

``` r
initialise_log(log_directory)
```

## Arguments

- log_directory:

  String containing directory to save log file

## Value

Character string containing the full path to the newly-created log file

## Examples

``` r
log_name <- initialise_log(".")

log_name
#> [1] "/__w/ropensci/ropensci/ropensci_docs_website/reference/daiquiri_20260817%H1249.log"
```
