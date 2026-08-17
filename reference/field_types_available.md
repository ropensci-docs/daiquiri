# Types of data fields available for specification

Each column in the source dataset must be assigned to a particular
`ft_xx` depending on the type of data that it contains. This is done
through a
[`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md)
specification.

## Usage

``` r
ft_timepoint(includes_time = TRUE, format = "", na = NULL)

ft_uniqueidentifier(na = NULL)

ft_categorical(aggregate_by_each_category = FALSE, na = NULL)

ft_numeric(na = NULL)

ft_datetime(includes_time = TRUE, format = "", na = NULL)

ft_freetext(na = NULL)

ft_simple(na = NULL)

ft_strata(na = NULL)

ft_ignore()
```

## Arguments

- includes_time:

  If `TRUE`, additional aggregated values will be generated using the
  time portion (and if no time portion is present then midnight will be
  assumed). If `FALSE`, aggregated values will ignore any time portion.
  Default = `TRUE`

- format:

  Where datetime values are not in the format `YYYY-MM-DD` or
  `YYYY-MM-DD HH:MM:SS`, an alternative format can be specified at the
  per field level, using
  [`readr::col_datetime()`](https://readr.tidyverse.org/reference/parse_datetime.html)
  format specifications, e.g. `format = "%d/%m/%Y"`. When a format is
  supplied, it must match the complete string.

- na:

  Column-specific vector of strings that should be interpreted as
  missing values (in addition to those specified at dataset level)

- aggregate_by_each_category:

  If `TRUE`, aggregated values will be generated for each distinct
  subcategory as well as for the field overall. If `FALSE`, aggregated
  values will only be generated for the field overall. Default = `FALSE`

## Value

A `field_type` object denoting the type of data in the column

## Details

`ft_timepoint()` - identifies the data field which should be used as the
independent time variable. There should be one and only one of these
specified.

`ft_uniqueidentifier()` - identifies data fields which contain a
(usually computer-generated) identifier for an entity, e.g. a patient.
It does not need to be unique within the dataset.

`ft_categorical()` - identifies data fields which should be treated as
categorical.

`ft_numeric()` - identifies data fields which contain numeric values
that should be treated as continuous. Any values which contain
non-numeric characters (including grouping marks) will be classed as
non-conformant

`ft_datetime()` - identifies data fields which contain date values that
should be treated as continuous.

`ft_freetext()` - identifies data fields which contain free text values.
Only presence/missingness will be evaluated.

`ft_simple()` - identifies data fields where you only want
presence/missingness to be evaluated (but which are not necessarily free
text).

`ft_strata()` - identifies a categorical data field which should be used
to stratify the rest of the data.

`ft_ignore()` - identifies data fields which should be ignored. These
will not be loaded.

## See also

[`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md),
[`template_field_types()`](https://docs.ropensci.org/daiquiri/reference/template_field_types.md)

## Examples

``` r
fts <- field_types(
  PatientID = ft_uniqueidentifier(),
  TestID = ft_ignore(),
  TestDate = ft_timepoint(),
  TestName = ft_categorical(aggregate_by_each_category = FALSE),
  TestResult = ft_numeric(),
  ResultDate = ft_datetime(),
  ResultComment = ft_freetext(),
  Location = ft_categorical()
)

ft_simple()
#> $type
#> [1] "simple"
#> 
#> $collector
#> <collector_character>
#> 
#> $data_class
#> [1] "character"
#> 
#> $aggregation_functions
#> [1] "n"            "missing_n"    "missing_perc"
#> 
#> $na
#> NULL
#> 
#> $options
#> NULL
#> 
#> attr(,"class")
#> [1] "daiquiri_field_type_simple" "daiquiri_field_type"       
```
