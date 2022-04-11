---
title: "Course Project For Predicting Personal Activity "
output: html_document
date: '2022-04-10'
---

## Introduction

Using records obtained from Jawbone Up, Nike FuelBand, and Fitbit, we will build a machine learning model to predict personal activity. The dataset contains several measurements, such as accelerometers on the belt, forearm, arm, and dumbbell.

This report is organized as follows: first, exploratory analysis is performed. Next, a data preprocessing step is applied to get a tidy dataset. Then, a machine learning model is built and tested. Finally, conclusions are presented.

\## Exploratory Analysis

This section performs several tasks until we get a tidy dataset. The steps are: 1) Download the training and testing datasets, 2) Perform an exploratory data analysis, and 3) Preprocess the dataset.

The training dataset is downloaded from <https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv>

The test dataset is downloaded from

<https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv>

Both can be get using the R-command download.file()
