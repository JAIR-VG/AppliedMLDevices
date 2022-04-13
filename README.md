# AppliedMLDevices
Course Project to predict personal activity from accelerometers on the belt, forearm, arm, and dumbell of 6 participants.

This report can be found in https://rpubs.com/vgarciaj/889816

# Introduction
Using records obtained from Jawbone Up, Nike FuelBand, and Fitbit, we will build a machine learning model to predict personal activity. The dataset contains several measurements, such as accelerometers on the belt, forearm, arm, and dumbbell. This report is organized as follows: first, exploratory analysis is performed. Next, a data preprocessing step is applied to get a tidy dataset. Then, a machine learning model is built and tested. Finally, conclusions are presented.

#Exploratory Analysis
This section performs several tasks until we get a tidy dataset. The steps are: 1) Download the training and testing datasets, 2) Perform an exploratory data analysis, and 3) Preprocess the dataset.

The training dataset is downloaded from https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

The test dataset is downloaded from

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

Both can be get using the R-command download.file()

Now, we load both datasets

training<-read.csv("pml-training.csv", header=TRUE, stringsAsFactors = FALSE)

testing <-read.csv("pml-testing.csv", header=TRUE, stringsAsFactors = FALSE)

We show the internal structure of the training set

str(training)

'data.frame':    19622 obs. of  160 variables:

$ X                       : int  1 2 3 4 5 6 7 8 9 10 ...

$ user_name               : chr  "carlitos" "carlitos" "carlitos" "carlitos" ...

$ raw_timestamp_part_1    : int  1323084231 1323084231 1323084231 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 ...

$ raw_timestamp_part_2    : int  788290 808298 820366 120339 196328 304277 368296 440390 484323 484434 ...

$ cvtd_timestamp          : chr  "05/12/2011 11:23" "05/12/2011 11:23" "05/12/2011 11:23" "05/12/2011 11:23" ...

$ new_window              : chr  "no" "no" "no" "no" ...

$ num_window              : int  11 11 11 12 12 12 12 12 12 12 ...

$ roll_belt               : num  1.41 1.41 1.42 1.48 1.48 1.45 1.42 1.42 1.43 1.45 ...

$ pitch_belt              : num  8.07 8.07 8.07 8.05 8.07 8.06 8.09 8.13 8.16 8.17 ...

$ yaw_belt                : num  -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 ...

$ total_accel_belt        : int  3 3 3 3 3 3 3 3 3 3 ...

$ kurtosis_roll_belt      : chr  "" "" "" "" ...

$ kurtosis_picth_belt     : chr  "" "" "" "" ...

$ kurtosis_yaw_belt       : chr  "" "" "" "" ...

$ skewness_roll_belt      : chr  "" "" "" "" ...

$ skewness_roll_belt.1    : chr  "" "" "" "" ...

$ skewness_yaw_belt       : chr  "" "" "" "" ...

$ max_roll_belt           : num  NA NA NA NA NA NA NA NA NA NA ...

$ max_picth_belt          : int  NA NA NA NA NA NA NA NA NA NA ...

[list output truncated]

The first preprocessing step, in the training and testing dataset is to remove all the variables that are zero variance predictors. Also, as can been observed there are several NA, DIV0 and blank values. The empty values and DIV0 can be seemed as NA, therefore, we set both values to NA. Besides, we compute the columns with NA values.

idxnzv <-nearZeroVar(training)

training<-training[,-idxnzv]

testing <-testing[,-idxnzv]

training[training == ""] <- NA

training[training == "#DIV/0!"] <- NA

which(colSums(is.na(training))>0)


