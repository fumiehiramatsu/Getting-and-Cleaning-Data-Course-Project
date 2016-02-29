# Getting-and-Cleaning-Data-Course-Project

##Download zip file
download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip", "data.zip", method = "curl")

##Unzip the downloaded file
unzip("data.zip")

## Load the respective Librarys
library(data.table)
library(dplyr)

## Set working directory to appropriate folder
setwd("~/Desktop/R Coursera/UCI HAR Dataset")

##Data divided into Subject, Activity, and Features

## Load the related data
Features <- read.table("features.txt")
ActivityLabels <- read.table("activity_labels.txt", header = FALSE)

## Load the training data set
train_subject <- read.table("train/subject_train.txt", header = FALSE)
train_activity <- read.table("train/y_train.txt", header = FALSE)
train_features <- read.table("train/X_train.txt", header = FALSE)

## Load the testing data set
test_subject <- read.table("test/subject_test.txt", header = FALSE)
test_activity <- read.table("test/y_test.txt", header = FALSE)
test_features <- read.table("test/X_test.txt", header = FALSE)

## Part1: Merge training and testing data sets 
Subject <- rbind(train_subject, test_subject)
Activity <- rbind(train_activity, test_activity)
Features <- rbind(train_features, test_features)

## Name the columns from the features file
colnames(Features) <- t(Features[2])

##Add activity and subject as columns
colnames(Activity) <- "Activity"
colnames(Subject) <- "Subject"

## Create a complete data set
CompleteData <- cbind(Features, Activity, Subject)

## Part2: Extract the means and standards deviation
colMeanStd <- grep(".*Mean.*|.*Std.*", x = names(CompleteData), ignore.case = TRUE)
filteredCols <- c(colMeanStd, 562:563)
extractedData <- CompleteData[,filteredCols]

## Part3: Assign names to the activity field
extractedData$Activity <- as.character(extractedData$Activity)
for(i in 1:6) {
  extractedData$Activity[extractedData$Activity == i] <- as.character(ActivityLabels[,2])
}

##Set activity variable as a factor
extractedData$Activity <- as.factor(extractedData$Activity)

## Part4: Label the data set with descriptive variable names
names(extractedData)<-gsub("Acc", "Accelerometer", names(extractedData))
names(extractedData)<-gsub("Gyro", "Gyroscope", names(extractedData))
names(extractedData)<-gsub("BodyBody", "Body", names(extractedData))
names(extractedData)<-gsub("Mag", "Magnitude", names(extractedData))
names(extractedData)<-gsub("^t", "Time", names(extractedData))
names(extractedData)<-gsub("^f", "Frequency", names(extractedData))
names(extractedData)<-gsub("tBody", "TimeBody", names(extractedData))
names(extractedData)<-gsub("-mean()", "Mean", names(extractedData), ignore.case = TRUE)
names(extractedData)<-gsub("-std()", "STD", names(extractedData), ignore.case = TRUE)
names(extractedData)<-gsub("-freq()", "Frequency", names(extractedData), ignore.case = TRUE)
names(extractedData)<-gsub("angle", "Angle", names(extractedData))
names(extractedData)<-gsub("gravity", "Gravity", names(extractedData))

## Part5: Extract tidy data set
##Set subject variable as a factor
extractedData$Subject <- as.factor(extractedData$Subject)
extractedData <- data.table(extractedData)

##Create average for every activity and subject
tidydata <- aggregate(extractedData, list(extractedData$Subject, extractedData$Activity), mean) 
tidydata <- tidydata[order(tidydata$Subject,tidydata$Activity),]

##Write tidydata into a text file
write.table(x = tidydata, file = "tidy.txt", row.names = FALSE)
