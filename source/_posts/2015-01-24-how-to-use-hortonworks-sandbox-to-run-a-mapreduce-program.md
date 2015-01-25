---
layout: post
title: "How to use Hortonworks Sandbox to run a mapreduce program?"
date: 2015-01-24 20:00:09 -0500
comments: true
categories: 
---

In this tutorial, I will let you to know how to use a virtual machine based hadoop environment to run the mapreduce program.

The VM image that we use is the HDP 2.2 Sandbox. The example is also posted on the apache hadoop offical website, which is a word count program. 

First, let’s upload the input file to the file directory in the VM by using `scp` command.
	# upload: local -> remote
	scp local_file user@remote_host:remote_file
![](images/2015-01-24-how-to-use-hortonworks-sandbox-to-run-a-mapreduce-program/DraggedImage.png)

Now, we can check whether our input file has been uploaded into the sandbox, and put it into HDFS.
![](images/2015-01-24-how-to-use-hortonworks-sandbox-to-run-a-mapreduce-program/DraggedImage-1.png)
Using the `hadoop fs -put` can easily put the input file into HDFS.
![](images/2015-01-24-how-to-use-hortonworks-sandbox-to-run-a-mapreduce-program/DraggedImage-2.png)
Here, I also use `hadoop fs -ls` to check any specific HDFS directory. You may have already found that it quite similar to the Unix command `ls`. In fact, most of HDFS command map to the common Unix commands, so it may be familiar to the CLI users.

One more thing, if there is no such `/user/hadoop/hw3/input/` in HDFS, you may use `hadoop fs -mkdir` to create first.

Then, I will locate my mapreduce jar file and use `hadoop jar` command to execute the program.
	hadoop jar *YourJarFile* *DriverClassName* /your/input/file/location /your/output/file/location
![](images/2015-01-24-how-to-use-hortonworks-sandbox-to-run-a-mapreduce-program/DraggedImage-3.png)

When it finishes the job, you may check out the output within your HDFS by using `hadoop fs -cat`. The output file is named as  part-r-00000.
![](images/2015-01-24-how-to-use-hortonworks-sandbox-to-run-a-mapreduce-program/DraggedImage-4.png)

Last but not the least, it is also possible to retrieve the output file from HDFS to local file system by using `hadoop fs -get`.
![](images/2015-01-24-how-to-use-hortonworks-sandbox-to-run-a-mapreduce-program/DraggedImage-5.png)
