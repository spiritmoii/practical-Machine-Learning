---
title: "Assigment 1"
author: "Robot_124"
date: "24 septembre 2015"
output: html_document
---

Package used:

```{}
library(caret)
library(randomForest)
```

Extraction of the data:
I had a problem with missing values, so I decided to set them to 0 :/.

```{}
pml.testing <- read.csv("~/coursera/ml/pratical/pml-testing.csv", header=TRUE)
pml.training <- read.csv("~/coursera/ml/pratical/pml-training.csv", header=TRUE)
pml.testing[pml.testing == ""] <- NA
pml.training[pml.training == ""] <- NA

classe = pml.training$classe
set.seed(7665)
training=subset(pml.training,select =-c(classe))
training[]<-lapply(training,as.numeric)
adData = data.frame(classe,training)}
```

Data slicing:
I slice it 70%(train)/30(test)
```{}
inTestTrain = createDataPartition(y=adData$classe, p = 6/10, list=FALSE)
training = adData[ inTestTrain,]
testing = adData[-inTestTrain,]
training[is.na(training) ]  <- 0
testing[is.na(testing) ]  <- 0
```
I train a random forest model and use pca in preprocess to speed up the process and hopefully reduce the incidence of the 0, I put on the data.
```{}
modelFit <- train(training$classe~.,data=training,  method = "rf", preProcess="pca",trControl = trainControl(method = "cv"))
```
I tested it with my remaining data:
This test show an out of sample accuracy > 95% which is enough for me.
```{}
pred = predict(modelFit,newdata=testing)
cm = confusionMatrix(pred, testing$classe)
cm
plot(cm[[2]], main="Confusion Matrix")
```

```{r whar,echo=FALSE}

load(file="cm")
cm
plot(cm[[2]], main="Confusion Matrix")
```



