# daiquiri: Data Quality Reporting for Temporal Datasets

Generate reports that enable quick visual review of temporal shifts in
record-level data. Time series plots showing aggregated values are
automatically created for each data field (column) depending on its
contents (e.g. min/max/mean values for numeric data, no. of distinct
values for categorical data), as well as overviews for missing values,
non-conformant values, and duplicated rows. The resulting reports are
shareable and can contribute to forming a transparent record of the
entire analysis process. It is designed with Electronic Health Records
in mind, but can be used for any type of record-level temporal data
(i.e. tabular data where each row represents a single "event", one
column contains the "event date", and other columns contain any
associated values for the event).

## See also

Useful links:

- <https://github.com/ropensci/daiquiri>

- <https://ropensci.github.io/daiquiri/>

- Report bugs at <https://github.com/ropensci/daiquiri/issues>

## Author

**Maintainer**: T. Phuong Quan <phuongquan567@outlook.com>
([ORCID](https://orcid.org/0000-0001-8566-1817))

Other contributors:

- Jack Cregan \[contributor\]

- University of Oxford \[copyright holder\]

- National Institute for Health Research (NIHR) \[funder\]

- Brad Cannell \[reviewer\]
