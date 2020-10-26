After setting the correct pathway for the file via file.path()
Read .txt file into R with read.table()
Rename columns for the datasets you have and then merge them(training set, test set and subject)
Into together with cbind() 
By using grepl(), subset your data as by the column names those including mean and standart deviation measurements
Add descriptions to activities column while using merge() and setting by="activityId", all.x=T
Downloading reshape2 package to melt your data 
And then rehaping it with dcast(), while adding mean values into it
Lastly save your tidy data in a ".txt" file with write.table () used


