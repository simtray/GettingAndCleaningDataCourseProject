Getting and Cleaning Data Course Project
=========================================

The zip file getdata-projectfiles-UCI HAR Dataset.zip is downloaded and then extracted in the working directory. This creates a UCI HAR Dataset folder in the working directory that contains test and train subfolders and four files - activity_labels.txt, features.txt, features_info.txt, and README.txt. The test subfolder contains Inertial Signals subfolder and three files - subject_test.txt, X_test.txt, and y_test.txt. The train subfolder contains Inertial Signals subfolder and three files - subject_train.txt, X_train.txt, y_train.txt.

The R script file run_analysis.R contains code that does the following:

### Merges the training and test sets

1. Reads the test data files subject_test.txt, X_test.txt, and y_test.txt using `read.table()`. The row number of the three test datasets is equal - 2947 rows.<br>
2. Combines the columns of the three test datasets using `cbind()` and creates testSet with total of 563 columns (1+1+561).<br>
3. Reads the train data files subject_train.txt, X_train.txt, and y_train.txt using `read.table()`. The row number of the three train datasets is equal - 7352 rows.<br>
4. Combines the columns of the three train datasets using `cbind()` and creates trainSet with total of 563 columns (1+1+561).<br>
5. Combine the rows of the test set and the train set using `rbind()`. This gives a total of 10299 rows (2947+7352).

### Extracts the measurements on the mean and standard deviation for each measurement

1. Reads the features.txt file to get the names of the columns.<br>
2. Create a vector with the column names. First two column names will be "subject", and "activity". For the rest will use the data from the second column of features.<br>
3. Set the names of the columns to be the created vector using `names()`.<br>
4. Create a vector with column names that contain "subject" or "activity" or "mean()" or "std()" using `grep()` and regular expression "subject|activity|-mean\\(\\)|-std\\(\\)".<br>
5. Extract only the data that is in the columns with column names in this vector - 68 columns.

### Gives descriptive activity names to the activities in the data set

1. From the activity_labels.txt file we can get the activity lable for each numeric value ( 1 - WALKING, 2 - WALKING_UPSTAIRS, etc.).<br>
2. Using `factor()` function on the activity column we can convert the numeric values to the activity labels.

### Appropriately labels the data set with descriptive variable names

Even though in Week 3 "Editing text variables" lecture it says to use all lower cases IF POSSIBLE, I think that in this case the camel case is more readable, so I didn't change it to lower case. I also don't think that replacing "Acc" with more descriptive "Accelerometer" or "t" with "time" would be better, this would make the names unnecessary long. The meaning of the abbreviations will be listed in the Code Book.

1. Using `gsub()` removed "()" and replaced "-" with "_", so "tBodyAcc-mean()-X" became "tBodyAcc_mean_X".

### Creates a tidy data set with the average of each variable for each activity and each subject

1. Using `aggregate()` create a data set with the average of each variable for each activity and each subject. It has 180 rows (30 subjects * 6 activities) and 68 columns("subject", "activity", and all the mean and standard deviation features). After reading Hadley Wickham's paper on Tidy Data http://vita.had.co.nz/papers/tidy-data.pdf (listed in the week 3 Reshaping data lecture) and considering that this dataset has four variables - subject, activity, feature and average. To make it tidy, we can turn columns into rows. <br>
2. Using `melt()` from the reshape2 package create a tidy data set with ID variables "subject" and "activity" that identify individual rows of data, variable column with the features, and value column with the averages. It has 11880 rows (30 subjects * 6 activities * 66 features) and 4 columns ("subject", "activity", "feature", and "average").<br>
3. Write the tidy data set to a tidyDataSet.txt file.

Can read the text file with the tidy data set into R using: `read.table("tidyDataSet.txt", header = TRUE)`



    

