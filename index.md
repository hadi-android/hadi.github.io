---
title: "Zillow Property Estimate Exploratory Data Analysis and Prediction"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## R Markdown

load the library needed for some basic data processing

```{r cars}
library(caret)
library(data.table)
```

load the training target (logerror)
```{r}
tr_lbl = read.csv("D:\\Kaggle\\Zestimate\\train_2016_v2.csv")
tr_lbl = tr_lbl[order(tr_lbl$parcelid),]
```

load the training data set

```{r}
tr_data = fread("D:\\Kaggle\\Zestimate\\properties_2016.csv")
tr_data_2017 = fread("D:\\Kaggle\\Zestimate\\properties_2017.csv")
```
merge the training data with the corresponding labels using the parcelid as the key
```{r}
mergedData = merge(tr_lbl, tr_data, by="parcelid")
attach(mergedData)
```

plot logerror against year built
```{r}
data = data.frame(cbind(yearbuilt, logerror))
data_agg = aggregate(data, by=list(data$yearbuilt), FUN=mean)
data_agg = data_agg[,-1]
plot(data_agg$yearbuilt, data_agg$logerror, type = "l")
```
the logerror tends to be higher for homes built before the 1900s
