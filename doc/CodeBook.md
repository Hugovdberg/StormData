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

`COUNTYNAME` is a character variable containing the county or counties in which
the event occurred.
It contains both references to county names and to county codes prepended with
the [state code](#state).

### STATE

`STATE` is a two-letter character variable containing an abbreviated state
name.
It contains 79 unique values, 50 of which can be related to the built in
`state.abb` variable.

Using the combination of state code and [countyname](#countyname) the meaning 
of the other 22 values was inferred.
Below is the ammended list of the used codes with their full description.

<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Mon Aug 22 21:19:31 2016 -->
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

`EVTYPE` is character variable containing the type of weather event that
occurred.
It contains 977 unique values (890 unique values ignoring upper/lower case),
ranging from "?" to "WND".

A large number of misspelled eventtypes was present.
The mapping below has been used to create uniform event types.

<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Mon Aug 22 21:19:31 2016 -->
<table >
<tr> <th> Event type (original) </th> <th> Event type (unified) </th>  </tr>
  <tr> <td> ? </td> <td> Unknown </td> </tr>
  <tr> <td> ABNORMALLY DRY </td> <td> Abnormally dry </td> </tr>
  <tr> <td> ABNORMALLY WET </td> <td> Abnormally wet </td> </tr>
  <tr> <td> ABNORMAL WARMTH </td> <td> Abnormal warmth </td> </tr>
  <tr> <td> ACCUMULATED SNOWFALL </td> <td> Accumulated snowfall </td> </tr>
  <tr> <td> AGRICULTURAL FREEZE </td> <td> Agricultural freeze </td> </tr>
  <tr> <td> APACHE COUNTY </td> <td> apache county </td> </tr>
  <tr> <td> ASTRONOMICAL HIGH TIDE </td> <td> Astronomical high tide </td> </tr>
  <tr> <td> ASTRONOMICAL LOW TIDE </td> <td> Astronomical low tide </td> </tr>
  <tr> <td> AVALANCE </td> <td> Avalanche </td> </tr>
  <tr> <td> AVALANCHE </td> <td> Avalanche </td> </tr>
  <tr> <td> BEACH EROSIN </td> <td> Beach erosion </td> </tr>
  <tr> <td> Beach Erosion </td> <td> Beach erosion </td> </tr>
  <tr> <td> BEACH EROSION </td> <td> Beach erosion </td> </tr>
  <tr> <td> BEACH EROSION/COASTAL FLOOD </td> <td> Beach erosion/Coastal flood </td> </tr>
  <tr> <td> BEACH FLOOD </td> <td> Beach flood </td> </tr>
  <tr> <td> BELOW NORMAL PRECIPITATION </td> <td> Below normal precipitation </td> </tr>
  <tr> <td> BITTER WIND CHILL </td> <td> Bitter wind chill </td> </tr>
  <tr> <td> BITTER WIND CHILL TEMPERATURES </td> <td> Bitter wind chill temperatures </td> </tr>
  <tr> <td> Black Ice </td> <td> Black ice </td> </tr>
  <tr> <td> BLACK ICE </td> <td> Black ice </td> </tr>
  <tr> <td> BLIZZARD </td> <td> Blizzard </td> </tr>
  <tr> <td> BLIZZARD AND EXTREME WIND CHIL </td> <td> Blizzard/Extreme wind chill </td> </tr>
  <tr> <td> BLIZZARD AND HEAVY SNOW </td> <td> Blizzard/Heavy snow </td> </tr>
  <tr> <td> BLIZZARD/FREEZING RAIN </td> <td> Blizzard/Freezing rain </td> </tr>
  <tr> <td> BLIZZARD/HEAVY SNOW </td> <td> Blizzard/Heavy snow </td> </tr>
  <tr> <td> BLIZZARD/HIGH WIND </td> <td> Blizzard/High wind </td> </tr>
  <tr> <td> Blizzard Summary </td> <td> Blizzard summary </td> </tr>
  <tr> <td> BLIZZARD WEATHER </td> <td> Blizzard weather </td> </tr>
  <tr> <td> BLIZZARD/WINTER STORM </td> <td> Blizzard/Winter storm </td> </tr>
  <tr> <td> BLOWING DUST </td> <td> Blowing dust </td> </tr>
  <tr> <td> blowing snow </td> <td> Blowing snow </td> </tr>
  <tr> <td> Blowing Snow </td> <td> Blowing snow </td> </tr>
  <tr> <td> BLOWING SNOW </td> <td> Blowing snow </td> </tr>
  <tr> <td> BLOWING SNOW &amp; EXTREME WIND CH </td> <td> Blowing snow/extreme wind chill </td> </tr>
  <tr> <td> BLOWING SNOW- EXTREME WIND CHI </td> <td> Blowing snow/extreme wind chill </td> </tr>
  <tr> <td> BLOWING SNOW/EXTREME WIND CHIL </td> <td> Blowing snow/extreme wind chill </td> </tr>
  <tr> <td> BLOW-OUT TIDE </td> <td> Blow-out tide </td> </tr>
  <tr> <td> BLOW-OUT TIDES </td> <td> Blow-out tide </td> </tr>
  <tr> <td> BREAKUP FLOODING </td> <td> Breakup flooding </td> </tr>
  <tr> <td> BRUSH FIRE </td> <td> Brush fire </td> </tr>
  <tr> <td> BRUSH FIRES </td> <td> Brush fire </td> </tr>
  <tr> <td> COASTAL EROSION </td> <td> Coastal erosion </td> </tr>
  <tr> <td> Coastal Flood </td> <td> Coastal flood </td> </tr>
  <tr> <td> COASTALFLOOD </td> <td> Coastal flood </td> </tr>
  <tr> <td> COASTAL FLOOD </td> <td> Coastal flood </td> </tr>
  <tr> <td> coastal flooding </td> <td> Coastal flood </td> </tr>
  <tr> <td> Coastal Flooding </td> <td> Coastal flood </td> </tr>
  <tr> <td> COASTAL FLOODING </td> <td> Coastal flood </td> </tr>
  <tr> <td> COASTAL  FLOODING/EROSION </td> <td> Coastal flood/Erosion </td> </tr>
  <tr> <td> COASTAL FLOODING/EROSION </td> <td> Coastal flood/Erosion </td> </tr>
  <tr> <td> Coastal Storm </td> <td> Coastal storm </td> </tr>
  <tr> <td> COASTALSTORM </td> <td> Coastal storm </td> </tr>
  <tr> <td> COASTAL STORM </td> <td> Coastal storm </td> </tr>
  <tr> <td> COASTAL SURGE </td> <td> Coastal surge </td> </tr>
  <tr> <td> COASTAL/TIDAL FLOOD </td> <td> Coastal/Tidal flood </td> </tr>
  <tr> <td> Cold </td> <td> Cold </td> </tr>
  <tr> <td> COLD </td> <td> Cold </td> </tr>
  <tr> <td> COLD AIR FUNNEL </td> <td> Cold air funnel </td> </tr>
  <tr> <td> COLD AIR FUNNELS </td> <td> Cold air funnel </td> </tr>
  <tr> <td> COLD AIR TORNADO </td> <td> Cold air tornado </td> </tr>
  <tr> <td> Cold and Frost </td> <td> Cold/Frost </td> </tr>
  <tr> <td> COLD AND FROST </td> <td> Cold/Frost </td> </tr>
  <tr> <td> COLD AND SNOW </td> <td> Cold/Snow </td> </tr>
  <tr> <td> COLD AND WET CONDITIONS </td> <td> Cold/Wet conditions </td> </tr>
  <tr> <td> Cold Temperature </td> <td> Cold temperature </td> </tr>
  <tr> <td> COLD TEMPERATURES </td> <td> Cold temperature </td> </tr>
  <tr> <td> COLD WAVE </td> <td> Cold wave </td> </tr>
  <tr> <td> COLD WEATHER </td> <td> Cold weather </td> </tr>
  <tr> <td> COLD/WIND CHILL </td> <td> Cold/Wind chill </td> </tr>
  <tr> <td> COLD WIND CHILL TEMPERATURES </td> <td> Cold/Wind chill </td> </tr>
  <tr> <td> COLD/WINDS </td> <td> cold/winds </td> </tr>
  <tr> <td> COOL AND WET </td> <td> Cool/Wet </td> </tr>
  <tr> <td> COOL SPELL </td> <td> Cool spell </td> </tr>
  <tr> <td> CSTL FLOODING/EROSION </td> <td> Coastal flood/Erosion </td> </tr>
  <tr> <td> Damaging Freeze </td> <td> Damaging freeze </td> </tr>
  <tr> <td> DAMAGING FREEZE </td> <td> Damaging freeze </td> </tr>
  <tr> <td> DAM BREAK </td> <td> Dam failure </td> </tr>
  <tr> <td> DAM FAILURE </td> <td> Dam failure </td> </tr>
  <tr> <td> DEEP HAIL </td> <td> Deep hail </td> </tr>
  <tr> <td> DENSE FOG </td> <td> Dense fog </td> </tr>
  <tr> <td> DENSE SMOKE </td> <td> Dense smoke </td> </tr>
  <tr> <td> DOWNBURST </td> <td> Downburst winds </td> </tr>
  <tr> <td> DOWNBURST WINDS </td> <td> Downburst winds </td> </tr>
  <tr> <td> DRIEST MONTH </td> <td> Driest month </td> </tr>
  <tr> <td> Drifting Snow </td> <td> Drifting snow </td> </tr>
  <tr> <td> DROUGHT </td> <td> Drought </td> </tr>
  <tr> <td> DROUGHT/EXCESSIVE HEAT </td> <td> Drought/Excessive heat </td> </tr>
  <tr> <td> DROWNING </td> <td> Drowning </td> </tr>
  <tr> <td> DRY </td> <td> Dry </td> </tr>
  <tr> <td> DRY CONDITIONS </td> <td> Dry conditions </td> </tr>
  <tr> <td> DRY HOT WEATHER </td> <td> Dry hot weather </td> </tr>
  <tr> <td> DRY MICROBURST </td> <td> Dry microburst </td> </tr>
  <tr> <td> DRY MICROBURST 50 </td> <td> Dry microburst 50 </td> </tr>
  <tr> <td> DRY MICROBURST 53 </td> <td> Dry microburst 53 </td> </tr>
  <tr> <td> DRY MICROBURST 58 </td> <td> Dry microburst 58 </td> </tr>
  <tr> <td> DRY MICROBURST 61 </td> <td> Dry microburst 61 </td> </tr>
  <tr> <td> DRY MICROBURST 84 </td> <td> Dry microburst 84 </td> </tr>
  <tr> <td> DRY MICROBURST WINDS </td> <td> Dry microburst winds </td> </tr>
  <tr> <td> DRY MIRCOBURST WINDS </td> <td> Dry mircoburst winds </td> </tr>
  <tr> <td> DRYNESS </td> <td> Dryness </td> </tr>
  <tr> <td> DRY PATTERN </td> <td> Dry pattern </td> </tr>
  <tr> <td> DRY SPELL </td> <td> Dry spell </td> </tr>
  <tr> <td> DRY WEATHER </td> <td> Dry weather </td> </tr>
  <tr> <td> DUST DEVEL </td> <td> Dust devil </td> </tr>
  <tr> <td> Dust Devil </td> <td> Dust devil </td> </tr>
  <tr> <td> DUST DEVIL </td> <td> Dust devil </td> </tr>
  <tr> <td> DUST DEVIL WATERSPOUT </td> <td> Dust devil/Waterspout </td> </tr>
  <tr> <td> DUSTSTORM </td> <td> Dust storm </td> </tr>
  <tr> <td> DUST STORM </td> <td> Dust storm </td> </tr>
  <tr> <td> DUST STORM/HIGH WINDS </td> <td> Dust storm/High winds </td> </tr>
  <tr> <td> EARLY FREEZE </td> <td> Early freeze </td> </tr>
  <tr> <td> Early Frost </td> <td> Early frost </td> </tr>
  <tr> <td> EARLY FROST </td> <td> Early frost </td> </tr>
  <tr> <td> EARLY RAIN </td> <td> Early rain </td> </tr>
  <tr> <td> EARLY SNOW </td> <td> Early snow </td> </tr>
  <tr> <td> Early snowfall </td> <td> Early snowfall </td> </tr>
  <tr> <td> EARLY SNOWFALL </td> <td> Early snowfall </td> </tr>
  <tr> <td> Erosion/Cstl Flood </td> <td> Erosion/Coastal flood </td> </tr>
  <tr> <td> EXCESSIVE </td> <td> Excessive </td> </tr>
  <tr> <td> Excessive Cold </td> <td> Excessive cold </td> </tr>
  <tr> <td> EXCESSIVE HEAT </td> <td> Excessive heat </td> </tr>
  <tr> <td> EXCESSIVE HEAT/DROUGHT </td> <td> Excessive heat/Drought </td> </tr>
  <tr> <td> EXCESSIVELY DRY </td> <td> Excessively dry </td> </tr>
  <tr> <td> EXCESSIVE PRECIPITATION </td> <td> Excessive precipitation </td> </tr>
  <tr> <td> EXCESSIVE RAIN </td> <td> Excessive rain </td> </tr>
  <tr> <td> EXCESSIVE RAINFALL </td> <td> Excessive rainfall </td> </tr>
  <tr> <td> EXCESSIVE SNOW </td> <td> Excessive snow </td> </tr>
  <tr> <td> EXCESSIVE WETNESS </td> <td> Excessive wetness </td> </tr>
  <tr> <td> Extended Cold </td> <td> Extended cold </td> </tr>
  <tr> <td> Extreme Cold </td> <td> Extreme cold </td> </tr>
  <tr> <td> EXTREME COLD </td> <td> Extreme cold </td> </tr>
  <tr> <td> EXTREME COLD/WIND CHILL </td> <td> Extreme cold/Wind chill </td> </tr>
  <tr> <td> EXTREME HEAT </td> <td> Extreme heat </td> </tr>
  <tr> <td> EXTREMELY WET </td> <td> Extremely wet </td> </tr>
  <tr> <td> EXTREME/RECORD COLD </td> <td> Extreme/Record cold </td> </tr>
  <tr> <td> EXTREME WINDCHILL </td> <td> Extreme wind chill </td> </tr>
  <tr> <td> EXTREME WIND CHILL </td> <td> Extreme wind chill </td> </tr>
  <tr> <td> EXTREME WIND CHILL/BLOWING SNO </td> <td> Extreme wind chill/Blowing snow </td> </tr>
  <tr> <td> EXTREME WIND CHILLS </td> <td> Extreme wind chill </td> </tr>
  <tr> <td> EXTREME WINDCHILL TEMPERATURES </td> <td> Extreme wind chill temperatures </td> </tr>
  <tr> <td> FALLING SNOW/ICE </td> <td> Falling snow/ice </td> </tr>
  <tr> <td> FIRST FROST </td> <td> First frost </td> </tr>
  <tr> <td> FIRST SNOW </td> <td> First snow </td> </tr>
  <tr> <td> FLASH FLOOD </td> <td> Flash flood </td> </tr>
  <tr> <td> FLASH FLOOD/ </td> <td> Flash flood </td> </tr>
  <tr> <td> FLASH FLOOD/FLOOD </td> <td> Flash flood/Flood </td> </tr>
  <tr> <td> FLASH FLOOD/ FLOOD </td> <td> Flash flood/Flood </td> </tr>
  <tr> <td> FLASH FLOOD FROM ICE JAMS </td> <td> Flash flood from ice jams </td> </tr>
  <tr> <td> FLASH FLOOD - HEAVY RAIN </td> <td> Flash flood/Heavy rain </td> </tr>
  <tr> <td> FLASH FLOOD/HEAVY RAIN </td> <td> Flash flood/Heavy rain </td> </tr>
  <tr> <td> FLASH FLOODING </td> <td> Flash flood </td> </tr>
  <tr> <td> FLASH FLOODING/FLOOD </td> <td> Flash flood/Flood </td> </tr>
  <tr> <td> FLASH FLOODING/THUNDERSTORM WI </td> <td> Flash flood/Thunderstorm wind </td> </tr>
  <tr> <td> FLASH FLOOD/LANDSLIDE </td> <td> Flash flood/Landslide </td> </tr>
  <tr> <td> FLASH FLOOD LANDSLIDES </td> <td> Flash flood landslides </td> </tr>
  <tr> <td> FLASH FLOODS </td> <td> Flash flood </td> </tr>
  <tr> <td> FLASH FLOOD/ STREET </td> <td> Flash flood/Street </td> </tr>
  <tr> <td> FLASH FLOOD WINDS </td> <td> Flash flood winds </td> </tr>
  <tr> <td> FLASH FLOOODING </td> <td> Flash flood </td> </tr>
  <tr> <td> Flood </td> <td> Flood </td> </tr>
  <tr> <td> FLOOD </td> <td> Flood </td> </tr>
  <tr> <td> FLOOD FLASH </td> <td> Flood flash </td> </tr>
  <tr> <td> FLOOD/FLASH </td> <td> Flood/Flash </td> </tr>
  <tr> <td> Flood/Flash Flood </td> <td> Flood/Flash flood </td> </tr>
  <tr> <td> FLOOD/FLASHFLOOD </td> <td> Flood/Flash flood </td> </tr>
  <tr> <td> FLOOD/FLASH FLOOD </td> <td> Flood/Flash flood </td> </tr>
  <tr> <td> FLOOD/FLASH/FLOOD </td> <td> Flood/Flash flood </td> </tr>
  <tr> <td> FLOOD/FLASH FLOODING </td> <td> Flood/Flash flood </td> </tr>
  <tr> <td> FLOOD FLOOD/FLASH </td> <td> Flood/Flash flood </td> </tr>
  <tr> <td> FLOOD &amp; HEAVY RAIN </td> <td> Flood/Heavy rain </td> </tr>
  <tr> <td> FLOODING </td> <td> Flood </td> </tr>
  <tr> <td> FLOODING/HEAVY RAIN </td> <td> Flood/Heavy rain </td> </tr>
  <tr> <td> FLOOD/RAIN/WIND </td> <td> Flood/Rain/Wind </td> </tr>
  <tr> <td> FLOOD/RAIN/WINDS </td> <td> Flood/Rain/Wind </td> </tr>
  <tr> <td> FLOOD/RIVER FLOOD </td> <td> Flood/River flood </td> </tr>
  <tr> <td> FLOODS </td> <td> Flood </td> </tr>
  <tr> <td> Flood/Strong Wind </td> <td> Flood/Strong wind </td> </tr>
  <tr> <td> FLOOD WATCH/ </td> <td> Flood watch </td> </tr>
  <tr> <td> FOG </td> <td> Fog </td> </tr>
  <tr> <td> FOG AND COLD TEMPERATURES </td> <td> Fog/Cold temperatures </td> </tr>
  <tr> <td> FOREST FIRES </td> <td> Forest fires </td> </tr>
  <tr> <td> Freeze </td> <td> Freeze </td> </tr>
  <tr> <td> FREEZE </td> <td> Freeze </td> </tr>
  <tr> <td> Freezing drizzle </td> <td> Freezing drizzle </td> </tr>
  <tr> <td> Freezing Drizzle </td> <td> Freezing drizzle </td> </tr>
  <tr> <td> FREEZING DRIZZLE </td> <td> Freezing drizzle </td> </tr>
  <tr> <td> FREEZING DRIZZLE AND FREEZING </td> <td> Freezing drizzle/Freezing </td> </tr>
  <tr> <td> Freezing Fog </td> <td> Freezing fog </td> </tr>
  <tr> <td> FREEZING FOG </td> <td> Freezing fog </td> </tr>
  <tr> <td> Freezing rain </td> <td> Freezing rain </td> </tr>
  <tr> <td> Freezing Rain </td> <td> Freezing rain </td> </tr>
  <tr> <td> FREEZING RAIN </td> <td> Freezing rain </td> </tr>
  <tr> <td> FREEZING RAIN AND SLEET </td> <td> Freezing rain/Sleet </td> </tr>
  <tr> <td> FREEZING RAIN AND SNOW </td> <td> Freezing rain/Snow </td> </tr>
  <tr> <td> FREEZING RAIN/SLEET </td> <td> Freezing rain/Sleet </td> </tr>
  <tr> <td> FREEZING RAIN SLEET AND </td> <td> Freezing rain/Sleet </td> </tr>
  <tr> <td> FREEZING RAIN SLEET AND LIGHT </td> <td> Freezing rain/Sleet/Light </td> </tr>
  <tr> <td> FREEZING RAIN/SNOW </td> <td> Freezing rain/Snow </td> </tr>
  <tr> <td> Freezing Spray </td> <td> Freezing spray </td> </tr>
  <tr> <td> Frost </td> <td> Frost </td> </tr>
  <tr> <td> FROST </td> <td> Frost </td> </tr>
  <tr> <td> Frost/Freeze </td> <td> Frost/Freeze </td> </tr>
  <tr> <td> FROST/FREEZE </td> <td> Frost/Freeze </td> </tr>
  <tr> <td> FROST\FREEZE </td> <td> Frost/Freeze </td> </tr>
  <tr> <td> FUNNEL </td> <td> Funnel </td> </tr>
  <tr> <td> Funnel Cloud </td> <td> Funnel cloud </td> </tr>
  <tr> <td> FUNNEL CLOUD </td> <td> Funnel cloud </td> </tr>
  <tr> <td> FUNNEL CLOUD. </td> <td> Funnel cloud </td> </tr>
  <tr> <td> FUNNEL CLOUD/HAIL </td> <td> Funnel cloud/Hail </td> </tr>
  <tr> <td> FUNNEL CLOUDS </td> <td> Funnel cloud </td> </tr>
  <tr> <td> FUNNELS </td> <td> Funnel </td> </tr>
  <tr> <td> Glaze </td> <td> Glaze </td> </tr>
  <tr> <td> GLAZE </td> <td> Glaze </td> </tr>
  <tr> <td> GLAZE ICE </td> <td> Glaze ice </td> </tr>
  <tr> <td> GLAZE/ICE STORM </td> <td> Glaze/Ice storm </td> </tr>
  <tr> <td> gradient wind </td> <td> Gradient wind </td> </tr>
  <tr> <td> Gradient wind </td> <td> Gradient wind </td> </tr>
  <tr> <td> GRADIENT WIND </td> <td> Gradient wind </td> </tr>
  <tr> <td> GRADIENT WINDS </td> <td> Gradient wind </td> </tr>
  <tr> <td> GRASS FIRES </td> <td> Grass fires </td> </tr>
  <tr> <td> GROUND BLIZZARD </td> <td> Ground blizzard </td> </tr>
  <tr> <td> GUSTNADO </td> <td> Gustnado </td> </tr>
  <tr> <td> GUSTNADO AND </td> <td> Gustnado </td> </tr>
  <tr> <td> GUSTY LAKE WIND </td> <td> Gusty lake wind </td> </tr>
  <tr> <td> GUSTY THUNDERSTORM WIND </td> <td> Gusty thunderstorm wind </td> </tr>
  <tr> <td> GUSTY THUNDERSTORM WINDS </td> <td> Gusty thunderstorm wind </td> </tr>
  <tr> <td> Gusty Wind </td> <td> Gusty wind </td> </tr>
  <tr> <td> GUSTY WIND </td> <td> Gusty wind </td> </tr>
  <tr> <td> GUSTY WIND/HAIL </td> <td> Gusty wind/Hail </td> </tr>
  <tr> <td> GUSTY WIND/HVY RAIN </td> <td> Gusty wind/Heavy rain </td> </tr>
  <tr> <td> Gusty wind/rain </td> <td> Gusty wind/Rain </td> </tr>
  <tr> <td> Gusty winds </td> <td> Gusty wind </td> </tr>
  <tr> <td> Gusty Winds </td> <td> Gusty wind </td> </tr>
  <tr> <td> GUSTY WINDS </td> <td> Gusty wind </td> </tr>
  <tr> <td> HAIL </td> <td> Hail </td> </tr>
  <tr> <td> Hail(0.75) </td> <td> Hail 0.75 </td> </tr>
  <tr> <td> HAIL 075 </td> <td> Hail 0.75 </td> </tr>
  <tr> <td> HAIL 0.75 </td> <td> Hail 0.75 </td> </tr>
  <tr> <td> HAIL 088 </td> <td> Hail 0.88 </td> </tr>
  <tr> <td> HAIL 0.88 </td> <td> Hail 0.88 </td> </tr>
  <tr> <td> HAIL 100 </td> <td> Hail 1.00 </td> </tr>
  <tr> <td> HAIL 1.00 </td> <td> Hail 1.00 </td> </tr>
  <tr> <td> HAIL 125 </td> <td> Hail 1.25 </td> </tr>
  <tr> <td> HAIL 150 </td> <td> Hail 1.50 </td> </tr>
  <tr> <td> HAIL 175 </td> <td> Hail 1.75 </td> </tr>
  <tr> <td> HAIL 1.75 </td> <td> Hail 1.75 </td> </tr>
  <tr> <td> HAIL 1.75) </td> <td> Hail 1.75) </td> </tr>
  <tr> <td> HAIL 200 </td> <td> Hail 2.00 </td> </tr>
  <tr> <td> HAIL 225 </td> <td> Hail 2.25 </td> </tr>
  <tr> <td> HAIL 275 </td> <td> Hail 2.75 </td> </tr>
  <tr> <td> HAIL 450 </td> <td> Hail 4.50 </td> </tr>
  <tr> <td> HAIL 75 </td> <td> Hail 0.75 </td> </tr>
  <tr> <td> HAIL 80 </td> <td> Hail 0.80 </td> </tr>
  <tr> <td> HAIL 88 </td> <td> Hail 0.88 </td> </tr>
  <tr> <td> HAIL ALOFT </td> <td> Hail aloft </td> </tr>
  <tr> <td> HAIL DAMAGE </td> <td> Hail damage </td> </tr>
  <tr> <td> HAIL FLOODING </td> <td> Hail flooding </td> </tr>
  <tr> <td> HAIL/ICY ROADS </td> <td> Hail/Icy roads </td> </tr>
  <tr> <td> HAILSTORM </td> <td> Hailstorm </td> </tr>
  <tr> <td> HAIL STORM </td> <td> Hailstorm </td> </tr>
  <tr> <td> HAILSTORMS </td> <td> Hailstorm </td> </tr>
  <tr> <td> HAIL/WIND </td> <td> Hail/Wind </td> </tr>
  <tr> <td> HAIL/WINDS </td> <td> Hail/Wind </td> </tr>
  <tr> <td> HARD FREEZE </td> <td> Hard freeze </td> </tr>
  <tr> <td> HAZARDOUS SURF </td> <td> Hazardous surf </td> </tr>
  <tr> <td> HEAT </td> <td> Heat </td> </tr>
  <tr> <td> Heatburst </td> <td> Heatburst </td> </tr>
  <tr> <td> HEAT DROUGHT </td> <td> Heat/drought </td> </tr>
  <tr> <td> HEAT/DROUGHT </td> <td> Heat/drought </td> </tr>
  <tr> <td> Heat Wave </td> <td> Heat wave </td> </tr>
  <tr> <td> HEAT WAVE </td> <td> Heat wave </td> </tr>
  <tr> <td> HEAT WAVE DROUGHT </td> <td> Heat wave/drought </td> </tr>
  <tr> <td> HEAT WAVES </td> <td> Heat wave </td> </tr>
  <tr> <td> HEAVY LAKE SNOW </td> <td> Heavy lake snow </td> </tr>
  <tr> <td> HEAVY MIX </td> <td> Heavy mix </td> </tr>
  <tr> <td> HEAVY PRECIPATATION </td> <td> Heavy precipatation </td> </tr>
  <tr> <td> Heavy Precipitation </td> <td> Heavy precipitation </td> </tr>
  <tr> <td> HEAVY PRECIPITATION </td> <td> Heavy precipitation </td> </tr>
  <tr> <td> Heavy rain </td> <td> Heavy rain </td> </tr>
  <tr> <td> Heavy Rain </td> <td> Heavy rain </td> </tr>
  <tr> <td> HEAVY RAIN </td> <td> Heavy rain </td> </tr>
  <tr> <td> HEAVY RAIN AND FLOOD </td> <td> Heavy rain/Flood </td> </tr>
  <tr> <td> Heavy Rain and Wind </td> <td> Heavy rain/Wind </td> </tr>
  <tr> <td> HEAVY RAIN EFFECTS </td> <td> Heavy rain effects </td> </tr>
  <tr> <td> HEAVY RAINFALL </td> <td> Heavy rainfall </td> </tr>
  <tr> <td> HEAVY RAIN/FLOODING </td> <td> Heavy rain/Flood </td> </tr>
  <tr> <td> Heavy Rain/High Surf </td> <td> Heavy rain/High surf </td> </tr>
  <tr> <td> HEAVY RAIN/LIGHTNING </td> <td> Heavy rain/Lightning </td> </tr>
  <tr> <td> HEAVY RAIN/MUDSLIDES/FLOOD </td> <td> Heavy rain/Mudslides/Flood </td> </tr>
  <tr> <td> HEAVY RAINS </td> <td> Heavy rain </td> </tr>
  <tr> <td> HEAVY RAIN/SEVERE WEATHER </td> <td> Heavy rain/Severe weather </td> </tr>
  <tr> <td> HEAVY RAINS/FLOODING </td> <td> Heavy rain/Flood </td> </tr>
  <tr> <td> HEAVY RAIN/SMALL STREAM URBAN </td> <td> Heavy rain/Small stream urban </td> </tr>
  <tr> <td> HEAVY RAIN/SNOW </td> <td> Heavy rain/Snow </td> </tr>
  <tr> <td> HEAVY RAIN/URBAN FLOOD </td> <td> Heavy rain/Urban flood </td> </tr>
  <tr> <td> HEAVY RAIN; URBAN FLOOD WINDS; </td> <td> Heavy rain/Urban flood/Winds </td> </tr>
  <tr> <td> HEAVY RAIN/WIND </td> <td> Heavy rain/Wind </td> </tr>
  <tr> <td> HEAVY SEAS </td> <td> Heavy seas </td> </tr>
  <tr> <td> HEAVY SHOWER </td> <td> Heavy shower </td> </tr>
  <tr> <td> HEAVY SHOWERS </td> <td> Heavy shower </td> </tr>
  <tr> <td> HEAVY SNOW </td> <td> Heavy snow </td> </tr>
  <tr> <td> HEAVY SNOW AND </td> <td> Heavy snow </td> </tr>
  <tr> <td> HEAVY SNOW ANDBLOWING SNOW </td> <td> Heavy snow/Blowing snow </td> </tr>
  <tr> <td> HEAVY SNOW AND HIGH WINDS </td> <td> Heavy snow/High wind </td> </tr>
  <tr> <td> HEAVY SNOW AND ICE </td> <td> Heavy snow/Ice </td> </tr>
  <tr> <td> HEAVY SNOW AND ICE STORM </td> <td> Heavy snow/Ice storm </td> </tr>
  <tr> <td> HEAVY SNOW AND STRONG WINDS </td> <td> Heavy snow/Strong winds </td> </tr>
  <tr> <td> HEAVY SNOW/BLIZZARD </td> <td> Heavy snow/Blizzard </td> </tr>
  <tr> <td> HEAVY SNOW/BLIZZARD/AVALANCHE </td> <td> Heavy snow/Blizzard/Avalanche </td> </tr>
  <tr> <td> HEAVY SNOW/BLOWING SNOW </td> <td> Heavy snow/Blowing snow </td> </tr>
  <tr> <td> HEAVY SNOW   FREEZING RAIN </td> <td> Heavy snow/Freezing rain </td> </tr>
  <tr> <td> HEAVY SNOW/FREEZING RAIN </td> <td> Heavy snow/Freezing rain </td> </tr>
  <tr> <td> HEAVY SNOW/HIGH </td> <td> Heavy snow/High wind </td> </tr>
  <tr> <td> HEAVY SNOW/HIGH WIND </td> <td> Heavy snow/High wind </td> </tr>
  <tr> <td> HEAVY SNOW/HIGH WINDS </td> <td> Heavy snow/High wind </td> </tr>
  <tr> <td> HEAVY SNOW/HIGH WINDS &amp; FLOOD </td> <td> Heavy snow/High winds/Flood </td> </tr>
  <tr> <td> HEAVY SNOW/HIGH WINDS/FREEZING </td> <td> Heavy snow/High winds/Freezing </td> </tr>
  <tr> <td> HEAVY SNOW &amp; ICE </td> <td> Heavy snow/Ice </td> </tr>
  <tr> <td> HEAVY SNOW/ICE </td> <td> Heavy snow/Ice </td> </tr>
  <tr> <td> HEAVY SNOW/ICE STORM </td> <td> Heavy snow/Ice storm </td> </tr>
  <tr> <td> HEAVY SNOWPACK </td> <td> Heavy snowpack </td> </tr>
  <tr> <td> Heavy snow shower </td> <td> Heavy snow shower </td> </tr>
  <tr> <td> HEAVY SNOW/SLEET </td> <td> Heavy snow/Sleet </td> </tr>
  <tr> <td> HEAVY SNOW SQUALLS </td> <td> Heavy snow/squalls </td> </tr>
  <tr> <td> HEAVY SNOW-SQUALLS </td> <td> Heavy snow/squalls </td> </tr>
  <tr> <td> HEAVY SNOW/SQUALLS </td> <td> Heavy snow/squalls </td> </tr>
  <tr> <td> HEAVY SNOW/WIND </td> <td> Heavy snow/wind </td> </tr>
  <tr> <td> HEAVY SNOW/WINTER STORM </td> <td> Heavy snow/Winter storm </td> </tr>
  <tr> <td> Heavy Surf </td> <td> Heavy surf </td> </tr>
  <tr> <td> HEAVY SURF </td> <td> Heavy surf </td> </tr>
  <tr> <td> Heavy surf and wind </td> <td> Heavy surf/Wind </td> </tr>
  <tr> <td> HEAVY SURF COASTAL FLOODING </td> <td> Heavy surf/Coastal flooding </td> </tr>
  <tr> <td> HEAVY SURF/HIGH SURF </td> <td> Heavy surf/High surf </td> </tr>
  <tr> <td> HEAVY SWELLS </td> <td> Heavy swell </td> </tr>
  <tr> <td> HEAVY WET SNOW </td> <td> Heavy wet snow </td> </tr>
  <tr> <td> HIGH </td> <td> High </td> </tr>
  <tr> <td> HIGH SEAS </td> <td> High seas </td> </tr>
  <tr> <td> High Surf </td> <td> High surf </td> </tr>
  <tr> <td> HIGH SURF </td> <td> High surf </td> </tr>
  <tr> <td> HIGH SURF ADVISORIES </td> <td> High surf advisory </td> </tr>
  <tr> <td> HIGH SURF ADVISORY </td> <td> High surf advisory </td> </tr>
  <tr> <td> HIGH SWELLS </td> <td> High swells </td> </tr>
  <tr> <td> HIGH  SWELLS </td> <td> High swells </td> </tr>
  <tr> <td> HIGH TEMPERATURE RECORD </td> <td> High temperature record </td> </tr>
  <tr> <td> HIGH TIDES </td> <td> High tide </td> </tr>
  <tr> <td> HIGH WATER </td> <td> High water </td> </tr>
  <tr> <td> HIGH WAVES </td> <td> High waves </td> </tr>
  <tr> <td> HIGHWAY FLOODING </td> <td> Highway flooding </td> </tr>
  <tr> <td> High Wind </td> <td> High wind </td> </tr>
  <tr> <td> HIGH WIND </td> <td> High wind </td> </tr>
  <tr> <td> HIGH WIND 48 </td> <td> High wind 48 </td> </tr>
  <tr> <td> HIGH WIND 63 </td> <td> High wind 63 </td> </tr>
  <tr> <td> HIGH WIND 70 </td> <td> High wind 70 </td> </tr>
  <tr> <td> HIGH WIND AND HEAVY SNOW </td> <td> High wind/Heavy snow </td> </tr>
  <tr> <td> HIGH WIND AND HIGH TIDES </td> <td> High wind/High tides </td> </tr>
  <tr> <td> HIGH WIND AND SEAS </td> <td> High wind/Seas </td> </tr>
  <tr> <td> HIGH WIND/BLIZZARD </td> <td> High wind/Blizzard </td> </tr>
  <tr> <td> HIGH WIND/ BLIZZARD </td> <td> High wind/Blizzard </td> </tr>
  <tr> <td> HIGH WIND/BLIZZARD/FREEZING RA </td> <td> High wind/Blizzard/Freezing rain </td> </tr>
  <tr> <td> HIGH WIND DAMAGE </td> <td> High wind damage </td> </tr>
  <tr> <td> HIGH WIND (G40) </td> <td> High wind (g40) </td> </tr>
  <tr> <td> HIGH WIND/HEAVY SNOW </td> <td> High wind/Heavy snow </td> </tr>
  <tr> <td> HIGH WIND/LOW WIND CHILL </td> <td> High wind/Low wind chill </td> </tr>
  <tr> <td> HIGH WINDS </td> <td> High wind </td> </tr>
  <tr> <td> HIGH  WINDS </td> <td> High wind </td> </tr>
  <tr> <td> HIGH WINDS/ </td> <td> High wind </td> </tr>
  <tr> <td> HIGH WINDS 55 </td> <td> High wind 55 </td> </tr>
  <tr> <td> HIGH WINDS 57 </td> <td> High wind 57 </td> </tr>
  <tr> <td> HIGH WINDS 58 </td> <td> High wind 58 </td> </tr>
  <tr> <td> HIGH WINDS 63 </td> <td> High wind 63 </td> </tr>
  <tr> <td> HIGH WINDS 66 </td> <td> High wind 66 </td> </tr>
  <tr> <td> HIGH WINDS 67 </td> <td> High wind 67 </td> </tr>
  <tr> <td> HIGH WINDS 73 </td> <td> High wind 73 </td> </tr>
  <tr> <td> HIGH WINDS 76 </td> <td> High wind 76 </td> </tr>
  <tr> <td> HIGH WINDS 80 </td> <td> High wind 80 </td> </tr>
  <tr> <td> HIGH WINDS 82 </td> <td> High wind 82 </td> </tr>
  <tr> <td> HIGH WINDS AND WIND CHILL </td> <td> High wind/Wind chill </td> </tr>
  <tr> <td> HIGH WINDS/COASTAL FLOOD </td> <td> High wind/Coastal flood </td> </tr>
  <tr> <td> HIGH WINDS/COLD </td> <td> High wind/Cold </td> </tr>
  <tr> <td> HIGH WINDS DUST STORM </td> <td> High wind/Dust storm </td> </tr>
  <tr> <td> HIGH WIND/SEAS </td> <td> High wind/Seas </td> </tr>
  <tr> <td> HIGH WINDS/FLOODING </td> <td> High wind/Flood </td> </tr>
  <tr> <td> HIGH WINDS/HEAVY RAIN </td> <td> High wind/Heavy rain </td> </tr>
  <tr> <td> HIGH WINDS HEAVY RAINS </td> <td> High wind/Heavy rain </td> </tr>
  <tr> <td> HIGH WINDS/SNOW </td> <td> High wind/Snow </td> </tr>
  <tr> <td> HIGH WIND/WIND CHILL </td> <td> High wind/Wind chill </td> </tr>
  <tr> <td> HIGH WIND/WIND CHILL/BLIZZARD </td> <td> High wind/Wind chill/Blizzard </td> </tr>
  <tr> <td> Hot and Dry </td> <td> Hot/dry </td> </tr>
  <tr> <td> HOT/DRY PATTERN </td> <td> Hot/dry pattern </td> </tr>
  <tr> <td> HOT PATTERN </td> <td> Hot pattern </td> </tr>
  <tr> <td> HOT SPELL </td> <td> Hot spell </td> </tr>
  <tr> <td> HOT WEATHER </td> <td> Hot weather </td> </tr>
  <tr> <td> HURRICANE </td> <td> Hurricane </td> </tr>
  <tr> <td> Hurricane Edouard </td> <td> Hurricane Edouard </td> </tr>
  <tr> <td> HURRICANE EMILY </td> <td> Hurricane Emily </td> </tr>
  <tr> <td> HURRICANE ERIN </td> <td> Hurricane Erin </td> </tr>
  <tr> <td> HURRICANE FELIX </td> <td> hurricane Felix </td> </tr>
  <tr> <td> HURRICANE-GENERATED SWELLS </td> <td> Hurricane-generated swells </td> </tr>
  <tr> <td> HURRICANE GORDON </td> <td> Hurricane Gordon </td> </tr>
  <tr> <td> HURRICANE OPAL </td> <td> Hurricane Opal </td> </tr>
  <tr> <td> HURRICANE OPAL/HIGH WINDS </td> <td> Hurricane Opal/High winds </td> </tr>
  <tr> <td> HURRICANE/TYPHOON </td> <td> Hurricane/Typhoon </td> </tr>
  <tr> <td> HVY RAIN </td> <td> Heavy rain </td> </tr>
  <tr> <td> HYPERTHERMIA/EXPOSURE </td> <td> Hyperthermia/Exposure </td> </tr>
  <tr> <td> HYPOTHERMIA </td> <td> Hypothermia </td> </tr>
  <tr> <td> Hypothermia/Exposure </td> <td> Hypothermia/Exposure </td> </tr>
  <tr> <td> HYPOTHERMIA/EXPOSURE </td> <td> Hypothermia/Exposure </td> </tr>
  <tr> <td> ICE </td> <td> Ice </td> </tr>
  <tr> <td> ICE AND SNOW </td> <td> Ice/Snow </td> </tr>
  <tr> <td> ICE FLOES </td> <td> Ice floes </td> </tr>
  <tr> <td> Ice Fog </td> <td> Ice fog </td> </tr>
  <tr> <td> ICE JAM </td> <td> Ice jam </td> </tr>
  <tr> <td> ICE JAM FLOODING </td> <td> Ice jam flood </td> </tr>
  <tr> <td> Ice jam flood (minor </td> <td> Ice jam flood minor </td> </tr>
  <tr> <td> ICE ON ROAD </td> <td> Icy road </td> </tr>
  <tr> <td> ICE PELLETS </td> <td> Ice pellets </td> </tr>
  <tr> <td> ICE ROADS </td> <td> Icy road </td> </tr>
  <tr> <td> Ice/Snow </td> <td> Ice/Snow </td> </tr>
  <tr> <td> ICE/SNOW </td> <td> Ice/Snow </td> </tr>
  <tr> <td> ICE STORM </td> <td> Ice storm </td> </tr>
  <tr> <td> ICE STORM AND SNOW </td> <td> Ice storm/Snow </td> </tr>
  <tr> <td> Icestorm/Blizzard </td> <td> Ice storm/Blizzard </td> </tr>
  <tr> <td> ICE STORM/FLASH FLOOD </td> <td> Ice storm/Flash flood </td> </tr>
  <tr> <td> ICE/STRONG WINDS </td> <td> Ice/Strong winds </td> </tr>
  <tr> <td> Icy Roads </td> <td> Icy road </td> </tr>
  <tr> <td> ICY ROADS </td> <td> Icy road </td> </tr>
  <tr> <td> LACK OF SNOW </td> <td> Lack of snow </td> </tr>
  <tr> <td> Lake Effect Snow </td> <td> Lake effect snow </td> </tr>
  <tr> <td> LAKE EFFECT SNOW </td> <td> Lake effect snow </td> </tr>
  <tr> <td> LAKE-EFFECT SNOW </td> <td> Lake effect snow </td> </tr>
  <tr> <td> LAKE FLOOD </td> <td> Lake flood </td> </tr>
  <tr> <td> LAKESHORE FLOOD </td> <td> Lakeshore flood </td> </tr>
  <tr> <td> LANDSLIDE </td> <td> Landslide </td> </tr>
  <tr> <td> LANDSLIDES </td> <td> Landslide </td> </tr>
  <tr> <td> LANDSLIDE/URBAN FLOOD </td> <td> Landslide/Urban flood </td> </tr>
  <tr> <td> Landslump </td> <td> Landslump </td> </tr>
  <tr> <td> LANDSLUMP </td> <td> Landslump </td> </tr>
  <tr> <td> LANDSPOUT </td> <td> Landspout </td> </tr>
  <tr> <td> LARGE WALL CLOUD </td> <td> Large wall cloud </td> </tr>
  <tr> <td> LATE FREEZE </td> <td> Late freeze </td> </tr>
  <tr> <td> LATE SEASON HAIL </td> <td> Late season hail </td> </tr>
  <tr> <td> LATE SEASON SNOW </td> <td> Late season snow </td> </tr>
  <tr> <td> Late-season Snowfall </td> <td> Late season snow </td> </tr>
  <tr> <td> Late Season Snowfall </td> <td> Late season snow </td> </tr>
  <tr> <td> LATE SNOW </td> <td> Late season snow </td> </tr>
  <tr> <td> LIGHT FREEZING RAIN </td> <td> Light freezing rain </td> </tr>
  <tr> <td> LIGHTING </td> <td> Lightning </td> </tr>
  <tr> <td> LIGHTNING </td> <td> Lightning </td> </tr>
  <tr> <td> LIGHTNING. </td> <td> Lightning. </td> </tr>
  <tr> <td> LIGHTNING AND HEAVY RAIN </td> <td> Lightning/Heavy rain </td> </tr>
  <tr> <td> LIGHTNING AND THUNDERSTORM WIN </td> <td> Lightning/Thunderstorm wind </td> </tr>
  <tr> <td> LIGHTNING AND WINDS </td> <td> Lightning/wind </td> </tr>
  <tr> <td> LIGHTNING DAMAGE </td> <td> Lightning damage </td> </tr>
  <tr> <td> LIGHTNING FIRE </td> <td> Lightning fire </td> </tr>
  <tr> <td> LIGHTNING/HEAVY RAIN </td> <td> Lightning/Heavy rain </td> </tr>
  <tr> <td> LIGHTNING INJURY </td> <td> Lightning injury </td> </tr>
  <tr> <td> LIGHTNING THUNDERSTORM WINDS </td> <td> Lightning/Thunderstorm wind </td> </tr>
  <tr> <td> LIGHTNING THUNDERSTORM WINDSS </td> <td> Lightning/Thunderstorm wind </td> </tr>
  <tr> <td> LIGHTNING  WAUSEON </td> <td> Lightning wauseon </td> </tr>
  <tr> <td> Light snow </td> <td> Light snow </td> </tr>
  <tr> <td> Light Snow </td> <td> Light snow </td> </tr>
  <tr> <td> LIGHT SNOW </td> <td> Light snow </td> </tr>
  <tr> <td> LIGHT SNOW AND SLEET </td> <td> Light snow/Sleet </td> </tr>
  <tr> <td> Light Snowfall </td> <td> Light snow </td> </tr>
  <tr> <td> Light Snow/Flurries </td> <td> Light snow/Flurries </td> </tr>
  <tr> <td> LIGHT SNOW/FREEZING PRECIP </td> <td> Light snow/Freezing precip </td> </tr>
  <tr> <td> LIGNTNING </td> <td> Lightning </td> </tr>
  <tr> <td> LOCAL FLASH FLOOD </td> <td> Local flash flood </td> </tr>
  <tr> <td> LOCAL FLOOD </td> <td> Local flood </td> </tr>
  <tr> <td> LOCALLY HEAVY RAIN </td> <td> Local heavy rain </td> </tr>
  <tr> <td> LOW TEMPERATURE </td> <td> Low temperature </td> </tr>
  <tr> <td> LOW TEMPERATURE RECORD </td> <td> Low temperature record </td> </tr>
  <tr> <td> LOW WIND CHILL </td> <td> Low wind chill </td> </tr>
  <tr> <td> MAJOR FLOOD </td> <td> Major flood </td> </tr>
  <tr> <td> Marine Accident </td> <td> Marine accident </td> </tr>
  <tr> <td> MARINE HAIL </td> <td> Marine hail </td> </tr>
  <tr> <td> MARINE HIGH WIND </td> <td> Marine high wind </td> </tr>
  <tr> <td> MARINE MISHAP </td> <td> Marine mishap </td> </tr>
  <tr> <td> MARINE STRONG WIND </td> <td> Marine strong wind </td> </tr>
  <tr> <td> MARINE THUNDERSTORM WIND </td> <td> Marine thunderstorm wind </td> </tr>
  <tr> <td> MARINE TSTM WIND </td> <td> Marine thunderstorm wind </td> </tr>
  <tr> <td> Metro Storm, May 26 </td> <td> Metro storm, may 26 </td> </tr>
  <tr> <td> Microburst </td> <td> Microburst </td> </tr>
  <tr> <td> MICROBURST </td> <td> Microburst </td> </tr>
  <tr> <td> MICROBURST WINDS </td> <td> Microburst wind </td> </tr>
  <tr> <td> Mild and Dry Pattern </td> <td> Mild/Dry pattern </td> </tr>
  <tr> <td> MILD/DRY PATTERN </td> <td> Mild/Dry pattern </td> </tr>
  <tr> <td> MILD PATTERN </td> <td> Mild pattern </td> </tr>
  <tr> <td> MINOR FLOOD </td> <td> Minor flood </td> </tr>
  <tr> <td> Minor Flooding </td> <td> Minor flood </td> </tr>
  <tr> <td> MINOR FLOODING </td> <td> Minor flood </td> </tr>
  <tr> <td> MIXED PRECIP </td> <td> Mixed precipitation </td> </tr>
  <tr> <td> Mixed Precipitation </td> <td> Mixed precipitation </td> </tr>
  <tr> <td> MIXED PRECIPITATION </td> <td> Mixed precipitation </td> </tr>
  <tr> <td> MODERATE SNOW </td> <td> Moderate snow </td> </tr>
  <tr> <td> MODERATE SNOWFALL </td> <td> Moderate snow </td> </tr>
  <tr> <td> MONTHLY PRECIPITATION </td> <td> Monthly precipitation </td> </tr>
  <tr> <td> Monthly Rainfall </td> <td> Monthly rainfall </td> </tr>
  <tr> <td> MONTHLY RAINFALL </td> <td> Monthly rainfall </td> </tr>
  <tr> <td> Monthly Snowfall </td> <td> Monthly snowfall </td> </tr>
  <tr> <td> MONTHLY SNOWFALL </td> <td> Monthly snowfall </td> </tr>
  <tr> <td> MONTHLY TEMPERATURE </td> <td> Monthly temperature </td> </tr>
  <tr> <td> Mountain Snows </td> <td> Mountain snows </td> </tr>
  <tr> <td> MUD/ROCK SLIDE </td> <td> Mud/Rockslide </td> </tr>
  <tr> <td> Mudslide </td> <td> Mudslide </td> </tr>
  <tr> <td> MUDSLIDE </td> <td> Mudslide </td> </tr>
  <tr> <td> MUD SLIDE </td> <td> Mudslide </td> </tr>
  <tr> <td> MUDSLIDE/LANDSLIDE </td> <td> Mudslide/Landslide </td> </tr>
  <tr> <td> Mudslides </td> <td> Mudslide </td> </tr>
  <tr> <td> MUDSLIDES </td> <td> Mudslide </td> </tr>
  <tr> <td> MUD SLIDES </td> <td> Mudslide </td> </tr>
  <tr> <td> MUD SLIDES URBAN FLOODING </td> <td> Mudslide/Urban flood </td> </tr>
  <tr> <td> NEAR RECORD SNOW </td> <td> Near record snow </td> </tr>
  <tr> <td> NONE </td> <td> None </td> </tr>
  <tr> <td> NON SEVERE HAIL </td> <td> Non-severe hail </td> </tr>
  <tr> <td> NON-SEVERE WIND DAMAGE </td> <td> Non-severe wind damage </td> </tr>
  <tr> <td> NON TSTM WIND </td> <td> Non-thunderstorm wind </td> </tr>
  <tr> <td> NON-TSTM WIND </td> <td> Non-thunderstorm wind </td> </tr>
  <tr> <td> NORMAL PRECIPITATION </td> <td> Normal precipitation </td> </tr>
  <tr> <td> NORTHERN LIGHTS </td> <td> Northern lights </td> </tr>
  <tr> <td> No Severe Weather </td> <td> Non-severe weather </td> </tr>
  <tr> <td> Other </td> <td> Other </td> </tr>
  <tr> <td> OTHER </td> <td> Other </td> </tr>
  <tr> <td> PATCHY DENSE FOG </td> <td> Patchy dense fog </td> </tr>
  <tr> <td> PATCHY ICE </td> <td> Patchy ice </td> </tr>
  <tr> <td> Prolong Cold </td> <td> Prolonged cold </td> </tr>
  <tr> <td> PROLONG COLD </td> <td> Prolonged cold </td> </tr>
  <tr> <td> PROLONG COLD/SNOW </td> <td> Prolonged cold/Snow </td> </tr>
  <tr> <td> PROLONGED RAIN </td> <td> Prolonged rain </td> </tr>
  <tr> <td> PROLONG WARMTH </td> <td> Prolonged warmth </td> </tr>
  <tr> <td> RAIN </td> <td> Rain </td> </tr>
  <tr> <td> RAIN AND WIND </td> <td> Rain/Wind </td> </tr>
  <tr> <td> Rain Damage </td> <td> Rain damage </td> </tr>
  <tr> <td> RAIN (HEAVY) </td> <td> Heavy rain </td> </tr>
  <tr> <td> RAIN/SNOW </td> <td> Rain/ssnow </td> </tr>
  <tr> <td> RAINSTORM </td> <td> Rainstorm </td> </tr>
  <tr> <td> RAIN/WIND </td> <td> Rain/Wind </td> </tr>
  <tr> <td> RAPIDLY RISING WATER </td> <td> Rapidly rising water </td> </tr>
  <tr> <td> Record Cold </td> <td> Record cold </td> </tr>
  <tr> <td> RECORD COLD </td> <td> Record cold </td> </tr>
  <tr> <td> RECORD  COLD </td> <td> Record cold </td> </tr>
  <tr> <td> RECORD COLD AND HIGH WIND </td> <td> Record cold/High wind </td> </tr>
  <tr> <td> RECORD COLD/FROST </td> <td> Record cold/Frost </td> </tr>
  <tr> <td> RECORD COOL </td> <td> Record cool </td> </tr>
  <tr> <td> Record dry month </td> <td> Record dry month </td> </tr>
  <tr> <td> RECORD DRYNESS </td> <td> Record dryness </td> </tr>
  <tr> <td> RECORD/EXCESSIVE HEAT </td> <td> Record/Excessive heat </td> </tr>
  <tr> <td> RECORD/EXCESSIVE RAINFALL </td> <td> Record/Excessive rainfall </td> </tr>
  <tr> <td> Record Heat </td> <td> Record heat </td> </tr>
  <tr> <td> RECORD HEAT </td> <td> Record heat </td> </tr>
  <tr> <td> RECORD HEAT WAVE </td> <td> Record heat wave </td> </tr>
  <tr> <td> Record High </td> <td> Record high </td> </tr>
  <tr> <td> RECORD HIGH </td> <td> Record high </td> </tr>
  <tr> <td> RECORD HIGH TEMPERATURE </td> <td> Record high temperature </td> </tr>
  <tr> <td> RECORD HIGH TEMPERATURES </td> <td> Record high temperature </td> </tr>
  <tr> <td> RECORD LOW </td> <td> Record low </td> </tr>
  <tr> <td> RECORD LOW RAINFALL </td> <td> Record low rainfall </td> </tr>
  <tr> <td> Record May Snow </td> <td> Record may snow </td> </tr>
  <tr> <td> RECORD PRECIPITATION </td> <td> Record precipitation </td> </tr>
  <tr> <td> RECORD RAINFALL </td> <td> Record rainfall </td> </tr>
  <tr> <td> RECORD SNOW </td> <td> Record snow </td> </tr>
  <tr> <td> RECORD SNOW/COLD </td> <td> Record snow/Cold </td> </tr>
  <tr> <td> RECORD SNOWFALL </td> <td> Record snow </td> </tr>
  <tr> <td> Record temperature </td> <td> Record temperature </td> </tr>
  <tr> <td> RECORD TEMPERATURE </td> <td> Record temperature </td> </tr>
  <tr> <td> Record Temperatures </td> <td> Record temperature </td> </tr>
  <tr> <td> RECORD TEMPERATURES </td> <td> Record temperature </td> </tr>
  <tr> <td> RECORD WARM </td> <td> Record warmth </td> </tr>
  <tr> <td> RECORD WARM TEMPS. </td> <td> Record warmth </td> </tr>
  <tr> <td> Record Warmth </td> <td> Record warmth </td> </tr>
  <tr> <td> RECORD WARMTH </td> <td> Record warmth </td> </tr>
  <tr> <td> Record Winter Snow </td> <td> Record winter snow </td> </tr>
  <tr> <td> RED FLAG CRITERIA </td> <td> Red flag criteria </td> </tr>
  <tr> <td> RED FLAG FIRE WX </td> <td> Red flag fire wx </td> </tr>
  <tr> <td> REMNANTS OF FLOYD </td> <td> Remnants of Floyd </td> </tr>
  <tr> <td> RIP CURRENT </td> <td> Rip current </td> </tr>
  <tr> <td> RIP CURRENTS </td> <td> Rip current </td> </tr>
  <tr> <td> RIP CURRENTS HEAVY SURF </td> <td> Rip current/Heavy surf </td> </tr>
  <tr> <td> RIP CURRENTS/HEAVY SURF </td> <td> Rip current/Heavy surf </td> </tr>
  <tr> <td> RIVER AND STREAM FLOOD </td> <td> River flood/Stream flood </td> </tr>
  <tr> <td> RIVER FLOOD </td> <td> River flood </td> </tr>
  <tr> <td> River Flooding </td> <td> River flood </td> </tr>
  <tr> <td> RIVER FLOODING </td> <td> River flood </td> </tr>
  <tr> <td> ROCK SLIDE </td> <td> Rock slide </td> </tr>
  <tr> <td> ROGUE WAVE </td> <td> Rogue wave </td> </tr>
  <tr> <td> ROTATING WALL CLOUD </td> <td> Rotating wall cloud </td> </tr>
  <tr> <td> ROUGH SEAS </td> <td> Rough seas </td> </tr>
  <tr> <td> ROUGH SURF </td> <td> Rough surf </td> </tr>
  <tr> <td> RURAL FLOOD </td> <td> Rural flood </td> </tr>
  <tr> <td> Saharan Dust </td> <td> Saharan dust </td> </tr>
  <tr> <td> SAHARAN DUST </td> <td> Saharan dust </td> </tr>
  <tr> <td> Seasonal Snowfall </td> <td> Seasonal snowfall </td> </tr>
  <tr> <td> SEICHE </td> <td> Seiche </td> </tr>
  <tr> <td> SEVERE COLD </td> <td> Severe cold </td> </tr>
  <tr> <td> SEVERE THUNDERSTORM </td> <td> Severe thunderstorm </td> </tr>
  <tr> <td> SEVERE THUNDERSTORMS </td> <td> Severe thunderstorm </td> </tr>
  <tr> <td> SEVERE THUNDERSTORM WINDS </td> <td> Severe thunderstorm wind </td> </tr>
  <tr> <td> SEVERE TURBULENCE </td> <td> Severe turbulence </td> </tr>
  <tr> <td> SLEET </td> <td> Sleet </td> </tr>
  <tr> <td> SLEET &amp; FREEZING RAIN </td> <td> Sleet/Freezing rain </td> </tr>
  <tr> <td> SLEET/FREEZING RAIN </td> <td> Sleet/Freezing rain </td> </tr>
  <tr> <td> SLEET/ICE STORM </td> <td> Sleet/Ice storm </td> </tr>
  <tr> <td> SLEET/RAIN/SNOW </td> <td> Sleet/Rain/Snow </td> </tr>
  <tr> <td> SLEET/SNOW </td> <td> Sleet/Snow </td> </tr>
  <tr> <td> SLEET STORM </td> <td> Sleet storm </td> </tr>
  <tr> <td> small hail </td> <td> Small hail </td> </tr>
  <tr> <td> Small Hail </td> <td> Small hail </td> </tr>
  <tr> <td> SMALL HAIL </td> <td> Small hail </td> </tr>
  <tr> <td> SMALL STREAM </td> <td> Small stream </td> </tr>
  <tr> <td> SMALL STREAM AND </td> <td> Small stream </td> </tr>
  <tr> <td> SMALL STREAM AND URBAN FLOOD </td> <td> Small stream/Urban flood </td> </tr>
  <tr> <td> SMALL STREAM AND URBAN FLOODIN </td> <td> Small stream/Urban flood </td> </tr>
  <tr> <td> SMALL STREAM FLOOD </td> <td> Small stream flood </td> </tr>
  <tr> <td> SMALL STREAM FLOODING </td> <td> Small stream flood </td> </tr>
  <tr> <td> SMALL STREAM URBAN FLOOD </td> <td> Small stream/Urban flood </td> </tr>
  <tr> <td> SMALL STREAM/URBAN FLOOD </td> <td> Small stream/Urban flood </td> </tr>
  <tr> <td> Sml Stream Fld </td> <td> Small stream flood </td> </tr>
  <tr> <td> SMOKE </td> <td> Smoke </td> </tr>
  <tr> <td> Snow </td> <td> Snow </td> </tr>
  <tr> <td> SNOW </td> <td> Snow </td> </tr>
  <tr> <td> Snow Accumulation </td> <td> Snow accumulation </td> </tr>
  <tr> <td> SNOW ACCUMULATION </td> <td> Snow accumulation </td> </tr>
  <tr> <td> SNOW ADVISORY </td> <td> Snow advisory </td> </tr>
  <tr> <td> SNOW AND COLD </td> <td> Snow/Cold </td> </tr>
  <tr> <td> SNOW AND HEAVY SNOW </td> <td> Snow/Heavy snow </td> </tr>
  <tr> <td> Snow and Ice </td> <td> Snow/Ice </td> </tr>
  <tr> <td> SNOW AND ICE </td> <td> Snow/Ice </td> </tr>
  <tr> <td> SNOW AND ICE STORM </td> <td> Snow/Ice storm </td> </tr>
  <tr> <td> Snow and sleet </td> <td> Snow/Sleet </td> </tr>
  <tr> <td> SNOW AND SLEET </td> <td> Snow/Sleet </td> </tr>
  <tr> <td> SNOW AND WIND </td> <td> Snow/Wind </td> </tr>
  <tr> <td> SNOW/ BITTER COLD </td> <td> Snow/Bitter cold </td> </tr>
  <tr> <td> SNOW/BLOWING SNOW </td> <td> Snow/Blowing snow </td> </tr>
  <tr> <td> SNOW/COLD </td> <td> Snow/Cold </td> </tr>
  <tr> <td> SNOW\COLD </td> <td> Snow/Cold </td> </tr>
  <tr> <td> SNOW DROUGHT </td> <td> Snow drought </td> </tr>
  <tr> <td> SNOWFALL RECORD </td> <td> Snowfall record </td> </tr>
  <tr> <td> SNOW FREEZING RAIN </td> <td> Snow/Freezing rain </td> </tr>
  <tr> <td> SNOW/FREEZING RAIN </td> <td> Snow/Freezing rain </td> </tr>
  <tr> <td> SNOW/HEAVY SNOW </td> <td> Snow/Heavy snow </td> </tr>
  <tr> <td> SNOW/HIGH WINDS </td> <td> Snow/High wind </td> </tr>
  <tr> <td> SNOW- HIGH WIND- WIND CHILL </td> <td> Snow/High wind/wind chill </td> </tr>
  <tr> <td> SNOW/ICE </td> <td> Snow/Ice </td> </tr>
  <tr> <td> SNOW/ ICE </td> <td> Snow/Ice </td> </tr>
  <tr> <td> SNOW/ICE STORM </td> <td> Snow/Ice storm </td> </tr>
  <tr> <td> SNOWMELT FLOODING </td> <td> Snowmelt flooding </td> </tr>
  <tr> <td> SNOW/RAIN </td> <td> Snow/Rain </td> </tr>
  <tr> <td> SNOW/RAIN/SLEET </td> <td> Snow/Rain/Sleet </td> </tr>
  <tr> <td> SNOW SHOWERS </td> <td> Snow showers </td> </tr>
  <tr> <td> SNOW SLEET </td> <td> Snow/Sleet </td> </tr>
  <tr> <td> SNOW/SLEET </td> <td> Snow/Sleet </td> </tr>
  <tr> <td> SNOW/SLEET/FREEZING RAIN </td> <td> Snow/Sleet/Freezing rain </td> </tr>
  <tr> <td> SNOW/SLEET/RAIN </td> <td> Snow/Sleet/Rain </td> </tr>
  <tr> <td> SNOW SQUALL </td> <td> Snow squall </td> </tr>
  <tr> <td> Snow squalls </td> <td> Snow squall </td> </tr>
  <tr> <td> Snow Squalls </td> <td> Snow squall </td> </tr>
  <tr> <td> SNOW SQUALLS </td> <td> Snow squall </td> </tr>
  <tr> <td> SNOWSTORM </td> <td> Snowstorm </td> </tr>
  <tr> <td> SOUTHEAST </td> <td> Southeast </td> </tr>
  <tr> <td> STORM FORCE WINDS </td> <td> Storm force winds </td> </tr>
  <tr> <td> STORM SURGE </td> <td> Storm surge </td> </tr>
  <tr> <td> STORM SURGE/TIDE </td> <td> Storm surge/Tide </td> </tr>
  <tr> <td> STREAM FLOODING </td> <td> Stream flood </td> </tr>
  <tr> <td> STREET FLOOD </td> <td> Street flood </td> </tr>
  <tr> <td> STREET FLOODING </td> <td> Street flood </td> </tr>
  <tr> <td> Strong Wind </td> <td> Strong wind </td> </tr>
  <tr> <td> STRONG WIND </td> <td> Strong wind </td> </tr>
  <tr> <td> STRONG WIND GUST </td> <td> Strong wind gust </td> </tr>
  <tr> <td> Strong winds </td> <td> Strong wind </td> </tr>
  <tr> <td> Strong Winds </td> <td> Strong wind </td> </tr>
  <tr> <td> STRONG WINDS </td> <td> Strong wind </td> </tr>
  <tr> <td> Summary August 10 </td> <td> Summary august 10 </td> </tr>
  <tr> <td> Summary August 11 </td> <td> Summary august 11 </td> </tr>
  <tr> <td> Summary August 17 </td> <td> Summary august 17 </td> </tr>
  <tr> <td> Summary August 21 </td> <td> Summary august 21 </td> </tr>
  <tr> <td> Summary August 2-3 </td> <td> Summary august 2-3 </td> </tr>
  <tr> <td> Summary August 28 </td> <td> Summary august 28 </td> </tr>
  <tr> <td> Summary August 4 </td> <td> Summary august 4 </td> </tr>
  <tr> <td> Summary August 7 </td> <td> Summary august 7 </td> </tr>
  <tr> <td> Summary August 9 </td> <td> Summary august 9 </td> </tr>
  <tr> <td> Summary Jan 17 </td> <td> Summary jan 17 </td> </tr>
  <tr> <td> Summary July 23-24 </td> <td> Summary july 23-24 </td> </tr>
  <tr> <td> Summary June 18-19 </td> <td> Summary june 18-19 </td> </tr>
  <tr> <td> Summary June 5-6 </td> <td> Summary june 5-6 </td> </tr>
  <tr> <td> Summary June 6 </td> <td> Summary june 6 </td> </tr>
  <tr> <td> Summary: Nov. 16 </td> <td> Summary november 16 </td> </tr>
  <tr> <td> Summary: Nov. 6-7 </td> <td> Summary november 6-7 </td> </tr>
  <tr> <td> Summary: Oct. 20-21 </td> <td> Summary october 20-21 </td> </tr>
  <tr> <td> Summary: October 31 </td> <td> Summary october 31 </td> </tr>
  <tr> <td> Summary of April 12 </td> <td> Summary april 12 </td> </tr>
  <tr> <td> Summary of April 13 </td> <td> Summary april 13 </td> </tr>
  <tr> <td> Summary of April 21 </td> <td> Summary april 21 </td> </tr>
  <tr> <td> Summary of April 27 </td> <td> Summary april 27 </td> </tr>
  <tr> <td> Summary of April 3rd </td> <td> Summary april 3 </td> </tr>
  <tr> <td> Summary of August 1 </td> <td> Summary august 1 </td> </tr>
  <tr> <td> Summary of July 11 </td> <td> Summary july 11 </td> </tr>
  <tr> <td> Summary of July 2 </td> <td> Summary july 2 </td> </tr>
  <tr> <td> Summary of July 22 </td> <td> Summary july 22 </td> </tr>
  <tr> <td> Summary of July 26 </td> <td> Summary july 26 </td> </tr>
  <tr> <td> Summary of July 29 </td> <td> Summary july 29 </td> </tr>
  <tr> <td> Summary of July 3 </td> <td> Summary july 3 </td> </tr>
  <tr> <td> Summary of June 10 </td> <td> Summary june 10 </td> </tr>
  <tr> <td> Summary of June 11 </td> <td> Summary june 11 </td> </tr>
  <tr> <td> Summary of June 12 </td> <td> Summary june 12 </td> </tr>
  <tr> <td> Summary of June 13 </td> <td> Summary june 13 </td> </tr>
  <tr> <td> Summary of June 15 </td> <td> Summary june 15 </td> </tr>
  <tr> <td> Summary of June 16 </td> <td> Summary june 16 </td> </tr>
  <tr> <td> Summary of June 18 </td> <td> Summary june 18 </td> </tr>
  <tr> <td> Summary of June 23 </td> <td> Summary june 23 </td> </tr>
  <tr> <td> Summary of June 24 </td> <td> Summary june 24 </td> </tr>
  <tr> <td> Summary of June 3 </td> <td> Summary june 3 </td> </tr>
  <tr> <td> Summary of June 30 </td> <td> Summary june 30 </td> </tr>
  <tr> <td> Summary of June 4 </td> <td> Summary june 4 </td> </tr>
  <tr> <td> Summary of June 6 </td> <td> Summary june 6 </td> </tr>
  <tr> <td> Summary of March 14 </td> <td> Summary march 14 </td> </tr>
  <tr> <td> Summary of March 23 </td> <td> Summary march 23 </td> </tr>
  <tr> <td> Summary of March 24 </td> <td> Summary march 24 </td> </tr>
  <tr> <td> SUMMARY OF MARCH 24-25 </td> <td> Summary march 24-25 </td> </tr>
  <tr> <td> SUMMARY OF MARCH 27 </td> <td> Summary march 27 </td> </tr>
  <tr> <td> SUMMARY OF MARCH 29 </td> <td> Summary march 29 </td> </tr>
  <tr> <td> Summary of May 10 </td> <td> Summary may 10 </td> </tr>
  <tr> <td> Summary of May 13 </td> <td> Summary may 13 </td> </tr>
  <tr> <td> Summary of May 14 </td> <td> Summary may 14 </td> </tr>
  <tr> <td> Summary of May 22 </td> <td> Summary may 22 </td> </tr>
  <tr> <td> Summary of May 22 am </td> <td> Summary may 22 am </td> </tr>
  <tr> <td> Summary of May 22 pm </td> <td> Summary may 22 pm </td> </tr>
  <tr> <td> Summary of May 26 am </td> <td> Summary may 26 am </td> </tr>
  <tr> <td> Summary of May 26 pm </td> <td> Summary may 26 pm </td> </tr>
  <tr> <td> Summary of May 31 am </td> <td> Summary may 31 am </td> </tr>
  <tr> <td> Summary of May 31 pm </td> <td> Summary may 31 pm </td> </tr>
  <tr> <td> Summary of May 9-10 </td> <td> Summary may 9-10 </td> </tr>
  <tr> <td> Summary: Sept. 18 </td> <td> Summary september 18 </td> </tr>
  <tr> <td> Summary Sept. 25-26 </td> <td> Summary september 25-26 </td> </tr>
  <tr> <td> Summary September 20 </td> <td> Summary september 20 </td> </tr>
  <tr> <td> Summary September 23 </td> <td> Summary september 23 </td> </tr>
  <tr> <td> Summary September 3 </td> <td> Summary september 3 </td> </tr>
  <tr> <td> Summary September 4 </td> <td> Summary september 4 </td> </tr>
  <tr> <td> Temperature record </td> <td> Temperature record </td> </tr>
  <tr> <td> THUDERSTORM WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDEERSTORM WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERESTORM WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSNOW </td> <td> Thundersnow </td> </tr>
  <tr> <td> Thundersnow shower </td> <td> Thundersnow shower </td> </tr>
  <tr> <td> THUNDERSTORM </td> <td> Thunderstorm </td> </tr>
  <tr> <td> THUNDERSTORM DAMAGE </td> <td> Thunderstorm damage </td> </tr>
  <tr> <td> THUNDERSTORM DAMAGE TO </td> <td> Thunderstorm damage </td> </tr>
  <tr> <td> THUNDERSTORM HAIL </td> <td> Thunderstorm hail </td> </tr>
  <tr> <td> THUNDERSTORMS </td> <td> Thunderstorm </td> </tr>
  <tr> <td> THUNDERSTORMS WIND </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORMS WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORMW </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORMW 50 </td> <td> Thunderstorm wind 50 mph </td> </tr>
  <tr> <td> Thunderstorm Wind </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM WIND </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM WIND. </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM WIND 50 </td> <td> Thunderstorm wind 50 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND 52 </td> <td> Thunderstorm wind 52 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND 56 </td> <td> Thunderstorm wind 56 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND 59 </td> <td> Thunderstorm wind 59 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND 59 MPH </td> <td> Thunderstorm wind 59 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND 59 MPH. </td> <td> Thunderstorm wind 59 mph. </td> </tr>
  <tr> <td> THUNDERSTORM WIND 60 MPH </td> <td> Thunderstorm wind 60 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND 65MPH </td> <td> Thunderstorm wind 65 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND 65 MPH </td> <td> Thunderstorm wind 65 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND 69 </td> <td> Thunderstorm wind 69 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND 98 MPH </td> <td> Thunderstorm wind 98 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND/AWNING </td> <td> Thunderstorm wind/Awning </td> </tr>
  <tr> <td> THUNDERSTORM WIND (G40) </td> <td> Thunderstorm wind gusts 40 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND G50 </td> <td> Thunderstorm wind gusts 50 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND G51 </td> <td> Thunderstorm wind gusts 51 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND G52 </td> <td> Thunderstorm wind gusts 52 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND G55 </td> <td> Thunderstorm wind gusts 55 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND G60 </td> <td> Thunderstorm wind gusts 60 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND G61 </td> <td> Thunderstorm wind gusts 61 mph </td> </tr>
  <tr> <td> THUNDERSTORM WIND/HAIL </td> <td> Thunderstorm wind/Hail </td> </tr>
  <tr> <td> THUNDERSTORM WIND/LIGHTNING </td> <td> Thunderstorm wind/Lightning </td> </tr>
  <tr> <td> THUNDERSTORMWINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM  WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM W INDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM WINDS. </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM WINDS 13 </td> <td> Thunderstorm wind 13 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS 2 </td> <td> Thunderstorm wind 2 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS 50 </td> <td> Thunderstorm wind 50 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS 52 </td> <td> Thunderstorm wind 52 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS53 </td> <td> Thunderstorm wind 53 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS 53 </td> <td> Thunderstorm wind 53 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS 60 </td> <td> Thunderstorm wind 60 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS 61 </td> <td> Thunderstorm wind 61 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS 62 </td> <td> Thunderstorm wind 62 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS 63 MPH </td> <td> Thunderstorm wind 63 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDS AND </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM WINDS/FLASH FLOOD </td> <td> Thunderstorm wind/Flash flood </td> </tr>
  <tr> <td> THUNDERSTORM WINDS/ FLOOD </td> <td> Thunderstorm wind/Flood </td> </tr>
  <tr> <td> THUNDERSTORM WINDS/FLOODING </td> <td> Thunderstorm wind/Flood </td> </tr>
  <tr> <td> THUNDERSTORM WINDS FUNNEL CLOU </td> <td> Thunderstorm wind/Funnel cloud </td> </tr>
  <tr> <td> THUNDERSTORM WINDS/FUNNEL CLOU </td> <td> Thunderstorm wind/Funnel cloud </td> </tr>
  <tr> <td> THUNDERSTORM WINDS G </td> <td> Thunderstorm wind gusts </td> </tr>
  <tr> <td> THUNDERSTORM WINDS G60 </td> <td> Thunderstorm wind gusts 60 mph </td> </tr>
  <tr> <td> THUNDERSTORM WINDSHAIL </td> <td> Thunderstorm wind/Hail </td> </tr>
  <tr> <td> THUNDERSTORM WINDS HAIL </td> <td> Thunderstorm wind/Hail </td> </tr>
  <tr> <td> THUNDERSTORM WINDS/HAIL </td> <td> Thunderstorm wind/Hail </td> </tr>
  <tr> <td> THUNDERSTORM WINDS/ HAIL </td> <td> Thunderstorm wind/Hail </td> </tr>
  <tr> <td> THUNDERSTORM WINDS HEAVY RAIN </td> <td> Thunderstorm wind/Heavy rain </td> </tr>
  <tr> <td> THUNDERSTORM WINDS/HEAVY RAIN </td> <td> Thunderstorm wind/Heavy rain </td> </tr>
  <tr> <td> THUNDERSTORM WINDS      LE CEN </td> <td> Thunderstorm wind/le cen </td> </tr>
  <tr> <td> THUNDERSTORM WINDS LIGHTNING </td> <td> Thunderstorm wind/Lightning </td> </tr>
  <tr> <td> THUNDERSTORM WINDSS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORM WINDS SMALL STREA </td> <td> Thunderstorm wind/Small strea </td> </tr>
  <tr> <td> THUNDERSTORM WINDS URBAN FLOOD </td> <td> Thunderstorm wind/Urban flood </td> </tr>
  <tr> <td> THUNDERSTORM WIND/ TREE </td> <td> Thunderstorm wind/Tree </td> </tr>
  <tr> <td> THUNDERSTORM WIND TREES </td> <td> Thunderstorm wind/Tree </td> </tr>
  <tr> <td> THUNDERSTORM WIND/ TREES </td> <td> Thunderstorm wind/Tree </td> </tr>
  <tr> <td> THUNDERSTORM WINS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTORMW WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTROM WIND </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERSTROM WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERTORM WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDERTSORM WIND </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNDESTORM WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> THUNERSTORM WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> TIDAL FLOOD </td> <td> Tidal flood </td> </tr>
  <tr> <td> Tidal Flooding </td> <td> Tidal flood </td> </tr>
  <tr> <td> TIDAL FLOODING </td> <td> Tidal flood </td> </tr>
  <tr> <td> TORNADO </td> <td> Tornado </td> </tr>
  <tr> <td> TORNADO DEBRIS </td> <td> Tornado debris </td> </tr>
  <tr> <td> TORNADOES </td> <td> Tornado </td> </tr>
  <tr> <td> TORNADOES, TSTM WIND, HAIL </td> <td> Tornado/Thunderstorm wind/Hail </td> </tr>
  <tr> <td> TORNADO F0 </td> <td> Tornado f0 </td> </tr>
  <tr> <td> TORNADO F1 </td> <td> Tornado f1 </td> </tr>
  <tr> <td> TORNADO F2 </td> <td> Tornado f2 </td> </tr>
  <tr> <td> TORNADO F3 </td> <td> Tornado f3 </td> </tr>
  <tr> <td> TORNADOS </td> <td> Tornado </td> </tr>
  <tr> <td> TORNADO/WATERSPOUT </td> <td> Tornado/Waterspout </td> </tr>
  <tr> <td> TORNDAO </td> <td> Tornado </td> </tr>
  <tr> <td> TORRENTIAL RAIN </td> <td> Torrential rain </td> </tr>
  <tr> <td> Torrential Rainfall </td> <td> Torrential rain </td> </tr>
  <tr> <td> TROPICAL DEPRESSION </td> <td> Tropical depression </td> </tr>
  <tr> <td> TROPICAL STORM </td> <td> Tropical storm </td> </tr>
  <tr> <td> TROPICAL STORM ALBERTO </td> <td> Tropical storm Alberto </td> </tr>
  <tr> <td> TROPICAL STORM DEAN </td> <td> Tropical storm Dean </td> </tr>
  <tr> <td> TROPICAL STORM GORDON </td> <td> Tropical storm Gordon </td> </tr>
  <tr> <td> TROPICAL STORM JERRY </td> <td> Tropical storm Jerry </td> </tr>
  <tr> <td> TSTM </td> <td> Thunderstorm </td> </tr>
  <tr> <td> TSTM HEAVY RAIN </td> <td> Thunderstorm/Heavy rain </td> </tr>
  <tr> <td> TSTMW </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> Tstm Wind </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> TSTM WIND </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> TSTM WIND 40 </td> <td> Thunderstorm wind 40 mph </td> </tr>
  <tr> <td> TSTM WIND (41) </td> <td> Thunderstorm wind 41 mph </td> </tr>
  <tr> <td> TSTM WIND 45 </td> <td> Thunderstorm wind 45 mph </td> </tr>
  <tr> <td> TSTM WIND 50 </td> <td> Thunderstorm wind 50 mph </td> </tr>
  <tr> <td> TSTM WIND 51 </td> <td> Thunderstorm wind 51 mph </td> </tr>
  <tr> <td> TSTM WIND 52 </td> <td> Thunderstorm wind 52 mph </td> </tr>
  <tr> <td> TSTM WIND 55 </td> <td> Thunderstorm wind 55 mph </td> </tr>
  <tr> <td> TSTM WIND 65) </td> <td> Thunderstorm wind 65 mph </td> </tr>
  <tr> <td> TSTM WIND AND LIGHTNING </td> <td> Thunderstorm wind/Lightning </td> </tr>
  <tr> <td> TSTM WIND DAMAGE </td> <td> Thunderstorm wind damage </td> </tr>
  <tr> <td> TSTM WIND (G35) </td> <td> Thunderstorm wind gusts 35 mph </td> </tr>
  <tr> <td> TSTM WIND (G40) </td> <td> Thunderstorm wind gusts 40 mph </td> </tr>
  <tr> <td> TSTM WIND G45 </td> <td> Thunderstorm wind gusts 45 mph </td> </tr>
  <tr> <td> TSTM WIND  (G45) </td> <td> Thunderstorm wind gusts 45 mph </td> </tr>
  <tr> <td> TSTM WIND (G45) </td> <td> Thunderstorm wind gusts 45 mph </td> </tr>
  <tr> <td> TSTM WIND G58 </td> <td> Thunderstorm wind gusts 58 mph </td> </tr>
  <tr> <td> TSTM WIND/HAIL </td> <td> Thunderstorm wind/Hail </td> </tr>
  <tr> <td> TSTM WINDS </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> TSTM WND </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> TSUNAMI </td> <td> Tsunami </td> </tr>
  <tr> <td> TUNDERSTORM WIND </td> <td> Thunderstorm wind </td> </tr>
  <tr> <td> TYPHOON </td> <td> Typhoon </td> </tr>
  <tr> <td> Unseasonable Cold </td> <td> Unseasonably cold </td> </tr>
  <tr> <td> UNSEASONABLY COLD </td> <td> Unseasonably cold </td> </tr>
  <tr> <td> UNSEASONABLY COOL </td> <td> Unseasonably cool </td> </tr>
  <tr> <td> UNSEASONABLY COOL &amp; WET </td> <td> Unseasonably cool/Wet </td> </tr>
  <tr> <td> UNSEASONABLY DRY </td> <td> Unseasonably dry </td> </tr>
  <tr> <td> UNSEASONABLY HOT </td> <td> Unseasonably hot </td> </tr>
  <tr> <td> UNSEASONABLY WARM </td> <td> Unseasonably warm </td> </tr>
  <tr> <td> UNSEASONABLY WARM AND DRY </td> <td> Unseasonably warm/Dry </td> </tr>
  <tr> <td> UNSEASONABLY WARM &amp; WET </td> <td> Unseasonably warm/Wet </td> </tr>
  <tr> <td> UNSEASONABLY WARM/WET </td> <td> Unseasonably warm/Wet </td> </tr>
  <tr> <td> UNSEASONABLY WARM YEAR </td> <td> Unseasonably warm year </td> </tr>
  <tr> <td> UNSEASONABLY WET </td> <td> Unseasonably wet </td> </tr>
  <tr> <td> UNSEASONAL LOW TEMP </td> <td> Unseasonal low temp </td> </tr>
  <tr> <td> UNSEASONAL RAIN </td> <td> Unseasonal rain </td> </tr>
  <tr> <td> UNUSUALLY COLD </td> <td> Unusually cold </td> </tr>
  <tr> <td> UNUSUALLY LATE SNOW </td> <td> Unusually late snow </td> </tr>
  <tr> <td> UNUSUALLY WARM </td> <td> Unusually warm </td> </tr>
  <tr> <td> UNUSUAL/RECORD WARMTH </td> <td> Unusually/Record warmth </td> </tr>
  <tr> <td> UNUSUAL WARMTH </td> <td> Unusually warm </td> </tr>
  <tr> <td> URBAN AND SMALL </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN AND SMALL STREAM </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN AND SMALL STREAM FLOOD </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN AND SMALL STREAM FLOODIN </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> Urban flood </td> <td> Urban flood </td> </tr>
  <tr> <td> Urban Flood </td> <td> Urban flood </td> </tr>
  <tr> <td> URBAN FLOOD </td> <td> Urban flood </td> </tr>
  <tr> <td> Urban Flooding </td> <td> Urban flood </td> </tr>
  <tr> <td> URBAN FLOODING </td> <td> Urban flood </td> </tr>
  <tr> <td> URBAN FLOOD LANDSLIDE </td> <td> Urban flood/Landslide </td> </tr>
  <tr> <td> URBAN FLOODS </td> <td> Urban flood </td> </tr>
  <tr> <td> URBAN SMALL </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/SMALL </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/SMALL FLOODING </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/SMALL STREAM </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN SMALL STREAM FLOOD </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/SMALL STREAM FLOOD </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/SMALL STREAM  FLOOD </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/SMALL STREAM FLOODING </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/SMALL STRM FLDG </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/SML STREAM FLD </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/SML STREAM FLDG </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> URBAN/STREET FLOODING </td> <td> Urban stream flood/Small stream flood </td> </tr>
  <tr> <td> VERY DRY </td> <td> Very dry </td> </tr>
  <tr> <td> VERY WARM </td> <td> Very warm </td> </tr>
  <tr> <td> VOG </td> <td> Fog </td> </tr>
  <tr> <td> Volcanic Ash </td> <td> Volcanic ash </td> </tr>
  <tr> <td> VOLCANIC ASH </td> <td> Volcanic ash </td> </tr>
  <tr> <td> VOLCANIC ASHFALL </td> <td> Volcanic ash </td> </tr>
  <tr> <td> Volcanic Ash Plume </td> <td> Volcanic ash </td> </tr>
  <tr> <td> VOLCANIC ERUPTION </td> <td> Volcanic eruption </td> </tr>
  <tr> <td> WAKE LOW WIND </td> <td> Wake low wind </td> </tr>
  <tr> <td> WALL CLOUD </td> <td> Wall cloud </td> </tr>
  <tr> <td> WALL CLOUD/FUNNEL CLOUD </td> <td> Wall cloud/Funnel cloud </td> </tr>
  <tr> <td> WARM DRY CONDITIONS </td> <td> Warm dry conditions </td> </tr>
  <tr> <td> WARM WEATHER </td> <td> Warm weather </td> </tr>
  <tr> <td> WATERSPOUT </td> <td> Waterspout </td> </tr>
  <tr> <td> WATER SPOUT </td> <td> Waterspout </td> </tr>
  <tr> <td> WATERSPOUT- </td> <td> Waterspout </td> </tr>
  <tr> <td> WATERSPOUT/ </td> <td> Waterspout </td> </tr>
  <tr> <td> WATERSPOUT FUNNEL CLOUD </td> <td> Waterspout/Funnel cloud </td> </tr>
  <tr> <td> WATERSPOUTS </td> <td> Waterspout </td> </tr>
  <tr> <td> WATERSPOUT TORNADO </td> <td> Waterspout/Tornado </td> </tr>
  <tr> <td> WATERSPOUT-TORNADO </td> <td> Waterspout/Tornado </td> </tr>
  <tr> <td> WATERSPOUT/TORNADO </td> <td> Waterspout/Tornado </td> </tr>
  <tr> <td> WATERSPOUT/ TORNADO </td> <td> Waterspout/Tornado </td> </tr>
  <tr> <td> WAYTERSPOUT </td> <td> Waterspout </td> </tr>
  <tr> <td> wet micoburst </td> <td> Wet microburst </td> </tr>
  <tr> <td> WET MICROBURST </td> <td> Wet microburst </td> </tr>
  <tr> <td> Wet Month </td> <td> Wet month </td> </tr>
  <tr> <td> WET SNOW </td> <td> Wet snow </td> </tr>
  <tr> <td> WET WEATHER </td> <td> Wet weather </td> </tr>
  <tr> <td> Wet Year </td> <td> Wet year </td> </tr>
  <tr> <td> Whirlwind </td> <td> Whirlwind </td> </tr>
  <tr> <td> WHIRLWIND </td> <td> Whirlwind </td> </tr>
  <tr> <td> WILDFIRE </td> <td> Wildfire </td> </tr>
  <tr> <td> WILDFIRES </td> <td> Wildfire </td> </tr>
  <tr> <td> WILD FIRES </td> <td> Wildfire </td> </tr>
  <tr> <td> WILD/FOREST FIRE </td> <td> Wildfire/Forest fire </td> </tr>
  <tr> <td> WILD/FOREST FIRES </td> <td> Wildfire/Forest fire </td> </tr>
  <tr> <td> Wind </td> <td> Wind </td> </tr>
  <tr> <td> WIND </td> <td> Wind </td> </tr>
  <tr> <td> WIND ADVISORY </td> <td> Wind advisory </td> </tr>
  <tr> <td> WIND AND WAVE </td> <td> Wind/Wave </td> </tr>
  <tr> <td> WIND CHILL </td> <td> Wind chill </td> </tr>
  <tr> <td> WIND CHILL/HIGH WIND </td> <td> Wind chill/High wind </td> </tr>
  <tr> <td> Wind Damage </td> <td> Wind damage </td> </tr>
  <tr> <td> WIND DAMAGE </td> <td> Wind damage </td> </tr>
  <tr> <td> WIND GUSTS </td> <td> Wind gusts </td> </tr>
  <tr> <td> WIND/HAIL </td> <td> Wind/Hail </td> </tr>
  <tr> <td> WINDS </td> <td> Wind </td> </tr>
  <tr> <td> WIND STORM </td> <td> Wind storm </td> </tr>
  <tr> <td> WINTER MIX </td> <td> Winter mix </td> </tr>
  <tr> <td> WINTER STORM </td> <td> Winter storm </td> </tr>
  <tr> <td> WINTER STORM/HIGH WIND </td> <td> Winter storm/High wind </td> </tr>
  <tr> <td> WINTER STORM HIGH WINDS </td> <td> Winter storm/High wind </td> </tr>
  <tr> <td> WINTER STORM/HIGH WINDS </td> <td> Winter storm/High wind </td> </tr>
  <tr> <td> WINTER STORMS </td> <td> Winter storm </td> </tr>
  <tr> <td> Winter Weather </td> <td> Winter weather </td> </tr>
  <tr> <td> WINTER WEATHER </td> <td> Winter weather </td> </tr>
  <tr> <td> WINTER WEATHER MIX </td> <td> Winter weather mix </td> </tr>
  <tr> <td> WINTER WEATHER/MIX </td> <td> Winter weather mix </td> </tr>
  <tr> <td> WINTERY MIX </td> <td> Winter mix </td> </tr>
  <tr> <td> Wintry mix </td> <td> Winter mix </td> </tr>
  <tr> <td> Wintry Mix </td> <td> Winter mix </td> </tr>
  <tr> <td> WINTRY MIX </td> <td> Winter mix </td> </tr>
  <tr> <td> WND </td> <td> Wind </td> </tr>
   </table>

*This table is available in [EVTYPE.csv](../data/EVTYPE.csv)*

### BGN_RANGE

`BGN_RANGE` is a numeric variable.
It contains 272 unique values ranging from 0 to 3749.

### BGN_AZI

`BGN_AZI` is a character variable describing the azimuth

### BGN_LOCATI

`BGN_LOCATI`

### END_DATE

`END_DATE`

### END_TIME

`END_TIME`

### COUNTY_END

`COUNTY_END`

### COUNTYENDN

`COUNTYENDN`

### END_RANGE

`END_RANGE`

### END_AZI

`END_AZI`

### END_LOCATI

`END_LOCATI`

### LENGTH

`LENGTH`

### WIDTH

`WIDTH`

### F

`F`

### MAG

`MAG`

### FATALITIES

`FATALITIES` fatalities

### INJURIES

`INJURIES` injuries

### PROPDMG

`PROPDMG` property damage

### PROPDMGEXP

`PROPDMGEXP` property damage expense magnitude

### CROPDMG

`CROPDMG` crop damage

### CROPDMGEXP

`CROPDMGEXP` crop damage expense magnitude

### WFO

`WFO`

### STATEOFFIC

`STATEOFFIC`

### ZONENAMES

`ZONENAMES`

### LATITUDE

`LATITUDE`

### LONGITUDE

`LONGITUDE`

### LATITUDE_E

`LATITUDE_E`

### LONGITUDE_

`LONGITUDE_`

### REMARKS

`REMARKS`

### REFNUM

`REFNUM`


## Preprocessed data

### event.type.major

`event.type.major` is a factor variable containing uniform event types,
based on the original [EVTYPE](#evtype) and the listed transformations.
They contain only the part before the first "/" in the original variable.
`event.type.major` contains 485 unique values, ranging from "Abnormally dry" to
"Winter weather mix".

### event.type.minor

`event.type.minor` is a factor variable containing the minor event types,
based on the original [EVTYPE](#evtype) and the listed transformations.
`event.type.minor` contains 98 distinct values, ranging from "Awning" to
"Winter storm".

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
