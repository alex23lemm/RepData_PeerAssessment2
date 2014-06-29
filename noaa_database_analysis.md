Human costs of severe weather events in the United States (1950 - 2011)
=======================================================================

Synopsis
--------

Among the different storm event types, tornados seem to have the most
severe impact on population health taking into account numbers of people
injured or killed. In addition, there are a couple of other storm event
types which influence both, injuries and fatalities, more drastically
than others: Thunderstorm wind, excessive heat, flood and lightning.
However, compared to tornados their impact is smaller in regards to
people affected.

In general, the overall trend shows an increase of people injured or
killed by storm events since the mid 90s. In parts, this can be
explained with the massive increase of reported storm observations since
1993. In general, storm events seem to have a larger impact in the east
/ south east of the United States on population health compared to the
rest of the country.

In the first chapter *Data Processing* of this report we will derive a
processed dataset based on the raw data. Based on the processed dataset,
we will conduct a detailed data analysis in the subsequent chapter
*Results*.

**Note**: Based on time constraints on my side I was not able to answer
the second question of the assignment.

Data Processing
---------------

### Loading libraries

    library(stringr)
    library(plyr)
    library(dplyr)
    library(ggplot2)
    library(xtable)
    library(grid)
    library(lubridate)
    library(reshape2)
    library(maps)
    library(RColorBrewer)

### Loading data into R

    storms_df <- read.csv('repdata_data_StormData.csv.bz2', na.strings = c('NA', ''),
                          stringsAsFactors = FALSE)

### Examining raw data

    dim(storms_df)

    ## [1] 902297     37

    sum(complete.cases(storms_df))

    ## [1] 0

    names(storms_df)

    ##  [1] "STATE__"    "BGN_DATE"   "BGN_TIME"   "TIME_ZONE"  "COUNTY"    
    ##  [6] "COUNTYNAME" "STATE"      "EVTYPE"     "BGN_RANGE"  "BGN_AZI"   
    ## [11] "BGN_LOCATI" "END_DATE"   "END_TIME"   "COUNTY_END" "COUNTYENDN"
    ## [16] "END_RANGE"  "END_AZI"    "END_LOCATI" "LENGTH"     "WIDTH"     
    ## [21] "F"          "MAG"        "FATALITIES" "INJURIES"   "PROPDMG"   
    ## [26] "PROPDMGEXP" "CROPDMG"    "CROPDMGEXP" "WFO"        "STATEOFFIC"
    ## [31] "ZONENAMES"  "LATITUDE"   "LONGITUDE"  "LATITUDE_E" "LONGITUDE_"
    ## [36] "REMARKS"    "REFNUM"

### The EVTYPE variable

We take a close look at the `EVTYPE` variable which is extremely
important for answering both questions addressed in this report.

    unique_raw_events <- length(unique(storms_df$EVTYPE))
    unique_raw_events

    ## [1] 985

Although the official storm data documentation on the course assessment
page lists 48 storm events (page 6), our dataset includes 985 different
event types.

An examination of the data reveals that the reason for that huge
difference in event types is caused by using different wording for the
same kind of events in the raw data.

For instance, you can find more than 80 variations for the official
event type term *Thunderstorm Wind*:

    tstm_subset_df <- group_by(storms_df, EVTYPE) %>%
      summarize(
        number_of_obs = n()
        ) %>% 
      arrange(EVTYPE)

    print(xtable(tstm_subset_df[743:823, ], 
                 caption = "<b>Table 1:</b> Examples of event type name modifications"), type = 'html', include.rownames = FALSE)

<!-- html table generated in R 3.1.0 by xtable 1.7-3 package -->
<!-- Sun Jun 29 18:28:16 2014 -->
<TABLE border=1>
<CAPTION ALIGN="bottom"> 
<b>Table 1:</b> Examples of event type name modifications
</CAPTION>
<TR> <TH> 
EVTYPE
</TH> <TH> 
number\_of\_obs
</TH>  </TR>
  <TR> <TD> 
THUDERSTORM WINDS
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDEERSTORM WINDS
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERESTORM WINDS
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSNOW
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM
</TD> <TD align="right">  
45
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS
</TD> <TD align="right">   
7
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM DAMAGE
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM DAMAGE TO
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM HAIL
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM W INDS
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND
</TD> <TD align="right"> 
82563
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND (G40)
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 50
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 52
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 56
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 59
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 59 MPH
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 59 MPH.
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 60 MPH
</TD> <TD align="right">   
4
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 65 MPH
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 65MPH
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 69
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND 98 MPH
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND G50
</TD> <TD align="right">   
4
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND G51
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND G52
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND G55
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND G60
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND G61
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND TREES
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND.
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND/ TREE
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND/ TREES
</TD> <TD align="right">   
4
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND/AWNING
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND/HAIL
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WIND/LIGHTNING
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS
</TD> <TD align="right"> 
20843
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS LE CEN
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS 13
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS 2
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS 50
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS 52
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS 53
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS 60
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS 61
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS 62
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS 63 MPH
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS AND
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS FUNNEL CLOU
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS G
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS G60
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS HAIL
</TD> <TD align="right">  
61
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS HEAVY RAIN
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS LIGHTNING
</TD> <TD align="right">   
7
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS SMALL STREA
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS URBAN FLOOD
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS.
</TD> <TD align="right">   
3
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS/ FLOOD
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS/ HAIL
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS/FLASH FLOOD
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS/FLOODING
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS/FUNNEL CLOU
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS/HAIL
</TD> <TD align="right">  
24
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS/HEAVY RAIN
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDS53
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDSHAIL
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINDSS
</TD> <TD align="right">  
51
</TD> </TR>
  <TR> <TD> 
THUNDERSTORM WINS
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORMS
</TD> <TD align="right">   
4
</TD> </TR>
  <TR> <TD> 
THUNDERSTORMS WIND
</TD> <TD align="right">   
6
</TD> </TR>
  <TR> <TD> 
THUNDERSTORMS WINDS
</TD> <TD align="right">  
14
</TD> </TR>
  <TR> <TD> 
THUNDERSTORMW
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORMW 50
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTORMW WINDS
</TD> <TD align="right">   
3
</TD> </TR>
  <TR> <TD> 
THUNDERSTORMWINDS
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTROM WIND
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDERSTROM WINDS
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNDERTORM WINDS
</TD> <TD align="right">   
3
</TD> </TR>
  <TR> <TD> 
THUNDERTSORM WIND
</TD> <TD align="right">   
1
</TD> </TR>
  <TR> <TD> 
THUNDESTORM WINDS
</TD> <TD align="right">   
2
</TD> </TR>
  <TR> <TD> 
THUNERSTORM WINDS
</TD> <TD align="right">   
1
</TD> </TR>
   </TABLE>
<br>

This is caused by:

1.  Modifications of the official event type name (e.g., THUNDERSTORM
    WINDS or THUNDERSTORM)

2.  Typos (e.g., THUDERSTORM WINDS or THUNDERSTORM WINS)

However, 82563 observations tagged with the official term *Thunderstorm
Wind* are included in the dataset whereas most of the event name
modifications just include a single observation.

This shows that in the majority of the cases the event reporters used
the official event type name.

### Event type number reduction strategy

In order to reduce the number of event types to a fair amount, we use a
simple quantitative approach consisting of 5 steps:

1.  We convert the `EVTYPE` column to lower case and remove leading and
    trailing white spaces

2.  For each of the remaining unique event types we calculate how much
    it contributes to the overall completeness of the number of
    observations

3.  We decide on a threshold and select those event types which
    contribute the most to the overall amount of observations

4.  We filter the loaded raw data according to chosen event types

5.  The remaining set is further reduced by merging similar storm events
    with different wording

#### Step 1: Removing leading and trailing which spaces

    storms_df$EVTYPE <- tolower(storms_df$EVTYPE)

    storms_df$EVTYPE <- str_trim(storms_df$EVTYPE)

    paste0("Total number of unique event types after first pre-processing step: ",
           length(unique(storms_df$EVTYPE)))

    ## [1] "Total number of unique event types after first pre-processing step: 890"

#### Step 2: Calculating the percentage / cumulative percentage that each event type contributes to the total amount of observations

-   We will group the dataset by event type and calculate the total
    number of observations which belong to each event type.

-   After that we will calculate the **percentage** each event type
    contributes to the overall amount of the observations.

-   In addition, we will calculate the **cumulative percentage** for
    each event type and sort the rows by the number of observations in
    descending order.

<!-- -->

    storms_by_evtype_df <- group_by(storms_df, EVTYPE) %>%
      summarize(
        numb_of_occ = n()
        ) %>%
      mutate(
        contrib_in_percent = round(numb_of_occ / nrow(storms_df), digits = 7),
        cumb_contrib_in_percent = round(
          order_by(desc(numb_of_occ), cumsum(contrib_in_percent)), digits = 7
          )
        ) %>%
      arrange(desc(numb_of_occ))

    storms_by_evtype_df$rank <-  1:nrow(storms_by_evtype_df)

    print(xtable(storms_by_evtype_df[1:47, ], 
                 caption = "<b>Table 2:</b> The 47 event types with the highest observation percentage contribution", digits = 7), type = 'html', include.rownames = FALSE)

<!-- html table generated in R 3.1.0 by xtable 1.7-3 package -->
<!-- Sun Jun 29 18:28:18 2014 -->
<TABLE border=1>
<CAPTION ALIGN="bottom"> 
<b>Table 2:</b> The 47 event types with the highest observation
percentage contribution
</CAPTION>
<TR> <TH> 
EVTYPE
</TH> <TH> 
numb\_of\_occ
</TH> <TH> 
contrib\_in\_percent
</TH> <TH> 
cumb\_contrib\_in\_percent
</TH> <TH> 
rank
</TH>  </TR>
  <TR> <TD> 
hail
</TD> <TD align="right">   
288661
</TD> <TD align="right"> 
0.3199179
</TD> <TD align="right"> 
0.3199179
</TD> <TD align="right">        
1
</TD> </TR>
  <TR> <TD> 
tstm wind
</TD> <TD align="right">   
219946
</TD> <TD align="right"> 
0.2437623
</TD> <TD align="right"> 
0.5636802
</TD> <TD align="right">        
2
</TD> </TR>
  <TR> <TD> 
thunderstorm wind
</TD> <TD align="right">    
82564
</TD> <TD align="right"> 
0.0915042
</TD> <TD align="right"> 
0.6551844
</TD> <TD align="right">        
3
</TD> </TR>
  <TR> <TD> 
tornado
</TD> <TD align="right">    
60652
</TD> <TD align="right"> 
0.0672196
</TD> <TD align="right"> 
0.7224040
</TD> <TD align="right">        
4
</TD> </TR>
  <TR> <TD> 
flash flood
</TD> <TD align="right">    
54278
</TD> <TD align="right"> 
0.0601554
</TD> <TD align="right"> 
0.7825594
</TD> <TD align="right">        
5
</TD> </TR>
  <TR> <TD> 
flood
</TD> <TD align="right">    
25327
</TD> <TD align="right"> 
0.0280695
</TD> <TD align="right"> 
0.8106289
</TD> <TD align="right">        
6
</TD> </TR>
  <TR> <TD> 
thunderstorm winds
</TD> <TD align="right">    
20843
</TD> <TD align="right"> 
0.0230999
</TD> <TD align="right"> 
0.8337288
</TD> <TD align="right">        
7
</TD> </TR>
  <TR> <TD> 
high wind
</TD> <TD align="right">    
20214
</TD> <TD align="right"> 
0.0224028
</TD> <TD align="right"> 
0.8561316
</TD> <TD align="right">        
8
</TD> </TR>
  <TR> <TD> 
lightning
</TD> <TD align="right">    
15755
</TD> <TD align="right"> 
0.0174610
</TD> <TD align="right"> 
0.8735926
</TD> <TD align="right">        
9
</TD> </TR>
  <TR> <TD> 
heavy snow
</TD> <TD align="right">    
15708
</TD> <TD align="right"> 
0.0174089
</TD> <TD align="right"> 
0.8910015
</TD> <TD align="right">       
10
</TD> </TR>
  <TR> <TD> 
heavy rain
</TD> <TD align="right">    
11742
</TD> <TD align="right"> 
0.0130135
</TD> <TD align="right"> 
0.9040150
</TD> <TD align="right">       
11
</TD> </TR>
  <TR> <TD> 
winter storm
</TD> <TD align="right">    
11433
</TD> <TD align="right"> 
0.0126710
</TD> <TD align="right"> 
0.9166860
</TD> <TD align="right">       
12
</TD> </TR>
  <TR> <TD> 
winter weather
</TD> <TD align="right">     
7045
</TD> <TD align="right"> 
0.0078079
</TD> <TD align="right"> 
0.9244939
</TD> <TD align="right">       
13
</TD> </TR>
  <TR> <TD> 
funnel cloud
</TD> <TD align="right">     
6844
</TD> <TD align="right"> 
0.0075851
</TD> <TD align="right"> 
0.9320790
</TD> <TD align="right">       
14
</TD> </TR>
  <TR> <TD> 
marine tstm wind
</TD> <TD align="right">     
6175
</TD> <TD align="right"> 
0.0068436
</TD> <TD align="right"> 
0.9389226
</TD> <TD align="right">       
15
</TD> </TR>
  <TR> <TD> 
marine thunderstorm wind
</TD> <TD align="right">     
5812
</TD> <TD align="right"> 
0.0064413
</TD> <TD align="right"> 
0.9453639
</TD> <TD align="right">       
16
</TD> </TR>
  <TR> <TD> 
waterspout
</TD> <TD align="right">     
3797
</TD> <TD align="right"> 
0.0042081
</TD> <TD align="right"> 
0.9495720
</TD> <TD align="right">       
17
</TD> </TR>
  <TR> <TD> 
strong wind
</TD> <TD align="right">     
3569
</TD> <TD align="right"> 
0.0039555
</TD> <TD align="right"> 
0.9535275
</TD> <TD align="right">       
18
</TD> </TR>
  <TR> <TD> 
urban/sml stream fld
</TD> <TD align="right">     
3392
</TD> <TD align="right"> 
0.0037593
</TD> <TD align="right"> 
0.9572868
</TD> <TD align="right">       
19
</TD> </TR>
  <TR> <TD> 
wildfire
</TD> <TD align="right">     
2761
</TD> <TD align="right"> 
0.0030600
</TD> <TD align="right"> 
0.9603468
</TD> <TD align="right">       
20
</TD> </TR>
  <TR> <TD> 
blizzard
</TD> <TD align="right">     
2719
</TD> <TD align="right"> 
0.0030134
</TD> <TD align="right"> 
0.9633602
</TD> <TD align="right">       
21
</TD> </TR>
  <TR> <TD> 
drought
</TD> <TD align="right">     
2488
</TD> <TD align="right"> 
0.0027574
</TD> <TD align="right"> 
0.9661176
</TD> <TD align="right">       
22
</TD> </TR>
  <TR> <TD> 
ice storm
</TD> <TD align="right">     
2006
</TD> <TD align="right"> 
0.0022232
</TD> <TD align="right"> 
0.9683408
</TD> <TD align="right">       
23
</TD> </TR>
  <TR> <TD> 
excessive heat
</TD> <TD align="right">     
1678
</TD> <TD align="right"> 
0.0018597
</TD> <TD align="right"> 
0.9702005
</TD> <TD align="right">       
24
</TD> </TR>
  <TR> <TD> 
high winds
</TD> <TD align="right">     
1533
</TD> <TD align="right"> 
0.0016990
</TD> <TD align="right"> 
0.9718995
</TD> <TD align="right">       
25
</TD> </TR>
  <TR> <TD> 
wild/forest fire
</TD> <TD align="right">     
1457
</TD> <TD align="right"> 
0.0016148
</TD> <TD align="right"> 
0.9735143
</TD> <TD align="right">       
26
</TD> </TR>
  <TR> <TD> 
frost/freeze
</TD> <TD align="right">     
1343
</TD> <TD align="right"> 
0.0014884
</TD> <TD align="right"> 
0.9750027
</TD> <TD align="right">       
27
</TD> </TR>
  <TR> <TD> 
dense fog
</TD> <TD align="right">     
1293
</TD> <TD align="right"> 
0.0014330
</TD> <TD align="right"> 
0.9764357
</TD> <TD align="right">       
28
</TD> </TR>
  <TR> <TD> 
winter weather/mix
</TD> <TD align="right">     
1104
</TD> <TD align="right"> 
0.0012235
</TD> <TD align="right"> 
0.9776592
</TD> <TD align="right">       
29
</TD> </TR>
  <TR> <TD> 
tstm wind/hail
</TD> <TD align="right">     
1028
</TD> <TD align="right"> 
0.0011393
</TD> <TD align="right"> 
0.9787985
</TD> <TD align="right">       
30
</TD> </TR>
  <TR> <TD> 
extreme cold/wind chill
</TD> <TD align="right">     
1002
</TD> <TD align="right"> 
0.0011105
</TD> <TD align="right"> 
0.9799090
</TD> <TD align="right">       
31
</TD> </TR>
  <TR> <TD> 
heat
</TD> <TD align="right">      
767
</TD> <TD align="right"> 
0.0008501
</TD> <TD align="right"> 
0.9807591
</TD> <TD align="right">       
32
</TD> </TR>
  <TR> <TD> 
high surf
</TD> <TD align="right">      
734
</TD> <TD align="right"> 
0.0008135
</TD> <TD align="right"> 
0.9815726
</TD> <TD align="right">       
33
</TD> </TR>
  <TR> <TD> 
tropical storm
</TD> <TD align="right">      
690
</TD> <TD align="right"> 
0.0007647
</TD> <TD align="right"> 
0.9823373
</TD> <TD align="right">       
34
</TD> </TR>
  <TR> <TD> 
flash flooding
</TD> <TD align="right">      
682
</TD> <TD align="right"> 
0.0007558
</TD> <TD align="right"> 
0.9830931
</TD> <TD align="right">       
35
</TD> </TR>
  <TR> <TD> 
coastal flood
</TD> <TD align="right">      
657
</TD> <TD align="right"> 
0.0007281
</TD> <TD align="right"> 
0.9838212
</TD> <TD align="right">       
36
</TD> </TR>
  <TR> <TD> 
extreme cold
</TD> <TD align="right">      
657
</TD> <TD align="right"> 
0.0007281
</TD> <TD align="right"> 
0.9845493
</TD> <TD align="right">       
37
</TD> </TR>
  <TR> <TD> 
lake-effect snow
</TD> <TD align="right">      
636
</TD> <TD align="right"> 
0.0007049
</TD> <TD align="right"> 
0.9852542
</TD> <TD align="right">       
38
</TD> </TR>
  <TR> <TD> 
flood/flash flood
</TD> <TD align="right">      
625
</TD> <TD align="right"> 
0.0006927
</TD> <TD align="right"> 
0.9859469
</TD> <TD align="right">       
39
</TD> </TR>
  <TR> <TD> 
snow
</TD> <TD align="right">      
617
</TD> <TD align="right"> 
0.0006838
</TD> <TD align="right"> 
0.9866307
</TD> <TD align="right">       
40
</TD> </TR>
  <TR> <TD> 
landslide
</TD> <TD align="right">      
600
</TD> <TD align="right"> 
0.0006650
</TD> <TD align="right"> 
0.9872957
</TD> <TD align="right">       
41
</TD> </TR>
  <TR> <TD> 
cold/wind chill
</TD> <TD align="right">      
539
</TD> <TD align="right"> 
0.0005974
</TD> <TD align="right"> 
0.9878931
</TD> <TD align="right">       
42
</TD> </TR>
  <TR> <TD> 
fog
</TD> <TD align="right">      
538
</TD> <TD align="right"> 
0.0005963
</TD> <TD align="right"> 
0.9884894
</TD> <TD align="right">       
43
</TD> </TR>
  <TR> <TD> 
rip current
</TD> <TD align="right">      
470
</TD> <TD align="right"> 
0.0005209
</TD> <TD align="right"> 
0.9890103
</TD> <TD align="right">       
44
</TD> </TR>
  <TR> <TD> 
marine hail
</TD> <TD align="right">      
442
</TD> <TD align="right"> 
0.0004899
</TD> <TD align="right"> 
0.9895002
</TD> <TD align="right">       
45
</TD> </TR>
  <TR> <TD> 
dust storm
</TD> <TD align="right">      
427
</TD> <TD align="right"> 
0.0004732
</TD> <TD align="right"> 
0.9899734
</TD> <TD align="right">       
46
</TD> </TR>
  <TR> <TD> 
avalanche
</TD> <TD align="right">      
386
</TD> <TD align="right"> 
0.0004278
</TD> <TD align="right"> 
0.9904012
</TD> <TD align="right">       
47
</TD> </TR>
   </TABLE>
<br>

As the table above shows, the first 47 event types already make up for
99 % of the total amount of observations.

This can be shown even better when plotting the grouped data.

    # Add dummy row at position (0,0) for plotting purposes
    storms_by_evtype_df <- rbind(storms_by_evtype_df, c(0, 0, 0, 0, 0)) %>%
      arrange(cumb_contrib_in_percent)

<figure>

    ggplot(storms_by_evtype_df, aes(rank, cumb_contrib_in_percent)) + 
      geom_line(color = 'steelblue') + 
      geom_vline(xintercept = 69) +
      xlab('Unique event types (ordered by descending percentage contribution)') +
      ylab('Observation completeness (in %)') +
      ggtitle('Cumulative observation completeness') + 
      theme_bw() +
      scale_x_continuous(breaks=seq(0, nrow(storms_by_evtype_df) , by = 100))

![plot of chunk
cumulativeContributionFunction](./noaa_database_analysis_files/figure-markdown_strict/cumulativeContributionFunction.png)
<figcaption>
**Figure 1: Percentage contribution of each event type to total amount
of observations.** The x-axis represents all remaining 891 event types
(after the first pre-processing step) in the dataset expressed in
numbers and ordered by their descending percentage contribution. The
first 69 event types (vertical line) account for 99,5 % of the total
amount of obervations. Especially, the last 676 event types only
contribute a tiny fraction to the overall percentage completeness of the
observations, due to the fact that less than 6 observations belong to
each of those event types. The last 449 event types even only contribute
a single observation.
</figcaption>
</figure>


    # Remove dummy row
    storms_by_evtype_df <- storms_by_evtype_df[-1, ]

#### Step 3: Choosing the event types which contribute the most to the amount of observations

We decide to include the first 69 ordered event types which account for
99,5 % of the total amount of observations as you can see in the
`cumb_contrib_in_percent` column below.

    print(xtable(filter(storms_by_evtype_df, rank == 69), digits = 7), type = 'html', 
          include.rownames = FALSE)

<!-- html table generated in R 3.1.0 by xtable 1.7-3 package -->
<!-- Sun Jun 29 18:28:18 2014 -->
<TABLE border=1>
<TR> <TH> 
EVTYPE
</TH> <TH> 
numb\_of\_occ
</TH> <TH> 
contrib\_in\_percent
</TH> <TH> 
cumb\_contrib\_in\_percent
</TH> <TH> 
rank
</TH>  </TR>
  <TR> <TD> 
moderate snowfall
</TD> <TD align="right"> 
101.0000000
</TD> <TD align="right"> 
0.0001119
</TD> <TD align="right"> 
0.9950127
</TD> <TD align="right"> 
69.0000000
</TD> </TR>
   </TABLE>
<br>

This is equivalent to include all event types which contribute more than
100 observations to the dataset.

    storms_by_evtype_df <- filter(storms_by_evtype_df, numb_of_occ > 100)

    selected_evtypes <- storms_by_evtype_df$EVTYPE

#### Step 4: Filter the loaded raw data according to chosen event types

From the raw data we just select those observations which belong to one
of the derived event types.

    original_row_numbers <- nrow(storms_df)

    storms_df <- filter(storms_df, EVTYPE %in% selected_evtypes)

    new_row_numbers <- nrow(storms_df)

    paste0('The reduced dataset includes ', round(new_row_numbers/original_row_numbers, 
                                                  digits = 3) * 100, 
           ' % of the original observation amount, exactly as decided by our threshold choice in Step 3.')

    ## [1] "The reduced dataset includes 99.5 % of the original observation amount, exactly as decided by our threshold choice in Step 3."

#### Step 5: Merge similar event types

After that we merge similar event types with different wording to reduce
the amount of unique event type names even further.

    storms_df$EVTYPE <- gsub('extreme windchill|extreme cold$', 
                                       'extreme cold/wind chill',
                                       storms_df$EVTYPE)
    storms_df$EVTYPE[storms_df$EVTYPE == 'marine tstm wind'] <- 'marine thunderstorm wind'
    storms_df$EVTYPE <- gsub('thunderstorm winds|^tstm wind$|tstm wind/hail', 
                                       'thunderstorm wind',
                                       storms_df$EVTYPE)
    storms_df$EVTYPE <- gsub('flood/flash flood|flash flooding',
                                       'flash flood',
                                       storms_df$EVTYPE)
    storms_df$EVTYPE[storms_df$EVTYPE == 'storm surge'] <-  'storm surge/tide'
    storms_df$EVTYPE[storms_df$EVTYPE == 'hurricane'] <-  'hurricane (typhoon)'
    storms_df$EVTYPE[storms_df$EVTYPE == 'flooding'] <-  'flood'
    storms_df$EVTYPE[storms_df$EVTYPE == 'heavy surf/high surf'] <- 'high surf'                                  
    storms_df$EVTYPE[storms_df$EVTYPE == 'high winds'] <-  'high wind'
    storms_df$EVTYPE[storms_df$EVTYPE == 'winter weather/mix'] <- 'winter weather'
    storms_df$EVTYPE[storms_df$EVTYPE == 'rip currents'] <- 'rip current'
    storms_df$EVTYPE[storms_df$EVTYPE == 'strong winds'] <- 'strong wind'
    storms_df$EVTYPE[storms_df$EVTYPE == 'coastal flooding']  <- 'coastal flood'
    storms_df$EVTYPE[storms_df$EVTYPE == 'wild/forest fire']  <- 'wildfire'
    storms_df$EVTYPE[storms_df$EVTYPE == 'moderate snowfall']  <- 'moderate snow'
    storms_df$EVTYPE[storms_df$EVTYPE == 'urban/sml stream fld']  <- 'urban flood'
    storms_df$EVTYPE[storms_df$EVTYPE == 'unseasonably warm']  <- 'excessive heat'
    storms_df$EVTYPE[storms_df$EVTYPE == 'record warmth']  <- 'excessive heat'

    paste0("Total number of unique event types after last pre-processing step: ",
           length(unique(storms_df$EVTYPE)))

    ## [1] "Total number of unique event types after last pre-processing step: 49"

By applying our event type reduction strategy, we were able to reduce
the number of event types from 985 to 49. In the process we omitted 0.05
% of the observations from the original raw data. However, slight
differences remain when you compare the unique event type names in our
processed dataset with the official ones described in the storms data
documentation on page 6:

<table>
<caption><b>Table 3:</b> Differences in event type names between original documentation and processed data</caption>
<thead>
<tr class="header">
<th align="left">Additional event types</th>
<th align="left">Missing event types</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">astronomical high tide</td>
<td align="left">debris flow</td>
</tr>
<tr class="even">
<td align="left">dry microburst</td>
<td align="left">dense smoke</td>
</tr>
<tr class="odd">
<td align="left">fog</td>
<td align="left">freezing fog</td>
</tr>
<tr class="even">
<td align="left">freezing rain</td>
<td align="left">lakeshore flood</td>
</tr>
<tr class="odd">
<td align="left">landslide</td>
<td align="left">marine strong wind</td>
</tr>
<tr class="even">
<td align="left">light snow</td>
<td align="left">seiche</td>
</tr>
<tr class="odd">
<td align="left">moderate snow</td>
<td align="left">sleet</td>
</tr>
<tr class="even">
<td align="left">snow</td>
<td align="left">tropcial depression</td>
</tr>
<tr class="odd">
<td align="left">river flood</td>
<td align="left">tsunami</td>
</tr>
<tr class="even">
<td align="left">urban flood</td>
<td align="left">volcanic ash</td>
</tr>
<tr class="odd">
<td align="left">wind</td>
<td align="left"></td>
</tr>
</tbody>
</table>

### The STATE variable

The STATE variable includes the following values:

    unique(storms_df$STATE)

    ##  [1] "AL" "AZ" "AR" "CA" "CO" "CT" "DE" "DC" "FL" "GA" "HI" "ID" "IL" "IN"
    ## [15] "IA" "KS" "KY" "LA" "ME" "MD" "MA" "MI" "MN" "MS" "MO" "MT" "NE" "NV"
    ## [29] "NH" "NJ" "NM" "NY" "NC" "ND" "OH" "OK" "OR" "PA" "RI" "SC" "SD" "TN"
    ## [43] "TX" "UT" "VT" "VA" "WA" "WV" "WI" "WY" "PR" "AK" "ST" "AS" "GU" "MH"
    ## [57] "VI" "AM" "LC" "PH" "GM" "PZ" "AN" "LH" "LM" "LE" "LS" "SL" "LO" "PM"
    ## [71] "PK" "XX"

We will only take into account observations associated with one of the
50 official U.S. federal states. Every other observation will be
dropped.

    dim(storms_df)

    ## [1] 897797     37

    storms_df <- filter(storms_df, STATE %in% state.abb)

    dim(storms_df)

    ## [1] 878926     37

### Derive final version of processed dataset

Besides **EVTYPE** and **STATE** the following other variables in the
raw dataset are of interest for our latter analysis:

-   **BGN\_DATE**: Date the storm event began
-   **FATALATIES**: Number of directly killed
-   **INJURIES**: Number directly killed

We will reduce our final processed dataset to those 5 variables with a
slight transformation of the **BGN\_DATE** column to represent the year
the storm event occured instead of the date.

    storms_df <- select(storms_df, BGN_DATE, STATE, EVTYPE, FATALITIES, INJURIES) %>%
      mutate(
        year = year(mdy_hms(BGN_DATE))
        ) %>%
      select(-BGN_DATE)

    names(storms_df) <- tolower(names(storms_df))

Our final tidy dataset looks as follows:

    str(storms_df)

    ## 'data.frame':    878926 obs. of  5 variables:
    ##  $ state     : chr  "AL" "AL" "AL" "AL" ...
    ##  $ evtype    : chr  "tornado" "tornado" "tornado" "tornado" ...
    ##  $ fatalities: num  0 0 0 0 0 0 0 0 1 0 ...
    ##  $ injuries  : num  15 0 2 2 2 6 1 0 14 0 ...
    ##  $ year      : num  1950 1950 1951 1951 1951 ...

Results
-------

### Storm event types in the United States most harmful with respect to population health

The two variables giving an indication of the impact of storm events on
population health are:

-   **fatalities**: Number of directly killed
-   **injuries**: Number directly killed

We will examine both variables in detail:

    summary(storms_df$fatalities)

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0       0       0       0       0     583

    summary(storms_df$injuries)

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     0.0     0.0     0.0     0.2     0.0  1700.0

<figure>

    par(mfrow = c(1,2))
    with(storms_df, {
      hist(injuries, col='steelblue', border = 'steelblue', xlab = 'Injuries', 
           ylim = c(0, 800000), main = '(a)')
      hist(fatalities, col='steelblue', border = 'steelblue', xlab = 'Fatalities',
           main = '(b)')
      })

![plot of chunk
inj\_fat\_histograms](./noaa_database_analysis_files/figure-markdown_strict/inj_fat_histograms.png)
<figcaption>
**Figure 2: Distribution of injuries and fatalities** **(a)** Injuries
are right skewed **(b)** Fatalities are right skewed. In the majority of
the reported storm events either 0 or a tiny number of people were
injured or killed
</figcaption>
</figure>

We take a look at the total amount of persons injured or killed by event
type:

<figure>

    inj_fat_by_evtype_df <- group_by(storms_df, evtype) %>%
      summarize(
        injuries = sum(injuries),
        fatalities = sum(fatalities)
        )

    inj_fat_by_evtype_df <- arrange(inj_fat_by_evtype_df, desc(injuries))

    ggplot(inj_fat_by_evtype_df[1:10, ], aes(reorder(evtype, injuries), injuries)) + 
      geom_bar(stat='identity', fill = 'steelblue') +
      geom_text(aes(label = injuries), size = 4, hjust = -0.1) +
      xlab('Storm event types') +
      ylab('Number of people injured') +
      ggtitle('Top 10 storm event types causing injuries') +
      ylim(0, max(inj_fat_by_evtype_df$injuries * 1.05)) +
      coord_flip() +
      theme_bw()

![plot of chunk
injuries\_barplot](./noaa_database_analysis_files/figure-markdown_strict/injuries_barplot.png)
<figcaption>
<b>Figure 3: Injuries by storm event type in the United States (1950 -
2011)</b> Tornado ranks first with 91346 caused injuries followed by
thunderstorm wind with 9402 persons injured. The only other storm events
above the 5000 threshold are flood (6788), excessive heat (6225), and
lightning (5212).
</figcaption>
</figure>

<figure>

    inj_fat_by_evtype_df <- arrange(inj_fat_by_evtype_df, desc(fatalities))

    ggplot(inj_fat_by_evtype_df[1:10, ], aes(reorder(evtype, fatalities), fatalities)) + 
      geom_bar(stat='identity', fill = 'steelblue') +
      geom_text(aes(label = fatalities), size = 4 , hjust = -0.1) +
      xlab('Storm event types') +
      ylab('Number of people killed') +
      ggtitle('Top 10 storm event types causing fatalities') +
      ylim(0, max(inj_fat_by_evtype_df$fatalities * 1.05)) +
      coord_flip() +
      theme_bw() 

![plot of chunk
fatalities\_barplot](./noaa_database_analysis_files/figure-markdown_strict/fatalities_barplot.png)
<figcaption>
<b>Figure 4: Fatalities by storm event type in the United States (1950 -
2011)</b> Tornado ranks first with 5633 caused fatalities followed by
excessive heat with 1894 persons killed. The only other storm events
which have killed more than 500 persons in the last six decaces are
flash flood (975), heat (935), lightning (806) thunderstorm wind (703),
and rip current (518).
</figcaption>
</figure>


Next, we examine how the numbers of people injured / people killed did
change over time:

<figure>

    inj_fat_by_year_df <- group_by(storms_df, year) %>%
      summarize(
        Injuries = sum(injuries),
        Fatalities = sum(fatalities)
        )

    inj_fat_by_year_df <- melt(inj_fat_by_year_df, id = 'year', variable = 'Outcome')

    ggplot(inj_fat_by_year_df, aes(year, value)) +
      geom_line(colour = 'steelblue') +
      geom_smooth() +
      xlab('Number of people affected by storm events') +
      ylab('Year') +
      ggtitle('People injured/killed in the United States by storm events over time') +
      theme_bw() +
      facet_wrap(~ Outcome, scales = 'free_y') 

![plot of chunk
inj\_fat\_time\_series](./noaa_database_analysis_files/figure-markdown_strict/inj_fat_time_series.png)
<figcaption>
<b>Figure 5: </b>Both outcomes show a similar pattern highlighted by the
smoother: Injuries and fatalities both drop at the end of the 70s before
they start increasing again. In addition, the absolute numbers show a
sudden increase followed by a quick drop for both outcomes at the end of
the 90s. However, 2011 again shows a sudden rise for injuries and
fatalities.
</figcaption>
</figure>

The general pattern which the smoother highlights in the time series
plot above, can partly be explained with the progression of available
observations per year:

<figure>

    obs_by_year_df <- group_by(storms_df, year) %>%
      summarize(
        numb_of_obs = n()
        )

    ggplot(obs_by_year_df, aes(year, numb_of_obs)) +
      geom_line(colour = 'steelblue') + 
      xlab('Year') +
      ylab('Number of observations') +
      ggtitle('Number of observations in dataset per year') +
      theme_bw()

![plot of chunk
obs\_time\_series](./noaa_database_analysis_files/figure-markdown_strict/obs_time_series.png)
<figcaption>
<b>Figure 6: </b>There was a slight increase of reported observations
pear year till 1988 (7252 obs). 1989 (10410) marks the first year with
more than 10000 reported observations. A massive rise started in the mid
90s with 2011 as the new peak with 62134 observations.
</figcaption>
</figure>

In the last step, we show how injuries and fatalities are distributed
among the different U.S. federal states.

    # Create two data frames needed for plotting chloropleth maps

    states <- map_data('state')

    # (1) Group storm data by state
    # (2) Calculate sum of fatalities and injuries for each subgroup
    # (3) Map state abbreviations to full names
    storms_by_state <- group_by(storms_df, state) %>%
      summarize(
        injuries = sum(injuries),
        fatalities = sum(fatalities)
        ) %>%
      mutate (
        state = tolower(mapvalues(state, state.abb, state.name))
        )

    # Create additional data frame for plotting the federal state labels later
    state_name_pos <- as.data.frame(state.center)
    state_name_pos <- cbind(state_name_pos, state.abb)
    names(state_name_pos) <- c('long', 'lat', 'abb')
    # Exclude Alaska and Hawaii
    state_name_pos <- filter(state_name_pos, !abb %in% c('AK', 'HI'))

    ggplot(storms_by_state, aes(map_id = state, fill = injuries)) +
      geom_map(map = states, colour = 'grey', linestyle = 2) +
      geom_text(aes(x = long, y = lat, label = abb, fill = NULL, map_id = NULL), 
                state_name_pos,
               size = 2) +
      ggtitle('Distribution of injuries by storm events among U.S. federal states \n (1950 - 2011)') +
      coord_map('polyconic') +
      expand_limits(x = states$long, y = states$lat) +
      
      scale_fill_gradient(low="white", 
                          high="#756bb1", 
                          breaks=seq(0, max(storms_by_state$injuries), by = 2000)) +
      theme(
          axis.title = element_blank(),
          axis.text = element_blank(),
          panel.background = element_blank(),
          panel.grid = element_blank(),
          axis.ticks.length = unit(0, "cm"),
          axis.ticks.margin = unit(0, "cm"),
          panel.margin = unit(0, "lines"),
          plot.margin = unit(c(0, 0, 0, 0), "lines"), 
          complete = TRUE
        ) 

![plot of chunk
inj\_chloropleth](./noaa_database_analysis_files/figure-markdown_strict/inj_chloropleth.png)
<p>
<b>Figure 7</b>: In general, more people in the south east / east got
injured in storm events compared to the rest of the country. Most of the
people got injured in Texas.
</p>



    ggplot(storms_by_state, aes(map_id = state, fill = fatalities)) +
      geom_map(map = states, colour = 'grey', linestyle = 2) +
      geom_text(aes(x = long, y = lat, label = abb, fill = NULL, map_id = NULL), 
                state_name_pos,
                size = 2) +
      ggtitle('Distribution of fatalities by storm events among U.S. federal states \n (1950 - 2011)') +
      coord_map('polyconic') +
      expand_limits(x = states$long, y = states$lat) +
      
      scale_fill_gradient(low="white", 
                          high="#756bb1", 
                          breaks=seq(0, max(storms_by_state$injuries), by = 200)) +
      theme(
          axis.title = element_blank(),
          axis.text = element_blank(),
          panel.background = element_blank(),
          panel.grid = element_blank(),
          axis.ticks.length = unit(0, "cm"),
          axis.ticks.margin = unit(0, "cm"),
          panel.margin = unit(0, "lines"),
          plot.margin = unit(c(0, 0, 0, 0), "lines"), 
          complete = TRUE
        ) 

![plot of chunk
fat\_chloropleth](./noaa_database_analysis_files/figure-markdown_strict/fat_chloropleth.png)
<p>
<b>Figure 8</b>: In general, more people in the east were killed during
storm events compared to the rest of the country. Most of the people
were killed in Texas and Illinois.
</p>
