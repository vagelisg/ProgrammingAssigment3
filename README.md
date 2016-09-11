# ProgrammingAssigment3
# make 3 variables withe the file potitions for training data
train_file<-"course3/UCI HAR Dataset/train/X_train.txt"
trainy_file<-"course3/UCI HAR Dataset/train/y_train.txt"
trainbub_file<-"course3/UCI HAR Dataset/train/subject_train.txt"
#Training data are the data in X_train file
traindata<-read.table(train_file)
#Trainingydata are the data in Y training file
trainydata<-read.table(trainy_file)
#Trainingsubdata are the data of the subject training file.
trainsubdata<-read.table(trainbub_file)
#I cbind the training Y data   and the training subject data to the Trainingdata 
traindata <- cbind(traindata,trainydata)
traindata <- cbind(traindata,trainsubdata)
# I Do the same for the test data
test_file<-"course3/UCI HAR Dataset/test/X_test.txt"
testy_file<-"course3/UCI HAR Dataset/test/y_test.txt"
testsub_file<-"course3/UCI HAR Dataset/test/subject_test.txt"
testdata<-read.table(test_file)
testydata<-read.table(testy_file)
testsubdata<-read.table(testsub_file)
testdata <- cbind(testdata,testydata)
testdata <- cbind(testdata,testsubdata)

# I read the features file for names of the previus data
head_file<-"course3/UCI HAR Dataset/features.txt"
namehead<-read.table(lfile)
# I add a row for the activities
newrow <- data.frame("562","Activity")
names(newrow)<-names(namehead)
namehead<-rbind(namehead,newrow)
#And a row for the subject
newrow <- data.frame("563","Subject")
names(newrow)<-names(namehead)
namehead<-rbind(namehead,newrow)
#I put the names of the columns to the headname variable
headname<-namehead$V2
#I make them lowercase
headname<-tolower(headname)

colnames(traindata)<-headname
colnames(testdata)<-headname

#And I bind by rows the two tables (training and tests)
mydata<-rbind(testdata,traindata)
#I find the columns whit the "mean()" and "std()" and i put them to a variable inordr
inordr<-sort(c(grep("mean\\(\\)",headname),grep("std\\(\\)",headname)))
#The variable lastdata has the inrdr columns plus the last two, activity and subject
lastdata<-mydata[c(562,563,inordr)]
#I rename the activity from numeric to descriptive value
lastdata$activity[lastdata$activity==1]<-"WALKING"
lastdata$activity[lastdata$activity==2]<-"WALKING_UPSTAIRS"
lastdata$activity[lastdata$activity==3]<-"WALKING_DOWNSTAIRS"
lastdata$activity[lastdata$activity==4]<-"SITTING"
lastdata$activity[lastdata$activity==5]<-"STANDING"
lastdata$activity[lastdata$activity==6]<-"LAYING"
#Finaly I create the aggregate table aggdata by activity and subject
aggdata<-aggregate(lastdata[,3:68],list(lastdata$activity,lastdata$subject),mean)
