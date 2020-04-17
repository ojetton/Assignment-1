Showing off some ggplot2 skills
================
Benn Jesser and Owen Jetton
</br>11 4 2020

## Goal and requirements

The goal of this assignment is simple: I want you to produce *four*
different figures using **ggplot2**. They don’t have to be identify
causal relationships, or anything like that. I just want you to stretch
your visualization legs and demonstrate any new (or existing)
**ggplot2** skills that you have acquired since our first lecture.

Some additional points:

  - You are free to use any dataset that comes in-built with base R, or
    bundled together with an external R package. See
    [here](https://vincentarelbundock.github.io/Rdatasets/datasets.html)
    for an impressive list.

  - That being said, I would especially encourage you to use your own
    data.
    
      - I know we haven’t gotten to data importation yet, but take a
        look
        [here](https://support.rstudio.com/hc/en-us/articles/218611977-Importing-Data-with-RStudio)
        if you need help. I would recommend that you install the
        **readr**, **read\_excel** and **haven** packages first, though.
      - If your dataset isn’t proprietary (or isn’t being read directly
        off the web), please save it in the (empty) `data/` folder of
        this repo.

  - You can use the same dataset for all four of your plots. Or you can
    use a new dataset for each plot. Regardless of what you choose, I
    want you to try and use different geoms for each figure.

  - Any other **ggplot2** skills and add-ons like faceting, changing
    aesthetic scales or legends, using different themes (e.g. from the
    **ggthemes** package), animation, etc. are all welcome and
    encouraged.

  - I want to *see* the code that produces the figures. (Don’t use
    `echo=FALSE` in any of the code chunks, if that means anything to
    you.)

### What you will be graded on

  - Are your figures clear? (e.g. lack of chart chunk, non-overlapping
    labels)
  - Are your figures compelling? (e.g. use an appropriate geom for the
    insight that you want to convey)
  - Variation (I don’t want to see four line charts of the same dataset…
    Be creative)
  - Did you read and follow my instructions (e.g. describe your data and
    figures, show the code that produced the figures, include data in
    the `/data` folder, etc)
  - etc.

Lastly, don’t forget to knit the assignment (click the “Knit” button, or
press `Ctrl+Shift+K`) before submitting\!

## Start the assignment

Here is a R code chunk for you to load your packages. Feel free to load
as many other packages and insert as many additional code chunks
(`Ctr+Alt+I`) as you need. You can always load additional packages in
the their own chunk blocks, but I recommend loading all of your packages
together at the top.)

``` r
if (!require("pacman")) install.packages("pacman")
```

    ## Loading required package: pacman

``` r
## Note: The `p_load()` function from the pacman package is a convenient way to 
## install (if necessary) and load packages all at once. You can think of this
## as an alternative to the normal `library(ggplot2); library(here);...` way of
## loading R packages.
pacman::p_load(ggplot2, here)
```

### Figure 1: Water Use in Minnesota

Load/read in the data. (Delete this chunk if you don’t need it.)

``` r
library(alr4)
```

    ## Loading required package: car

    ## Loading required package: carData

    ## Loading required package: effects

    ## Registered S3 methods overwritten by 'lme4':
    ##   method                          from
    ##   cooks.distance.influence.merMod car 
    ##   influence.merMod                car 
    ##   dfbeta.influence.merMod         car 
    ##   dfbetas.influence.merMod        car

    ## lattice theme set by effectsTheme()
    ## See ?effectsTheme for details.

``` r
cool_data <- alr4::MinnWater
```

This is a data set from my Math 463 class that displays water
consumption in Minnesota, from 1988 to 2011. I graphed water used for
agriculture and water used in municiple areas (not sure of the units :/
)

``` r
ggplot(cool_data)+geom_point(aes(x=year, y=muniUse,col= 'Municiple Use'))+geom_point(aes(x=year, y=irrUse,col='Irrigation Use'))+geom_smooth(aes(x=year, y=muniUse),method = 'lm')+geom_smooth(aes(x=year, y=irrUse),method = 'lm')+labs(x='Year',y='Water Usage',title = 'Minnesota Water Consumption')
```

    ## `geom_smooth()` using formula 'y ~ x'
    ## `geom_smooth()` using formula 'y ~ x'

![](ggplot2_files/figure-gfm/fig1-1.png)<!-- --> This graph shows the
how much water is used for irrigation and municiple use in Minnesota. I
threw in a linear best fit line with standard error. Its interesting to
see how much more variance there is in the irrigation because of
weather, but it can also be seen in municiple use (watering lawns most
likely). Intersting topic with increased droughts and water crises
thanks to global warming, that often put farmers against municiple water
use.

### Figure 2: S\&P 500

``` r
setwd("~/assignment-01-ggplot2-team-09/data")
cooler_data<-read.csv(file = '^GSPC.csv')
```

``` r
library(scales)
ggplot(cooler_data)+geom_line(aes(x=Volume,y=Close,col='Close'))+geom_smooth(aes(x=Volume,y=Close,col='lm'),method ='lm',se=FALSE)+geom_smooth(aes(x=Volume,y=Close,col='gam'),method = 'gam',se=FALSE)+geom_smooth(aes(x=Volume,y=Close,col='loess'),method = 'loess',se=FALSE)+labs(y='S&P 500 Closing Price',title = 'S&P 500 in the Last 6 months')
```

    ## `geom_smooth()` using formula 'y ~ x'

    ## `geom_smooth()` using formula 'y ~ s(x, bs = "cs")'

    ## `geom_smooth()` using formula 'y ~ x'

![](ggplot2_files/figure-gfm/fig2-1.png)<!-- --> This data is from Yahoo
Finance of the S\&P 500 Index from the last 6 months. I decided to look
at the closing price and trade volume. I graphed the data and then put
three regression lines on it. Its interesting to see how badly the
models fit the data at very high trading volumes. This makes sense since
high trade volume is synonymous with a volatile market. The effects of
COVID-19 are apparent in the fact that high trade volume has shown to be
correlated with a lower closing price (contraction/recession). I tried
for quite sometime to be able to use the time series data, but could not
figure out the scale\_x so the dates were obnoxious along the x axis.

### Figure 3: Foreign Currency Exchange Rate to U.S. Dollar

``` r
library(tidyquant, tidyverse)
```

    ## Loading required package: lubridate

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:here':
    ## 
    ##     here

    ## The following object is masked from 'package:base':
    ## 
    ##     date

    ## Loading required package: PerformanceAnalytics

    ## Loading required package: xts

    ## Loading required package: zoo

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    ## 
    ## Attaching package: 'PerformanceAnalytics'

    ## The following object is masked from 'package:graphics':
    ## 
    ##     legend

    ## Loading required package: quantmod

    ## Loading required package: TTR

    ## Registered S3 method overwritten by 'quantmod':
    ##   method            from
    ##   as.zoo.data.frame zoo

    ## Version 0.4-0 included new data defaults. See ?getSymbols.

    ## == Need to Learn tidyquant? ==================================================
    ## Business Science offers a 1-hour course - Learning Lab #9: Performance Analysis & Portfolio Optimization with tidyquant!
    ## </> Learn more at: https://university.business-science.io/p/learning-labs-pro </>

``` r
Jan = tq_get("DEXJPUS", get="economic.data", from="2000-01-01")
Chi = tq_get("DEXCHUS", get="economic.data", from="2000-01-01")
Can = tq_get("DEXCAUS", get="economic.data", from="2000-01-01")
Sko = tq_get("DEXKOUS", get="economic.data", from="2000-01-01")
Mex = tq_get("DEXMXUS", get="economic.data", from="2000-01-01")
```

The data I have chosen for this comes from Federal Reserve Economic
Data, and the tq\_get() command allows me to grab data sets straight
from the website. The datasets I have collected are, in order, the
exchange rates of one Japanese Yen, one Chinese Yuan, one Canadian
Dollar, one South Korean Won, and one Mexican New Peso to one U.S.
dollar. All of the data is daily, and I selected the data after January
1st, 2001.

``` r
ggplot() + geom_line(data=Jan, aes(x=date, y=price, color="Black")) +
  geom_line(data=Chi, aes(x=date, y=price, color="Red")) +
  geom_line(data=Can, aes(x=date, y=price, color="Green")) +
  geom_line(data=Sko, aes(x=date, y=price, color= "Blue")) +
  geom_line(data=Mex, aes(x=date, y=price, color = "Orange")) +
  scale_y_continuous(trans='log10') + 
  labs(x = "Date", y = "Foreign Currency Unit to One U.S. Dollar", 
       title = "Foreign Exchange Rates to U.S. Dollar") +
  scale_color_identity(name = "Foreign Currencies", guide = "legend",
      labels = c("Japanese Yen", "South Korean Won", "Canadian Dollar",
         "Mexican New Peso", "Chinese Yuan")) + theme_bw()
```

![](ggplot2_files/figure-gfm/fig3-1.png)<!-- --> This graph plots the
different exchange rates of five foreign currencies to one U.S. dollar
from January 1st, 2000, to April 10th, 2020. What this shows is how the
relationship these currencies have to the U.S. Dollar has changed over
the last 20 years. Most have stayed steady, with few shocks to the
exchange rate. We can see a similar shock to the Canadian Dollar,
Mexican New Peso, and South Korean Yen around 2008, which I attribute to
the U.S. financial crisis. It is interesting how the Japanese Yen and
Chinese Yuan appear to have embraced no shock during that period.

### Figure 4: The Change in the Phillips Curve over Time

``` r
#Collecting data
library(gganimate)
library(png)
library(gifski)
library(roll)

UNR = tq_get("UNRATE", get="economic.data", from="1955-01-01")
CPI = tq_get("CPIAUCSL", get="economic.data", from="1955-01-01")
CPI$LPrice = log(CPI$price)
INFts = ts(diff(CPI$LPrice, lag=12, differences=1), start=c(1955,1), frequency=12)*100
dates = seq(as.Date('1955-01-01'), as.Date('2019-03-01'), by = 'months')
INF = data.frame(date= dates, infr= INFts)

Rdata = merge(UNR, INF, by="date")

regs = roll_lm(
  Rdata$price,
  Rdata$infr,
  width = 120,
  weights = rep(1, 120),
  intercept = TRUE,
  min_obs = 120,
  complete_obs = TRUE,
  na_restore = FALSE,
  online = TRUE
)

lines = data.frame(date = dates, regs["coefficients"])
Pdata = merge(Rdata, lines, by="date")

#plotting the graph
ggplot(Pdata) +
  geom_point(aes(x=price, y=infr)) +
  labs(title = 'Phillips Curve Over Time',
       subtitle = 'Using Rolling Regressions Over 10 Year Periods',
       caption = 'Date: {frame_time}',
       x = 'Unemployment Rate (%)', y = 'Inflation Rate (%)') +  
  transition_time(Pdata$date) +
  shadow_mark(past = TRUE, exclude_layer = 2) +
  geom_abline(intercept = Pdata$coefficients..Intercept.,
              slope = Pdata$coefficients.x1, color = 'blue')
```

    ## Warning: Removed 4 rows containing missing values (geom_abline).
    
    ## Warning: Removed 4 rows containing missing values (geom_abline).

    ## Warning: Removed 8 rows containing missing values (geom_abline).
    
    ## Warning: Removed 8 rows containing missing values (geom_abline).
    
    ## Warning: Removed 8 rows containing missing values (geom_abline).
    
    ## Warning: Removed 8 rows containing missing values (geom_abline).

    ## Warning: Removed 7 rows containing missing values (geom_abline).

    ## Warning: Removed 8 rows containing missing values (geom_abline).
    
    ## Warning: Removed 8 rows containing missing values (geom_abline).
    
    ## Warning: Removed 8 rows containing missing values (geom_abline).

    ## Warning: Removed 7 rows containing missing values (geom_abline).

    ## Warning: Removed 8 rows containing missing values (geom_abline).
    
    ## Warning: Removed 8 rows containing missing values (geom_abline).
    
    ## Warning: Removed 8 rows containing missing values (geom_abline).
    
    ## Warning: Removed 8 rows containing missing values (geom_abline).

    ## Warning: Removed 7 rows containing missing values (geom_abline).

    ## Warning: Removed 6 rows containing missing values (geom_abline).

![](ggplot2_files/figure-gfm/fig4-1.gif)<!-- --> This graph considers
the macroeconomic concept of the Phillips curve, and tracks the actual
data over time using rolling regressions. The Phillips Curve theorizes
an inverse relationship between unemployment and inflation. Using
unemployment and price index data (transformed into inflation) from
FRED, this graph shows how that relationship has changed over the last
60 years. We can see that an inverse relationship was present from the
1960s until around 2000, when the curve flattened. Since then, as the
graph shows, the curve has remained relatively flat. This indicates that
inflation in the U.S.is not as responsive to unemployment as it was in
the last half of the 20th century.
