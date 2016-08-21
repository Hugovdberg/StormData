# Storm Data - Code Book
Hugo van den Berg  
August 21, 2016  



## Introduction

This document describes the variables of the Storm Database, including
possible ranges.
As the original data seem to be poorly documented this code book describes
both the [original data](#original-data) as well as the 
[preprocessed data](#preprocessed-data).
Although with the preprocessed variables is described on what original 
variable(s) it is based, the exact transformation can be found in the
[main report](../reports/StormData.md)

Since it is an R Markdown based codebook, this actually generates the
transformation files for factor variables used in the main report.

## Original data

### STATE__

`STATE__` is a numeric variable, ranging from [x1, x2].
Its meaning is unclear, it appears to be a numeric representation of the state
abbreviation (see [STATE](#state)).

### BGN_DATE

`BGN_DATE` is a date variable ranging from 1950-01-03 until 2011-11-30.
It describes the start date of a weather event in the local timezone (see
[TIME_ZONE](#time_zone)).
Note that the original file includes a time in this variable.
However this is 00:00:00 throughout the file and can be ignored, and the value
from the [BGN_TIME](#bgn_time) should be used instead.

### BGN_TIME

`BGN_TIME` is time variable containing the local time of day for the start of a
weather event.
It should be used together with [BGN_DATE](#bgn_date) and
[TIME_ZONE](#time_zone).

### TIME_ZONE

`TIME_ZONE` is a three letter code describing the timezone in which the event
occurred.

### COUNTY

`COUNTY` is a numeric variable ranging from 0 to 873, describing the county in
which the weather event started.

### COUNTYNAME

### STATE

<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Sun Aug 21 15:52:49 2016 -->
<table >
<tr> <th> State Code </th> <th> State Name </th>  </tr>
  <tr> <td> AL </td> <td> Alabama </td> </tr>
  <tr> <td> AK </td> <td> Alaska </td> </tr>
  <tr> <td> AZ </td> <td> Arizona </td> </tr>
  <tr> <td> AR </td> <td> Arkansas </td> </tr>
  <tr> <td> CA </td> <td> California </td> </tr>
  <tr> <td> CO </td> <td> Colorado </td> </tr>
  <tr> <td> CT </td> <td> Connecticut </td> </tr>
  <tr> <td> DE </td> <td> Delaware </td> </tr>
  <tr> <td> FL </td> <td> Florida </td> </tr>
  <tr> <td> GA </td> <td> Georgia </td> </tr>
  <tr> <td> HI </td> <td> Hawaii </td> </tr>
  <tr> <td> ID </td> <td> Idaho </td> </tr>
  <tr> <td> IL </td> <td> Illinois </td> </tr>
  <tr> <td> IN </td> <td> Indiana </td> </tr>
  <tr> <td> IA </td> <td> Iowa </td> </tr>
  <tr> <td> KS </td> <td> Kansas </td> </tr>
  <tr> <td> KY </td> <td> Kentucky </td> </tr>
  <tr> <td> LA </td> <td> Louisiana </td> </tr>
  <tr> <td> ME </td> <td> Maine </td> </tr>
  <tr> <td> MD </td> <td> Maryland </td> </tr>
  <tr> <td> MA </td> <td> Massachusetts </td> </tr>
  <tr> <td> MI </td> <td> Michigan </td> </tr>
  <tr> <td> MN </td> <td> Minnesota </td> </tr>
  <tr> <td> MS </td> <td> Mississippi </td> </tr>
  <tr> <td> MO </td> <td> Missouri </td> </tr>
  <tr> <td> MT </td> <td> Montana </td> </tr>
  <tr> <td> NE </td> <td> Nebraska </td> </tr>
  <tr> <td> NV </td> <td> Nevada </td> </tr>
  <tr> <td> NH </td> <td> New Hampshire </td> </tr>
  <tr> <td> NJ </td> <td> New Jersey </td> </tr>
  <tr> <td> NM </td> <td> New Mexico </td> </tr>
  <tr> <td> NY </td> <td> New York </td> </tr>
  <tr> <td> NC </td> <td> North Carolina </td> </tr>
  <tr> <td> ND </td> <td> North Dakota </td> </tr>
  <tr> <td> OH </td> <td> Ohio </td> </tr>
  <tr> <td> OK </td> <td> Oklahoma </td> </tr>
  <tr> <td> OR </td> <td> Oregon </td> </tr>
  <tr> <td> PA </td> <td> Pennsylvania </td> </tr>
  <tr> <td> RI </td> <td> Rhode Island </td> </tr>
  <tr> <td> SC </td> <td> South Carolina </td> </tr>
  <tr> <td> SD </td> <td> South Dakota </td> </tr>
  <tr> <td> TN </td> <td> Tennessee </td> </tr>
  <tr> <td> TX </td> <td> Texas </td> </tr>
  <tr> <td> UT </td> <td> Utah </td> </tr>
  <tr> <td> VT </td> <td> Vermont </td> </tr>
  <tr> <td> VA </td> <td> Virginia </td> </tr>
  <tr> <td> WA </td> <td> Washington </td> </tr>
  <tr> <td> WV </td> <td> West Virginia </td> </tr>
  <tr> <td> WI </td> <td> Wisconsin </td> </tr>
  <tr> <td> WY </td> <td> Wyoming </td> </tr>
  <tr> <td> GU </td> <td> Guam </td> </tr>
   </table>

### EVTYPE

### BGN_RANGE

### BGN_AZI

### BGN_LOCATI

### END_DATE

### END_TIME

### COUNTY_END

### COUNTYENDN

### END_RANGE

### END_AZI

### END_LOCATI

### LENGTH

### WIDTH

### F

### MAG

### FATALITIES

### INJURIES

### PROPDMG

### PROPDMGEXP

### CROPDMG

### CROPDMGEXP

### WFO

### STATEOFFIC

### ZONENAMES

### LATITUDE

### LONGITUDE

### LATITUDE_E

### LONGITUDE_

### REMARKS

### REFNUM

## Preprocessed data

### event.type

`event.type` is a factor variable containing the uniform event types, based on
the original [EVTYPE](#evtype) and the listed transformations.

### state

### begin.date

### begin.county

### begin.latitude

### begin.longitude

### end.date

### end.county

### end.latitude

### end.longitude

### fatalities

### injuries

### property.damages

### crop.damages
