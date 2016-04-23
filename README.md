require(XML)
require(RCurl)
require(plyr)

require(gtools)

# DATA FROM FOOTBALL DATA WEBSITD SPANISH LEAGUE 2014-15 
fileURL <- "http://www.football-data.co.uk/mmz4281/1516/SP1.csv?accessType=DOWNLOAD"
download.file(fileURL, destfile = "./fbData.csv", method = "curl")

# DATA FROM FOOTBALL DATA WEBSITD SPANISH LEAGUE 2015-6
fileURLTwo <- "http://www.football-data.co.uk/mmz4281/1415/SP1.csv?accessType=DOWNLOAD"
download.file(fileURLTwo, destfile = "./fbDataTwo.csv", method = "curl")

# KEEPING THE DATE IN OUR FOLDER
dateDownloaded <- date()
dateDownloaded

# OPEN THE DATA AFTER BEEN SAVE AS CSV FILE ON MY FOLDER
footballData <- read.csv("./fbData.csv", sep = ",", header=TRUE)
footballDataTwo <- read.csv("./fbDataTwo.csv", sep = ",", header=TRUE)

# SUBSET DROPING THE UNNESSECERY COL WHICH WE DONT NEED FOR FUTURE CALCS
footballData <- subset(footballData, select = -c(B365H : BbAvAHA))
footballDataTwo <- subset(footballDataTwo, select = -c(B365H : BbAvAHA))

# DISPLAYING FOOTBALL STATISTICS
head(footballData)
head(footballDataTwo)

# BIND THE TWO FILES TOGTHER
total <- rbind(footballData,footballDataTwo)
head(total)

# GETTING ONLY TEAMS NAMES 
teamName <- subset(footballData,select=c(HomeTeam) )
head(teamName)
# UNIQUE FUNCTION ORDER THE TEAM NAMES COL
teamNameUni <- unique(teamName)
head(teamNameUni)

install.packages("eeptools")
library(eeptools)
library(gtools)

# FUNCTION TO SUBSET ALL TEAMS BY THEY GOALS AND HOLD THEM IN ONE DATA FRAME
my_data <- list()
for(i in 1:20){
tmp <- toString(teamNameUni$HomeTeam[i])
listHome <- subset(total,total$HomeTeam == tmp ,select=c(HTHG,FTHG))
listAway <- subset(total,total$AwayTeam == tmp ,select=c(HTAG,FTAG))
my_data[[i]] <- smartbind(listHome,listAway)
}
# SHOW ALL THE DATA
listGames <- subset(total,select=c(HomeTeam,AwayTeam,HTHG,FTHG,HTAG,FTAG))
listH <- listGames$HTHG - listGames$HTAG
listF <- listGames$FTHG - listGames$FTAG
listGames <- cbind(listGames,listH,listF)
colH <- ifelse(listGames$listH<0, 1, ifelse(listGames$listH>0, 2, 0))
colF <- ifelse(listGames$listF<0, 1, ifelse(listGames$listF>0, 2, 0))
listGames <- cbind(listGames,colH,colF)

# SHOW THE DENISTY OF HALF TIME SCORE AND FULL TIME SCORE
par(mfrow=c(2,1),mar=c(4,4,2,1))
d <- density(listGames$colH)
plot(d,main = "Half Time score ")
d <- density(listGames$colF)
plot(d,main = "Full Time score ")



# Simple Dotplot
dotchart(listGames$listF,listGames$listH,labels = teamNameUni$HomeTeam ,cex=.9, main="Gas Milage for Car Models", xlab="Miles Per Gallon")

library('sm')


plot(listGames$colH, listGames$colF, main="Example", xlab="W/L", ylab="L/S")

boxplot(colH~colF,data=listGames,col="red")

boxplot(HTHG~FTHG,data=listHome,col="red")
boxplot(HTAG~FTAG,data=listAway,col="green")

abline(v=median(newDataHome$FTHG),lwd=4,col="yellow")

par(mfrow=c(2,1),mar=c(4,4,2,1))
hist(newDataHome$FTHG,col="green")
abline(v=median(newDataHome$FTHG),lwd=4,col="yellow")
hist(newDataHome$HTHG,col="green")
abline(v=median(newDataHome$HTHG),lwd=4,col="yellow")
