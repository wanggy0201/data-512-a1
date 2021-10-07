# data-512-a1

Welcome to the Wikipedia monthly traffic analysis project!

## Goal
The goal of this project is to construct, analyze, and publish a dataset of monthly traffic on English Wikipedia from January 1 2008 through August 30 2021. The page view data will be obtained through WikiMedia public APIs.

## Data source
We will be using the WikiMedia public APIs to query historic page view data for Wikipedia, the use of data complies with the terms of use for WikiMedka and its REST API:

* WikiMedia terms of use: https://foundation.wikimedia.org/wiki/Terms_of_Use/en
* WikiMedia REST API terms and conditions: https://www.mediawiki.org/wiki/Wikimedia_REST_API#Terms_and_conditions

Two APIs will be used in this project:

* Legacy Pagecounts: https://wikitech.wikimedia.org/wiki/Analytics/AQS/Legacy_Pagecounts
  The Legacy Pagecounts API provides access to desktop and mobile traffic data from December 2007 through July 2016.
* Pageviews: https://wikitech.wikimedia.org/wiki/Analytics/AQS/Legacy_Pagecounts
The Pageviews API provides access to desktop, mobile web, and mobile app traffic data from July 2015 through last month.

## Project structure

### Python notebook (under `src/`)
This will be the main noteboook that contains code that performs data acquisition, data processing and analysis. The notebook `hcds-a1-data-curation.ipynb` will be the main execution point for the project.

### Raw data (under `raw-data/`)
This directory contains raw data files that contains direct output from the WikiMedia APIs. For each API call, the raw data will be stored separately in different json files. They will following the name format of 

```
apiname_accesstype_firstmonth-lastmonth.json
```

### Procssed data (under `processed-data/`)
This directory contains the csv file that is converted and cleaned from the raw data files. This file will contain page view counts obtained at different weeks and from different APIs. It will follow the following schema:

| Column                  | Value     | Description                                              |
|-------------------------|-----------|----------------------------------------------------------|
| year                    | YYYY      | Year of weekly timestamp                                 |
| month                   | MM        | Month of weekly timestamp                                |
| pagecount_all_views     | num_views | Total page views from the legacy API, desktop + mobile   |
| pagecount_desktop_views | num_views | Total page views from the legacy API, desktop only       |
| pagecount_mobile_views  | num_views | Total page views from the legacy API, mobile only        |
| pageview_all_views      | num_views | Total page views from the pageview API, desktop + mobile |
| pageview_desktop_views  | num_views | Total page views from the pageview API, desktop only     |
| pageview_mobile_views   | num_views | Total page views from the pageview API, mobile only      |

### Analysis results (under `/results`)
This directory contains analysis results, where currently in this project we only have a time series visualization graph.

## How to run the project
To run the project, open the notebook `hcds-a1-data-curation` under `src/`. Select `Cell -> Run All`. This will generate the analysis results and all intermediate datasets including the raw data and procssed data.

## Callouts for the project

### Duplicate dates in the raw data
From the legacy API, the results contained duplicate dates, for example there were multiple entires for Jan 2008, and the view counts were close but not the same. In this project we will take the last entry returned by the API, since we cannot determine which count will be closer to truth.

### Explanation on gap between the two APIs
The Pageview API excludes spiders/crawlers for desktop counts, while data from the Pagecounts API does not. This is why you will see a gap between the lines for `pagecount_all_views` and `pageview_all_views` in the plot, while the mobile time series lines roughly overlap across the 2 APIs.