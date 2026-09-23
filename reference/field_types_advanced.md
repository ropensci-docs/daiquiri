# Create field_types_advanced specification

Specify only a subset of the names and types of fields in the source
data frame. The remaining fields will be given the same 'default' type.

## Usage

``` r
field_types_advanced(..., .default_field_type = ft_simple())
```

## Arguments

- ...:

  names and types of fields (columns) in source data.

- .default_field_type:

  `field_type` to use for any remaining fields (columns) in source data.
  Note, this means there can not be a field in the data named
  `.default_field_type`

## Value

A `field_types` object

## See also

[`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md),
[`field_types_available()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md),
[`template_field_types()`](https://docs.ropensci.org/daiquiri/reference/template_field_types.md)

## Examples

``` r
fts <- field_types_advanced(
  PrescriptionDate = ft_timepoint(),
  PatientID = ft_ignore(),
  .default_field_type = ft_simple()
)

fts
#> PrescriptionDate <timepoint>   options: includes_time
#> PatientID    <ignore>
#> .default_field_type  <simple>
```
