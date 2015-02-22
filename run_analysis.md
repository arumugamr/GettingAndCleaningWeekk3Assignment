# url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip" #download zip and manually extract contents.

labelfile <- "C:\\Users\\RamSai\\Documents\\GettingandCleaningData\\Weekk3Assignment\\UCI HAR Dataset\\activity_labels.txt"
xtestfile <- "C:\\Users\\RamSai\\Documents\\GettingandCleaningData\\Weekk3Assignment\\UCI HAR Dataset\\test\\X_test.txt"
featuresfile <- "C:\\Users\\RamSai\\Documents\\GettingandCleaningData\\Weekk3Assignment\\UCI HAR Dataset\\features.txt"
ytestfile <- "C:\\Users\\RamSai\\Documents\\GettingandCleaningData\\Weekk3Assignment\\UCI HAR Dataset\\/test/y_test.txt"
subjecttestfile <- "C:\\Users\\RamSai\\Documents\\GettingandCleaningData\\Weekk3Assignment\\UCI HAR Dataset\\test\\subject_test.txt"

#import activity lables and rename the columns
activity_labels <- read.table(labelfile, sep="") 
colnames(activity_labels) <-c("class", "activity")

#import subject test identifiers and rename the columns
subjecttest <- read.table(subjecttestfile, sep="")
colnames(subjecttest) <- c("subject")

#import x test file and the features file. assign the features file as the names of the xtest file
xtest <- read.table(xtestfile, sep="") 
features <- read.table(featuresfile, sep="")
features <- as.character(t(features[,2]))
features <- gsub("-", "", features)
features <- gsub("\\(", "", features)
features <- gsub(")", "", features)
colnames(xtest) <- c(features)

#import y test file and rename column
ytest <- read.table(ytestfile, sep="") 
colnames(ytest) <- c("class")

# combine all test data into single dataframe
test <- cbind(subjecttest, ytest, xtest)

#Remove the importing objects that are no longer needed from the environment
rm(xtestfile, ytestfile, subjecttestfile)
rm(subjecttest, xtest, ytest)

xtrainfile <- "C:\\Users\\RamSai\\Documents\\GettingandCleaningData\\Weekk3Assignment\\UCI HAR Dataset\\train\\X_train.txt"
ytrainfile <- "C:\\Users\\RamSai\\Documents\\GettingandCleaningData\\Weekk3Assignment\\UCI HAR Dataset\\train\\y_train.txt"
subjecttrainfile <- "C:\\Users\\RamSai\\Documents\\GettingandCleaningData\\Weekk3Assignment\\UCI HAR Dataset\\train\\subject_train.txt"

#import subject train identifiers and rename the columns
subjecttrain <- read.table(subjecttrainfile, sep="")
colnames(subjecttrain) <- c("subject")

#import x test file and the features file. assign the features file as the names of the xtest file
xtrain <- read.table(xtrainfile, sep="") 
colnames(xtrain) <- c(features)

#import y test file and rename column
ytrain <- read.table(ytrainfile, sep="") 
colnames(ytrain) <- c("class")

# combine all test data into single dataframe
train <- cbind(subjecttrain, ytrain, xtrain)

#Remove the importing objects that are no longer needed from the environment
rm(xtrainfile, ytrainfile, subjecttrainfile)
rm(subjecttrain, xtrain, ytrain)

data <- rbind(test, train)
rm(test, train, features, featuresfile, labelfile, testfeatures, testfeaturesfile)

#replace all the activity data with a discriptive name from the activity_labels data.frame
data$class <- as.factor(data$class) #convert to factors
y <- activity_labels$activity
data$class <- y[data$class]
names(data) <- sub("class", "activity" , names(data))


#subset the columns that contain mean or std in the name.
means <- grep("mean", names(data))
std <- grep("std", names(data))
tidydata <- data[, c(1, 2, means, std)]
rm(means, std, y, activity_labels, data)

library(data.table)  #load the data.table package
# library("plyr") #load the plyr package
summarycolumns <- names(tidydata)
summarycolumns <- summarycolumns[3:81]
tidysummary <- data.table(tidydata)
attach(tidysummary)
setkey(tidysummary, subject)
summary <- tidysummary[, lapply(.SD,mean), by=list(subject,activity)]
detach(tidysummary)
write.table(summary, "tidysummary.txt", row.names=F)