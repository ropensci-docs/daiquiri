# Export aggregated data

Export aggregated data to disk. Creates a separate file for each
aggregated field in dataset.

## Usage

``` r
export_aggregated_data(
  aggregated_data,
  save_directory,
  save_file_prefix = "",
  save_file_type = "csv"
)
```

## Arguments

- aggregated_data:

  A `daiquiri_aggregated_data` object

- save_directory:

  String. Full or relative path for save folder

- save_file_prefix:

  String. Optional prefix for the exported filenames

- save_file_type:

  String. Filetype extension supported by `readr`, currently only csv
  allowed

## Value

(invisibly) The `daiquiri_aggregated_data` object that was passed in

## Examples

``` r
# \donttest{
raw_data <- read_data(
  system.file("extdata", "example_prescriptions.csv", package = "daiquiri"),
  delim = ",",
  col_names = TRUE
)

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
  na = c("", "NULL")
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
#> Importing source data [NULL]... 
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

aggregated_data <- aggregate_data(
  source_data,
  aggregation_timeunit = "day"
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

export_aggregated_data(
  aggregated_data,
  save_directory = ".",
  save_file_prefix = "ex_"
)

# }
```
