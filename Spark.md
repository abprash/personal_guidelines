# Spark Basics

## What is it?
* General purpose compute engine - which allows batch, streaming and real time jobs on the cluster.
* Spark is written in Scala.
* It does not force you to chunk up your job into different parts. You can directly put the entire workload into memory and Spark will take care of it.
* Has a much better API than Hadoop.
* RDDs -> Resilient Distributed Datasets. These are understood easily like the objects in the Spark environment. They are resilient because they use lineage, which can compute themselves using prior information and retrieve the lost correct value before failure.
* Transformations -> What operations you do on RDDs in order to get other results. These include things like, opening a file and reading them into an RDD, or filtering an RDD on some criteria etc.
* Action -> It is something like a direct answer which the system will provide you. eg. count.
* Spark also does lazy evaluation, they are not loaded into the memory until the required action is to be done.
* Data Sources - It can read data from multiple sources - Local file system, S3, HDFS, HBase.

## Example of how it works..
1. Simple operations to create Spark environment objects. Or, creation of RDDs. 
```
txt = sc.textFile("foo.txt")
```

2. Now, we transform the RDD to something we actually want.
```
linesWithSpark = txt.filter(lambda line : "Spark" inline)
```
Note that Spark will not even load the data into the memory at this point.

3. Now, we need to do some action to get the final result
```
linesWithSpark.count() // to display the final count
linesWithSpark.first() // to display the first line in the result.
```


## RDDs vs Distributed Shared Memory (DSM)
1. Writes - RDD writes are coarse grained., DSM writes are fine grained.
2. Recovery - RDDs use lineage to recompute the latest values, and DSMs use checkpoints.
3. 

## Chief differences between Spark and Hadoop
* It is blazingly fast when compared to Hadoop.
* Works entirely from main memory. If you have machines with sufficient memory sizes, then they should run Spark code well. But if not, then Spark will suffer. Hadoop on the other hand, persists everything on disks.
* The APIs of Spark are much better, when compared to the Hadoop API.
* Spark is well suited for real time processing as well as graph processing. Hadoop on the other hand is much more suited for batch processing. 
* Both Spark and Hadoop are good at handling failures. Hadoop is a bit more tolerant than Spark. Why?? Because it operates entirely off the main memory. 
* Security - Spark is still in its infancy, but Hadoop is much better in its security features.



## Credits
* [This](https://www.youtube.com/watch?v=KzFe4T0PwQ8)
* [This also](https://www.youtube.com/watch?v=SxAxAhn-BDU)