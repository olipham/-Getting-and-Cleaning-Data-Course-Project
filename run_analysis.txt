setwd("/Users/oliviapham/Documents/Coursera/data_science")

library(dplyr)

## Read in tables
test <- read.table("UCI HAR Dataset/test/x_test.txt")
test_labels <- read.table("UCI HAR Dataset/test/y_test.txt")
test_id <- read.table("UCI HAR Dataset/test/subject_test.txt")

train <- read.table("UCI HAR Dataset/train/x_train.txt")
train_labels <- read.table("UCI HAR Dataset/train/y_train.txt")
train_id <- read.table("UCI HAR Dataset/train/subject_train.txt")

activity_labels <- read.table("UCI HAR Dataset/activity_labels.txt")
features <- read.table("UCI HAR Dataset/features.txt")

# Merge test data with subject id and activity id
test_df <- cbind(test_id, test_labels, test)

# Merge training data with subject id and activity id
train_df <- cbind(train_id, train_labels, train)

# Combine test and training data
all_df <- rbind(test_df, train_df)

# Rename variables with "features"
names(all_df) <- c("subject", "activity", as.character(features[,2]))

# Select columns with standard deviation and mean measures
select_col <- all_df[, c(1, 2, grep("[Mm]ean\\()|[Ss]td\\()", names(all_df)))]

# Rename activity code with activity description
act_desc <- merge(select_col, activity_labels, by.x = "activity", by.y = "V1", all = TRUE) %>%
  select(-activity) %>%
  rename(activity = V2) %>%
  select(subject, activity, everything())

# Clean variable names
test <- names(act_desc)
varNames <- gsub("\\(|\\)|\\-|,", "", test)
varNames <- gsub("mean", "Mean", varNames)
varNames <- gsub("std", "Std", varNames)
varNames <- gsub("gravity", "Gravity", varNames)

# Set new variable names
names(act_desc) <- varNames

# Group by subject and activity
tidy_df <- act_desc %>%
  group_by(subjectID, activity) %>%
  summarise(mean()) 

# Write tidy dataset to txt file
write.table(tidy_df, file = "tidy.txt", row.names = FALSE)
