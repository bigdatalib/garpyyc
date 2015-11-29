Introduction to R for Risk Managers and Trading Professionals
========================================================
author: N. Safaian  
date: December 2015 

Data Science and Trading Risk Management
========================================================

- Decision Sciences
- Data Driven vs. Data Informed Decision Making
- Role of Risk Manager as a Decision Engineer

[New Approach to Risk Management](http://bigdatalib.github.io/blogs/energyrisk/)

Learning Objective
========================================================

- Grammar of R Programming
- Reading Data from Different Sources
- Cleaning Data 
- Plotting Data


Why R?
========================================================

- Open source - over 6000 > packages 
- Best tool for exploratory analysis 
- Best tool for statistical learning
- Integrates well with other tools
- It is all for free!!!


R Hello world of sort!
========================================================


```r
install.packages("xts")
x<-4
head(x)
x<-c(1,3,4)
str(x)
```

Read and write Basics
========================================================

```r
tmp<-read.csv("http://s3-us-west-2.amazonaws.com/bigdatalibopen/blogfiles/Net_generation_from_electricity_plants_for_all_sectors_monthly.csv",skip = 4); head(tmp)
names(tmp)
write.csv(tmp,file="/home/rstudio/test.csv")
```

Cleaning Data 
========================================================


```r
names(tmp)<-gsub("\\.","",names(tmp))
head(tmp)
str(tmp)
tmp$Month<-paste("1",tmp$Month)
tmp$Month<-as.Date(tmp$Month, format="%d %b %Y")
str(tmp)
```

Data types 
========================================================
- numeric
- character
- vector
- data.frame
- list
- S3/S4 (advance topic)


```r
x.n<-3; x=3
x.c<-"rs"
x.v<-c(1,2,3,5,7)
x.df<-data.frame(k=c("v1","v2"),v=c(1,2))
x.l<-list(x.n,x.c,x.df)
```

Grammar of R Programming 
========
- if/else
- for loop
- defining function
- apply family (advance)


```r
x<-1
if(x==1){print("yes")}else{print("no")}
for(i in 1:10) {print(paste0("number",":",i))}
adder<-function(x,y){return(sum(x,y))}
apply(tmp[,2:5],2,FUN=mean)
summary(tmp[,2:5])
```

Reading Data from Web
========================================================
["Quandl Example"](http://bigdatalib.github.io/blog/other/2015/10/22/Quandl-Package/)

```r
library(Quandl)
mydata = Quandl("CHRIS/CME_NG1",trim_start="1983-03-30", trim_end="2015-10-20")
head(mydata)
```

R and Database Connection
========================================================

["mongolite"](https://cran.r-project.org/web/packages/mongolite/vignettes/intro.html)


```r
require(mongolite)
m <- mongo(db="test",collection = "collectiontest",url = "mongodb://localhost:27017")
m$insert(mydata)
m$find()
m$find('{"Date": "2011-05-27"}')
```

Data Munging - dplyr 
========================================================

["dplyr"](https://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html)


```r
require(dplyr)
mydata<-mydata %>% dplyr::mutate(Date=as.Date(Date,format="%Y-%m-%d")) %>% dplyr::arrange(-desc(Date)) %>% dplyr::mutate(Year=lubridate::year(Date)) %>% dplyr::select(Date,Settle,Year,Volume) %>% dplyr::filter(Date > as.Date("2012-01-01","%Y-%m-%d"))
```

Plotting using ggplot
========================================================

[ggplot](http://docs.ggplot2.org/current/)


```r
require(ggplot2)
ggplot(data=mydata, aes(x=Settle, y=Volume, color=factor(Year)))+
geom_point()+ggtitle("")+stat_smooth()
```

Next Session:
========================================================

+ Topic: **Creating Interactive Online Data Products**
+ Date: **Late December**


