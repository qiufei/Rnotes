# data manipulation with R 2nd
qiufei  
September 10, 2015  

R is a domain-specific programming language, specially designed to perform statistical analysis on data.



# introduction to R data types and basic operation

R has a very steep learning curve like python,octave,and sas. However unlike octave and sas, we can find a larger number of freely available resources and tutorials on the web to get help.

a collection of data values stored in a single object is known as vector.

## data type

whatever we do in R, is stored as objects. An R objects is anything thant can be assigned to a variable of interest.

the mode and class are the most important attributes of an R objects.

if an R object contains both numeric and logical elements, the mode of that object will be numberic and ,in this case, the logical element automatically gets converted into a numeric element. the logical element `TRUE` convets to 1 and `FALSE` converts to 0.

on the other hand, if any R object contains a character element, along with both numerica and logical elements, it automatically converts to character mode.


```r
xz=c(1,3,TRUE,5,FALSE,9)
mode(xz)
```

```
## [1] "numeric"
```

```r
xw=c(1,2,TRUE,FALSE,'a')
mode(xw)
```

```
## [1] "character"
```

## an interesting calculation


```r
1-5*0.2
```

```
## [1] 0
```

```r
# you think they are same
1-0.2-0.2-0.2-0.2-0.2
```

```
## [1] 5.551115e-17
```

## a logical vector can be thought as a vector of an index


```r
age=c(20,30,22,25)
willing=c(TRUE,FALSE,TRUE,FALSE)
age[willing]
```

```
## [1] 20 22
```

## as.numeric() function only returns internal values of the factor


```r
num.factor=factor(c(5,7,9,5,6,7,3,5,3,9,7))
num.factor
```

```
##  [1] 5 7 9 5 6 7 3 5 3 9 7
## Levels: 3 5 6 7 9
```

```r
as.numeric(num.factor)
```

```
##  [1] 2 4 5 2 3 4 1 2 1 5 4
```

```r
levels(num.factor)
```

```
## [1] "3" "5" "6" "7" "9"
```

```r
as.character(num.factor)
```

```
##  [1] "5" "7" "9" "5" "6" "7" "3" "5" "3" "9" "7"
```

```r
# now to convert the 'num.factor' to numeric there are two methods
# method 1
as.numeric(as.character(num.factor))
```

```
##  [1] 5 7 9 5 6 7 3 5 3 9 7
```

```r
# method 2
as.numeric(levels(num.factor)[num.factor])
```

```
##  [1] 5 7 9 5 6 7 3 5 3 9 7
```

```r
# explain of method 2
levels(num.factor)
```

```
## [1] "3" "5" "6" "7" "9"
```

```r
levels(num.factor)[num.factor]
```

```
##  [1] "5" "7" "9" "5" "6" "7" "3" "5" "3" "9" "7"
```
## missing values in R

In R, a numeric missing value is represented by `NA`, while character missing values are represented by `<NA>`. 

# basic data manipulation

## remove unused factors


```r
char.obj="datamanipulation"

substring(char.obj,1:nchar(char.obj),1:nchar(char.obj))
```

```
##  [1] "d" "a" "t" "a" "m" "a" "n" "i" "p" "u" "l" "a" "t" "i" "o" "n"
```

```r
# some explain
substring("abcdef", 1:6, 1:6)
```

```
## [1] "a" "b" "c" "d" "e" "f"
```

```r
substring("abcdef", 1:3, 1:6)
```

```
## [1] "a"    "b"    "c"    "abcd" "bcde" "cdef"
```

```r
factor.obj=factor(substring(char.obj,1:nchar(char.obj),1:nchar(char.obj)),levels=letters)

table(factor.obj)
```

```
## factor.obj
## a b c d e f g h i j k l m n o p q r s t u v w x y z 
## 4 0 0 1 0 0 0 0 2 0 0 1 1 2 1 1 0 0 0 2 1 0 0 0 0 0
```

```r
factor.obj1=factor(factor.obj)

factor.obj2=droplevels(factor.obj)

table(factor.obj1)
```

```
## factor.obj1
## a d i l m n o p t u 
## 4 1 2 1 1 2 1 1 2 1
```

```r
table(factor.obj2)
```

```
## factor.obj2
## a d i l m n o p t u 
## 4 1 2 1 1 2 1 1 2 1
```

## lubridate package


```r
library(lubridate)
mdy("1-1-1970")
```

```
## [1] "1970-01-01 UTC"
```

```r
mdy("01-01-1970")
```

```
## [1] "1970-01-01 UTC"
```

```r
mdy("1-01-1970")
```

```
## [1] "1970-01-01 UTC"
```

```r
mdy("1-01-70")
```

```
## [1] "2070-01-01 UTC"
```

```r
# change month

date=dmy("23-07-2013")
date
```

```
## [1] "2013-07-23 UTC"
```

```r
month(date)
```

```
## [1] 7
```

```r
month(date)=8
date
```

```
## [1] "2013-08-23 UTC"
```

## keeping only values greater than 6

usually, numerical integers are used for subscripting, but logical vectors can also be used for the same purpose.


```r
num10=c(3,2,5,3,9,6,7,9,2,3)
num10>6
```

```
##  [1] FALSE FALSE FALSE FALSE  TRUE FALSE  TRUE  TRUE FALSE FALSE
```

```r
num10[num10>6]
```

```
## [1] 9 7 9
```

# chapter 3 applying the split-apply-combine strategy


## split-apply-combine using base function of R

```r
library(xlsx)
```

```
## Loading required package: rJava
## Loading required package: xlsxjars
```

```r
price=read.xlsx(file='chapter3.xls',sheetIndex = 1)

### step 1: split

# detailed code to implement the split-apply-combine approach

shanghai=subset(price,region=='shanghai',select = equipment:agriculture)

zhejiang=subset(price,region=='zhejiang',select = equipment:agriculture)

jiangsu=subset(price,region=='jiangsu',select = equipment:agriculture)

# less code to implement the split-apply-combine approach

price.split=split(price,price$region)
    # if region is not factor ,you should use as.factor(price$region)

### step 2 : apply
# apply mean funtion to calculate mean

price.apply=lapply(price.split,function(x) colMeans(x[,3:6]))

### step 3 : combine

price.combine=do.call(rbind,price.apply)

price.combine
```

```
##          equipment      gdp investment agriculture
## jiangsu   99.71905 112.0476   102.4548    105.4595
## shanghai  98.52143 109.3857   101.7524    105.7762
## zhejiang  99.93333 110.2905   102.2119    106.9119
```

```r
str(price.combine)
```

```
##  num [1:3, 1:4] 99.7 98.5 99.9 112 109.4 ...
##  - attr(*, "dimnames")=List of 2
##   ..$ : chr [1:3] "jiangsu" "shanghai" "zhejiang"
##   ..$ : chr [1:4] "equipment" "gdp" "investment" "agriculture"
```


## split-apply-combine using `plyr` package

the `plyr` package works on every type of data structure, whereas the `dplyr` package is designed to workonly on data frames.


the most important utility of the `plyr` package is that a single line of code can perform all the split,apply and combine steps.


```r
library(plyr)
```

```
## 
## Attaching package: 'plyr'
## 
## The following object is masked from 'package:lubridate':
## 
##     here
```

```r
price.mean=ddply(price,.(region),function(x) colMeans(x[,3:6]))

price.mean
```

```
##     region equipment      gdp investment agriculture
## 1  jiangsu  99.71905 112.0476   102.4548    105.4595
## 2 shanghai  98.52143 109.3857   101.7524    105.7762
## 3 zhejiang  99.93333 110.2905   102.2119    106.9119
```

```r
str(price.mean)
```

```
## 'data.frame':	3 obs. of  5 variables:
##  $ region     : Factor w/ 3 levels "jiangsu","shanghai",..: 1 2 3
##  $ equipment  : num  99.7 98.5 99.9
##  $ gdp        : num  112 109 110
##  $ investment : num  102 102 102
##  $ agriculture: num  105 106 107
```

## an array example


```r
class(iris3)
```

```
## [1] "array"
```

```r
dim(iris3)
```

```
## [1] 50  4  3
```

```r
iris.mean1=adply(iris3,.margins = 3,colMeans)
iris.mean1
```

```
##           X1 Sepal L. Sepal W. Petal L. Petal W.
## 1     Setosa    5.006    3.428    1.462    0.246
## 2 Versicolor    5.936    2.770    4.260    1.326
## 3  Virginica    6.588    2.974    5.552    2.026
```

```r
class(iris.mean1)
```

```
## [1] "data.frame"
```

```r
iris.mean2=aaply(iris3,.margins = 3,colMeans)
iris.mean2
```

```
##             
## X1           Sepal L. Sepal W. Petal L. Petal W.
##   Setosa        5.006    3.428    1.462    0.246
##   Versicolor    5.936    2.770    4.260    1.326
##   Virginica     6.588    2.974    5.552    2.026
```

```r
class(iris.mean2)  # note that here the class is showing "matrix",since the output is a two dimensional array which represents matrix. 
```

```
## [1] "matrix"
```

```r
iris.mean3=alply(iris3,.margins = 3,colMeans)
iris.mean3
```

```
## $`1`
## Sepal L. Sepal W. Petal L. Petal W. 
##    5.006    3.428    1.462    0.246 
## 
## $`2`
## Sepal L. Sepal W. Petal L. Petal W. 
##    5.936    2.770    4.260    1.326 
## 
## $`3`
## Sepal L. Sepal W. Petal L. Petal W. 
##    6.588    2.974    5.552    2.026 
## 
## attr(,"split_type")
## [1] "array"
## attr(,"split_labels")
##           X1
## 1     Setosa
## 2 Versicolor
## 3  Virginica
```

```r
class(iris.mean2)
```

```
## [1] "matrix"
```


## 3 main arguments for the `plyr` common functions 

1. a*ply(.data, .margins, .fun, ..., .progress = "none")
2. d*ply(.data, .variables, .fun, ..., .progress = "none")
3. l*ply(.data, .fun, ..., .progress = "none")

我的理解:list最本质的特征是它能处理非齐整的数据,所以没办法按照margin或者variable来分层.或者这么说,如果是一个完全齐整的list的,何不转化为array或datafame来处理?

If we want to monitor the progress of the processing task, the `.progress` argument should be specified. It will not show the progress status by default,that is `.progress = "none"'`.



