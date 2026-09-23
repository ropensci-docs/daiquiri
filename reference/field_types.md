# Create field_types specification

Specify the names and types of fields in the source data frame. This is
important because the data in each field will be aggregated in different
ways, depending on its `field_type`. See
[field_types_available](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)

## Usage

``` r
field_types(...)
```

## Arguments

- ...:

  names and types of fields (columns) in source data.

## Value

A `field_types` object

## See also

[`field_types_available()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md),
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

fts
#> PatientID    <uniqueidentifier>
#> TestID   <ignore>
#> TestDate <timepoint>   options: includes_time
#> TestName <categorical>
#> TestResult   <numeric>
#> ResultDate   <datetime>    options: includes_time
#> ResultComment    <freetext>
#> Location <categorical>
```
