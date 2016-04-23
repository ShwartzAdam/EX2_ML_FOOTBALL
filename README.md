---
title: "Football_spLEAGUE"
author: "Adam Daniel"
date: "April 23, 2016"
output: html_document
---


### Reuired Libaries

```{r cars}
require(XML)
require(RCurl)
require(plyr)
require(gtools)
```

### Data from football statistics website of spanish league 2014-15 & 2015-16


```{r pressure, echo=FALSE}
fileURL <- "http://www.football-data.co.uk/mmz4281/1516/SP1.csv?accessType=DOWNLOAD"
download.file(fileURL, destfile = "./fbData.csv", method = "curl")
fileURLTwo <- "http://www.football-data.co.uk/mmz4281/1415/SP1.csv?accessType=DOWNLOAD"
download.file(fileURLTwo, destfile = "./fbDataTwo.csv", method = "curl")
```
