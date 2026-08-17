# Prepare source data

Validate a data frame against a
[`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md)
specification, and prepare for aggregation.

## Usage

``` r
prepare_data(
  df,
  field_types,
  override_column_names = FALSE,
  na = c("", "NA", "NULL"),
  dataset_description = NULL,
  show_progress = TRUE
)
```

## Arguments

- df:

  A data frame

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
  values. Default = `c("","NA","NULL")`. Additional column-specific
  values can be specified in the
  [`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md)
  object

- dataset_description:

  Short description of the dataset being checked. This will appear on
  the report. If blank, the name of the data frame object will be used

- show_progress:

  Print progress to console. Default = `TRUE`

## Value

A `daiquiri_source_data` object

## See also

[`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md),
[`field_types_available()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md),
[`aggregate_data()`](https://docs.ropensci.org/daiquiri/reference/aggregate_data.md),
[`report_data()`](https://docs.ropensci.org/daiquiri/reference/report_data.md),
[`daiquiri_report()`](https://docs.ropensci.org/daiquiri/reference/daiquiri_report.md)

## Examples

``` r
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
  dataset_description = "Example data provided with package"
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

source_data
#> Dataset: Example data provided with package 
#> 
#> Overall:
#> Columns in source: 8 
#> Columns imported: 7 
#> Rows in source: 8996 
#> Duplicate rows removed: 1 
#> Rows imported: 8993 
#> Column used for timepoint: PrescriptionDate 
#> Min timepoint value: 2021-01-01 
#> Max timepoint value: 2021-12-31 23:00:00 
#> Rows missing timepoint values removed: 2 
#> Strings interpreted as missing values: "","NULL" 
#> Total validation warnings: 8 
#> 
#> Datafields:
#>         field_name       field_type  datatype count    missing
#> 1 PrescriptionID   uniqueidentifier character  8993 0 (0%)    
#> 2 PrescriptionDate timepoint        double     8993 0 (0%)    
#> 3 AdmissionDate    datetime         double     4991 4002 (45%)
#> 4 Drug             freetext         character  8993 0 (0%)    
#> 5 Dose             numeric          double     8984 9 (0.1%)  
#> 6 DoseUnit         categorical      character  8964 29 (0.3%) 
#> 7 PatientID        ignore           NA           NA NA        
#> 8 Location         categorical      character  8993 0 (0%)    
#>                     min                 max validation_warnings
#> 1                 10000                9999                   0
#> 2            2021-01-01 2021-12-31 23:00:00                   2
#> 3            1800-01-01          2021-12-31                   1
#> 4 Abacavir + lamiVUDine          vancomycin                   0
#> 5                   0.2               7e+05                   5
#> 6              MegaUnit             unit(s)                   0
#> 7                    NA                  NA                  NA
#> 8                 SITE1               SITE4                   0
#> 
#> Validation warnings:
#> 
#>          field_name                                              message
#>              <char>                                               <char>
#> 1: PrescriptionDate          Missing or invalid value in Timepoint field
#> 2:    AdmissionDate            expected valid date, but got '2021-06-31'
#> 3:             Dose      expected no trailing characters, but got '1.5g'
#> 4:             Dose expected no trailing characters, but got '4.5 grams'
#> 5:             Dose        expected a double, but got 'See Instructions'
#> 6:             Dose expected no trailing characters, but got '80/400 mg'
#>    instances
#>        <int>
#> 1:         2
#> 2:         1
#> 3:         1
#> 4:         1
#> 5:         2
#> 6:         1
```
