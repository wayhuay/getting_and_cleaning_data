## Getting and Cleaning Data
This is a repository for codes written for Module 3 - Getting and Cleaning Data Coursera course.



## Objective
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set.



## Goal
The goal is to prepare tidy data that can be used for later analysis.
The data for the project: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 



## Course Project
Required to submit:
<ul>
  <li> a tidy data set. </li>
  <li> an R Script (run_analysis.R) link from the Github repository. </li>
  <li> a code book (CodeBook.md) that describes the variables, the data, and any transformations or work performed to clean up the data. </li>
</ul>



## The R script (run_analysis.R) should do the following: 
<ul>
  <li> Merges the training and the test sets to create one data set. </li>
  <li> Extracts only the measurements on the mean and standard deviation for each measurement. </li>
  <li> Uses descriptive activity names to name the activities in the data set. </li>
  <li> Appropriately labels the data set with descriptive variable names. </li>
  <li> From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject. </li>
</ul>



## Packages
Packages required for this project are <b>dplyr</b> and <b>data.table</b>.

```{r, message=FALSE}
install.packages("data.table")
install.packages("dplyr")

library(data.table)
library(dplyr)
```



## Read dataset from file
The <b>metadata</b> is stored in the respective variables called feature and activity_labels.
```{r, message=FALSE}
feature <- read.table("UCI HAR Dataset/features.txt")
activity_labels <- read.table("UCI HAR Dataset/activity_labels.txt", header = FALSE)
```

The <b>training data</b> is stored in the respective variables called subjectTrain, activityTrain and featuresTrain.
```{r, message=FALSE}
subjectTrain <- read.table("UCI HAR Dataset/train/subject_train.txt", header = FALSE)
activityTrain <- read.table("UCI HAR Dataset/train/y_train.txt", header = FALSE)
featuresTrain <- read.table("UCI HAR Dataset/train/X_train.txt", header = FALSE)
```

The <b>test data</b> is stored in the respective variables called subjectTest, activityTest and featuresTest.
```{r, message=FALSE}
subjectTest <- read.table("UCI HAR Dataset/test/subject_test.txt", header = FALSE)
activityTest <- read.table("UCI HAR Dataset/test/y_test.txt", header = FALSE)
featuresTest <- read.table("UCI HAR Dataset/test/X_test.txt", header = FALSE)
```



## Step 1: Merge the <i>training data</i> and the <i>test data</i> sets to create one data set.
Combine the data into respective variables called subject, activity and features.
```{r, message=FALSE}
subject <- rbind(subjectTrain, subjectTest)
activity <- rbind(activityTrain, activityTest)
features <- rbind(featuresTrain, featuresTest)
```

Rename the column in the <b>feature</b> variable in <b>metadata</b>.
```{r, message=FALSE}
colnames(features) <- t(feature[2])
```

Merge the Data.
```{r, message=FALSE}
colnames(activity) <- "Activity"
colnames(subject) <- "Subject"
completeData <- cbind(features,activity,subject)
```



## Step 2: Extract the mean and standard deviation for each measurements.
Extract column that has mean or std.
```{r, message=FALSE}
columnsWithMeanSTD <- grep(".*Mean.*|.*Std.*", names(completeData), ignore.case=TRUE)
```

Create a dimension for activity and subject.
```{r, message=FALSE}
requiredColumns <- c(columnsWithMeanSTD, 562, 563)
dim(completeData)
```

<b>Results:</b>
```{r, message=FALSE}
# [1] 10299   563
```

Create a dimension to extract data from requiredColumns.
```{r, message=FALSE}
extractedData <- completeData[,requiredColumns]
dim(extractedData)
```

<b>Results:</b>
```{r, message=FALSE}
#[1] 10299    88
```



## Step 3: Name the activities in the data set.

Change the field type from numeric to character for the variable <b>activity</b>.
```{r, message=FALSE}
extractedData$Activity <- as.character(extractedData$Activity)
for (i in 1:6){
  extractedData$Activity[extractedData$Activity == i] <- as.character(activity_labels[i,2])
}
```

Create a factor for variable <b>activity</b>.
```{r, message=FALSE}
extractedData$Activity <- as.factor(extractedData$Activity)
```



## Step 4: Label the data set with descriptive variable names.

Name the variable <b>extractedData</b>.
```{r, message=FALSE}
names(extractedData)
```

Acronyms for extractedData can be replaced.
<ul>
  <li>From BodyBody To Body</li>
  <li>From Mag to Magnitude</li>
  <li>From character f to Frequency</li>
  <li>From character t to Time</li>
</ul>

```{r, message=FALSE}
names(extractedData)<-gsub("BodyBody", "Body", names(extractedData))
names(extractedData)<-gsub("Mag", "Magnitude", names(extractedData))
names(extractedData)<-gsub("^t", "Time", names(extractedData))
names(extractedData)<-gsub("-freq()", "Frequency", names(extractedData), ignore.case = TRUE)
names(extractedData)
```



## Step 5: From the data set in step 4, get the average of variables <i>activity</i> and <i>subject</i> and create a tidy data set.

Set Subject as factor.
```{r, message=FALSE}
extractedData$Subject <- as.factor(extractedData$Subject)
extractedData <- data.table(extractedData)
```

Generate tidy data file (Tidy.txt).
```{r, message=FALSE}
tidyData <- aggregate(. ~Subject + Activity, extractedData, mean)
tidyData <- tidyData[order(tidyData$Subject,tidyData$Activity),]
write.table(tidyData, file = "Tidy.txt", row.names = FALSE)
```
















