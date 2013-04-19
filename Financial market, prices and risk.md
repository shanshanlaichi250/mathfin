1 金融市场，价格和风险
========================================================
1.1 价格，回报率和股票指数
--------------------------------------
### 1.1.1 股票指数
#### 价格权重指数
价格权重指数是根据价格来定义各个股票的权重，价格高则权重高，但这个方法并不能真正反映基础市场的价值
#### 价值权重指数
价值权重指数是根据整个市场已发行股票的价值来定义权重的
### 1.1.2 价格和收益
\( p_t=\frac{p_t-p_{t-1}}{p_{t-1}}\)

1.2 S&P500 回报率
----------------------------------------

```r
library("tseries")  # load the tseries library
```

```
## Warning: package 'tseries' was built under R version 2.15.3
```

```r
library("zoo")
```

```
## Warning: package 'zoo' was built under R version 2.15.3
```

```
## Attaching package: 'zoo'
```

```
## The following object(s) are masked from 'package:base':
## 
## as.Date, as.Date.numeric
```

```r
price = get.hist.quote(instrument = "^gspc", start = "2000-01-01", quote = "AdjClose")  # download the prices,from January 1, 2000 until today
```

```
## time series starts 2000-01-03
```

```r
y = diff(log(price))  # convert the prices into returns
plot(y)  # plot the returns
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-11.png) 

```r
y = coredata(y)  # strip date information from returns
library("moments")
sd(y)
```

```
## Warning: sd(<matrix>) is deprecated.  Use apply(*, 2, sd) instead.
```

```
## AdjClose 
##   0.0134
```

```r
min(y)
```

```
## [1] -0.0947
```

```r
max(y)
```

```
## [1] 0.1096
```

```r
skewness(y)
```

```
## AdjClose 
##  -0.1635
```

```r
kurtosis(y)
```

```
## AdjClose 
##    10.43
```

```r
acf(y, 1)
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-12.png) 

```r
acf(y^2, 1)
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-13.png) 

```r
jarque.bera.test(y)
```

```
## 
## 	Jarque Bera Test
## 
## data:  y 
## X-squared = 7703, df = 2, p-value < 2.2e-16
```

```r
Box.test(y, lag = 20, type = c("Ljung-Box"))
```

```
## 
## 	Box-Ljung test
## 
## data:  y 
## X-squared = 103, df = 20, p-value = 3.716e-13
```

```r
Box.test(y^2, lag = 20, type = c("Ljung-Box"))
```

```
## 
## 	Box-Ljung test
## 
## data:  y^2 
## X-squared = 4618, df = 20, p-value < 2.2e-16
```

1.3 回报率的程式化因子
-------------------------
* 波动集群
* 后尾
* 非线性相关

1.4 波动率
-----------------------
收益率的标准偏差称为波动率

```r
library(MASS, stats)  # load stats and MASS package
q = acf(y, 20)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-21.png) 

```r
plot(q[2:20])
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-22.png) 

```r
q = acf(y^2, 20)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-23.png) 

```r
plot(q[2:20])
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-24.png) 

```r
b = Box.test(y, lag = 21, type = "Ljung-Box")
b
```

```
## 
## 	Box-Ljung test
## 
## data:  y 
## X-squared = 111.1, df = 21, p-value = 3.02e-14
```



1.6 后尾的定义
----------
### 1.6.1 后尾的检验
相比于同方差同均值的正态分布而言，具有更多极端值的分布称之为后尾分布

