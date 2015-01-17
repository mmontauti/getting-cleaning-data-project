<body>
<p>################################################################################################</p>

<h3>Coursera Getting and Cleaning Data Course Project</h3>

<p>Mauro Montauti</p>

<h2>Description:</h2>

<h1>This script will perform the following steps:</h1>

<ol>
<li>Download and extract files from zip</li>
<li>Read Activity Files</li>
<li>Read Subject Files</li>
<li>Read Features Files </li>
<li>rBinding tables by rows</li>
<li>Naming Variables</li>
<li>cBinding data</li>
<li>Subsetting name of feature by the measurement of the mean and std</li>
<li>Subsetting data by selected features names</li>
<li>Reading descriptive activity names</li>
<li>label dataset</li>
<li>Creating dataset</li>
</ol>

<p>################################################################################################</p>

<h2>Download and extract files from zip</h2>

<p>if(!file.exists(&ldquo;./data&rdquo;)){dir.create(&ldquo;./data&rdquo;)}
fileUrl &lt;- &ldquo;<a href="https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip">https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip</a>&rdquo;
download.file(fileUrl,destfile=&ldquo;./data/Dataset.zip&rdquo;,method=&ldquo;curl&rdquo;)</p>

<p>unzip(zipfile=&ldquo;./data/Dataset.zip&rdquo;,exdir=&ldquo;./data&rdquo;)</p>

<p>path_rf &lt;- file.path(&ldquo;./data&rdquo; , &ldquo;UCI HAR Dataset&rdquo;)
files&lt;-list.files(path_rf, recursive=TRUE)
files</p>

<h2>Read Activity Files</h2>

<p>dataActivityTest  &lt;- read.table(file.path(path_rf, &ldquo;test&rdquo; , &ldquo;Y_test.txt&rdquo; ),header = FALSE)
dataActivityTrain &lt;- read.table(file.path(path_rf, &ldquo;train&rdquo;, &ldquo;Y_train.txt&rdquo;),header = FALSE)</p>

<h2>Read Subject Files</h2>

<p>dataSubjectTrain &lt;- read.table(file.path(path_rf, &ldquo;train&rdquo;, &ldquo;subject_train.txt&rdquo;),header = FALSE)
dataSubjectTest  &lt;- read.table(file.path(path_rf, &ldquo;test&rdquo; , &ldquo;subject_test.txt&rdquo;),header = FALSE)</p>

<h2>Read Features Files</h2>

<p>dataFeaturesTest  &lt;- read.table(file.path(path_rf, &ldquo;test&rdquo; , &ldquo;X_test.txt&rdquo; ),header = FALSE)
dataFeaturesTrain &lt;- read.table(file.path(path_rf, &ldquo;train&rdquo;, &ldquo;X_train.txt&rdquo;),header = FALSE)</p>

<h2>rBinding tables by rows</h2>

<p>dataSubject &lt;- rbind(dataSubjectTrain, dataSubjectTest)
dataActivity&lt;- rbind(dataActivityTrain, dataActivityTest)
dataFeatures&lt;- rbind(dataFeaturesTrain, dataFeaturesTest)</p>

<h2>Naming Variables</h2>

<p>names(dataSubject)&lt;-c(&ldquo;subject&rdquo;)
names(dataActivity)&lt;- c(&ldquo;activity&rdquo;)
dataFeaturesNames &lt;- read.table(file.path(path_rf, &ldquo;features.txt&rdquo;),head=FALSE)
names(dataFeatures)&lt;- dataFeaturesNames$V2</p>

<h2>cBinding data</h2>

<p>dataCombine &lt;- cbind(dataSubject, dataActivity)
Data &lt;- cbind(dataFeatures, dataCombine)</p>

<h2>Subsetting name of feature by the measurement of the mean and std</h2>

<p>subdataFeaturesNames&lt;-dataFeaturesNames$V2[grep(&ldquo;mean\(\)|std\(\)&rdquo;, dataFeaturesNames$V2)]</p>

<h2>Subsetting data by selected features names</h2>

<p>selectedNames&lt;-c(as.character(subdataFeaturesNames), &ldquo;subject&rdquo;, &ldquo;activity&rdquo; )
Data&lt;-subset(Data,select=selectedNames)</p>

<h2>Reading descriptive activity names</h2>

<p>activityLabels &lt;- read.table(file.path(path_rf, &ldquo;activity_labels.txt&rdquo;),header = FALSE)</p>

<h2>label dataset</h2>

<p>names(Data)&lt;-gsub(&ldquo;<sup>t&rdquo;,</sup> &ldquo;time&rdquo;, names(Data))
names(Data)&lt;-gsub(&ldquo;<sup>f&rdquo;,</sup> &ldquo;frequency&rdquo;, names(Data))
names(Data)&lt;-gsub(&ldquo;Acc&rdquo;, &ldquo;Accelerometer&rdquo;, names(Data))
names(Data)&lt;-gsub(&ldquo;Gyro&rdquo;, &ldquo;Gyroscope&rdquo;, names(Data))
names(Data)&lt;-gsub(&ldquo;Mag&rdquo;, &ldquo;Magnitude&rdquo;, names(Data))
names(Data)&lt;-gsub(&ldquo;BodyBody&rdquo;, &ldquo;Body&rdquo;, names(Data))</p>

<h2>Creating dataset</h2>

<p>library(plyr);
Data2&lt;-aggregate(. ~subject + activity, Data, mean)
Data2&lt;-Data2[order(Data2$subject,Data2$activity),]
write.table(Data2, file = &ldquo;tidydataset.txt&rdquo;,row.name=FALSE)</p>

</body>

</html>
