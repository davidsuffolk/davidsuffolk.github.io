---
title: "Newspaper Customer Churn Data Analysis in R"
excerpt: "Data Science, R"
header:
  teaser: /images/newspapers.jpg
sidebar:
  - title: "Newspaper Customer Churn Data Analysis in R"
    image: /images/newspapers.jpg
    image_alt: "Newspapers"
    text: "Newspaper Customer Churn Data Analysis in R"
tags: [Data Science, R]
---
A project that uses R to explore various statistics of customer churn for a newspaper.

The code below displays highlights from the project. For more details, please view the GitHub Repository.

Link to GitHub Repository:

[Click Here](https://github.com/davidsuffolk/Newspaper-Customer-Churn-Project)

Libraries and Data

```r
library(readxl)
library(ggplot2)
library(pROC)
library(caTools)
library(caret)
library(class)
library(e1071)
library(mlogit)
library(Hmisc)
ChurnData <- read_excel("NewspaperChurn.xlsx")
names(ChurnData) <- gsub(" ","_",names(ChurnData))
```
Convert Subscriber values to numbers
Subscribers = 1  
Non-subscribers = 2  

```r
ChurnData$Subscriber_Number <- as.character(ChurnData$Subscriber)
ChurnData$Subscriber_Number[ChurnData$Subscriber_Number == "YES"] <- "1"
ChurnData$Subscriber_Number[ChurnData$Subscriber_Number == "NO"] <- "2"
ChurnData$Subscriber_Number <- as.numeric(ChurnData$Subscriber_Number)
```

Convert Home Ownership values to numbers  
Renter = 1  
Owner = 2  

```r
ChurnData$Home_Ownership_Number <- as.character(ChurnData$Home_Ownership)
ChurnData$Home_Ownership_Number[ChurnData$Home_Ownership_Number == "RENTER"] <- "1"
ChurnData$Home_Ownership_Number[ChurnData$Home_Ownership_Number == "OWNER"] <- "2"
ChurnData$Home_Ownership_Number <- as.numeric(ChurnData$Home_Ownership_Number)
```

Convert Income to numeric values  
I converted the lower number of the bucket to a number  
For less than $20,000, I assigned the value of 1

```r
ChurnData$HH_Income_Number <- as.character(ChurnData$HH_Income)
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$  30,000 - $39,999"] <- "30"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$500,000 Plus"] <- "500"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$100,000 - $124,999"] <- "100"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$200,000 - $249,999"] <- "200"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$  50,000 - $59,999"] <- "50"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$150,000 - $174,999"] <- "150"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$400,000 - $499,999"] <- "400"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$175,000 - $199,999"] <- "175"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$125,000 - $149,999"] <- "125"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "Under $20,000"] <- "1"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$  80,000 - $89,999"] <- "80"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$  90,000 - $99,999"] <- "90"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$300,000 - $399,999"] <- "300"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$  20,000 - $29,999"] <- "20"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$  70,000 - $79,999"] <- "70"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$  60,000 - $69,999"] <- "60"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$  40,000 - $49,999"] <- "40"
ChurnData$HH_Income_Number[ChurnData$HH_Income_Number == "$250,000 - $299,999"] <- "250"
ChurnData$HH_Income_Number <- as.numeric(ChurnData$HH_Income_Number)
```

Convert Dummy for Children
Y -> 1  
N -> 2  

```r
ChurnData$dummy_for_Children_Number <- as.character(ChurnData$dummy_for_Children)
ChurnData$dummy_for_Children_Number[ChurnData$dummy_for_Children_Number == "Y"] <- "1"
ChurnData$dummy_for_Children_Number[ChurnData$dummy_for_Children_Number == "N"] <- "2"
ChurnData$dummy_for_Children_Number <- as.numeric(ChurnData$dummy_for_Children_Number)
```

Convert Age Range  
I converted the lower number of the bucket to a number  
For <24, I changed it to 18

```r
ChurnData$Age_range_Number <- as.character(ChurnData$Age_range)
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "25-29"] <- "25"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "50-54"] <- "50"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "45-49"] <- "45"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "55-59"] <- "55"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "60-64"] <- "60"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "75 years or more"] <- "75"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "65-69"] <- "65"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "70-74"] <- "70"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "40-44"] <- "40"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "24 years or less"] <- "18"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "35-39"] <- "35"
ChurnData$Age_range_Number[ChurnData$Age_range_Number == "30-34"] <- "30"
ChurnData$Age_range_Number <- as.numeric(ChurnData$Age_range_Number)
```

Weekly Fee  
For most levels, I converted the lower number of the bucket to a number  
For '$0' and '$0-0.01', I made them both 0  

```r
ChurnData$weekly_fee_number <- as.character(ChurnData$weekly_fee)
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$7.00 - $7.99"] <- "7"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$0.01 - $0.50"] <- "0.01"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$1.00 - $1.99"] <- "1"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$8.00 - $8.99"] <- "8"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$0 - $0.01"] <- "0"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$2.00 - $2.99"] <- "2"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$9.00 - $9.99"] <- "9"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$4.00 - $4.99"] <- "4"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$0.51 - $0.99"] <- "0.51"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$3.00 - $3.99"] <- "3"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$5.00 - $5.99"] <- "5"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$6.00 - $6.99"] <- "6"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$10.00 - $10.99"] <- "10"
ChurnData$weekly_fee_number[ChurnData$weekly_fee_number == "$0"] <- "0"
ChurnData$weekly_fee_number <- as.numeric(ChurnData$weekly_fee_number)
```

Create new dataframe with select columns and remove N/A values

```r
ChurnDataModified <- ChurnData[, c("Subscriber", "Subscriber_Number", "Home_Ownership_Number", "HH_Income_Number", "dummy_for_Children_Number", "Year_Of_Residence", "Age_range_Number", "weekly_fee_number", "Zip_Code")]
ChurnData_2 <- na.omit(ChurnDataModified)
```

Histogram of Subscribers Variable
Subscribers = 1
 Non-subscribers = 2

```r
ggplot(data = ChurnData_2, aes(Subscriber_Number)) +   
  geom_histogram(bins = 2, binwidth = 0.5)+   
  xlab("Subscribers vs. Non-Subscribers") +   
  ylab("Count")
```
![image-center](/images/newspaper_output01.png){: .align-center}

Home Ownership -> Year of Residence -> Subscription Status

```r
bar <- ggplot(data = ChurnData_2, aes(x = Home_Ownership_Number, y = Year_Of_Residence, group = Subscriber_Number, fill = Subscriber_Number)) bar +
  geom_bar(stat = "identity", position = position_dodge(1), width = 0.5) +
  scale_fill_continuous(name="Subscription",  breaks=c(1, 2),                    labels=c("Subscribers", "Non-Subscribers"))+   
  xlab("Home Ownership")+   
  ylab("Year of Residence")
```
![image-center](/images/newspaper_output02.png){: .align-center}

Histogram of Age Range

```r
ggplot(data = ChurnData_2, aes(Age_range_Number)) +   
  geom_histogram(bins = 6, binwidth = 2)+   
  xlab("Age Range") +   
  ylab("Count")
```
![image-center](/images/newspaper_output03.png){: .align-center}
