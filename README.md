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

max_roll_belt           max_picth_belt            min_roll_belt 

11                       12                       13 

min_pitch_belt      amplitude_roll_belt     amplitude_pitch_belt 

14                       15                       16 

var_total_accel_belt            avg_roll_belt         stddev_roll_belt 

17                       18                       19 

var_roll_belt           avg_pitch_belt        stddev_pitch_belt 

20                       21                       22 

var_pitch_belt             avg_yaw_belt          stddev_yaw_belt 

23                       24                       25 

var_yaw_belt            var_accel_arm            max_picth_arm 

26                       40                       50 

max_yaw_arm              min_yaw_arm        amplitude_yaw_arm 

51                       52                       53 

max_roll_dumbbell       max_picth_dumbbell        min_roll_dumbbell 

57                       58                       59 

min_pitch_dumbbell  amplitude_roll_dumbbell amplitude_pitch_dumbbell 

60                       61                       62 

var_accel_dumbbell        avg_roll_dumbbell     stddev_roll_dumbbell 

64                       65                       66 

var_roll_dumbbell       avg_pitch_dumbbell    stddev_pitch_dumbbell 

67                       68                       69 

var_pitch_dumbbell         avg_yaw_dumbbell      stddev_yaw_dumbbell 

70                       71                       72 

var_yaw_dumbbell        max_picth_forearm        min_pitch_forearm 

73                       86                       87 

amplitude_pitch_forearm        var_accel_forearm 

88                       90

The columns with NA values can be dealt with imputation. However, there are several strategies. Therefore, the columns are removed. Besides, X, user_name and time columns also are removed.Other preprocessing steps are performed, converting string to factors.

training <- training[,colSums(is.na(training))==0]

testing <- testing[,colSums(is.na(testing))==0]

training$X <- NULL

training$user_name <- NULL

training$raw_timestamp_part_1<-NULL

training$raw_timestamp_part_2<-NULL

training$cvtd_timestamp<-NULL

training$num_window<-NULL

testing$X <- NULL

testing$user_name <- NULL

testing$raw_timestamp_part_1<-NULL

testing$raw_timestamp_part_2<-NULL

testing$cvtd_timestamp<-NULL

testing$num_window<-NULL

training<-as.data.frame(unclass(training),stringsAsFactors = TRUE)

# Experimental Setup and Results

For the experiments, we use an random forest classifier. The training process is performed using a ten fold cross-validation with 50 trees.

fitControl<-trainControl(method="cv", number = 10, allowParallel=TRUE)

We train the classifier

modelRF<-train(classe ~ ., data=training, method="rf", trControl=fitControl, ntree=50)

We compute the accuracy using the training set

predicttra<-predict(modelRF,training)

confusionMatrix(predicttra,training$classe)

 Confusion Matrix and Statistics



Reference

Prediction    A    B    C    D    E

A 5580    0    0    0    0

B    0 3797    0    0    0

C    0    0 3422    0    0

D    0    0    0 3216    0

E    0    0    0    0 3607



Overall Statistics



Accuracy : 1          

95% CI : (0.9998, 1)

No Information Rate : 0.2844     

P-Value [Acc > NIR] : < 2.2e-16  



Kappa : 1          

Mcnemar's Test P-Value : NA         

Statistics by Class:

Class: A Class: B Class: C Class: D Class: E

Sensitivity            1.0000   1.0000   1.0000   1.0000   1.0000

Specificity            1.0000   1.0000   1.0000   1.0000   1.0000

Pos Pred Value         1.0000   1.0000   1.0000   1.0000   1.0000

Neg Pred Value         1.0000   1.0000   1.0000   1.0000   1.0000

Prevalence             0.2844   0.1935   0.1744   0.1639   0.1838

Detection Rate         0.2844   0.1935   0.1744   0.1639   0.1838

Detection Prevalence   0.2844   0.1935   0.1744   0.1639   0.1838

Balanced Accuracy      1.0000   1.0000   1.0000   1.0000   1.0000
