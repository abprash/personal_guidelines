#Hadoop MapReduce Basics
## Credits - Udacity - [Intro to Hadoop and MapReduce](https://classroom.udacity.com/courses/ud617).
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


## Basic working of Mappers and Reducers
* We can see how they work as shown below by the pictures.
1. The initial data :
![How the input data looks for the Hadoop job][input]

[input]: input_for_hdfs.png "How the input data looks for the Hadoop job"

2. The mappers step
![Mappers step][mapper]

[mapper]: hadoop_mappers.png "How the mapper step looks like"

3. The final reducer step
![How the Reducer step looks like][reducer]

[reducer]: hadoop_mappers_reducers.png "How the Reducer step looks like"

