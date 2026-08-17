# Generate report from existing objects

Generate report from previously-created `daiquiri_source_data` and
`daiquiri_aggregated_data` objects

## Usage

``` r
report_data(
  source_data,
  aggregated_data,
  report_title = "daiquiri data quality report",
  save_directory = ".",
  save_filename = NULL,
  format = "html",
  show_progress = TRUE,
  ...
)
```

## Arguments

- source_data:

  A `daiquiri_source_data` object returned from
  [`prepare_data()`](https://docs.ropensci.org/daiquiri/reference/prepare_data.md)
  function

- aggregated_data:

  A `daiquiri_aggregated_data` object returned from
  [`aggregate_data()`](https://docs.ropensci.org/daiquiri/reference/aggregate_data.md)
  function

- report_title:

  Title to appear on the report

- save_directory:

  String specifying directory in which to save the report. Default is
  current directory.

- save_filename:

  String specifying filename for the report, excluding any file
  extension. If no filename is supplied, one will be automatically
  generated with the format `daiquiri_report_YYMMDD_HHMMSS`.

- format:

  File format of the report. Currently only `"html"` is supported

- show_progress:

  Print progress to console. Default = `TRUE`

- ...:

  Further parameters to be passed to
  [`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html).
  Cannot include any of `input`, `output_dir`, `output_file`, `params`,
  `quiet`.

## Value

A string containing the name and path of the saved report

## See also

[`prepare_data()`](https://docs.ropensci.org/daiquiri/reference/prepare_data.md),
[`aggregate_data()`](https://docs.ropensci.org/daiquiri/reference/aggregate_data.md),
[`daiquiri_report()`](https://docs.ropensci.org/daiquiri/reference/daiquiri_report.md)

## Examples

``` r
# \donttest{
# load example data into a data.frame
raw_data <- read_data(
  system.file("extdata", "example_prescriptions.csv", package = "daiquiri"),
  delim = ",",
  col_names = TRUE
)

# validate and prepare the data for aggregation
source_data <- prepare_data(
  raw_data,
  field_types = field_types(
    PrescriptionID = ft_uniqueidentifier(),
    PrescriptionDate = ft_timepoint(),
    AdmissionDate = ft_datetime(includes_time = FALSE),
    Drug = ft_freetext(),
    Dose = ft_numeric(),
    DoseUnit = ft_categorical(),
    PatientID = ft_ignore(),
    Location = ft_categorical(aggregate_by_each_category = TRUE)
  ),
  override_column_names = FALSE,
  na = c("", "NULL"),
  dataset_description = "Example data provided with package",
  show_progress = TRUE
)
#> field_types supplied:
#> PrescriptionID   <uniqueidentifier>
#> PrescriptionDate <timepoint>   options: includes_time
#> AdmissionDate    <datetime>
#> Drug <freetext>
#> Dose <numeric>
#> DoseUnit <categorical>
#> PatientID    <ignore>
#> Location <categorical> options: aggregate_by_each_category
#>  
#> Checking column names against field_types... 
#> Importing source data [Example data provided with package]... 
#> Removing column-specific na values... 
#> Checking data against field_types... 
#>   Selecting relevant warnings... 
#>   Identifying nonconformant values... 
#>   Checking and removing missing timepoints... 
#> Checking for duplicates... 
#>   Sorting data... 
#> Loading into source_data structure... 
#>   PrescriptionID 
#>   PrescriptionDate 
#>   AdmissionDate 
#>   Drug 
#>   Dose 
#>   DoseUnit 
#>   PatientID 
#>   Location 
#> Finished 

# aggregate the data
aggregated_data <- aggregate_data(
  source_data,
  aggregation_timeunit = "day",
  show_progress = TRUE
)
#> Aggregating [] by [day]... 
#> Aggregating overall dataset... 
#> Aggregating each data_field in turn... 
#> 1: PrescriptionID 
#> Preparing... 
#> Aggregating character field... 
#>   By n 
#>   By missing_n 
#>   By missing_perc 
#>   By min_length 
#>   By max_length 
#>   By mean_length 
#> Finished 
#> 2: PrescriptionDate 
#> Preparing... 
#> Aggregating double field... 
#>   By n 
#>   By midnight_n 
#>   By midnight_perc 
#> Finished 
#> 3: AdmissionDate 
#> Preparing... 
#> Aggregating double field... 
#>   By n 
#>   By missing_n 
#>   By missing_perc 
#>   By nonconformant_n 
#>   By nonconformant_perc 
#>   By min 
#>   By max 
#> Finished 
#> 4: Drug 
#> Preparing... 
#> Aggregating character field... 
#>   By n 
#>   By missing_n 
#>   By missing_perc 
#> Finished 
#> 5: Dose 
#> Preparing... 
#> Aggregating double field... 
#>   By n 
#>   By missing_n 
#>   By missing_perc 
#>   By nonconformant_n 
#>   By nonconformant_perc 
#>   By min 
#>   By max 
#>   By mean 
#>   By median 
#> Finished 
#> 6: DoseUnit 
#> Preparing... 
#> Aggregating character field... 
#>   By n 
#>   By missing_n 
#>   By missing_perc 
#>   By distinct 
#> Finished 
#> 7: Location 
#> Preparing... 
#> Aggregating character field... 
#>   By n 
#>   By missing_n 
#>   By missing_perc 
#>   By distinct 
#>   By subcat_n 
#>     4 categories found 
#>     1: SITE1 
#>     2: SITE2 
#>     3: SITE3 
#>     4: SITE4 
#>   By subcat_perc 
#>     4 categories found 
#>     1: SITE1 
#>     2: SITE2 
#>     3: SITE3 
#>     4: SITE4 
#> Finished 
#> Aggregating calculated fields... 
#> [DUPLICATES]: 
#> Preparing... 
#> Aggregating integer field... 
#>   By sum 
#>   By nonzero_perc 
#> Finished 
#> [ALL_FIELDS_COMBINED]: 
#> Finished 

# save a report in the current directory using the previously-created objects
report_data(
  source_data,
  aggregated_data,
  report_title = "daiquiri data quality report",
  save_directory = ".",
  save_filename = "example_data_report",
  show_progress = TRUE
)
#> Generating html report... 
#> 
#> 
#> processing file: report_htmldoc.Rmd
#> 1/36                                            
#> 2/36 [daiquiri-setup]                           
#> 3/36                                            
#> 4/36 [daiquiri-styles]                          
#> 5/36                                            
#> 6/36 [daiquiri-strata-info]                     
#> 7/36                                            
#> 8/36 [daiquiri-source-data]                     
#> 9/36                                            
#> 10/36 [daiquiri-fields-imported]                 
#> 11/36                                            
#> 12/36 [daiquiri-fields-ignored]                  
#> 13/36                                            
#> 14/36 [daiquiri-validation-warnings]             
#> 15/36                                            
#> 16/36 [daiquiri-source-data-summary]             
#> 17/36                                            
#> 18/36 [daiquiri-aggregated-data]                 
#> 19/36                                            
#> 20/36 [daiquiri-aggregated-data-set-fig-height]  
#> 21/36                                            
#> 22/36 [daiquiri-overview-strata]                 
#> 23/36                                            
#> 24/36 [daiquiri-overview-presence]               
#> 25/36                                            
#> 26/36 [daiquiri-overview-missing]                
#> 27/36                                            
#> 28/36 [daiquiri-overview-nonconformant]          
#> 29/36                                            
#> 30/36 [daiquiri-overview-duplicates]             
#> 31/36                                            
#> 32/36 [daiquiri-aggregated-data-summary]         
#> 33/36                                            
#> 34/36 [daiquiri-individual-fields-set-fig-height]
#> 35/36                                            
#> 36/36 [daiquiri-individual-fields]               
#> output file: report_htmldoc.knit.md
#> /usr/local/bin/pandoc +RTS -K512m -RTS report_htmldoc.knit.md --to html4 --from markdown+autolink_bare_uris+tex_math_single_backslash --output /__w/ropensci/ropensci/ropensci_docs_website/reference/example_data_report.html --lua-filter /github/home/R/x86_64-pc-linux-gnu-library/4.6/rmarkdown/rmarkdown/lua/pagebreak.lua --lua-filter /github/home/R/x86_64-pc-linux-gnu-library/4.6/rmarkdown/rmarkdown/lua/latex-div.lua --lua-filter /github/home/R/x86_64-pc-linux-gnu-library/4.6/rmarkdown/rmarkdown/lua/table-classes.lua --embed-resources --standalone --variable bs3=TRUE --section-divs --template /github/home/R/x86_64-pc-linux-gnu-library/4.6/rmarkdown/rmd/h/default.html --syntax-highlighting none --variable highlightjs=1 --variable theme=bootstrap --mathjax --variable 'mathjax-url=https://mathjax.rstudio.com/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML' --include-in-header /tmp/RtmpT0RsRB/rmarkdown-str5da647565f0.html 
#> 
#> Output created: example_data_report.html
#> Report saved to: ./example_data_report.html 
#> [1] "./example_data_report.html"
# }
```
