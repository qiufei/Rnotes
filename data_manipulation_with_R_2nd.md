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
# heterogeneous date
hetero_date=c("second chapter due on 2013, august, 24","first chapter submitted on 2013  aug  18","2013 aug 23")
ymd(hetero_date)
```

```
## Warning: All formats failed to parse. No formats found.
```

```
## [1] NA NA NA
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

