기술 통계 (Descriptive Staticstics)
================

# 기술 통계 (Descriptive Statistics)

``` r
# 사용 데이터: R 내장 데이터셋
data <- mtcars
```

``` r
# Structure
str(data)
```

    ## 'data.frame':    32 obs. of  11 variables:
    ##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
    ##  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
    ##  $ disp: num  160 160 108 258 360 ...
    ##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
    ##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
    ##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
    ##  $ qsec: num  16.5 17 18.6 19.4 17 ...
    ##  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
    ##  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
    ##  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
    ##  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...

``` r
summary(data)
```

    ##       mpg             cyl             disp             hp       
    ##  Min.   :10.40   Min.   :4.000   Min.   : 71.1   Min.   : 52.0  
    ##  1st Qu.:15.43   1st Qu.:4.000   1st Qu.:120.8   1st Qu.: 96.5  
    ##  Median :19.20   Median :6.000   Median :196.3   Median :123.0  
    ##  Mean   :20.09   Mean   :6.188   Mean   :230.7   Mean   :146.7  
    ##  3rd Qu.:22.80   3rd Qu.:8.000   3rd Qu.:326.0   3rd Qu.:180.0  
    ##  Max.   :33.90   Max.   :8.000   Max.   :472.0   Max.   :335.0  
    ##       drat             wt             qsec             vs        
    ##  Min.   :2.760   Min.   :1.513   Min.   :14.50   Min.   :0.0000  
    ##  1st Qu.:3.080   1st Qu.:2.581   1st Qu.:16.89   1st Qu.:0.0000  
    ##  Median :3.695   Median :3.325   Median :17.71   Median :0.0000  
    ##  Mean   :3.597   Mean   :3.217   Mean   :17.85   Mean   :0.4375  
    ##  3rd Qu.:3.920   3rd Qu.:3.610   3rd Qu.:18.90   3rd Qu.:1.0000  
    ##  Max.   :4.930   Max.   :5.424   Max.   :22.90   Max.   :1.0000  
    ##        am              gear            carb      
    ##  Min.   :0.0000   Min.   :3.000   Min.   :1.000  
    ##  1st Qu.:0.0000   1st Qu.:3.000   1st Qu.:2.000  
    ##  Median :0.0000   Median :4.000   Median :2.000  
    ##  Mean   :0.4062   Mean   :3.688   Mean   :2.812  
    ##  3rd Qu.:1.0000   3rd Qu.:4.000   3rd Qu.:4.000  
    ##  Max.   :1.0000   Max.   :5.000   Max.   :8.000

``` r
#Head of data
head(data)
```

    ##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

## Central Tendency

``` r
# 평균(Mean)
sapply(data, mean, na.rm=T)
```

    ##        mpg        cyl       disp         hp       drat         wt       qsec 
    ##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250  17.848750 
    ##         vs         am       gear       carb 
    ##   0.437500   0.406250   3.687500   2.812500

``` r
# 중앙값(Median)
sapply(data, median, na.rm=T)
```

    ##     mpg     cyl    disp      hp    drat      wt    qsec      vs      am    gear 
    ##  19.200   6.000 196.300 123.000   3.695   3.325  17.710   0.000   0.000   4.000 
    ##    carb 
    ##   2.000

``` r
# 최빈값(Mode)
# 내장함수가 없어 table을 활용
apply(data, 2, function(x) names(table(x))[table(x)==max(table(x))])
```

    ## $mpg
    ## [1] "10.4" "15.2" "19.2" "21"   "21.4" "22.8" "30.4"
    ## 
    ## $cyl
    ## [1] "8"
    ## 
    ## $disp
    ## [1] "275.8"
    ## 
    ## $hp
    ## [1] "110" "175" "180"
    ## 
    ## $drat
    ## [1] "3.07" "3.92"
    ## 
    ## $wt
    ## [1] "3.44"
    ## 
    ## $qsec
    ## [1] "17.02" "18.9" 
    ## 
    ## $vs
    ## [1] "0"
    ## 
    ## $am
    ## [1] "0"
    ## 
    ## $gear
    ## [1] "3"
    ## 
    ## $carb
    ## [1] "2" "4"

## Dispersion

``` r
# Max
sapply(data, max)
```

    ##     mpg     cyl    disp      hp    drat      wt    qsec      vs      am    gear 
    ##  33.900   8.000 472.000 335.000   4.930   5.424  22.900   1.000   1.000   5.000 
    ##    carb 
    ##   8.000

``` r
# min
sapply(data, min)
```

    ##    mpg    cyl   disp     hp   drat     wt   qsec     vs     am   gear   carb 
    ## 10.400  4.000 71.100 52.000  2.760  1.513 14.500  0.000  0.000  3.000  1.000

``` r
# range
sapply(data, range)
```

    ##       mpg cyl  disp  hp drat    wt qsec vs am gear carb
    ## [1,] 10.4   4  71.1  52 2.76 1.513 14.5  0  0    3    1
    ## [2,] 33.9   8 472.0 335 4.93 5.424 22.9  1  1    5    8

``` r
# Quantile
apply(data, 2, function(x) quantile(x, probs = c(0.25, 0.5, 0.75)))
```

    ##        mpg cyl    disp    hp  drat      wt    qsec vs am gear carb
    ## 25% 15.425   4 120.825  96.5 3.080 2.58125 16.8925  0  0    3    2
    ## 50% 19.200   6 196.300 123.0 3.695 3.32500 17.7100  0  0    4    2
    ## 75% 22.800   8 326.000 180.0 3.920 3.61000 18.9000  1  1    4    4

``` r
# Variance
options("scipen" = 100)
sapply(data,var)
```

    ##           mpg           cyl          disp            hp          drat 
    ##    36.3241028     3.1895161 15360.7998286  4700.8669355     0.2858814 
    ##            wt          qsec            vs            am          gear 
    ##     0.9573790     3.1931661     0.2540323     0.2489919     0.5443548 
    ##          carb 
    ##     2.6088710

``` r
# Standard Deviation
sapply(data,sd)
```

    ##         mpg         cyl        disp          hp        drat          wt 
    ##   6.0269481   1.7859216 123.9386938  68.5628685   0.5346787   0.9784574 
    ##        qsec          vs          am        gear        carb 
    ##   1.7869432   0.5040161   0.4989909   0.7378041   1.6152000

-----

EOD
