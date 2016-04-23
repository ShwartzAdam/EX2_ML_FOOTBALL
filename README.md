---
title: "Football_spLEAGUE"
author: "Adam Daniel"
date: "April 23, 2016"
output: html_document
---


```{r cars}
require(XML)
require(RCurl)
require(plyr)
require(gtools)
```

## DATA FROM FOOTBALL DATA WEBSITD SPANISH LEAGUE 2014-15 


```{r pressure, echo=FALSE}
fileURL <- "http://www.football-data.co.uk/mmz4281/1516/SP1.csv?accessType=DOWNLOAD"
download.file(fileURL, destfile = "./fbData.csv", method = "curl")
```

