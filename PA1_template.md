###### Reproducible Research | Peer Assessment 1 
###### Authro: Larry Lugo, Ing. M.Sc.
###### Environment: R x64 v3.1.1. SO: Windows 8.1, 8 MG RAM
###### On an AMD E-300 APU Radeon HD Graphics 1.3 GHz (HP 2000 laptop)
###### Date: October, 2014.

## Summary

This document was created to fulfill Reproducible Research's Peer Assessment 1 requirements on writing a report using **a single R markdown document** that can be processed by **knitr** and be transformed into an HTML file.  

## Setting Working Environment

### Setting global options. Set echo equal to **TRUE** and results equal to **'hold'** for the whole document.  

```r
library(knitr)
opts_chunk$set(echo = TRUE, results = 'hold')
```

### Load required libraries. ggplot was selected for plotting

```r
library(data.table)
```

```
## Error in library(data.table): there is no package called 'data.table'
```

```r
library(xtable)
```

```
## Error in library(xtable): there is no package called 'xtable'
```

```r
library(ggplot2) 
```

```
## Use suppressPackageStartupMessages to eliminate package startup messages.
```

## Getting and Cleaning Data

### Loading and preprocessing the data
According to information provided in this assessment, data is from a personal activity monitoring device that collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual registered during the months of Oct.-Nov., 2012. Additionally, data includes number of steps taken in 5 minute intervals per day.  

Assignment's instructions request to show any code that is needed to loading and preprocessing the data, so the whole process was:

### Load and unzip data

Check if "data" directory (data_dir) exists in working directory (WD). If not the case, create it and download and unzip data info.


```r
check_file_exist <- function(file_path) 
{
        if (!file.exists(file_path))
                stop("The ", file_path, " not found!") else TRUE 
}
```


```r
load_data <- function(data_dir , fileURL, fileSource) 
{
        # Dataset check and load 
        
        source_path <- paste(data_dir, "/", fileSource , sep="")
        txt_file <- paste(data_dir, "/","activity.csv", sep="")

        if (!file.exists(txt_file)) {
                if (!file.exists(source_path)) {
                        message(paste("Please Wait! Load", fileURL, "..."));
                        download.file(fileURL, destfile=source_path);
                } 
                else {
                    message(paste("Please Wait! Unzip", source_path, " file..."));
                    unzip(source_path, exdir = data_dir);
                }
        }
        message(paste("Please Wait! Load", txt_file, " to dataset..."));
        data <- read.csv(txt_file,
                         header=TRUE,  na.strings="NA",
                         colClasses=c("numeric", "character", "numeric"))
        data$interval <- factor(data$interval)
        data$date <- as.Date(data$date, format="%Y-%m-%d")
        data        
        
}
```

***

### Assign your WD to data_dir variable. Please change it to your own configuration.


```r
data_dir <- "C:/Users/Larry/DataScience-Coursera/ReproducibleResearch/PeerAssessment1";
```

### Load and preparation of *tidy* data


```r
fileURL <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip" 
fileSource <-"activity.zip"
source_path <- paste(data_dir, "/", fileSource , sep="")
txt_file <- paste(data_dir, "/","activity.csv", sep="")

        if (!file.exists(txt_file)) {
                if (!file.exists(source_path)) {
                        message(paste("Please Wait! Load", fileURL, "..."));
                        download.file(fileURL, destfile=source_path);
                } 
                else {
                    message(paste("Please Wait! Unzip", source_path, " file..."));
                    unzip(source_path, exdir = data_dir);
                }
        }
```

```
## Please Wait! Load https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip ...
```

```
## Error in download.file(fileURL, destfile = source_path): esquema de URL sin soporte
```

```r
        message(paste("Please Wait! Load", txt_file, " to dataset..."));
```

```
## Please Wait! Load C:/Users/Larry/DataScience-Coursera/ReproducibleResearch/PeerAssessment1/activity.csv  to dataset...
```

```r
        tidy <- read.csv(txt_file,
                         header=TRUE,   sep=",",
                         colClasses=c("numeric", "character", "numeric"))
```

```
## Warning in file(file, "rt"): no fue posible abrir el archivo
## 'C:/Users/Larry/DataScience-Coursera/ReproducibleResearch/PeerAssessment1/activity.csv':
## No such file or directory
```

```
## Error in file(file, "rt"): no se puede abrir la conexión
```

```r
        tidy$interval <- factor(tidy$interval)
```

```
## Error in factor(tidy$interval): objeto 'tidy' no encontrado
```

```r
        tidy$date <- as.Date(tidy$date, format="%Y-%m-%d")
```

```
## Error in as.Date(tidy$date, format = "%Y-%m-%d"): objeto 'tidy' no encontrado
```

As a very first and necessary step, see data structure. A preliminary view:  

























