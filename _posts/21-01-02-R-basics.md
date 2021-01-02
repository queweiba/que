### 读取数据
1. 将数据里的特定字符转化为NA
```python
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

**条件筛选行**

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
weight<-weight[weight$ID %in% idconc,] #用T or F 进行筛选
my_data[c(1:3),]  #根据行的位置来选择
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
doswt <- dos[,c("ID","samplemoment")]
char<-char[,-grep("birthdate|DOB|time",colnames(char))] #grep 返回的也是位置
dataly[ , -which(colnames(dataly) %in% c("b","d"))]  #which 返回的是位置而不是判断
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

3. plyr 的 `revalue()`
```r
library(plyr)
junk$nm <- revalue(junk$nm, c("B"="b"))
```

4. dplyr 的`recode()`
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

### 排序

1. 最基础的
```r
dos <- dos[order(dos$ID, dos$samplemoment, -dos$EVID),]
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

### 两个数据条件配对
1. match，这个返回的是位置，没找到就是NA
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
2. which () 这个返回的是对应的位置
```r
x <- c(1,5,8,4,6)
x
#[1] 1 5 8 4 6
which(x == 5)
#[1] 2
```
3. %in% 这个返回的T or F, 没找打返回的是F
```r
5 %in% c(1,2,9,5,3,6,7,4,5)
#[1] TRUE
#
v1 <- c("a1","b2","c1","d2")
v2 <- c("g1","x2","d2","e2","f1","a1","c2","b2","a2")
v1 %in% v2
#[1] TRUE TRUE FALSE TRUE
```

### 替换字符串
```r
gron$AMT <- gsub(",", ".", gron$AMT) 
```

### 去除重复列
```r
df.PNA <- df.tot[!duplicated(df.tot$ID),] 
```

### 统计分析
**n() count**
```r
if (require("nycflights13")) {
carriers <- group_by(flights, carrier)
summarise(carriers, n())
mutate(carriers, n = n())
filter(carriers, n() < 100)
```
**Interpolation**
1. na.approx
 an integer (of length 1 or 2) describing how interpolation is to take place outside the interval [min(x), max(x)].
 - If rule is 1 then NAs are returned for such points
 - if it is 2, the value at the closest data extreme is used.
 - Use, e.g., rule = 2:1, if the left and right side extrapolation should differ.
```r
df.tot <- df.tot %>% 
  group_by(ID) %>% 
  mutate(Weight2 = na.approx(WT, TIMECALC, rule=2)) %>% 
  ungroup()
```

### 日期
1. as. date
把character 转化为 日期用as.date ，会丢失掉时间的信息
```r
head(harMet_15Min$datetime)
#[1] "2005-01-01T00:15" "2005-01-01T00:30" "2005-01-01T00:45"
#[4] "2005-01-01T01:00" "2005-01-01T01:15" "2005-01-01T01:30"
#
# convert column to date class
dateOnly_HARV <- as.Date(harMet_15Min$datetime)
#
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

对号入座的

nrow(dataset) 列的数目

substring 选择截取字符

make order
```r
df.tot$order <- ave(row.names(df.tot), df.tot$ID, FUN = seq_along) 
```

