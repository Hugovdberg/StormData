# Impact of Storm Events on Public Health and Economics
Hugo van den Berg  
August 21, 2016  



## Synopsis

In this report we aim to identify the impact of storm events on public health
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

# Imputation
library(mice) # v2.25

# Plotting data
library(ggplot2) # v2.1.0
```

### 1.2 Raw data collection

The [NOAA Storm Database][1] is publicly available but for this report make use
of a dedicated subset specially provided for the Coursera course *[Reproducible
Research][2]*.
To speed up processing the data an intermediate file is generated 


```r
data.url <- paste0('https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2F',
                   'StormData.csv.bz2')
data.raw <- file.path('data', 'StormData.csv.bz2')
data.prep <- file.path('cache', 'StormData.csv')
useCache <- file.exists(data.prep)
if (useCache) {
    data <- read_csv(data.prep)
} else {
    if (!file.exists(data.raw)) {
        download.file(url = data.url, destfile = data.raw, mode = "wb")
    }
    
    data <- read_csv(file = data.raw,
                     col_types = 'dcccdcccdccccdcdccdddddddcdccccddddcc',
                     progress = FALSE)
}
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   STATE__ = col_double(),
##   COUNTY = col_double(),
##   BGN_RANGE = col_double(),
##   COUNTY_END = col_double(),
##   END_RANGE = col_double(),
##   LENGTH = col_double(),
##   WIDTH = col_double(),
##   F = col_double(),
##   MAG = col_double(),
##   FATALITIES = col_double(),
##   INJURIES = col_double(),
##   PROPDMG = col_double(),
##   CROPDMG = col_double(),
##   LATITUDE = col_double(),
##   LONGITUDE = col_double(),
##   LATITUDE_E = col_double(),
##   LONGITUDE_ = col_double(),
##   REFNUM = col_double()
## )
```

```
## See spec(...) for full column specifications.
```
Test

```r
if (!useCache) {
    write_csv(data[sample(1:nrow(data), 10000, replace = FALSE),], data.prep)
}
summary(data)
```

```
##     STATE__        BGN_DATE           BGN_TIME          TIME_ZONE        
##  Min.   : 1.00   Length:10000       Length:10000       Length:10000      
##  1st Qu.:19.00   Class :character   Class :character   Class :character  
##  Median :30.00   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :31.27                                                           
##  3rd Qu.:45.00                                                           
##  Max.   :95.00                                                           
##                                                                          
##      COUNTY     COUNTYNAME           STATE              EVTYPE         
##  Min.   :  0   Length:10000       Length:10000       Length:10000      
##  1st Qu.: 31   Class :character   Class :character   Class :character  
##  Median : 75   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :101                                                           
##  3rd Qu.:129                                                           
##  Max.   :856                                                           
##                                                                        
##    BGN_RANGE        BGN_AZI           BGN_LOCATI          END_DATE        
##  Min.   : 0.000   Length:10000       Length:10000       Length:10000      
##  1st Qu.: 0.000   Class :character   Class :character   Class :character  
##  Median : 0.000   Mode  :character   Mode  :character   Mode  :character  
##  Mean   : 1.501                                                           
##  3rd Qu.: 1.000                                                           
##  Max.   :70.000                                                           
##                                                                           
##    END_TIME           COUNTY_END  COUNTYENDN          END_RANGE     
##  Length:10000       Min.   :0    Length:10000       Min.   : 0.000  
##  Class :character   1st Qu.:0    Class :character   1st Qu.: 0.000  
##  Mode  :character   Median :0    Mode  :character   Median : 0.000  
##                     Mean   :0                       Mean   : 1.009  
##                     3rd Qu.:0                       3rd Qu.: 0.000  
##                     Max.   :0                       Max.   :70.000  
##                                                                     
##    END_AZI           END_LOCATI            LENGTH        
##  Length:10000       Length:10000       Min.   :  0.0000  
##  Class :character   Class :character   1st Qu.:  0.0000  
##  Mode  :character   Mode  :character   Median :  0.0000  
##                                        Mean   :  0.2131  
##                                        3rd Qu.:  0.0000  
##                                        Max.   :118.8000  
##                                                          
##      WIDTH                F              MAG           FATALITIES     
##  Min.   :   0.000   Min.   :0.000   Min.   :  0.00   Min.   : 0.0000  
##  1st Qu.:   0.000   1st Qu.:0.000   1st Qu.:  0.00   1st Qu.: 0.0000  
##  Median :   0.000   Median :1.000   Median : 50.00   Median : 0.0000  
##  Mean   :   6.956   Mean   :0.948   Mean   : 46.88   Mean   : 0.0125  
##  3rd Qu.:   0.000   3rd Qu.:1.000   3rd Qu.: 75.00   3rd Qu.: 0.0000  
##  Max.   :2500.000   Max.   :5.000   Max.   :600.00   Max.   :11.0000  
##                     NA's   :9379                                      
##     INJURIES          PROPDMG        PROPDMGEXP           CROPDMG      
##  Min.   :  0.000   Min.   :  0.00   Length:10000       Min.   :  0.00  
##  1st Qu.:  0.000   1st Qu.:  0.00   Class :character   1st Qu.:  0.00  
##  Median :  0.000   Median :  0.00   Mode  :character   Median :  0.00  
##  Mean   :  0.101   Mean   : 12.09                      Mean   :  1.48  
##  3rd Qu.:  0.000   3rd Qu.:  0.70                      3rd Qu.:  0.00  
##  Max.   :200.000   Max.   :995.00                      Max.   :850.00  
##                                                                        
##   CROPDMGEXP            WFO             STATEOFFIC       
##  Length:10000       Length:10000       Length:10000      
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##   ZONENAMES            LATITUDE      LONGITUDE       LATITUDE_E  
##  Length:10000       Min.   :   0   Min.   :    0   Min.   :   0  
##  Class :character   1st Qu.:2626   1st Qu.: 7115   1st Qu.:   0  
##  Mode  :character   Median :3534   Median : 8644   Median :   0  
##                     Mean   :2851   Mean   : 6872   Mean   :1425  
##                     3rd Qu.:4017   3rd Qu.: 9600   3rd Qu.:3533  
##                     Max.   :6247   Max.   :17031   Max.   :6248  
##                     NA's   :1                      NA's   :1     
##    LONGITUDE_      REMARKS              REFNUM      
##  Min.   :    0   Length:10000       Min.   :    86  
##  1st Qu.:    0   Class :character   1st Qu.:225988  
##  Median :    0   Mode  :character   Median :449886  
##  Mean   : 3446                      Mean   :452061  
##  3rd Qu.: 8704                      3rd Qu.:678959  
##  Max.   :17052                      Max.   :902282  
## 
```

### 1.3 Cleaning the data

The raw data contains various variables that are not in optimal form to perform
further analyses.


## 2. Results


[1]: http://www.ncdc.noaa.gov/stormevents/ 
[2]: https://www.coursera.org/learn/reproducible-research
