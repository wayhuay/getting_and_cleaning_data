## Course Project CodeBook
This document describes the variable definition used in run_analysis.R script to generate the tidy data (Tidy.txt).



## Data
The data for the project:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

The data collected from the accelerometers from the Samsung Galaxy S smartphone.

Original description:
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones



## Input Data and Variable assignment

The input data contains the following data files to be assigned to a variable:
<ul>
  <li>featuresTrain stores data from X_train.txt</li>
  <li>activityTrain stores data from y_train.txt</li>
  <li>subjectTrain stores data Data from subject_train.txt</li>
  <li>featuresTest stores data from X_test.txt</li>
  <li>activityTest stores data from y_test.txt</li>
  <li>subjectTest stores data from subject_test.txt</li>
  <li>featureNames stores data from features.txt</li>
  <li>activityLabels stores data from activity_labels.txt</li>
  <li>subject is the combination of subjects in training and test data</li>
  <li>activity is the combination of activities in training and test data</li>
  <li>features is the combination of feature test and training data</li>
  <li>featureNames is the features properties</li>
  <li>CompleteData is the combination of features, activity and subject</li>
  <li>Index columns that contains std or mean ignoring case sensitive and assign to requiredColumns</li>
  <li>create extractedData from columns from requiredColumns</li>
  <li>Update descriptive names for activity for Activity column in extractedData</li>
  <li>Acronyms in extractedData like 'Mag', 'f' and 't' being replaced with descriptive labels 'Magnitude', 'Frequency' and 'Time'</li>
  <li>tidyData is created based on activity and subjects from extractedData</li>
  <li>output data from tidyData was written to a file named Tidy.txt</li>
</ul>



## Output File

Tidy.txt is a a space-delimited value file with header containing variable names.
The data in the files are values of mean and standard deviation.
