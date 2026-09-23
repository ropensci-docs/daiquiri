# Changelog

## daiquiri 1.2.1 (2025-10-27)

CRAN release: 2025-10-27

No User-facing changes

## daiquiri 1.2.0 (2025-06-24)

CRAN release: 2025-06-24

### New features

- New
  [`field_types_advanced()`](https://docs.ropensci.org/daiquiri/reference/field_types_advanced.md)
  function. Allows just a subset of the columns in the source df to be
  named explicitly in the specification, with the remaining columns set
  to the `.default_field_type` parameter.
  ([\#16](https://github.com/ropensci/daiquiri/issues/16))

### Bug fixes and minor improvements

- Improved scaling of heatmaps when there are lots of fields.
  ([\#17](https://github.com/ropensci/daiquiri/issues/17))

## daiquiri 1.1.1 (2023-07-18)

CRAN release: 2023-07-18

### New features

- New
  [`ft_strata()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
  field type, which when specified, produces a report where the
  aggregated data and individual data fields plots are stratified by the
  [`ft_strata()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
  column’s distinct values

- Column-specific strings for missing values can now be set in the
  [`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md)
  specification ([\#13](https://github.com/ropensci/daiquiri/issues/13))

### Bug fixes and minor improvements

- Categorical data fields now show a heatmap plot as well as the
  individual time series plots when `aggregate_by_each_category` option
  is set to `TRUE`

- Categorical data fields now retain special characters in labels when
  `aggregate_by_each_category` option is set to `TRUE`

- Print method of `daiquiri_object` now displays the location the report
  was saved to

- When a data field contains all missing values, this now shows
  correctly in the various tabs
  ([\#12](https://github.com/ropensci/daiquiri/issues/12))

- When running package from within rmarkdown/quarto (rmd/qmd) files, the
  parent file can now contain a chunk labelled `setup` without causing
  an error. ([\#7](https://github.com/ropensci/daiquiri/issues/7))

- Hex logo now appears on reports, adding dependency to `xfun`

## daiquiri 1.0.3 (2022-12-06)

CRAN release: 2022-12-06

### Bug fixes and minor improvements

- Validation warnings now match column names correctly when
  `field_types` are specified in a different order to the supplied data
  frame columns

- Passing in a data frame containing integer columns no longer causes an
  aggregation error
  ([\#9](https://github.com/ropensci/daiquiri/issues/9))

- Calling functions with package prefix no longer causes an error
  ([\#10](https://github.com/ropensci/daiquiri/issues/10))

- Fixed (some) errors about duplicate chunk labels when running package
  from within rmarkdown/quarto (rmd/qmd) files (bug introduced in
  previous release 1.0.2). This now allows chunks in the parent file to
  be unlabelled but unfortunately still errors when there is a chunk
  labelled `setup`.
  ([\#7](https://github.com/ropensci/daiquiri/issues/7))

## daiquiri 1.0.2 (2022-11-21)

CRAN release: 2022-11-21

- When rendering reports, intermediate files are now written to
  [`tempdir()`](https://rdrr.io/r/base/tempfile.html) instead of to the
  directory of the `report_htmldoc.Rmd` file (the default behaviour of
  [`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html)).
  This fixes errors caused when the library location is read-only.

## daiquiri 1.0.1 (2022-11-11)

CRAN release: 2022-11-11

First release to CRAN

- Replaced calls to deprecated function `aes_string()` in `ggplot2`

## daiquiri 1.0.0 (2022-11-01)

This release incorporates changes requested for acceptance into
<https://ropensci.org/>. There are many breaking changes as objects have
been renamed for better consistency and style.

### Breaking changes

- [`daiquiri_report()`](https://docs.ropensci.org/daiquiri/reference/daiquiri_report.md)
  replaces `create_report()` and some parameters have been renamed.

- [`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md)
  replaces `fieldtypes()`.

- [`prepare_data()`](https://docs.ropensci.org/daiquiri/reference/prepare_data.md),
  [`aggregate_data()`](https://docs.ropensci.org/daiquiri/reference/aggregate_data.md),
  and
  [`report_data()`](https://docs.ropensci.org/daiquiri/reference/report_data.md)
  parameters have been renamed.

- [`initialise_log()`](https://docs.ropensci.org/daiquiri/reference/initialise_log.md)
  replaces `log_initialise()`.

- [`close_log()`](https://docs.ropensci.org/daiquiri/reference/close_log.md)
  replaces `log_close()`.

- [`template_field_types()`](https://docs.ropensci.org/daiquiri/reference/template_field_types.md)
  replaces `fieldtypes_template()`

### Bug fixes and minor improvements

- Fixed error when user passes in a `data.table` (to
  [`daiquiri_report()`](https://docs.ropensci.org/daiquiri/reference/daiquiri_report.md)
  or
  [`prepare_data()`](https://docs.ropensci.org/daiquiri/reference/prepare_data.md))
  that contains non-character columns.

- `daiquiri_report` (formerly `create_report()`) and
  [`report_data()`](https://docs.ropensci.org/daiquiri/reference/report_data.md)
  accept a new parameter `report_title`.

- [`report_data()`](https://docs.ropensci.org/daiquiri/reference/report_data.md)
  now accepts `...` parameter to be passed through to
  [`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html).

- [`close_log()`](https://docs.ropensci.org/daiquiri/reference/close_log.md)
  now returns the path to the closed log file (if any).

- `example_prescriptions.csv` replaces `example_dataset.csv` as the
  example dataset supplied with the package.

## daiquiri 0.7.0 (2022-04-20)

This release moves the reading of csv files out into a separate function
in order to make it more configurable and to handle the parsing of all
fields as character data for the user.

### Breaking changes

- `create_report()` now only accepts a dataframe as the first parameter.
  The `textfile_contains_columnnames` parameter has been removed.

- `load_data()` has been replaced with
  [`read_data()`](https://docs.ropensci.org/daiquiri/reference/read_data.md)
  and
  [`prepare_data()`](https://docs.ropensci.org/daiquiri/reference/prepare_data.md).

- `log_initialise()` function: `dirpath` parameter renamed to
  `log_directory`.

### New features

- New function
  [`read_data()`](https://docs.ropensci.org/daiquiri/reference/read_data.md)
  reads data from a delimited file, with all columns read in as
  character type.

- New function
  [`prepare_data()`](https://docs.ropensci.org/daiquiri/reference/prepare_data.md)
  validates a dataframe against a fieldtypes specification, and prepares
  it for aggregation.

- `create_report()` accepts a new parameter `dataset_shortdesc` for the
  user to specify a dataset description to appear on the report.

- [`export_aggregated_data()`](https://docs.ropensci.org/daiquiri/reference/export_aggregated_data.md)
  function accepts new `save_fileprefix` parameter.

- New function `fieldtypes_template()` generates template code for
  creating a fieldtypes specification based on an existing dataframe,
  and outputs it to the console.

### Bug fixes and minor improvements

- Fixed ALL_FIELDS_COMBINED calculated field rowsumming NAs incorrectly.

- Fixed plots failing when all values are missing.

- Fixed `log_message()` trying to write to different log file when
  called from Rmd folder (and relative path used).

- Made ‘\[DUPLICATES\]’ and ‘\[ALL_FIELDS_COMBINED\]’ reserved names for
  data fields.

- Allow column names in supplied dataframe to contain special
  characters.

- Reduced real estate at top of report.

- Removed datatype column and fixed validation warnings total from
  Source data tab in report.

- Updated example data.

- Added further validation checks for user-supplied params.

- Added CITATION file.

## daiquiri 0.6.1 (2022-02-23)

Beta release. Complete list of functions exported:

- [`aggregate_data()`](https://docs.ropensci.org/daiquiri/reference/aggregate_data.md)
- `create_report()` accepts either a dataframe or csv filename as the
  first parameter. This may change in future.
- [`export_aggregated_data()`](https://docs.ropensci.org/daiquiri/reference/export_aggregated_data.md)
- [`field_types()`](https://docs.ropensci.org/daiquiri/reference/field_types.md)
- [`ft_categorical()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
- [`ft_datetime()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
- [`ft_freetext()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
- [`ft_ignore()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
- [`ft_numeric()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
- [`ft_simple()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
- [`ft_timepoint()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
- [`ft_uniqueidentifier()`](https://docs.ropensci.org/daiquiri/reference/field_types_available.md)
- `load_data()` accepts either a dataframe or csv filename as the first
  parameter. This may change in future.
- `log_close()`
- `log_initialise()`
- [`report_data()`](https://docs.ropensci.org/daiquiri/reference/report_data.md)
