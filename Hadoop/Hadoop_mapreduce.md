# Hadoop MapReduce Basics
## Credits - Udacity - [Intro to Hadoop and MapReduce](https://www.udacity.com/course/intro-to-hadoop-and-mapreduce--ud617).
* 3Vs of big data - Variety, velocity and Volume
* Files are stored in something called HDFS.
* A hadoop mapreduce cluster will be having two different kinds of nodes -
	* Name node and
	* Data node
* Data node
	* This is actually a daemon running on each physical or virtual machine. This takes care of the interaction between itself and the name node. 
	* These are the ones that contain the replicated data.
	* the big 1TB file is usually split into chunks of 64MB and stored in each data node.
	* The smallest unit is 64MB.
	* Data redundancy - Hadoop replicates each block 3 times. And each block is on 3 different machines at random. So, if a single node fails, there is no problem. 
* Name node
	* Name node typically contains the information where each and every replicated block is stored. So, if this node goes down, then everything goes down. If it dies, the entire cluster will be inaccessible.
	* Name node stores the mapping of the data nodes, as metadata.
	* This is a Single Point of Failure if there is only one Name node. So, its a good practice to have a replicated name node.
	* There are two ways of duplicating the name node, 
		* It can be done through having a copy of the metadata on an NFS (Network File System)
		* Or, a better alternative is to have a standby name node. So, it will take over if the main name node goes down.
## Hadoop File system basic commands
* They mirror standard UNIX commands.
* They are all UNIX after `hadoop fs -`
* All HDFS commands start with `hadoop fs`.
* Uploading a file to Hadoop FS - `hadoop fs -put <path of file>`
* Viewing contents on HDFS - `hadoop fs -ls`
* Remove a file - `hadoop fs -rm <path of file>`
* To retrieve the results from the HDFS to the local file system - `hadoop fs -ls <path of file>`


## Basic working of Mappers and Reducers
* We can see how they work as shown below.
1. The initial data :
![How the input data looks for the Hadoop job][input]

[input]: input_for_hdfs.png "How the input data looks for the Hadoop job"

----

2. The mappers step
![Mappers step][mapper]

[mapper]: hadoop_mappers.png "How the mapper step looks like"

---

3. The final reducer step
![How the Reducer step looks like][reducer]

[reducer]: hadoop_mappers_reducers.png "How the Reducer step looks like"

---
* So, how it works is
	* There is a Map phase. This will be having a set of mappers who will work on each partition of the huge input data and consolidate the key value pairs of their results.
	* Then there is a Shuffle and Sort phase. Where the intermediate records are sent in no particular order to the reducers.
	* Now depending on the kind of operation, there are also some elements called combiners - these are used to combine the results obtained locally into some more efficient representation instead of sending the bulk of the calculated data over the network. (ie mapped data)
	* The reducers will now, reduce the consolidated intermediate records, into total key value pairs and output the result.


## A little deeper working of Hadoop and Mapreduce
* There are daemons running on each name and data node. The name node will run a job tracker, whereas a data node will run the task tracker.
* The job of the job tracker is to manage jobs.
* The task tracker is the dameon which commonly runs the mapper tasks. This is usually local to the data, so as to prevent the network transfer latency. So the task tracker will work on its own local data.
* We can also see what is going on with the jobs submitted, by visiting the hadoop job tracker local page.


## Other applications of Hadoop
* Can be used to aggregate huge amount of data.
* Can be used to process web server logs.

## MapReduce Design Patterns
* Design patterns for solving common problems.
* The ones we will be discussing for now include
	* Summarizing patterns
		* Counting problems
		* Finding Min and Max
		* Finding statistically relevant values like mean/ median and/ or mode
	* Filtering patterns
		* Finding top-N lists
		* Sampling
			* Sampling filters
			* Random sampling
		* Filters
			* Simple filters
			* Bloom filters - These are probabilitic more efficient filters
	* Structural patterns
		* Combining data sets


