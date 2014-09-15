Parkinson-Classification
========================
## Script 

##### download data from UCI website: http://archive.ics.uci.edu/ml/

download.file(url="http://archive.ics.uci.edu/ml/machine-learning-databases/parkinsons/parkinsons.data", destfile="parkinson.data")
data <- read.table('parkinsons.data',header=T,sep=',',na.strings=c('NA',''))

##### exploratory data analysis
summary(data)
