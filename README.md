# GettingandCleaningData

> The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project.You will be required to submit:
>
1. a tidy data set as described below
2. a link to a Github repository with your script for performing the analysis
3. codeBook.md that describes the variables, the data, and any work that you performed to clean up the data 
4. README.md that explains how all of the scripts work and how they are connected.  
>
> One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: 
> 
> http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

> Here are the data for the project: 

> https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
> 
> You should create one R script called run_analysis.R that does the following. 
> 
> 1. Merges the training and the test sets to create one data set.
> 2. Extracts only the measurements on the mean and standard deviation for each measurement.
> 3. Uses descriptive activity names to name the activities in the data set.
> 4. Appropriately labels the data set with descriptive activity names.
> 5. Creates a second, independent tidy data set with the average of each variable for each activity and each subject. 
> 
> Good luck!

# Code explanations
```javascript 
      ##Reading Features and ActivityLabels vector
      features <- read.csv("features.txt", sep = "", header = FALSE)[2]
      activities <- read.csv("activity_labels.txt", sep = "", header = FALSE)```

```javascript 
      ##Reading Sets
      testSet <- read.csv("test/X_test.txt", sep = "", header = FALSE)
      trainSet <- read.csv("train/X_train.txt", sep = "", header = FALSE)
      mergedSet <- rbind(testSet,trainSet)```        

```javascript 
      ##Reading Movement
      testMoves <- read.csv("test/Y_test.txt", sep = "", header = FALSE)
      trainMoves <- read.csv("train/Y_train.txt", sep = "", header = FALSE)
      mergedMoves <- rbind(testMoves, trainMoves)```

```javascript 
      ##Reading PersonID
      testPerson <- read.csv("test/subject_test.txt", sep = "", header = FALSE)
      trainPerson <- read.csv("train/subject_train.txt", sep = "", header = FALSE)
      mergedPerson <- rbind(testPerson, trainPerson)```
```javascript       
      ##Extracting columns which includes measurements
      names(mergedSet) <- features[ ,1]
      mergedSet <- mergedSet[ grepl("std|mean", names(mergedSet), ignore.case = TRUE) ] 
```javascript       
      #Descriptive ActivityName analysis
      mergedMoves <- merge(mergedMoves, activities, by.x = "V1", by.y = "V1")[2]
      mergedSet <- cbind(mergedPerson, mergedMoves, mergedSet)
      names(mergedSet)[1:2] <- c("PersonID", "Activities")```
      
```javascript       
      ##Tidying mergedSet
      group_by(mergedSet, PersonID, Activities) %>%
            summarise_each(funs(mean))```
      
      
