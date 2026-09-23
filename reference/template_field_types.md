# Print a template field_types() specification to console

Helper function to generate template code for a
[`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md)
specification, based on the supplied data frame. All fields (columns) in
the specification will be defined using the `default_field_type`, and
the console output can be copied and edited before being used as input
to
[`daiquiri_report()`](https://docs.ropensci.org/daiquiri/reference/daiquiri_report.md)
or
[`prepare_data()`](https://docs.ropensci.org/daiquiri/reference/prepare_data.md).

## Usage

``` r
template_field_types(df, default_field_type = ft_ignore())
```

## Arguments

- df:

  data frame including the column names for the template specification

- default_field_type:

  `field_type` to be used for each column. Default =
  [`ft_ignore()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md).
  See
  [`field_types_available()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)

## Value

(invisibly) Character string containing the template code

## See also

[`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md)

## Examples

``` r
df <- data.frame(
  col1 = rep("2022-01-01", 5),
  col2 = rep(1, 5),
  col3 = 1:5,
  col4 = rnorm(5)
)

template_field_types(df, default_field_type = ft_numeric())
#> field_types(
#>   "col1" = ft_numeric(),
#>   "col2" = ft_numeric(),
#>   "col3" = ft_numeric(),
#>   "col4" = ft_numeric()
#> )
```
