nnet - R
================

# 메모리 초기화

``` r
rm(list=ls())
```

# 데이터 읽어 오기

``` r
data(iris)
```

# EDA

``` r
str(iris)
```

    ## 'data.frame':    150 obs. of  5 variables:
    ##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
    ##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
    ##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
    ##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
    ##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...

``` r
summary(iris)
```

    ##   Sepal.Length    Sepal.Width     Petal.Length    Petal.Width   
    ##  Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100  
    ##  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300  
    ##  Median :5.800   Median :3.000   Median :4.350   Median :1.300  
    ##  Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199  
    ##  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800  
    ##  Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500  
    ##        Species  
    ##  setosa    :50  
    ##  versicolor:50  
    ##  virginica :50  
    ##                 
    ##                 
    ## 

``` r
rbind(head(iris,3), tail(iris,3))
```

    ##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ## 1            5.1         3.5          1.4         0.2    setosa
    ## 2            4.9         3.0          1.4         0.2    setosa
    ## 3            4.7         3.2          1.3         0.2    setosa
    ## 148          6.5         3.0          5.2         2.0 virginica
    ## 149          6.2         3.4          5.4         2.3 virginica
    ## 150          5.9         3.0          5.1         1.8 virginica

``` r
colSums(is.na(iris))
```

    ## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
    ##            0            0            0            0            0

# 데이터 전처리

``` r
library(nnet)

# 종속변수가 multiclass로 one hot encoding 실행
species.ind <- class.ind(iris$Species)
species.ind[c(1,51,101), ]
```

    ##      setosa versicolor virginica
    ## [1,]      1          0         0
    ## [2,]      0          1         0
    ## [3,]      0          0         1

``` r
# 생성된 변수 추가
iris.nn <- cbind(iris, species.ind)
rbind(head(iris.nn,3), tail(iris.nn,3))
```

    ##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species setosa
    ## 1            5.1         3.5          1.4         0.2    setosa      1
    ## 2            4.9         3.0          1.4         0.2    setosa      1
    ## 3            4.7         3.2          1.3         0.2    setosa      1
    ## 148          6.5         3.0          5.2         2.0 virginica      0
    ## 149          6.2         3.4          5.4         2.3 virginica      0
    ## 150          5.9         3.0          5.1         1.8 virginica      0
    ##     versicolor virginica
    ## 1            0         0
    ## 2            0         0
    ## 3            0         0
    ## 148          0         1
    ## 149          0         1
    ## 150          0         1

# 모델링

``` r
# 훈련:검정 데이터 분리 (7:)
idx <- sample(nrow(iris.nn), nrow(iris.nn)*0.7, replace = F)
iris.train <- iris.nn[idx, ]
iris.test <- iris.nn[-idx, ]
head(iris.train)
```

    ##     Sepal.Length Sepal.Width Petal.Length Petal.Width    Species setosa
    ## 125          6.7         3.3          5.7         2.1  virginica      0
    ## 140          6.9         3.1          5.4         2.1  virginica      0
    ## 86           6.0         3.4          4.5         1.6 versicolor      0
    ## 70           5.6         2.5          3.9         1.1 versicolor      0
    ## 91           5.5         2.6          4.4         1.2 versicolor      0
    ## 12           4.8         3.4          1.6         0.2     setosa      1
    ##     versicolor virginica
    ## 125          0         1
    ## 140          0         1
    ## 86           1         0
    ## 70           1         0
    ## 91           1         0
    ## 12           0         0

``` r
model.nn <- nnet(x = iris.train[, c(1:4)], y = iris.train[, c(6:8)], size = 10, softmax = T)
```

    ## # weights:  83
    ## initial  value 117.098554 
    ## iter  10 value 45.207190
    ## iter  20 value 45.053060
    ## iter  30 value 45.046899
    ## final  value 45.046870 
    ## converged

# 성능평가

``` r
iris.pred <- predict(model.nn, iris.test[, c(1:4)], type="class")
library(caret)
confusionMatrix(as.factor(iris.pred), iris.test[, c(5)])
```

    ## Confusion Matrix and Statistics
    ## 
    ##             Reference
    ## Prediction   setosa versicolor virginica
    ##   setosa         10          0         0
    ##   versicolor      0         17        18
    ##   virginica       0          0         0
    ## 
    ## Overall Statistics
    ##                                          
    ##                Accuracy : 0.6            
    ##                  95% CI : (0.4433, 0.743)
    ##     No Information Rate : 0.4            
    ##     P-Value [Acc > NIR] : 0.005281       
    ##                                          
    ##                   Kappa : 0.391          
    ##                                          
    ##  Mcnemar's Test P-Value : NA             
    ## 
    ## Statistics by Class:
    ## 
    ##                      Class: setosa Class: versicolor Class: virginica
    ## Sensitivity                 1.0000            1.0000              0.0
    ## Specificity                 1.0000            0.3571              1.0
    ## Pos Pred Value              1.0000            0.4857              NaN
    ## Neg Pred Value              1.0000            1.0000              0.6
    ## Prevalence                  0.2222            0.3778              0.4
    ## Detection Rate              0.2222            0.3778              0.0
    ## Detection Prevalence        0.2222            0.7778              0.0
    ## Balanced Accuracy           1.0000            0.6786              0.5

# 시각화 & 인사이트

``` r
# library("devtools")
# source_url('https://gist.githubusercontent.com/fawda123/7471137/raw/466c1474d0a505ff044412703516c34f1a4684a5/nnet_plot_update.r')
# plot.nnet(iris.nn)
```

# 성능 튜닝

``` r
# hidden layer node 수에 따른 성능 확인
set.seed(0982)
idx <- sample(nrow(iris.nn), nrow(iris.nn)*0.7, replace = F)
iris.train <- iris.nn[idx, ]
iris.test <- iris.nn[-idx, ]

results <- c()
for (nHL in 1:10) {
  model.nn <- nnet(x = iris.train[, c(1:4)], y = iris.train[, c(6:8)], size = nHL, softmax = T)
  iris.pred <- predict(model.nn, iris.test[, c(1:4)], type="class")
  cM <- confusionMatrix(as.factor(iris.pred), iris.test[, c(5)])
  results <- cbind(results, cM$overall[1])
}
```

    ## # weights:  11
    ## initial  value 118.850003 
    ## iter  10 value 40.953705
    ## iter  20 value 11.291315
    ## iter  30 value 4.533674
    ## iter  40 value 4.511915
    ## iter  50 value 4.509889
    ## iter  60 value 4.509622
    ## iter  70 value 4.507717
    ## iter  80 value 4.504318
    ## iter  90 value 4.500439
    ## iter 100 value 4.498591
    ## final  value 4.498591 
    ## stopped after 100 iterations
    ## # weights:  19
    ## initial  value 122.473414 
    ## iter  10 value 51.555530
    ## iter  20 value 44.975869
    ## iter  30 value 25.060849
    ## iter  40 value 5.300679
    ## iter  50 value 4.635000
    ## iter  60 value 4.452674
    ## iter  70 value 4.451192
    ## iter  80 value 4.450776
    ## iter  90 value 4.450404
    ## iter 100 value 4.450377
    ## final  value 4.450377 
    ## stopped after 100 iterations
    ## # weights:  27
    ## initial  value 133.187327 
    ## iter  10 value 55.848635
    ## iter  20 value 4.951841
    ## iter  30 value 4.597868
    ## iter  40 value 4.489144
    ## iter  50 value 4.457304
    ## iter  60 value 4.451501
    ## iter  70 value 4.450405
    ## iter  80 value 4.449950
    ## final  value 4.449946 
    ## converged
    ## # weights:  35
    ## initial  value 115.223740 
    ## iter  10 value 38.205070
    ## iter  20 value 35.124641
    ## final  value 32.051774 
    ## converged
    ## # weights:  43
    ## initial  value 126.428889 
    ## iter  10 value 45.138273
    ## iter  20 value 44.986064
    ## final  value 44.985482 
    ## converged
    ## # weights:  51
    ## initial  value 131.538633 
    ## iter  10 value 36.620868
    ## iter  20 value 4.480806
    ## iter  30 value 1.442143
    ## iter  40 value 0.033450
    ## final  value 0.000047 
    ## converged
    ## # weights:  59
    ## initial  value 175.882329 
    ## iter  10 value 45.833665
    ## iter  20 value 44.988931
    ## iter  30 value 16.280322
    ## iter  40 value 4.860421
    ## iter  50 value 4.769018
    ## iter  60 value 4.583990
    ## iter  70 value 4.560681
    ## iter  80 value 4.554846
    ## iter  90 value 4.553332
    ## iter 100 value 4.552608
    ## final  value 4.552608 
    ## stopped after 100 iterations
    ## # weights:  67
    ## initial  value 102.320078 
    ## iter  10 value 29.238341
    ## iter  20 value 4.190014
    ## iter  30 value 0.014158
    ## iter  40 value 0.000233
    ## final  value 0.000086 
    ## converged
    ## # weights:  75
    ## initial  value 134.701260 
    ## iter  10 value 18.697679
    ## iter  20 value 4.743537
    ## iter  30 value 0.597824
    ## iter  40 value 0.000410
    ## final  value 0.000066 
    ## converged
    ## # weights:  83
    ## initial  value 120.302342 
    ## iter  10 value 24.627077
    ## iter  20 value 4.970458
    ## iter  30 value 4.495297
    ## iter  40 value 1.822615
    ## iter  50 value 0.002782
    ## iter  60 value 0.000170
    ## final  value 0.000062 
    ## converged

``` r
cat(paste("Num.Node: ",  which.max(results), "\nAccuracy: ", max(results), sep = ""))
```

    ## Num.Node: 1
    ## Accuracy: 0.977777777777778
