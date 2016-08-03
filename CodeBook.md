##Dataset Used

The data set downloaded from <https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>. 

##Input Data Set

##1. Merges the training and the test sets to create one data set.

The input data containts the following data files:

- `X_train.txt` contains variable for training.
- `y_train.txt` contains the activities in `X_train.txt`.
- `subject_train.txt` contains information on the subjects.
- `X_test.txt` contains variable for testing.
- `y_test.txt` contains the activities in `X_test.txt`.
- `subject_test.txt` contains information on the subjects.
- `activity_labels.txt` contains data on different activities.
- `features.txt` contains the name of the features in the data sets.

Columns were assigned with appropriate names and were merged into one data set.

##2. Extracts only the measurements on the mean and standard deviation for each measurement.

Measurements on the mean and standard deviation for each measurement was extracted and merged with selected names of features.

##3. Uses descriptive activity names to name the activities in the data set.

The descriptive activity names were used to replac the activity values in the data set.

##4. Appropriately labels the data set with descriptive variable names.

Data set was labeled with descriptive variable names.
- "^t" <- "time"
- "^f" <- "frequency"
- "Acc" <- "Accelerometer"
- "Gyro" <- "Gyroscope"
- "Mag" <- "Magnitude"
- "BodyBody" <- "Body"

##5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
Created a second independent tidy data set with the average of each variable for each activity and each subject.
