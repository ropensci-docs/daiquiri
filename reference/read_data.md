# Read delimited data for optimal use with daiquiri

Popular file readers such as
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html)
perform datatype conversion by default, which can interfere with
daiquiri's ability to detect non-conformant values. Use this function
instead to ensure optimal compatibility with daiquiri's features.

## Usage

``` r
read_data(
  file,
  delim = NULL,
  col_names = TRUE,
  quote = "\"",
  trim_ws = TRUE,
  comment = "",
  skip = 0,
  n_max = Inf,
  show_progress = TRUE
)
```

## Arguments

- file:

  A string containing path of file containing data to load, or a URL
  starting `http://`, `file://`, etc. Compressed files with extension
  `.gz`, `.bz2`, `.xz` and `.zip` are supported.

- delim:

  Single character used to separate fields within a record. E.g. `","`
  or `"\t"`

- col_names:

  Either `TRUE`, `FALSE` or a character vector of column names. If
  `TRUE`, the first row of the input will be used as the column names,
  and will not be included in the data frame. If `FALSE`, column names
  will be generated automatically. Default = `TRUE`

- quote:

  Single character used to quote strings.

- trim_ws:

  Should leading and trailing whitespace be trimmed from each field?

- comment:

  A string used to identify comments. Any text after the comment
  characters will be silently ignored

- skip:

  Number of lines to skip before reading data. If `comment` is supplied
  any commented lines are ignored after skipping

- n_max:

  Maximum number of lines to read.

- show_progress:

  Display a progress bar? Default = `TRUE`

## Value

A data frame

## Details

This function is aimed at non-expert users of R, and operates as a
restricted implementation of
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html).
If you prefer to use `read_delim()` directly, ensure you set the
following parameters: `col_types = readr::cols(.default = "c")` and
`na = character()`

## See also

[`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md),
[`field_types_available()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md),
[`aggregate_data()`](https://docs.ropensci.org/daiquiri/reference/aggregate_data.md),
[`report_data()`](https://docs.ropensci.org/daiquiri/reference/report_data.md),
[`daiquiri_report()`](https://docs.ropensci.org/daiquiri/reference/daiquiri_report.md)

## Examples

``` r
raw_data <- read_data(
  system.file("extdata", "example_prescriptions.csv", package = "daiquiri"),
  delim = ",",
  col_names = TRUE
)

head(raw_data)
#> # A tibble: 6 × 8
#>   PrescriptionID PrescriptionDate   AdmissionDate Drug  Dose  DoseUnit PatientID
#>   <chr>          <chr>              <chr>         <chr> <chr> <chr>    <chr>    
#> 1 6000           2021-01-01 00:00:… 2020-12-31    Ceft… 500   mg       4993679  
#> 2 6001           NULL               2020-12-31    Fluc… 1000  mg       819452   
#> 3 6002           NULL               2020-12-30    Teic… 400   mg       275597   
#> 4 6003           2021-01-01 01:00:… 1800-01-01    Fluc… 1000  NULL     819452   
#> 5 6004           2021-01-01 02:00:… 1800-01-01    Fluc… 1000  NULL     528071   
#> 6 6005           2021-01-01 03:00:… 2020-12-30    Co-a… 1.2   g        1001434  
#> # ℹ 1 more variable: Location <chr>
```
