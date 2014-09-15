Parkinson-Classification
========================
## Script 

##### download data from UCI website: http://archive.ics.uci.edu/ml/

download.file(url="http://archive.ics.uci.edu/ml/machine-learning-databases/parkinsons/parkinsons.data", destfile="parkinson.data")
data <- read.table('parkinsons.data',header=T,sep=',',na.strings=c('NA',''))

##### exploratory data analysis

summary(data)

##### looking at data, we notice columns have different ranges. To avoid that different unit of measurement could affect wrongy the data during Knn euclidean distance algorithm, we prefer to standardize the data.

data$Status<-factor(data$status, labels = c("negative", "positive"))
columns<-setdiff(colnames(data),c("name","status"))
datastd<-apply(data[,columns],2,function(x) {scale(x,center=T,scale=T)})
datastd<-as.data.frame(apply(data[,columns],2,function(x) {scale(x,center=T,scale=T)}))

##### We define 80% of data belonging to Train and 20% of the remaining data to Test.

rgroup<-runif(dim(datastd)[[1]])
Train<-datastd[rgroup>0.2,]
Test<-datastd[rgroup<=0.2,]
TrainLabels<-data[rgroup>0.2,"Status"]
TestLabels<-data[rgroup<=0.2,"Status"]

##### K-Nearest Neighbour is called on Training and Test Data using Training Labels to predict Test classification.
##### Accuracy, Sensitivity, Specificity are indexes used to evaluate the algorithm performance.

library(class)
KNN<-knn(Train,Test,TrainLabels,k=13)
KNN<-knn(Train,Test,TrainLabels,k=7)
confusionMatrix(KNN,TestLabels)

##### AUC or Area Under the Curve is used to evaluate algorithm performance. 

library(ROCR)
KNNProb<-knn(Train,Test,TrainLabels,k=7,prob=T)
KNNDecision<-ifelse(KNNProb=="positive",attributes(KNNProb)$prob,1-(attributes(KNNProb)$prob))
perf<-performance(prediction(KNNDecision,TestLabels=="positive"),"auc")
auc<-perf@y.values
auc

##### Accuracy=95%, AUC=99%.



