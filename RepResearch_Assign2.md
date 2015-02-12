# RepResearch
Kaushank  
Thursday, February 12, 2015  
Reproducible Research: Peer Assessment 2

<b>1. Assignment</b>

This assignment explores the  NOAA Storm Database and attempts to answer some basic questions about severe weather events. 


<b>2. Synopsis></b>
NOAA maintains a public database for storm events depicting details like location,  estimates for damage to property,damage to life and property etc..
In this report I am attempting to  investigate which events have greater impact 
:Financial,Fatalities,Injuries, Damage to Property and Damage to Crops.

Conclusion:
From the analysis, most  physical phenomena result in injuries to people, which sometimes are fatal. By far, Tornadoes are the most dangerous events, caused ~100000 injuries on the last 60 years.

When analysing the event types by the impact on the economy, we observe that floods caused $15 billions damages on the last 60 years, mostly on properties.

Tornado's are by far the highest cause for injuries, and second in fatalities, whilst heat &amp; drought cause the most fatalities, but fourth in injuries. Both are in the top 5 of injuries &amp; fatalities next to Thunderstorms. Flooding  and Snow &amp; Ice . In economic damages, only the property damage really factors in the total damage, except for Heat &amp; Drought where more than 90% of damages is determined by crop damage. The #1 &amp; #2 of weather damage sources, resp. Flooding &amp; High Surf and Wind &amp; Storm cover more than 80% of all economic cost, while Wind &amp; Storm aren't even in the top 5 of victims.</p>


<b>3.Data Processing</b>

<b>3.1 Set the appropriate Working Directory </b>
Change the working Directory to Project Directory for Assignment 2

```r
setwd("C:/Users/Dell1/Desktop/DSSGProjects/RR/RepData_PeerAssessment2")
```

<b>3.2 Load the necessary Librarries</b>


```
## Warning: package 'R.utils' was built under R version 3.1.2
```

```
## Loading required package: R.oo
```

```
## Warning: package 'R.oo' was built under R version 3.1.2
```

```
## Loading required package: R.methodsS3
```

```
## Warning: package 'R.methodsS3' was built under R version 3.1.2
```

```
## R.methodsS3 v1.6.1 (2014-01-04) successfully loaded. See ?R.methodsS3 for help.
## R.oo v1.18.0 (2014-02-22) successfully loaded. See ?R.oo for help.
## 
## Attaching package: 'R.oo'
## 
## The following objects are masked from 'package:methods':
## 
##     getClasses, getMethods
## 
## The following objects are masked from 'package:base':
## 
##     attach, detach, gc, load, save
## 
## R.utils v1.34.0 (2014-10-07) successfully loaded. See ?R.utils for help.
## 
## Attaching package: 'R.utils'
## 
## The following object is masked from 'package:utils':
## 
##     timestamp
## 
## The following objects are masked from 'package:base':
## 
##     cat, commandArgs, getOption, inherits, isOpen, parse, warnings
```

```
## Warning: package 'plyr' was built under R version 3.1.2
```

```
## Warning: package 'reshape2' was built under R version 3.1.2
```

```
## Warning: package 'ggplot2' was built under R version 3.1.2
```

```
## Warning: package 'scales' was built under R version 3.1.2
```

```
## Warning: package 'Hmisc' was built under R version 3.1.2
```

```
## Loading required package: grid
## Loading required package: lattice
## Loading required package: survival
## Loading required package: splines
## Loading required package: Formula
```

```
## Warning: package 'Formula' was built under R version 3.1.2
```

```
## 
## Attaching package: 'Hmisc'
## 
## The following objects are masked from 'package:plyr':
## 
##     is.discrete, summarize
## 
## The following object is masked from 'package:R.utils':
## 
##     capitalize
## 
## The following objects are masked from 'package:base':
## 
##     format.pval, round.POSIXt, trunc.POSIXt, units
```

```
## Warning: package 'knitr' was built under R version 3.1.2
```

```
## Warning: package 'markdown' was built under R version 3.1.2
```

```
## Warning: package 'car' was built under R version 3.1.2
```

<b>3.2. Load source file and extract it</b>
Unzip the data


```r
stormdata <- read.csv("./StormData.csv")
damages<-aggregate(cbind(FATALITIES, INJURIES) ~ EVTYPE , stormdata, sum)
dam<-melt(head(damages[order(-damages$FATALITIES,-damages$INJURIES),],10))
```

```
## Using EVTYPE as id variables
```

```r
stormdata$PROPDMG<-stormdata$PROPDMG*as.numeric(Recode(stormdata$PROPDMGEXP, "'0'=1;'1'=10;'2'=100;'3'=1000;'4'=10000;'5'=100000;'6'=1000000;'7'=10000000;'8'=100000000;'B'=1000000000;'h'=100;'H'=100;'K'=1000;'m'=1000000;'M'=1000000;'-'=0;'?'=0;'+'=0",as.factor.result=FALSE))
stormdata$CROPDMG<-stormdata$CROPDMG*as.numeric(Recode(stormdata$CROPDMGEXP, "'0'=1;'2'=100;'B'=1000000000;'k'=1000;'K'=1000;'m'=1000000;'M'=1000000;''=0;'?'=0",as.factor.result=FALSE))

economic<-aggregate(cbind(PROPDMG, CROPDMG) ~ EVTYPE , stormdata, sum)
eco<-melt(head(economic[order(-economic$PROPDMG,-economic$CROPDMG),],10))
```

```
## Using EVTYPE as id variables
```

```r
ggplot(dam, aes(x=EVTYPE,y=value,fill=variable)) + geom_bar(stat = "identity") + coord_flip() +
  ggtitle("Harmful events") + labs(x = "", y="number of people impacted") +
  scale_fill_manual (values=c("blue","red"), labels=c("Deaths","Injuries"))
```

![](./RepResearch_Assgn2_files/figure-html/unnamed-chunk-3-1.png) 

```r
ggplot(eco, aes(x=EVTYPE,y=value,fill=variable)) + geom_bar(stat = "identity") + coord_flip() +
  ggtitle("Economic consequences") + labs(x = "", y="cost of damages in dollars") +
  scale_fill_manual (values=c("yellow","green"), labels=c("Property Damage","Crop Damage"))
```

![](./RepResearch_Assgn2_files/figure-html/unnamed-chunk-3-2.png) 
