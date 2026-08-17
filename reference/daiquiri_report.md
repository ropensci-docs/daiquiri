# Create a data quality report from a data frame

Accepts record-level data from a data frame, validates it against the
expected type of content of each column, generates a collection of time
series plots for visual inspection, and saves a report to disk.

## Usage

``` r
daiquiri_report(
  df,
  field_types,
  override_column_names = FALSE,
  na = c("", "NA", "NULL"),
  dataset_description = NULL,
  aggregation_timeunit = "day",
  report_title = "daiquiri data quality report",
  save_directory = ".",
  save_filename = NULL,
  show_progress = TRUE,
  log_directory = NULL
)
```

## Arguments

- df:

  A data frame. Rectangular data can be read from file using
  [`read_data()`](https://docs.ropensci.org/daiquiri/reference/read_data.md).
  See Details.

- field_types:

  [`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md)
  object specifying names and types of fields (columns) in the supplied
  `df`. See also
  [field_types_available](https://docs.ropensci.org/daiquiri/reference/field_types_available.md).

- override_column_names:

  If `FALSE`, column names in the supplied `df` must match the names
  specified in `field_types` exactly. If `TRUE`, column names in the
  supplied `df` will be replaced with the names specified in
  `field_types`. The specification must therefore contain the columns in
  the correct order. Default = `FALSE`

- na:

  vector containing strings that should be interpreted as missing
  values, Default = `c("","NA","NULL")`.

- dataset_description:

  Short description of the dataset being checked. This will appear on
  the report. If blank, the name of the data frame object will be used

- aggregation_timeunit:

  Unit of time to aggregate over. Specify one of `"day"`, `"week"`,
  `"month"`, `"quarter"`, `"year"`. The `"week"` option is Monday-based.
  Default = `"day"`

- report_title:

  Title to appear on the report

- save_directory:

  String specifying directory in which to save the report. Default is
  current directory.

- save_filename:

  String specifying filename for the report, excluding any file
  extension. If no filename is supplied, one will be automatically
  generated with the format `daiquiri_report_YYMMDD_HHMMSS`.

- show_progress:

  Print progress to console. Default = `TRUE`

- log_directory:

  String specifying directory in which to save log file. If no directory
  is supplied, progress is not logged.

## Value

A list containing information relating to the supplied parameters as
well as the resulting `daiquiri_source_data` and
`daiquiri_aggregated_data` objects.

## Details

In order for the package to detect any non-conformant values in numeric
or datetime fields, these should be present in the data frame in their
raw character format. Rectangular data from a text file will
automatically be read in as character type if you use the
[`read_data()`](https://docs.ropensci.org/daiquiri/reference/read_data.md)
function. Data frame columns that are not of class character will still
be processed according to the `field_types` specified.

## See also

[`read_data()`](https://docs.ropensci.org/daiquiri/reference/read_data.md),
[`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md),
[`field_types_available()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)

## Examples

``` r
# \donttest{
# load example data into a data.frame
raw_data <- read_data(
  system.file("extdata", "example_prescriptions.csv", package = "daiquiri"),
  delim = ",",
  col_names = TRUE
)

# create a report in the current directory
daiq_obj <- daiquiri_report(
  raw_data,
  field_types = field_types(
    PrescriptionID = ft_uniqueidentifier(),
    PrescriptionDate = ft_timepoint(),
    AdmissionDate = ft_datetime(includes_time = FALSE, na = "1800-01-01"),
    Drug = ft_freetext(),
    Dose = ft_numeric(),
    DoseUnit = ft_categorical(),
    PatientID = ft_ignore(),
    Location = ft_categorical(aggregate_by_each_category = TRUE)
  ),
  override_column_names = FALSE,
  na = c("", "NULL"),
  dataset_description = "Example data provided with package",
  aggregation_timeunit = "day",
  report_title = "daiquiri data quality report",
  save_directory = ".",
  save_filename = "example_data_report",
  show_progress = TRUE,
  log_directory = NULL
)
#> field_types supplied:
#> PrescriptionID   <uniqueidentifier>
#> PrescriptionDate <timepoint>   options: includes_time
#> AdmissionDate    <datetime>    na: "1800-01-01"
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
#> /usr/local/bin/pandoc +RTS -K512m -RTS report_htmldoc.knit.md --to html4 --from markdown+autolink_bare_uris+tex_math_single_backslash --output /__w/ropensci/ropensci/ropensci_docs_website/reference/example_data_report.html --lua-filter /github/home/R/x86_64-pc-linux-gnu-library/4.6/rmarkdown/rmarkdown/lua/pagebreak.lua --lua-filter /github/home/R/x86_64-pc-linux-gnu-library/4.6/rmarkdown/rmarkdown/lua/latex-div.lua --lua-filter /github/home/R/x86_64-pc-linux-gnu-library/4.6/rmarkdown/rmarkdown/lua/table-classes.lua --embed-resources --standalone --variable bs3=TRUE --section-divs --template /github/home/R/x86_64-pc-linux-gnu-library/4.6/rmarkdown/rmd/h/default.html --syntax-highlighting none --variable highlightjs=1 --variable theme=bootstrap --mathjax --variable 'mathjax-url=https://mathjax.rstudio.com/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML' --include-in-header /tmp/RtmpT0RsRB/rmarkdown-str5da3c09be33.html 
#> 
#> Output created: example_data_report.html
#> Report saved to: ./example_data_report.html 
# }
```
