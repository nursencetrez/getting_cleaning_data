
### README

getwd()
setwd("/Users/nursencetrez/Desktop/UCI HAR Dataset/")
pathway = file.path("/Users/nursencetrez/Desktop/", "UCI HAR Dataset")
# create a file which has the 28 file names
files = list.files(pathway, recursive=TRUE)
# show the files
files
# reading test & train files
x_train <- read.table(file.path(pathway, "train", "X_train.txt"), header = F)
y_train <- read.table(file.path(pathway, "train", "Y_train.txt"), header = F)
subject_train <- read.table(file.path(pathway, "train", "subject_train.txt"),  header=F)

x_test <- read.table(file.path(pathway, "test", "X_test.txt"), header = F)
y_test <- read.table(file.path(pathway, "test", "Y_test.txt"), header = F)
subject_test <- read.table(file.path(pathway, "test", "subject_test.txt"), header=F)

# reading features & activity labels files
features <- read.table(file.path(pathway, "features.txt"), header = F)
activity_labels <- read.table(file.path(pathway, "activity_labels.txt"), header=F)

# renaming variables
colnames(x_train) <-  features[,2]
colnames(y_train) <-  "activityId"
colnames(subject_train) <-  "subjectId"

colnames(x_test) <-  features[,2]
colnames(y_test) <-  "activityId"
colnames(subject_test) <-  "subjectId"
colnames(activity_labels) <- c("activityId", "activityName")

# merging datasets into together
mergedDTr <- cbind(y_train, subject_train, x_train)
mergedDTs <- cbind(y_test, subject_test, x_test )

dim(mergedDTr); dim(mergedDTs)

Allmerged <- rbind(mergedDTr, mergedDTs)

# subsetting acc. to mean() & std()
meanstd <- grepl("activityId", colnames(Allmerged)) | grepl("subjectId", colnames(Allmerged)) | grepl("mean\\(\\)", colnames(Allmerged)) | grepl("std\\(\\)", colnames(Allmerged))
table(meanstd)
subset_meanstd <- Allmerged[, meanstd == "TRUE"]

# descriptions of activities
df1 <- merge(subset_meanstd, activity_labels, by="activityId", all.x = T)

# shaping it into a tidy dataset
install.packages("reshape2")
library(reshape2)
meltedData <- melt(df1, id = c("subjectId", "activityId"))
meltedData$value <- as.numeric(meltedData$value)
tidyData <- dcast(meltedData, subjectId ~ variable, mean)
write.table(tidyData, file="tidyData.txt", row.names = FALSE)

