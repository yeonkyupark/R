분류분석 - KNN
================

# Prerequisite

``` r
rm(list=ls())
# getwd()
# setwd("./R") # if necessary

viewSamples <- function(x,n=5) {
  x[sort(sample(1:nrow(x), n)),]
}
```

# KNN (K-Nearest Neighbor)

`KNN`은 새로운 데이터의 클래스를 해당 데이터와 가장 가까운 K개 데이터들의 클래스(범주)로 결정한다.

``` r
titanic <- read.csv("titanic.csv")

titanic$Age <- ifelse(is.na(titanic$Age), mean(titanic$Age, na.rm = T), titanic$Age)

titanic$Survived <- as.factor(titanic$Survived)
titanic$Sex <- as.factor(titanic$Sex)
titanic <- titanic[, -c(1,4,9,11,12)]

idx <- sample(1:nrow(titanic), nrow(titanic)*.7, replace = F)
titanic.train <- titanic[idx,]
titanic.test <- titanic[-idx,]
titanic.train.k <- titanic[idx,-c(1,3)]
titanic.test.k <- titanic[-idx,-c(1,3)]
```

``` r
library(class)

# 모델링
class <- titanic.train[,1]
titanic.knn3 <- knn(titanic.train.k, titanic.test.k, class, k=3)
titanic.knn7 <- knn(titanic.train.k, titanic.test.k, class, k=7)
titanic.knn0 <- knn(titanic.train.k, titanic.test.k, class, k=10)
```

``` r
t.3 <- table(titanic.knn3, titanic.test$Survived)
t.3
```

    ##             
    ## titanic.knn3   0   1
    ##            0 114  48
    ##            1  52  54

``` r
(t.3[1,1] + t.3[2.2])/sum(t.3)
```

    ## [1] 0.619403

``` r
t.7 <- table(titanic.knn7, titanic.test$Survived)
t.7
```

    ##             
    ## titanic.knn7   0   1
    ##            0 116  49
    ##            1  50  53

``` r
(t.7[1,1] + t.7[2.2])/sum(t.7)
```

    ## [1] 0.619403

``` r
t.0 <- table(titanic.knn0, titanic.test$Survived)
t.0
```

    ##             
    ## titanic.knn0   0   1
    ##            0 123  55
    ##            1  43  47

``` r
(t.0[1,1] + t.0[2.2])/sum(t.0)
```

    ## [1] 0.619403

``` r
result <- numeric()
k=3:22
for(i in k) {
  pred <- knn(titanic.train.k, titanic.test.k, class, k=i-2)
  t <- table(pred, titanic.test$Survived)
  result[i-2] <- (t[1,1]+t[2,2])/sum(t)
}
result
```

    ##  [1] 0.6007463 0.5820896 0.6231343 0.6380597 0.6492537 0.6380597 0.6417910 0.6492537 0.6455224 0.6305970 0.6492537
    ## [12] 0.6679104 0.6791045 0.6604478 0.6865672 0.6828358 0.6753731 0.6940299 0.6902985 0.6865672

``` r
sort(result, decreasing = T)
```

    ##  [1] 0.6940299 0.6902985 0.6865672 0.6865672 0.6828358 0.6791045 0.6753731 0.6679104 0.6604478 0.6492537 0.6492537
    ## [12] 0.6492537 0.6455224 0.6417910 0.6380597 0.6380597 0.6305970 0.6231343 0.6007463 0.5820896

``` r
which(result == max(result))
```

    ## [1] 18

-----

EOD
