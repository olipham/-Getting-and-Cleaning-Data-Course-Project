# Getting-and-Cleaning-Data-Course-Project

This is the course project for the "Getting and Cleaning Data" Coursera course. 

Initial setup:
- Download the dataset and save to working directory:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
- Set the working directory in the "run_analysis.R" script.

The R script, "run_analysis.R", does the following:
- Read the test and training data into R dataframe:
    - test: subject_test.txt, X_test.txt, Y_test.txt
    - train: subject_train.txt, X_train.txt, Y_train.txt
- Read the activity and feature info into R dataframe
- Merge the test data to include subject and activity id. Merge the train data to include subject and activity id.
- Combine the test and train data into one dataframe.
- Relabel the column names with features description.
- Keep only those columns with mean and standard deviation measures ("mean()" and "std()").
- Relabel activity codes with activity description.
- Creates a tidy dataset that consists of the average (mean) value of each variable for each subject and activity pair.

The end result is shown in the file tidy.txt.
