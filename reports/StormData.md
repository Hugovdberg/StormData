Impact of Storm Events on Public Health and Economics
================
Hugo van den Berg
August 21, 2016

-   [Synopsis](#synopsis)
-   [1. Data Processing](#data-processing)
    -   [1.1 Libraries](#libraries)
    -   [1.2 Raw data collection](#raw-data-collection)
    -   [1.3 Cleaning the data](#cleaning-the-data)
-   [2. Results](#results)
    -   [2.1 Personal impact](#personal-impact)
    -   [2.2 Financial impact](#financial-impact)

Synopsis
--------

In this report I aim to identify the impact of storm events on public health and economics from (a subset of) the [NOAA Storm Database](http://www.ncdc.noaa.gov/stormevents/), collected between 1950 and 2011. The dataset contains a number of variables describing discrete weather events, on time, location, and trajectory, as well as the number of victims (direct or indirect), and damages to properties and crops.

The analysis is divided in two parts, the first of which identifies which type of events has caused the largest total number of victims, as well as per single event. The second part is focussed on the financial impact of weather events, again in total over all recorded events, as well as per event.

From the data I found that most victims are caused by tornadoes, thunderstorm winds, and excessive heat. The greatest financial impact is caused by floods, hurricanes and tornadoes.

The fact that both high profile events of tornadoes and hurricanes cause great damage is not surprising. What does surprise me is that hurricanes don't even occur in the top 10 of events ranked by number of victims.

1. Data Processing
------------------

The first step of the analysis is to retrieve the data and make sure it is in a format suitable for further analysis.

### 1.1 Libraries

For this analysis we used a range of libraries, which are listed below with their respective version numbers. Note this block was knitted with the option `message=FALSE` to hide startup messages.

``` r
# Reading data
library(readr) # v1.0.0

# Munging data
library(magrittr) # v1.5
library(tidyr) # v0.6.0
library(plyr) # v1.8.4
library(dplyr) # v0.5.0
library(lubridate) # v1.5.6
library(stringr) # v1.1.0

# Display data
library(ggplot2) # v2.1.0
library(xtable)
```

### 1.2 Raw data collection

The [NOAA Storm Database](http://www.ncdc.noaa.gov/stormevents/) is publicly available but for this report make use of a dedicated subset specially provided for the Coursera course *[Reproducible Research](https://www.coursera.org/learn/reproducible-research)*.

``` r
data.url <- paste0('https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2F',
                   'StormData.csv.bz2')
data.raw <- file.path('data', 'StormData.csv.bz2')
if (!file.exists(data.raw)) {
    download.file(url = data.url, destfile = data.raw, mode = "wb")
}

data <- read_csv(file = data.raw,
                 col_types = 'dcccdcccdccccdcdccdddddddcdccccddddcc',
                 progress = FALSE)
rm(data.url, data.raw)
```

The raw dataset has dimensions 902297 by 37.

### 1.3 Cleaning the data

The raw data contains various variables that are not in optimal form to perform further analyses.

#### Event types

The event types contain a lot of misspelled event types. The [codebook](https://github.com/Hugovdberg/StormData/blob/master/doc/CodeBook.md) describes a list of all event types in the `EVTYPE` variable and a mapping to more consistent names, which is stored in [EVTYPE.csv](https://github.com/Hugovdberg/StormData/blob/master/data/EVTYPE.csv). After renaming the event types they are split on the "/"-character into a major and minor event type, under the assumption that the major event type is listed first in the `EVTYPE` variable.

``` r
evtypes <- read_csv(file.path('data', 'EVTYPE.csv'), col_types = 'cc')
data %<>% 
    mutate(EVTYPE = mapvalues(EVTYPE, 
                              from = evtypes$from, 
                              to = evtypes$to)) %>%
    separate(EVTYPE, into = c("event.type.major", "event.type.minor"), 
             sep = '/', extra = 'merge', fill = "right") %>%
    mutate_at(vars(event.type.major, event.type.minor), funs(as.factor))
rm(evtypes)
```

#### Damages to Properties and Crops

``` r
data %<>%
    mutate(PROPDMGEXP = ifelse(toupper(PROPDMGEXP) %in% c("K", "M", "B"),
                               toupper(PROPDMGEXP),
                               "1"),
           PROPDMGEXP = mapvalues(PROPDMGEXP,
                                  c("K",   "M",   "B"),
                                  c("1e3", "1e6", "1e9")),
           property.damages = PROPDMG * as.numeric(PROPDMGEXP),
           CROPDMGEXP = ifelse(toupper(CROPDMGEXP) %in% c("K", "M", "B"),
                               toupper(CROPDMGEXP),
                               "1"),
           CROPDMGEXP = mapvalues(CROPDMGEXP,
                                  c("K",   "M",   "B"),
                                  c("1e3", "1e6", "1e9")),
           crop.damages = CROPDMG * as.numeric(CROPDMGEXP))
```

#### Relevant variables

Now all relevant variables have been cleaned up the final selection of the features can be made.

``` r
data %<>% select(
    event.type.major,
    FATALITIES, INJURIES,
    property.damages, crop.damages
)
names(data) %<>% tolower
```

The final dataset has dimensions 902297 by 5 and contains 0 incomplete observations.

``` r
print(xtable(summary(data)), type = "html")
```

<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Wed Aug 24 11:11:07 2016 -->
<table border=1>
<tr>
<th>
</th>
<th>
          event.type.major </th> <th>   fatalities </th> <th>    injuries </th> <th> property.damages </th> <th>  crop.damages </th>  </tr>

<tr>
<td align="right">
1
</td>
<td>
Thunderstorm wind:324612
</td>
<td>
Min. : 0.0000
</td>
<td>
Min. : 0.0000
</td>
<td>
Min. :0.000e+00
</td>
<td>
Min. :0.000e+00
</td>
</tr>
<tr>
<td align="right">
2
</td>
<td>
Hail :288667
</td>
<td>
1st Qu.: 0.0000
</td>
<td>
1st Qu.: 0.0000
</td>
<td>
1st Qu.:0.000e+00
</td>
<td>
1st Qu.:0.000e+00
</td>
</tr>
<tr>
<td align="right">
3
</td>
<td>
Tornado : 60658
</td>
<td>
Median : 0.0000
</td>
<td>
Median : 0.0000
</td>
<td>
Median :0.000e+00
</td>
<td>
Median :0.000e+00
</td>
</tr>
<tr>
<td align="right">
4
</td>
<td>
Flash flood : 55032
</td>
<td>
Mean : 0.0168
</td>
<td>
Mean : 0.1557
</td>
<td>
Mean :4.736e+05
</td>
<td>
Mean :5.442e+04
</td>
</tr>
<tr>
<td align="right">
5
</td>
<td>
Flood : 26094
</td>
<td>
3rd Qu.: 0.0000
</td>
<td>
3rd Qu.: 0.0000
</td>
<td>
3rd Qu.:5.000e+02
</td>
<td>
3rd Qu.:0.000e+00
</td>
</tr>
<tr>
<td align="right">
6
</td>
<td>
High wind : 21782
</td>
<td>
Max. :583.0000
</td>
<td>
Max. :1700.0000
</td>
<td>
Max. :1.150e+11
</td>
<td>
Max. :5.000e+09
</td>
</tr>
<tr>
<td align="right">
7
</td>
<td>
(Other) :125452
</td>
<td>
</td>
<td>
</td>
<td>
</td>
<td>
</td>
</tr>
</table>
2. Results
----------

The cleaned up dataset can be used to answer the following two questions:

1.  Across the United States, which types of events (as indicated in the EVTYPE variable) are most harmful with respect to population health?
2.  Across the United States, which types of events have the greatest economic consequences?

### 2.1 Personal impact

``` r
top_events <- data %>%
    group_by(event.type.major) %>%
    summarise(victims = sum(fatalities) + sum(injuries)) %>%
    top_n(10, victims) %>%
    droplevels()
event_levels <- levels(top_events$event.type.major)[order(top_events$victims,
                                                          decreasing = TRUE)]
personal <- data %>%
    filter(event.type.major %in% top_events$event.type.major) %>%
    select(event.type.major, fatalities, injuries) %>%
    mutate(
        event.type.major = ordered(event.type.major, levels = event_levels)
        ) %>%
    gather(impact, victims, fatalities, injuries) %>%
    group_by(event.type.major, impact) %>%
    summarise(sum = sum(victims, na.rm = TRUE),
              mean = mean(victims, na.rm = TRUE)) %>%
    gather(func, victims, sum, mean) %>%
    mutate(func = factor(func, levels = c("sum", "mean"),
                         labels = c("Total", "Avg. per event")))
```

``` r
personal %>%
    ggplot(aes(x = event.type.major, y = victims, fill = impact)) +
    geom_bar(stat = "identity", position = "dodge") +
    facet_grid(func ~ ., scales = "free_y")  +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1)) +
    scale_y_log10() +
    scale_fill_discrete(name = "Type of victim",
                        labels = c("Fatalities", "Injuries")) +
    coord_cartesian(xlim = c(0.8, 10.2)) +
    annotation_logticks(sides = "rl", alpha = 0.3) +
    labs(title = "Top 10 weather event types, ranked by total number of victims",
         x = "Major event type",
         y = "Number of victims")
```

![](StormData_files/figure-markdown_github/unnamed-chunk-1-1.png)

### 2.2 Financial impact

``` r
top_events <- data %>%
    group_by(event.type.major) %>%
    summarise(damages = sum(property.damages) + sum(crop.damages)) %>%
    top_n(10, damages) %>%
    droplevels()
event_levels <- levels(top_events$event.type.major)[order(top_events$damages,
                                                          decreasing = TRUE)]
financial <- data %>%
    filter(event.type.major %in% top_events$event.type.major) %>%
    select(event.type.major, property.damages, crop.damages) %>%
    mutate(
        event.type.major = ordered(event.type.major, levels = event_levels)
        ) %>%
    gather(impact, damages, property.damages, crop.damages) %>%
    group_by(event.type.major, impact) %>%
    summarise(sum = sum(damages, na.rm = TRUE),
              mean = mean(damages, na.rm = TRUE)) %>%
    gather(func, damages, sum, mean) %>%
    mutate(func = factor(func, levels = c("sum", "mean"),
                         labels = c("Total", "Avg. per event")))
```

``` r
financial %>%
    ggplot(aes(x = event.type.major, y = damages, fill = impact)) +
    geom_bar(stat = "identity", position = "dodge") +
    facet_grid(func ~ ., scales = "free_y")  +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1)) +
    scale_y_log10() +
    scale_fill_discrete(name = "Type of damages"
                        , labels = c("Crops", "Properties")
                        ) +
    coord_cartesian(xlim = c(0.8, 10.2)) +
    annotation_logticks(sides = "rl", alpha = 0.3) +
    labs(title = "Top 10 weather event types, ranked by total damages",
         x = "Major event type",
         y = "Damages")
```

![](StormData_files/figure-markdown_github/unnamed-chunk-2-1.png)
