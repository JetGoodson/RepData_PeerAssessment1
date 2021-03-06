# Reproducible Research: Peer Assessment 1

```r
library(ggplot2)
library(scales)
library(plyr)
```

## Loading and preprocessing the data
1) Load the data

```r
activityDataPreClean <- read.csv(file="activity.csv",header=TRUE,stringsAsFactors=FALSE)
summary(activityDataPreClean)
```

```
##      steps           date              interval   
##  Min.   :  0.0   Length:17568       Min.   :   0  
##  1st Qu.:  0.0   Class :character   1st Qu.: 589  
##  Median :  0.0   Mode  :character   Median :1178  
##  Mean   : 37.4                      Mean   :1178  
##  3rd Qu.: 12.0                      3rd Qu.:1766  
##  Max.   :806.0                      Max.   :2355  
##  NA's   :2304
```

```r
head(activityDataPreClean)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```

```r
tail(activityDataPreClean)
```

```
##       steps       date interval
## 17563    NA 2012-11-30     2330
## 17564    NA 2012-11-30     2335
## 17565    NA 2012-11-30     2340
## 17566    NA 2012-11-30     2345
## 17567    NA 2012-11-30     2350
## 17568    NA 2012-11-30     2355
```

2) It looks like the owner of this device wasn't wearing it for the first day; perhaps it was charging. Either way the frequent NAs at the beginning and end need to be removed for the next part of the assessment. However, I will preserve the non-cleaned data in the original data frame for now.


```r
activityData <- activityDataPreClean[complete.cases(activityDataPreClean$steps),]
summary(activityData)
```

```
##      steps           date              interval   
##  Min.   :  0.0   Length:15264       Min.   :   0  
##  1st Qu.:  0.0   Class :character   1st Qu.: 589  
##  Median :  0.0   Mode  :character   Median :1178  
##  Mean   : 37.4                      Mean   :1178  
##  3rd Qu.: 12.0                      3rd Qu.:1766  
##  Max.   :806.0                      Max.   :2355
```

```r
head(activityData)
```

```
##     steps       date interval
## 289     0 2012-10-02        0
## 290     0 2012-10-02        5
## 291     0 2012-10-02       10
## 292     0 2012-10-02       15
## 293     0 2012-10-02       20
## 294     0 2012-10-02       25
```

```r
tail(activityData)
```

```
##       steps       date interval
## 17275     0 2012-11-29     2330
## 17276     0 2012-11-29     2335
## 17277     0 2012-11-29     2340
## 17278     0 2012-11-29     2345
## 17279     0 2012-11-29     2350
## 17280     0 2012-11-29     2355
```


## What is mean total number of steps taken per day?
The total number of steps taken for each day is:

```r
dailyActivity <- aggregate(activityData$steps, by=list(activityData$date),sum)
colnames(dailyActivity) <- c("date","totalSteps")
dailyActivity$date <- as.Date(dailyActivity$date)
print.data.frame(dailyActivity,quote=FALSE)
```

```
##          date totalSteps
## 1  2012-10-02        126
## 2  2012-10-03      11352
## 3  2012-10-04      12116
## 4  2012-10-05      13294
## 5  2012-10-06      15420
## 6  2012-10-07      11015
## 7  2012-10-09      12811
## 8  2012-10-10       9900
## 9  2012-10-11      10304
## 10 2012-10-12      17382
## 11 2012-10-13      12426
## 12 2012-10-14      15098
## 13 2012-10-15      10139
## 14 2012-10-16      15084
## 15 2012-10-17      13452
## 16 2012-10-18      10056
## 17 2012-10-19      11829
## 18 2012-10-20      10395
## 19 2012-10-21       8821
## 20 2012-10-22      13460
## 21 2012-10-23       8918
## 22 2012-10-24       8355
## 23 2012-10-25       2492
## 24 2012-10-26       6778
## 25 2012-10-27      10119
## 26 2012-10-28      11458
## 27 2012-10-29       5018
## 28 2012-10-30       9819
## 29 2012-10-31      15414
## 30 2012-11-02      10600
## 31 2012-11-03      10571
## 32 2012-11-05      10439
## 33 2012-11-06       8334
## 34 2012-11-07      12883
## 35 2012-11-08       3219
## 36 2012-11-11      12608
## 37 2012-11-12      10765
## 38 2012-11-13       7336
## 39 2012-11-15         41
## 40 2012-11-16       5441
## 41 2012-11-17      14339
## 42 2012-11-18      15110
## 43 2012-11-19       8841
## 44 2012-11-20       4472
## 45 2012-11-21      12787
## 46 2012-11-22      20427
## 47 2012-11-23      21194
## 48 2012-11-24      14478
## 49 2012-11-25      11834
## 50 2012-11-26      11162
## 51 2012-11-27      13646
## 52 2012-11-28      10183
## 53 2012-11-29       7047
```

A histogram of the resulting data:<center>

```r
plot <- ggplot(dailyActivity, aes(date,weight=totalSteps))
plot + geom_histogram(binwidth=1) + scale_x_date(labels=date_format("%m/%d")) + xlab("Date") + ylab("Total Number of Steps/Date") +ggtitle("Total Number of Steps Recorded for each Date")
```

![plot of chunk totalStepsHist](figure/totalStepsHist.png) 

_Figure 1: The total number of steps recorded by the fitness device for each date in the period of the dataset._</center>

The mean value for the total number of steps is:

```r
mean(dailyActivity$totalSteps)
```

```
## [1] 10766
```
The median value for the total number of steps is:

```r
median(dailyActivity$totalSteps)
```

```
## [1] 10765
```
## What is the average daily activity pattern?
The average (using the mean) number of steps taken for each day is:

```r
activityPattern <- aggregate(activityData$steps, by=list(activityData$interval),mean)
colnames(activityPattern) <- c("interval","meanSteps")
activityPattern
```

```
##     interval meanSteps
## 1          0   1.71698
## 2          5   0.33962
## 3         10   0.13208
## 4         15   0.15094
## 5         20   0.07547
## 6         25   2.09434
## 7         30   0.52830
## 8         35   0.86792
## 9         40   0.00000
## 10        45   1.47170
## 11        50   0.30189
## 12        55   0.13208
## 13       100   0.32075
## 14       105   0.67925
## 15       110   0.15094
## 16       115   0.33962
## 17       120   0.00000
## 18       125   1.11321
## 19       130   1.83019
## 20       135   0.16981
## 21       140   0.16981
## 22       145   0.37736
## 23       150   0.26415
## 24       155   0.00000
## 25       200   0.00000
## 26       205   0.00000
## 27       210   1.13208
## 28       215   0.00000
## 29       220   0.00000
## 30       225   0.13208
## 31       230   0.00000
## 32       235   0.22642
## 33       240   0.00000
## 34       245   0.00000
## 35       250   1.54717
## 36       255   0.94340
## 37       300   0.00000
## 38       305   0.00000
## 39       310   0.00000
## 40       315   0.00000
## 41       320   0.20755
## 42       325   0.62264
## 43       330   1.62264
## 44       335   0.58491
## 45       340   0.49057
## 46       345   0.07547
## 47       350   0.00000
## 48       355   0.00000
## 49       400   1.18868
## 50       405   0.94340
## 51       410   2.56604
## 52       415   0.00000
## 53       420   0.33962
## 54       425   0.35849
## 55       430   4.11321
## 56       435   0.66038
## 57       440   3.49057
## 58       445   0.83019
## 59       450   3.11321
## 60       455   1.11321
## 61       500   0.00000
## 62       505   1.56604
## 63       510   3.00000
## 64       515   2.24528
## 65       520   3.32075
## 66       525   2.96226
## 67       530   2.09434
## 68       535   6.05660
## 69       540  16.01887
## 70       545  18.33962
## 71       550  39.45283
## 72       555  44.49057
## 73       600  31.49057
## 74       605  49.26415
## 75       610  53.77358
## 76       615  63.45283
## 77       620  49.96226
## 78       625  47.07547
## 79       630  52.15094
## 80       635  39.33962
## 81       640  44.01887
## 82       645  44.16981
## 83       650  37.35849
## 84       655  49.03774
## 85       700  43.81132
## 86       705  44.37736
## 87       710  50.50943
## 88       715  54.50943
## 89       720  49.92453
## 90       725  50.98113
## 91       730  55.67925
## 92       735  44.32075
## 93       740  52.26415
## 94       745  69.54717
## 95       750  57.84906
## 96       755  56.15094
## 97       800  73.37736
## 98       805  68.20755
## 99       810 129.43396
## 100      815 157.52830
## 101      820 171.15094
## 102      825 155.39623
## 103      830 177.30189
## 104      835 206.16981
## 105      840 195.92453
## 106      845 179.56604
## 107      850 183.39623
## 108      855 167.01887
## 109      900 143.45283
## 110      905 124.03774
## 111      910 109.11321
## 112      915 108.11321
## 113      920 103.71698
## 114      925  95.96226
## 115      930  66.20755
## 116      935  45.22642
## 117      940  24.79245
## 118      945  38.75472
## 119      950  34.98113
## 120      955  21.05660
## 121     1000  40.56604
## 122     1005  26.98113
## 123     1010  42.41509
## 124     1015  52.66038
## 125     1020  38.92453
## 126     1025  50.79245
## 127     1030  44.28302
## 128     1035  37.41509
## 129     1040  34.69811
## 130     1045  28.33962
## 131     1050  25.09434
## 132     1055  31.94340
## 133     1100  31.35849
## 134     1105  29.67925
## 135     1110  21.32075
## 136     1115  25.54717
## 137     1120  28.37736
## 138     1125  26.47170
## 139     1130  33.43396
## 140     1135  49.98113
## 141     1140  42.03774
## 142     1145  44.60377
## 143     1150  46.03774
## 144     1155  59.18868
## 145     1200  63.86792
## 146     1205  87.69811
## 147     1210  94.84906
## 148     1215  92.77358
## 149     1220  63.39623
## 150     1225  50.16981
## 151     1230  54.47170
## 152     1235  32.41509
## 153     1240  26.52830
## 154     1245  37.73585
## 155     1250  45.05660
## 156     1255  67.28302
## 157     1300  42.33962
## 158     1305  39.88679
## 159     1310  43.26415
## 160     1315  40.98113
## 161     1320  46.24528
## 162     1325  56.43396
## 163     1330  42.75472
## 164     1335  25.13208
## 165     1340  39.96226
## 166     1345  53.54717
## 167     1350  47.32075
## 168     1355  60.81132
## 169     1400  55.75472
## 170     1405  51.96226
## 171     1410  43.58491
## 172     1415  48.69811
## 173     1420  35.47170
## 174     1425  37.54717
## 175     1430  41.84906
## 176     1435  27.50943
## 177     1440  17.11321
## 178     1445  26.07547
## 179     1450  43.62264
## 180     1455  43.77358
## 181     1500  30.01887
## 182     1505  36.07547
## 183     1510  35.49057
## 184     1515  38.84906
## 185     1520  45.96226
## 186     1525  47.75472
## 187     1530  48.13208
## 188     1535  65.32075
## 189     1540  82.90566
## 190     1545  98.66038
## 191     1550 102.11321
## 192     1555  83.96226
## 193     1600  62.13208
## 194     1605  64.13208
## 195     1610  74.54717
## 196     1615  63.16981
## 197     1620  56.90566
## 198     1625  59.77358
## 199     1630  43.86792
## 200     1635  38.56604
## 201     1640  44.66038
## 202     1645  45.45283
## 203     1650  46.20755
## 204     1655  43.67925
## 205     1700  46.62264
## 206     1705  56.30189
## 207     1710  50.71698
## 208     1715  61.22642
## 209     1720  72.71698
## 210     1725  78.94340
## 211     1730  68.94340
## 212     1735  59.66038
## 213     1740  75.09434
## 214     1745  56.50943
## 215     1750  34.77358
## 216     1755  37.45283
## 217     1800  40.67925
## 218     1805  58.01887
## 219     1810  74.69811
## 220     1815  85.32075
## 221     1820  59.26415
## 222     1825  67.77358
## 223     1830  77.69811
## 224     1835  74.24528
## 225     1840  85.33962
## 226     1845  99.45283
## 227     1850  86.58491
## 228     1855  85.60377
## 229     1900  84.86792
## 230     1905  77.83019
## 231     1910  58.03774
## 232     1915  53.35849
## 233     1920  36.32075
## 234     1925  20.71698
## 235     1930  27.39623
## 236     1935  40.01887
## 237     1940  30.20755
## 238     1945  25.54717
## 239     1950  45.66038
## 240     1955  33.52830
## 241     2000  19.62264
## 242     2005  19.01887
## 243     2010  19.33962
## 244     2015  33.33962
## 245     2020  26.81132
## 246     2025  21.16981
## 247     2030  27.30189
## 248     2035  21.33962
## 249     2040  19.54717
## 250     2045  21.32075
## 251     2050  32.30189
## 252     2055  20.15094
## 253     2100  15.94340
## 254     2105  17.22642
## 255     2110  23.45283
## 256     2115  19.24528
## 257     2120  12.45283
## 258     2125   8.01887
## 259     2130  14.66038
## 260     2135  16.30189
## 261     2140   8.67925
## 262     2145   7.79245
## 263     2150   8.13208
## 264     2155   2.62264
## 265     2200   1.45283
## 266     2205   3.67925
## 267     2210   4.81132
## 268     2215   8.50943
## 269     2220   7.07547
## 270     2225   8.69811
## 271     2230   9.75472
## 272     2235   2.20755
## 273     2240   0.32075
## 274     2245   0.11321
## 275     2250   1.60377
## 276     2255   4.60377
## 277     2300   3.30189
## 278     2305   2.84906
## 279     2310   0.00000
## 280     2315   0.83019
## 281     2320   0.96226
## 282     2325   1.58491
## 283     2330   2.60377
## 284     2335   4.69811
## 285     2340   3.30189
## 286     2345   0.64151
## 287     2350   0.22642
## 288     2355   1.07547
```
A time series of the daily step pattern:<center>

```r
plot <- ggplot(activityPattern, aes(x=interval,y=meanSteps))
plot + geom_line() + xlab("Interval") + ylab("Mean Number of Steps/Interval [5 Minutes]") +ggtitle("Mean Number of Steps for each Five Minute Interval") + scale_x_discrete(breaks = c(0,600,1200,1800,2400), labels=c("0:00", "6:00", "12:00", "18:00","24:00"))
```

![plot of chunk activityPatternTimeSeries](figure/activityPatternTimeSeries.png) 

_Figure 2: A time series of the mean step number per five minute interval recorded by the device over the data period._</center>


```r
index <- which.max(activityPattern$meanSteps)
maxStepsInterval <-  activityPattern$interval[index]
maxStepsInSingleInterval <- activityPattern$meanSteps[index]
```
The interval with the largest mean number of steps is:


```r
maxStepsInterval
```

```
## [1] 835
```

The mean number of steps in that interval is:


```r
maxStepsInSingleInterval
```

```
## [1] 206.2
```
This is the peak and so it appears around a quarter after eight in the morning, the wearer of this device takes a morning walk.

## Imputing missing values

The total number of missing (NA) intervals in the original dataset is:

```r
naCount <- nrow(activityDataPreClean) - nrow(activityData)
naCount
```

```
## [1] 2304
```

To fill in the missing data I will use the mean for that interval over the period.


```r
calc.mean <- function(x) replace(x, is.na(x), mean(x, na.rm=TRUE))
activityDataCorrected <- ddply(activityDataPreClean, ~interval, transform, steps = calc.mean(steps))
head(activityDataPreClean)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```

```r
head(arrange(activityDataCorrected,date,interval))
```

```
##     steps       date interval
## 1 1.71698 2012-10-01        0
## 2 0.33962 2012-10-01        5
## 3 0.13208 2012-10-01       10
## 4 0.15094 2012-10-01       15
## 5 0.07547 2012-10-01       20
## 6 2.09434 2012-10-01       25
```

Looks like that worked; the values produced match the values I calculated for the mean in the section above. Now I will plot a histogram of the result and calculate the mean and median.


A histogram of the resulting data:<center>

```r
dailyActivityCorrected <- aggregate(activityDataCorrected$steps, by=list(activityDataCorrected$date),sum)
colnames(dailyActivityCorrected) <- c("date","totalSteps")
dailyActivityCorrected$date <- as.Date(dailyActivityCorrected$date)
plotCorr <- ggplot(dailyActivityCorrected, aes(date,weight=totalSteps))
plotCorr + geom_histogram(binwidth=1) + scale_x_date(labels=date_format("%m/%d")) + xlab("Date") + ylab("Total Number of Steps/Date") +ggtitle("Total Number of Steps Recorded for each Date with NA Data Corrected")
```

![plot of chunk totalStepsHistCorrected](figure/totalStepsHistCorrected.png) 

_Figure 3: The total number of steps recorded by the fitness device for each date in the period of the dataset, with missing data corrected by inserting the mean for the corresponding five minute interval._</center>

The mean value for the total number of steps is:

```r
mean(dailyActivityCorrected$totalSteps)
```

```
## [1] 10766
```
The median value for the total number of steps is:

```r
median(dailyActivityCorrected$totalSteps)
```

```
## [1] 10766
```
The mean did not change at all and the median shift by one to a higher value. This is not surprising at all. Adding data points equal to the mean will not shift the mean. Each additional point of value &mu; changes the mean to &mu;'=(n*&mu; + &mu;)/(n+1) which cancels out to &mu;. In the case of the median, more weight was given to the mean in the population of values, so th median was dragged to that value. The median is a robust statistic though, so it would not change greatly.

The difference betwen the plots is also only slight. The NA values only appeared on the first and last days of the data period, which we can see are filled in to the men value. The zero bin near 11/15 actually represents a day on which the user recorded no steps. Perhaps the user left it at home.

## Are there differences in activity patterns between weekdays and weekends?

First I will create a new factor column for the data. The simplest way to do that appears to be to just check if the first letter is an S for Saturday or Sunday.


```r
activityDataWeekend <- weekdays(as.Date(activityDataCorrected$date))
workOrPlay <- function(x){
x <- ifelse(substr(x,1,1)=="S", "weekend", "weekday")
  }
dayType<-workOrPlay(activityDataWeekend)
```

Now I will look at the head to compare my day names from the date column to my weekday/weekend classification.


```r
head(activityDataWeekend)
```

```
## [1] "Monday"    "Tuesday"   "Wednesday" "Thursday"  "Friday"    "Saturday"
```

```r
head(dayType)
```

```
## [1] "weekday" "weekday" "weekday" "weekday" "weekday" "weekend"
```

```r
activityDataWeekdayWeekend <- cbind(activityDataCorrected, dayType)

head(activityDataWeekdayWeekend)
```

```
##    steps       date interval dayType
## 1  1.717 2012-10-01        0 weekday
## 2  0.000 2012-10-02        0 weekday
## 3  0.000 2012-10-03        0 weekday
## 4 47.000 2012-10-04        0 weekday
## 5  0.000 2012-10-05        0 weekday
## 6  0.000 2012-10-06        0 weekend
```

Looks pretty good. I have now marked the data as one of two factors, marked as either a weekday or a weekend. I will now split the data into two datasets and plot it.


```r
activityDataWeekday <- subset(activityDataWeekdayWeekend,activityDataWeekdayWeekend$dayType=="weekday")
activityPatternWeekday <- aggregate(activityDataWeekday$steps, by=list(activityDataWeekday$interval),mean)
colnames(activityPatternWeekday) <- c("interval","meanSteps")
head(activityPatternWeekday)
```

```
##   interval meanSteps
## 1        0   2.25115
## 2        5   0.44528
## 3       10   0.17317
## 4       15   0.19790
## 5       20   0.09895
## 6       25   1.59036
```

```r
activityDataWeekend <- subset(activityDataWeekdayWeekend,activityDataWeekdayWeekend$dayType=="weekend")
activityPatternWeekend <- aggregate(activityDataWeekend$steps, by=list(activityDataWeekend$interval),mean)
colnames(activityPatternWeekend) <- c("interval","meanSteps")
head(activityPatternWeekend)
```

```
##   interval meanSteps
## 1        0  0.214623
## 2        5  0.042453
## 3       10  0.016509
## 4       15  0.018868
## 5       20  0.009434
## 6       25  3.511792
```

Graphed below is a time series of the device wearer's activity pattern for weekend days and weekday.<center>


```r
plotWeekend <- ggplot(activityPatternWeekend, aes(x=interval,y=meanSteps))
plotWeekday <- ggplot(activityPatternWeekday, aes(x=interval,y=meanSteps))

plotWeekend + geom_line() + xlab("Interval") + ylab("Mean Number of Steps/Interval [5 Minutes]") +ggtitle("Mean Number of Steps for each Five Minute Interval over Weekend") + scale_x_discrete(breaks = c(0,600,1200,1800,2400), labels=c("0:00", "6:00", "12:00", "18:00","24:00"))  
```

![plot of chunk panelPlot](figure/panelPlot1.png) 

```r
plotWeekday + geom_line() + xlab("Interval") + ylab("Mean Number of Steps/Interval [5 Minutes]") +ggtitle("Mean Number of Steps for each Five Minute Interval over Weekday") + scale_x_discrete(breaks = c(0,600,1200,1800,2400), labels=c("0:00", "6:00", "12:00", "18:00","24:00"))
```

![plot of chunk panelPlot](figure/panelPlot2.png) 

_Figure 4: A time series of the mean step number per five minute interval recorded by the device over the data period on weekend days (top) and weekdays (bottom)._</center>

There is an obvious difference in activity level on weekend afternoons and evenings. One can imagine that this is due to the device wearer sitting at a desk for much of the weekday (after a morning walk), but then pursuing activities on the weekend.

