### 读取数据
1. 将数据里的特定字符转化为NA
```r
vanco<-read.csv("Vancomycin_FINAL2.csv",na.strings=".")
```
2. 没有列名
```r
vanco<-read.csv("Vancomycin_FINAL2.csv",header=F)
```
3. 跳过第一行
```r
vanco<-read.csv("Vancomycin_FINAL2.csv",skip=1)
```
4. 设置列名
```r
row.names==NULL. 
#通常情况下不用设置row.names也能使用默认排序列，若需用数据中的列排序则需 row.names==列名 
#Using NULL for the value resets the row names to seq_len(nrow(x)), regarded as ‘automatic'
```


### 筛选数据

**筛选行**

1. base 里的subset()
```r
subset(x, subset, select, drop = FALSE, ...)
#正常情况下选择的条件是T or F
subset(airquality, Temp > 80, select = c(Ozone, Temp))
#现在R版本支持用位置作为筛选条件
nm <- rownames(state.x77)
subset(state.x77, grepl("^M", nm), Illiteracy:Murder)
#注:
#grepl () --返回具有相应字母的在一个序列里的T or F
#grep () 返回的是相应字母在一个序列里的位置
grep("[a-z]", letters)
#[1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26
grepl("[a-z]", letters)
#[1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
```

2. dplyr包的filter()
```r
filter(.data, ..., .preserve = FALSE)
#注意：filter的筛选命令生效是在条件（condition，有点类似于循环语句）为「TRUE」的情况下才能进行，往往并不包括「NA」值和「FALSE」条件;
#因此，如果筛选缺失值，不能直接使用比较运算符，只能通过R中的内置缺失值判断函数来进行「TRUE的操作」，比如：is.na(x)
df <- tibble(x = c(1, NA, 3))
filter(df, is.na(x) | x > 1)
#A tibble: 2 x 1
#1    NA
#2     3
```

3. 用dplyr 的 `slice ()` 根据行的位置来选择
```r
my_data <- as_tibble(iris)
my_data
my_data %>% slice(1:6)
```
4. 最基础的
```r
weight<-weight[weight$ID %in% idconc,] #用逻辑筛选
my_data[c(1:3),]  #根据行的位置来选择
#用列名筛选，但首先需要设置列名
read.csv(row.names=1)#将第一列设置为行名，然后就可以用行名筛选
```
```r
#注：
#用 "-"去 删除T or F 是错误，要用！
obs1<-obs[-(obs$ID==2016&obs$SAMPLENUMBER==201605),]
#因为会产生
#[1]  0  0  0  0  0  0  0  0  0  0  0 -1  0  0  0  0  0  0  0  
```

**筛选列--根据列名**
1. base 的 subset
```r
subset(airquality, select = Ozone:Wind)
```

2. dylyr 包的 select
```r
select(iris, starts_with("Petal"))
select(iris, ends_with("Width"))
#Move Species variable to the front
select(iris, Species, everything())
#Drop variables with -
select(iris, -starts_with("Petal"))
```

3. 最基础的(通过列名，通过位置，通过判断)
```r
doswt <- dos[,c("ID","samplemoment")]#通过列名
char<-char[,-grep("birthdate|DOB|time",colnames(char))] #grep 返回的也是位置
dataly[ , -which(colnames(dataly) %in% c("b","d"))]  #which 返回的是位置而不是判断
dat[dat$Group == 'Control', ] #通过判断
```

## 条件替换
1. `ifelse()` 
```r
gsample$Continent <- with(gsample, ifelse(MAKE=='HOLDEN', 'AUS', Continent))
```

2. 基础的
```r
gsample$Continent[gsample$MAKE=='HOLDEN'] <- 'AUS'
```

3. replace()
```r
x <- data.frame(a = c(0,1,2,NA), b = c(0,NA,1,2), c = c(NA, 0, 1, 2)) 
#   a  b  c
#1  0  0 NA
#2  1 NA  0
#3  2  1  1
#4 NA  2  2
x$a <- replace(x$a, is.na(x$a), 0)
x
#  a  b  c
#1 0  0 NA
#2 1 NA  0
#3 2  1  1
#4 0  2  2
x$b <- replace(x$b, x$b==2, 333)
x
#  a   b  c
#1 0   0 NA
#2 1  NA  0
#3 2   1  1
#4 0 333  2
```

4. plyr 的 `revalue()`
```r
library(plyr)
junk$nm <- revalue(junk$nm, c("B"="b"))
```

5. dplyr 的`recode()`
```r
char_vec <- sample(c("a", "b", "c"), 10, replace = TRUE)  
recode(char_vec, a = "Apple")
#>  [1] "Apple" "b"     "Apple" "b"     "c"     "Apple" "c"     "c"     "b"    
#> [10] "Apple"
#
#Use .default as replacement for unmatched values
recode(char_vec, a = "Apple", b = "Banana", .default = NA_character_)
#>  [1] "Apple"  "Banana" "Apple"  "Banana" NA       "Apple"  NA       NA      
#>  [9] "Banana" "Apple" 
#
#Use a named character vector for unquote splicing with !!!
level_key <- c(a = "apple", b = "banana", c = "carrot")
recode(char_vec, !!!level_key)
#>  [1] "apple"  "banana" "apple"  "banana" "carrot" "apple"  "carrot" "carrot"
#>  [9] "banana" "apple" 
#
#For numeric values, named arguments can also be used
num_vec <- c(1:4, NA)
recode(num_vec, `2` = 20L, `4` = 40L)
#> [1]  1 20  3 40 NA
```

6. dplyr的`case_when()`   

```r   
#the left hand side must be logical.   
#If none of the cases match, NA is used for right hand side(RHS).  
#All RHS values need to be of the same type. Inconsistent types will throw an error.   
#This applies also to NA values used in RHS: NA is logical, use typed values like NA_real_, NA_complex, NA_character_, NA_integer_ as appropriate.
  p <- case_when(
    BW <=1000 ~24.615,
    between(BW,1001,1250) ~31.814,
    between(BW,1251,1500) ~36.575,
    between(BW,1501,1750) ~39.839,
    between(BW,1751,2000) ~38.184,
    between(BW,2001,2250) ~44.88,
    BW >2250 ~38.203
  )
  #dplyr::between() is a shortcut for x >= left & x <= right,返回的是T or F；
  
#the output values after TURE  specifies a value to output if none of the other conditions were true
case_when(test_score_vector >= 90 ~ 'A'
          ,test_score_vector >= 80 ~ 'B'
          ,test_score_vector >= 70 ~ 'C'
          ,test_score_vector >= 60 ~ 'D'
          ,TRUE ~ 'F'
          )
```
7. 其他switch()
```r
switch (expression, list) #可以替换一个list
switch(input$var,
                   "Percent White" = list(counties$white, "darkgreen", "% White")
       )            
```
### 排序

1. 最基础的
```r
dos <- dos[order(dos$ID, dos$samplemoment, -dos$EVID),]
```

2. dplyr 的 `arrange()`     

  Unlike other dplyr verbs, arrange() largely ignores grouping; you need to explicitly mention grouping variables (or use .by_group = TRUE) in order to group by them, and functions of variables are evaluated once per data frame, not once per group.   
```r
iris %>% arrange(across(starts_with("Sepal")))
```

3. data.table
```r
dos<-dos[order[ID]]
```

### 修改列的名字
1. colnames（）
```r
colnames(weight)[4] <- "WT"
```
2. dplyr 包里的rename ()
```r
rename(dataset, newname=oldname)
``` 
3. data.table setname()
```r
setnames(x, old, new)
```

### 两个数据条件配对
1. match: The match() function returns a vector of the position of first occurrence of the vector1 in vector2. If the element of the vector1 does not exist in vector2, NA is returned. 即这个返回的是第二个序列在第一个序列中的位置。第一个序列在第二个序列中没有对应的值则为NA。
```r
print(match(5, c(1,2,9,5,3,6,7,4,5)))
#[1] 4
#
v1 <- c("a1","b2","c1","d2") 
v2 <- c("g1","x2","d2","e2","f1","a1","c2","b2","a2")
x <- match(v1,v2)
x
v2[match(v1,v2)]
#[1] 6 8 NA 3
#[1] "a1" "b2" NA   "d2"
#
#
v1 <- c("a1","b2","c1","d2")
v2 <- c("g1","x2","d2","e2","f1","a1","c2","b2","a2")
x <- match(v1,v2, nomatch = 0)
x
#[1] 6 8 0 3
```
nomatch option,如果想把NA替换为其他数字，可以使用match(..., nomatch=0)

2. which () 这个返回的是True对应的位置
```r
x <- c(1,5,8,4,6)
x
#[1] 1 5 8 4 6
which(x == 5)
#[1] 2
```

3. %in% 这个返回的T or F, 没找打返回的是F，返回的是第一个序列中能否在第二个序列中找到
```r
5 %in% c(1,2,9,5,3,6,7,4,5)
#[1] TRUE
#
v1 <- c("a1","b2","c1","d2")
v2 <- c("g1","x2","d2","e2","f1","a1","c2","b2","a2")
v1 %in% v2
#[1] TRUE TRUE FALSE TRUE
```
### 比较
1. identical

identical是比较严格的比较，如果只想比较数值可以用“==”
```r
identical(1, 1.)   ## TRUE in R (both are stored as doubles)
identical(1, as.integer(1)) ## FALSE, stored as different types
identical(as.double(8), as.integer(8))
#[1] FALSE
as.double(8) == as.integer(8)
#[1] TRUE
```
2. all.equal()

all.equal(x, y) is a utility to compare R objects x and y testing ‘near equality’. If they are different, comparison is still made to some extent, and a report of the differences is returned.还可以用来比较list, environment and POSIXt
```r
all.equal(pi, 355/113)
#[1] "Mean relative difference: 8.491368e-08"
#需要用tolerance来限制允许相似的位数
all.equal(pi, 355/113,tolerance = 0.006)
#[1] TRUE
#安全起见最好用is.TRUE()如果需要返回loagical，因为all.equal有时候并不返回logical
is.TRUE(all.equal(pi, 355/113))
#[1] FALSE
```

### 去除重复列
```r
df.PNA <- df.tot[!duplicated(df.tot$ID),] 
```

### 统计分析：Summarise
Useful functions
- Center: mean(), median()

- Spread: sd(), IQR(), mad()

- Range: min(), max(), quantile()

- Position: first(), last(), nth(),

- Count: n(), n_distinct()

- Logical: any(), all()
- summarize multiple columes: https://dplyr.tidyverse.org/reference/summarise_all.html

**n() count**
```r
if (require("nycflights13")) {
carriers <- group_by(flights, carrier)
summarise(carriers, n())
mutate(carriers, n = n())
filter(carriers, n() < 100)
```
### Interpolation
1. zoo::na.approx
 an integer (of length 1 or 2) describing how interpolation is to take place outside the interval [min(x), max(x)].
 - If rule is 1 then NAs are returned for such points
 - if it is 2, the value at the closest data extreme is used.
 - Use, e.g., rule = 2:1, if the left and right side extrapolation should differ.
 - use na.rm=False to keep those NA values that can not be interpolated.
```r
df.tot <- df.tot %>% 
  group_by(ID) %>% 
  mutate(Weight2 = na.approx(WT, TIMECALC, rule=2)) %>% 
  ungroup()
d0 <- as.Date("2000-01-01")
z <- zoo(c(11, NA, 13, NA, 15, NA), d0 + 1:6)
z
#2000-01-02 2000-01-03 2000-01-04 2000-01-05 2000-01-06 2000-01-07 
#        11         NA         13         NA         15         NA 
na.approx(z, xout = d0 + 7)
#Data:
#numeric(0)
na.approx(z, xout = d0 + 7, na.rm = FALSE)
#2000-01-08 
#        NA
na.approx(z, xout = d0 + 7, rule = 2)
#2000-01-08 
#        15 
```

2. LOCF公式
- Zoo:Generic function for replacing each NA with the most recent non-NA prior to it.
```r
na.locf(object, na.rm = TRUE, ...)
## Default S3 method:
na.locf(object, na.rm = TRUE, fromLast, rev,
        maxgap = Inf, rule = 2, ...)
na.locf0(object, fromLast = FALSE, maxgap = Inf, coredata = NULL)
#The function na.locf0 is the workhorse function underlying the default na.locf method. It has more limited capabilities but is faster for the special cases it covers. Implicitly, it uses na.rm=FALSE.
#example
x<-c(NA,NA,1,3,NA,6,NA)
na.locf(x)
#[1] 1 3 3 6 6
 na.locf(x,na.rm = F)
#[1] NA NA  1  3  3  6  6
na.locf0(x)
#[1] NA NA  1  3  3  6  6
na.locf(x,fromLast = T)
#[1] 1 1 1 3 6 6
```
3. tidyr 的 fill() 

Fills missing values in selected columns using the previous entry. This is useful in the common output format where values are not repeated, they're recorded each time they change. notice that return a dataset instead of a vector

```r
fill(data, ..., .direction = c("down", "up"))
``` 

4. Lag() or lead()

寻找序列中前n个值dplyr::lag; 寻找序列中后n个的值dplyr::lead. 注意与R basic 的lag区分开
```r
lag(x, n = 1L, default = NA, order_by = NULL, ...)
lead(x, n = 1L, default = NA, order_by = NULL, ...)
dplyr::lag(1:5)
## [1] NA  1  2  3  4
dplyr::lag(1:5, 2)
## [1] NA NA  1  2  3
dplyr::lead(1:5)
## [1]  2  3  4  5 NA
# If you want to define a value for non-existing rows, use `default`
lag(1:5, default = 0)
#[1] 0 1 2 3 4
```
### 日期
1. as. date
把character 转化为日期用as.date ，会丢失掉时间的信息
```r
head(harMet_15Min$datetime)
#[1] "2005-01-01T00:15" "2005-01-01T00:30" "2005-01-01T00:45"
#[4] "2005-01-01T01:00" "2005-01-01T01:15" "2005-01-01T01:30"
#
# convert column to date class
dateOnly_HARV <- as.Date(harMet_15Min$datetime)
# view data
head(dateOnly_HARV)
#[1] "2005-01-01" "2005-01-01" "2005-01-01" "2005-01-01" "2005-01-01"
#[6] "2005-01-01"
```
2. POSIXct   
  as.POSIXct stores both a date and time with an associated time zone. 

    The default time zone selected, is the time zone that your computer is set to which is most often your local time zone
```r
#Convert character data to date and time.
timeDate <- as.POSIXct("2015-10-19 10:15")   
str(timeDate) #变成POSIXct class
#POSIXct[1:1], format: "2015-10-19 10:15:00"
timeDate
##[1] "2015-10-19 10:15:00 MDT"
##useful sample
#DOM=as.POSIXct(DOM,tz="Europe/Amsterdam",format= "%Y-%m-%d %H:%M")
```
POSIXct stores date and time in seconds with the number of seconds beginning at 1 January 1970. Negative numbers are used to store dates prior to 1970.
  
    The unclass method in R allows you to view how a particular R object is stored.
```r
#to see the data in this 'raw' format, i.e., not formatted according to the class type to show us a date we recognize, use the `unclass()` function.
unclass(timeDate)
#
##[1] 1445271300
##attr(,"tzone")
##[1] "
``` 
To force a time zone change without change of time
```r
library(lubridate)
hu<- force_tz(hu, tzone = "Europe/Amsterdam")
```
If you want to change the time display and also the timezone
```r
#attr(hu$SAMPLEMOMENT, "tzone") <- "Europe/Amsterdam", this will definitely change the time display
``` 

3. POSIXlt   
 When we convert a string to POSIXlt, and view it in R, it still looks similar to the POSIXct format. 
 However, unclass() shows us that the data are stored differently. 
 The POSIXlt class stores the hour, minute, second, day, month, and year separately.
```r
#Convert character data to POSIXlt date and time
timeDatelt<- as.POSIXlt("2015-10-19 10:15")  
str(timeDatelt)
##POSIXlt[1:1], format: "2015-10-19 10:15:00"
timeDatelt
##[1] "2015-10-19 10:15:00 MDT"
unclass(timeDatelt)
##$sec
##[1] 0
##$min
##[1] 15
## $hour
## [1] 10
## 
## $mday
## [1] 19
## 
## $mon
## [1] 9
## 
## $year
## [1] 115
## 
## $wday
## [1] 1
## 
## $yday
## [1] 291
## 
## $isdst
## [1] 1
## 
## $zone
## [1] "MDT"
## 
## $gmtoff
## [1] NA
```

## 其它
去除factor——unfactor

nrow(dataset) 列的数目

substring 选择截取字符

make order
```r
df.tot$order <- ave(row.names(df.tot), df.tot$ID, FUN = seq_along) 
```

计算列中非NA的列数目
```r
mutate(nAMT=n()-sum(is.na(AMT)))
```


**Apply**
1. apply
Returns a vector or array or list of values obtained by applying a function to margins of an array or matrix.	
margins is a vector giving the subscripts which the function will be applied over. E.g., for a matrix 1 indicates rows, 2 indicates columns, c(1, 2) indicates rows and columns. Where X has named dimnames, it can be a character vector selecting dimension names.
```r
 x <- cbind(x1 = 3, x2 = c(4:1, 2:5))
 dimnames(x)[[1]] <- letters[1:8]
> x
#  x1 x2
#a  3  4
#b  3  3
#c  3  2
#d  3  1
#e  3  2
#f  3  3
#g  3  4
#h  3  5
apply(x, 2, mean, trim = .2)
#x1 x2 
# 3  3
```
2. lapply   
  将一个function联系运用到向量的每一个值, 直到序列得最后一个数值,但返回得总是list，且length跟输入的一样
```r
x <- list(a = 1:10, b = log10(2:78), c = c(TRUE,FALSE,FALSE,FALSE))
lapply(x, quantile)
#$`a`
#   0%   25%   50%   75%  100% 
# 1.00  3.25  5.50  7.75 10.00 
#$beta
#         0%         25%         50%         75%        100% 
# 0.04978707  0.25160736  1.00000000  5.05366896 20.08553692 
#$logic
#  0%  25%  50%  75% 100% 
# 0.0  0.0  0.5  1.0  1.0 
```
3. sapply  
    返回的根据function()返回的值，更自由一些，可以是vector ,list, matrix
```r
sapply(X, FUN, ..., simplify = TRUE, USE.NAMES = TRUE)
x <- list(a = 1:10, b = log10(2:78), c = c(TRUE,FALSE,FALSE,FALSE))
z <- sapply(x, quantile)
z
#         a        beta logic
#0%    1.00  0.04978707   0.0
#25%   3.25  0.25160736   0.0
#50%   5.50  1.00000000   0.5
#75%   7.75  5.05366896   1.0
#100% 10.00 20.08553692   1.0
```
4. mapply  
    将一个function联系运用到向量的每一个值, 直到序列得最后一个数值。与sapply相同，但是适用于可以出入两个以上参数的function
```r
mapply(FUN, ..., MoreArgs = NULL, SIMPLIFY = TRUE,
       USE.NAMES = TRUE)
combined$WT<-mapply(WT_calculation,combined$BW,combined$PNA1)

```
**多个列同时转换**
1. across() makes it easy to apply the same transformation to multiple columns.  
Note: across() is used within functions like summarise() and mutate(), you can't select or compute upon grouping variables.
```r
across(.cols = everything(), .fns = NULL, ..., .names = NULL)
```
### .fns
Functions to apply to each of the selected columns. Possible values are:
- A function, e.g. mean.
- A purrr-style lambda, e.g. ~ mean(.x, na.rm = TRUE)
- A list of functions/lambdas, e.g. list(mean = mean, n_miss = ~ sum(is.na(.x))
- NULL: the default value, returns the selected columns in a data frame without applying a transformation. This is useful for when you want to use a function that takes a data frame.

**self-develped functions and aceoss function**  
If a formula, e.g. ~ .x + 2, it is converted to a function. There are three ways to refer to the arguments:  
- For a single argument function, use .  
- For a two argument function, use .x and .y  
- For more arguments, use ..1, ..2, ..3 etc
```r
u_raw <- u_raw %>% mutate(across(.cols =ends_with("mg_ml"),~ round(.x,4)))
gdf %>% mutate(across(v1:v2, ~ .x + rnorm(1)))
mutate(across(c(1, 2), round))
u_raw <- u_raw %>% mutate(across(.cols =ends_with("mg_ml"),as.numeric ))
u_raw <- u_raw %>% mutate(across(.cols =ends_with("mg_ml"),~ ./1000))
```

### .mutate if
```r
library(dplyr)
df %>% 
 mutate_if(is.numeric, round)
```

**group_by 多个变量的特殊性**

When you group by multiple variables, each summary peels off one level of the grouping. That makes it easy to progressively roll-up a dataset.

```r
cars %>%
  group_by(am, gear) %>%
  summarise(n = n()) %>%
  mutate(freq = n / sum(n))

#   am gear  n      freq
# 1  0    3 15 0.7894737
# 2  0    4  4 0.2105263
# 3  1    4  8 0.6153846
# 4  1    5  5 0.3846154
```

**分割组别**  
cut()
```r
Total_Erasmus$GA_Group <- cut(as.numeric(Total_Erasmus$GA_week), breaks = c(-Inf, 28, 32, 37, Inf) , c("<=28", "28-32", "32-37", ">37"), include.lowest=TRUE)
```


## 字符串
**字符串的查找和替换** 
grep,grepl,sub,gsub,%like%

```r
#grepl () --返回具有相应字母的在一个序列里的T or F
#grep () 返回的是相应字母在一个序列里的位置
grep("[a-z]", letters)
#[1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26
grepl("[a-z]", letters)
#[1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE

#%like% 是grep()的简洁用法
# NOT RUN {
DT = data.table(Name=c("Mary","George","Martha"), Salary=c(2,3,4))
DT[Name %like% "^Mar"]
DT[Name %ilike% "mar"]
DT[Name %flike% "Mar"]
# }

#sub () 替换substring 中的第一个出现的词
#gsub () 替换substring 中的所有出现的词
x <- "aaabbb"
sub("a", "c", x)           # Apply sub function in R
# "caabbb"
gsub("a", "c", x)          # Apply gsub function in R
# "cccbbb"
```

**分割字符串**
1. dplyr separate(), 返回的是dataset, 它可以分开字符串或者数字，根据是第几个数字
```
separate(
  data,
  col,
  into,
  sep = "[^[:alnum:]]+",
  remove = TRUE,
  convert = FALSE,
  extra = "warn",
  fill = "warn",
  ...
)
```
2. Stringr. split a vector of strings into a matrix of substrings
```
library(stringr)

df[c('col1', 'col2')] <- str_split_fixed(df$original_column, 'sep', 2)
```

**提取字符串的数字**
1. readr 里面的parse_number()  
This parses the first number it finds, dropping any non-numeric characters before the first number and all characters after the first number. The grouping mark specified by the locale is ignored inside the number.
```r
parse_number("$1,000") ## leading `$` and grouping character `,` ignored
#> [1] 1000
parse_number("euro1,000") ## leading non-numeric euro ignored
#> [1] 1000
parse_number("t1000t1000") ## only parses first number found
#> [1] 1000
```
