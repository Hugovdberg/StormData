Impact of Storm Events on Public Health and Economics
================
Hugo van den Berg
August 24, 2016

-   [Synopsis](#synopsis)
-   [1. Data Processing](#data-processing)
    -   [1.1 Libraries](#libraries)
    -   [1.2 Raw data collection](#raw-data-collection)
    -   [1.3 Cleaning the data](#cleaning-the-data)
-   [2. Results](#results)
    -   [2.1 Personal impact](#personal-impact)
    -   [2.2 Financial impact](#financial-impact)
-   [3. Conclusion](#conclusion)

Synopsis
--------

In this report I aim to identify the impact of storm events on public health and economics from (a subset of) the [NOAA Storm Database](http://www.ncdc.noaa.gov/stormevents/), collected between 1950 and 2011. The dataset contains a number of variables describing discrete weather events, on time, location, and trajectory, as well as the number of victims (direct or indirect), and damages to properties and crops.

The analysis is divided in two parts, the first of which identifies which type of events has caused the largest total number of victims, as well as per single event. The second part is focussed on the financial impact of weather events, again in total over all recorded events, as well as per event.

From the data I found that most victims are caused by tornadoes, thunderstorm winds, and excessive heat. The greatest financial impact is caused by floods, hurricanes and tornadoes.

The fact that both high profile events of tornadoes and hurricanes cause great damage is not surprising. Surprisingly, hurricanes don't even occur in the top 10 of events ranked by number of victims.

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

The event types contain a lot of misspelled event types. The [codebook](https://github.com/Hugovdberg/StormData/blob/master/doc/CodeBook.md) describes a list of all event types in the `EVTYPE` variable and a mapping to more consistent names, which is stored in [EVTYPE.csv](https://github.com/Hugovdberg/StormData/blob/master/data/EVTYPE.csv) (To prevent flooding the report with 2000(!!) extra lines). After renaming the event types they are split on the "/"-character into a major and minor event type, under the assumption that the major event type is listed first in the `EVTYPE` variable.

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

Financial damages are reported as a numeric value with an associated exponent to express larger magnitudes ("K" for thousands, "M" for millions and "B" for billions). Other values for the exponent are undocumented and are set to "1" for multiplication with the numeric value.

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

The final dataset has dimensions 902297 by 5, and contains 0 observations with missing values.

``` r
html <- print(xtable(summary(data)), type = "html", include.rownames = FALSE,
              sanitize.colnames.function = function(x){gsub('( )+', ' ', x)},
              html.table.attributes = "", print.results = FALSE)
html %<>%
    gsub(':', '</td><td>', ., fixed = TRUE) %>%
    gsub('<td>', '<td style="padding: 3px">', ., fixed = TRUE) %>%
    gsub('<th>', '<th colspan="2">', ., fixed = TRUE)
cat(html)
```

<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Wed Aug 24 22</td><td style="padding: 3px">46</td><td style="padding: 3px">26 2016 -->
<table>
<tr>
<th colspan="2">
event.type.major
</th>
<th colspan="2">
fatalities
</th>
<th colspan="2">
injuries
</th>
<th colspan="2">
property.damages
</th>
<th colspan="2">
crop.damages
</th>
</tr>
<tr>
<td style="padding: 3px">
Thunderstorm wind
</td>
<td style="padding: 3px">
324612
</td>
<td style="padding: 3px">
Min.
</td>
<td style="padding: 3px">
0.0000
</td>
<td style="padding: 3px">
Min.
</td>
<td style="padding: 3px">
0.0000
</td>
<td style="padding: 3px">
Min.
</td>
<td style="padding: 3px">
0.000e+00
</td>
<td style="padding: 3px">
Min.
</td>
<td style="padding: 3px">
0.000e+00
</td>
</tr>
<tr>
<td style="padding: 3px">
Hail
</td>
<td style="padding: 3px">
288667
</td>
<td style="padding: 3px">
1st Qu.
</td>
<td style="padding: 3px">
0.0000
</td>
<td style="padding: 3px">
1st Qu.
</td>
<td style="padding: 3px">
0.0000
</td>
<td style="padding: 3px">
1st Qu.
</td>
<td style="padding: 3px">
0.000e+00
</td>
<td style="padding: 3px">
1st Qu.
</td>
<td style="padding: 3px">
0.000e+00
</td>
</tr>
<tr>
<td style="padding: 3px">
Tornado
</td>
<td style="padding: 3px">
60658
</td>
<td style="padding: 3px">
Median
</td>
<td style="padding: 3px">
0.0000
</td>
<td style="padding: 3px">
Median
</td>
<td style="padding: 3px">
0.0000
</td>
<td style="padding: 3px">
Median
</td>
<td style="padding: 3px">
0.000e+00
</td>
<td style="padding: 3px">
Median
</td>
<td style="padding: 3px">
0.000e+00
</td>
</tr>
<tr>
<td style="padding: 3px">
Flash flood
</td>
<td style="padding: 3px">
55032
</td>
<td style="padding: 3px">
Mean
</td>
<td style="padding: 3px">
0.0168
</td>
<td style="padding: 3px">
Mean
</td>
<td style="padding: 3px">
0.1557
</td>
<td style="padding: 3px">
Mean
</td>
<td style="padding: 3px">
4.736e+05
</td>
<td style="padding: 3px">
Mean
</td>
<td style="padding: 3px">
5.442e+04
</td>
</tr>
<tr>
<td style="padding: 3px">
Flood
</td>
<td style="padding: 3px">
26094
</td>
<td style="padding: 3px">
3rd Qu.
</td>
<td style="padding: 3px">
0.0000
</td>
<td style="padding: 3px">
3rd Qu.
</td>
<td style="padding: 3px">
0.0000
</td>
<td style="padding: 3px">
3rd Qu.
</td>
<td style="padding: 3px">
5.000e+02
</td>
<td style="padding: 3px">
3rd Qu.
</td>
<td style="padding: 3px">
0.000e+00
</td>
</tr>
<tr>
<td style="padding: 3px">
High wind
</td>
<td style="padding: 3px">
21782
</td>
<td style="padding: 3px">
Max.
</td>
<td style="padding: 3px">
583.0000
</td>
<td style="padding: 3px">
Max.
</td>
<td style="padding: 3px">
1700.0000
</td>
<td style="padding: 3px">
Max.
</td>
<td style="padding: 3px">
1.150e+11
</td>
<td style="padding: 3px">
Max.
</td>
<td style="padding: 3px">
5.000e+09
</td>
</tr>
<tr>
<td style="padding: 3px">
(Other)
</td>
<td style="padding: 3px">
125452
</td>
<td style="padding: 3px">
</td>
<td style="padding: 3px">
</td>
<td style="padding: 3px">
</td>
<td style="padding: 3px">
</td>
</tr>
</table>
2. Results
----------

The cleaned up dataset can be used to answer the following two questions:

1.  Across the United States, which types of events are most harmful with respect to population health?
2.  Across the United States, which types of events have the greatest economic consequences?

### 2.1 Personal impact

To address the first question we first select the major event types causing the largest total number of victims. This is done by adding the total number of fatalities and injuries together, per event type. This has to be done in a two-step process since we need to further summarise the data for the ranking than for plotting.

``` r
top_events <- data %>%
    # Calculate number of victims per major event type
    group_by(event.type.major) %>%
    summarise(total_victims = sum(fatalities) + sum(injuries)) %>%
    # Select top 10 events
    top_n(10, total_victims) %>%
    # Arrange by descending number of victims
    arrange(-total_victims) %>%
    # Remove unused factor levels from event.type.major
    droplevels()
# Store event types in descending order
event_levels <- as.character(top_events$event.type.major)
```

Now the order of the top 10 worst events is known we have to select those levels and prepare the data for plotting from worst to least bad. Furthermore, we want to make a distinction between fatalities and injuries, and also between the total number of victims as the average number of victims per single event.

``` r
# Prepare data for plotting
personal <- data %>%
    # Filter out the worst events
    filter(event.type.major %in% top_events$event.type.major) %>%
    # Select the relevant columns for this plot
    select(event.type.major, fatalities, injuries) %>%
    # Reset factor levels to display in correct order in plot
    mutate(event.type.major =
               factor(event.type.major, levels = event_levels)) %>%
    # Make fatalities and injuries two levels of the impact variable
    gather(impact, victims, fatalities, injuries) %>%
    # Calculate number of victims per event.type, impact combination
    group_by(event.type.major, impact) %>%
    summarise(sum = sum(victims),
              mean = mean(victims)) %>%
    # Make sum and mean two levels of the func variable for facetting the plot
    gather(func, victims, sum, mean) %>%
    # Change labels of function for prettier facet labels
    mutate(func = factor(func, levels = c("sum", "mean"),
                         labels = c("Total", "Avg. per event")))
```

Finally, we are able to plot the data and see what it looks like.

``` r
personal %>%
    ggplot(aes(x = event.type.major, y = victims, fill = impact)) +
    # Display as a barplot with fatalities and injuries next to each other
    geom_bar(stat = "identity", position = "dodge") +
    # Display the y-axis on a logarithmic scale with extra ticks for clarity
    scale_y_log10() +
    annotation_logticks(sides = "rl", alpha = 0.3) +
    # Expand the x-limits to make room for the logticks
    coord_cartesian(xlim = c(0.8, 10.2)) +
    # Display total and average above in two facets with separate y-scales
    facet_grid(func ~ ., scales = "free_y")  +
    # Rotate the labels on the x-axis
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1)) +
    # Set the labels for the legend
    scale_fill_brewer(palette = "Set1",
                      name = "Type of victim",
                      labels = c("Fatalities", "Injuries")) +
    # Add labels to the plot
    labs(title = "Top 10 weather event types, ranked by total number of victims",
         x = "Major event type",
         y = "Number of victims")
```

![](StormData_files/figure-markdown_github/plot_personal-1.png)

From this plot it is clear that tornadoes cause the most victims by an order of magnitude more than thunderstorms, both for fatalities and injuries. Also worth noting is that event type *Thunderstorm wind* is ranked higher by total number of victims, even though the average number of victims per event is far lower than for *Excessive heat*.

It also visible that (excessive) heat events on average cause more than one death and several injuries per event. For all other events (except people injured by tornadoes) it is one or more orders of magnitude less. This could signify that people either underestimate the potential effects of heat, or are structurally less able to cope with its effects.

### 2.2 Financial impact

The analysis of the financial impact of weather events is similar to the analysis of the impact on public health. First we rank the events by total damages, and then we create a new subset of the data containing total and mean damages per event type for the top 10 worst weather events.

``` r
top_events <- data %>%
    # Calculate total damages per major event type
    group_by(event.type.major) %>%
    summarise(damages = sum(property.damages) + sum(crop.damages)) %>%
    # Select top 10 worst events
    top_n(10, damages) %>%
    # Arrange by descending total damages
    arrange(-damages) %>%
    # Remove unused factor levels from event.type.major
    droplevels()
# Store event types
event_levels <- as.character(top_events$event.type.major)

# Prepare data for plotting
financial <- data %>%
    # Filter out the worst events
    filter(event.type.major %in% top_events$event.type.major) %>%
    # Select the relevant columns for this plot
    select(event.type.major, property.damages, crop.damages) %>%
    # Reset the factor levels to the predetermined levels
    mutate(event.type.major = 
               factor(event.type.major, levels = event_levels)) %>%
    # Make property and crop damages to levels of the impact variable
    gather(impact, damages, property.damages, crop.damages) %>%
    # Calculate the damages per event.type, impact combination
    group_by(event.type.major, impact) %>%
    summarise(sum = sum(damages),
              mean = mean(damages)) %>%
    # Make sum and mean two levels of the func variable for facetting the plot
    gather(func, damages, sum, mean) %>%
    # Change labels of function for prettier facet labels
    mutate(func = factor(func, levels = c("sum", "mean"),
                         labels = c("Total", "Avg. per event")))
```

``` r
financial %>%
    ggplot(aes(x = event.type.major, y = damages, fill = impact)) +
    # Display as a barplot with crop and property damages next to each other
    geom_bar(stat = "identity", position = "dodge") +
    # Display the y-axis on a logarithmic scale with extra ticks for clarity
    scale_y_log10() +
    annotation_logticks(sides = "rl", alpha = 0.3) +
    # Expand the x-limits to make room for the logticks
    coord_cartesian(xlim = c(0.8, 10.2)) +
    # Display total and average above in two facets with seperate y-scales
    facet_grid(func ~ ., scales = "free_y")  +
    # Rotate the labels on the x-axis
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1)) +
    # Set the labels for the legend
    scale_fill_brewer(palette = "Set1",
                      name = "Type of damages",
                      labels = c("Crops", "Properties")) +
    # Add labels to the plot
    labs(title = "Top 10 weather event types, ranked by total damages",
         x = "Major event type",
         y = "Damages")
```

![](StormData_files/figure-markdown_github/plot_financial-1.png)

When ranked by damages to crops and properties tornadoes are no longer the worst event type, with floods and hurricanes causing more damage, both in total and per event. However the differences seem to be not as stark as seen in the impact on public health, although it might be somewhat obscured by the wider range of the values on the y-axis.

Whereas heat related events appear several times in the top 10 worst events ranked by impact on public health, ranked by impact on economics it are floods and several related types that cause the most damage.

3. Conclusion
-------------

From the analysis of 902297 recorded weather events it is apparent that the impact of weather events on public health on the one hand, and economics on the other hand is quite different. That said, a few event types show in both top 10 lists, the worst of which are tornadoes, being the single worst event type for public health and third worst for economics. Surprisingly hurricanes, while taking the second spot on physical damages, do not cause enough victims to appear in the top 10 ranked by impact on public health.

As a final note, the source materials of this report are available at [GitHub](https://github.com/Hugovdberg/StormData/blob/master/), where the accompanying [codebook](https://github.com/Hugovdberg/StormData/blob/master/doc/CodeBook.md) is published as well (open the links in a new tab when viewing from RPubs, as RPubs fails to open the links within its own frame).
