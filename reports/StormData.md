# Impact of Storm Events on Public Health and Economics
Hugo van den Berg  
August 21, 2016  



## Synopsis

In this report I aim to identify the impact of storm events on public health
and economics from (a subset of) the [NOAA Storm Database][1], collected
between 1950 and 2011.

## 1. Data Processing

### 1.1 Libraries

For this analysis we used a range of libraries, which are listed below with
their respective version numbers.
Note this block was weaved with the option `message=FALSE` to hide startup
messages.


```r
# Reading data
library(readr) # v1.0.0

# Munging data
library(magrittr) # v1.5
library(tidyr) # v0.6.0
library(plyr) # v1.8.4
library(dplyr) # v0.5.0
library(lubridate) # v1.5.6
library(stringr) # v1.1.0

# Imputation
library(mice) # v2.25

# Plotting data
library(ggplot2) # v2.1.0
```

### 1.2 Raw data collection

The [NOAA Storm Database][1] is publicly available but for this report make use
of a dedicated subset specially provided for the Coursera course *[Reproducible
Research][2]*.


```r
data.url <- paste0('https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2F',
                   'StormData.csv.bz2')
data.raw <- file.path('data', 'StormData.csv.bz2')
if (!file.exists(data.raw)) {
    download.file(url = data.url, destfile = data.raw, mode = "wb")
}

data <- read_csv(file = data.raw,
                 col_types = 'dcccdcccdccccdcdccdddddddcdccccddddcc',
                 progress = FALSE)
```

### 1.3 Cleaning the data

The raw data contains various variables that are not in optimal form to perform
further analyses.

#### Dates

Dates in the original data are scattered over a number of variables, describing
dates in various different timezones.
To create uniform dates, that can be handled consistently, all dates and times
are converted to `POSIXct` objects.

Unfortunately timezones are hard to get right, and the conversion functions
don't support reading the timezone from the inputstring.
Therefore the dataset is split along the `TIME_ZONE` variable, after which for
each split the dates are converted before merging the data back together.


```r
date.data <- split(x = data, f = toupper(data$TIME_ZONE))
for (tz in seq_along(date.data)) {
    date.data[tz] %>% class
}
```

#### Event types

The event types contain a lot of misspelled event types.
The [codebook][3] describes a list of all event types in the `EVTYPE` variable
and a mapping to more consistent names, which is stored in 
[EVTYPE.csv][4].
After renaming the event types they are split on the "/"-character into a
major and minor event type, under the assumption that the major event type is
listed first in the `EVTYPE` variable.


```r
evtypes <- read_csv(file.path('data', 'EVTYPE.csv'), col_types = 'cc')
data %<>% 
    mutate(EVTYPE = mapvalues(EVTYPE, 
                              from = evtypes$from, 
                              to = evtypes$to)) %>%
    separate(EVTYPE, into = c("event.type.major", "event.type.minor"), 
             sep = '/', extra = 'merge', fill = "right")
```

## 2. Results

[1]: http://www.ncdc.noaa.gov/stormevents/ 
[2]: https://www.coursera.org/learn/reproducible-research
[3]: https://github.com/Hugovdberg/StormData/blob/master/doc/CodeBook.md
[4]: https://github.com/Hugovdberg/StormData/blob/master/data/EVTYPE.csv