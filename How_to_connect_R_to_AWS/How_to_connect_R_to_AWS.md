---
title: "How to connect R to AWS?"
output: 
  html_document: 
    keep_md: yes
---



## About  

I used *RJDBC* (unsuccessfully, see below) and *RPostgreSQL* to connect to our AWS database. They are documented here.  

## RPostgreSQL  

install and load *RPostgreSQL*  


```r
if(!"RPostgreSQL" %in% installed.packages()) install.packages("RPostgreSQL")
library(RPostgreSQL)
```

Make a connection to the database, using the values William provided to me (thanks). I have saved the password in a csv file which is NOT committed to github (just in case).  


```r
AWS_connection <-dbConnect(dbDriver("PostgreSQL"),
                           host='pmadness3.ce49qtomuo6o.us-east-1.redshift.amazonaws.com',
                           port='5439',
                           dbname='warehouse',
                           user='admin',
                           password=as.character(read.csv("password.csv")$password))
```

Table exists?  


```r
dbExistsTable(AWS_connection, "cm_beacon_fake_interaction_stats")
```

```
## [1] TRUE
```

run a query ...  


```r
dbGetQuery(AWS_connection,"SELECT COUNT(*) , campaign_id FROM cm_beacon_fake_interaction_stats GROUP BY campaign_id")
```

```
##    count campaign_id
## 1 683247        1735
## 2 512173        1734
## 3 567721        1736
## 4 141620        1732
## 5 248017        1733
```

## RJDBC

After struggling with *RJDBC* for a couple of hours, I gave up and decided to use *RPostgreSQL*. Issues: install Java, beware of 32/64 bit versions, point to the right directory, fix driver and dll issues, ... . *RPostgreSQL* doesn't need any of these.
Some of the commands I used: 


```r
# install.packages("rJava")
# Sys.setenv(JAVA_HOME='C:/Program Files/Java/jre1.8.0_171/')
# library(rJava)
# install.packages("RJDBC")
# library(RJDBC)
# download.file('http://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.1.9.1009.jar','RedshiftJDBC41-1.1.9.1009.jar')
# driver <- JDBC("com.amazon.redshift.jdbc41.Driver", "RedshiftJDBC41-1.1.9.1009.jar", identifier.quote="`")
# conn <- dbConnect(driver, url)
```



