##Python

### Major Data structures in Python

### References -> (here) [https://docs.python.org/3/tutorial/datastructures.html]

* There is **no new** keyword in Python. So do not use it while declaring anything

1. Lists  
> [] (or) list()
2. Tuples  
> () (or) tuple()
	1. Tuples are immutable. They cannot be changed after creation.
	2. They are almost similar to records. Used for associating one value to another.
3. Sets   
> set()
4. Dictionaries 
> {} or dict()
5. Queues and stacks -> Lists themselves can be used like stacks and queues. Lists have those methods like pop() and append(). But lists are **NOT EFFICIENT** for this purpose, because for every pop, we have to shift the elements by one. So use Collections -> deque
	1. Queues -> popleft(), and append() [**NOT EFFICIENT**]
		1. Queues are done with "from collections import deque",
		deque.popleft() and deque.append("new one")
	2. Stacks -> pop() and append(). They are sufficiently handled by lists itself. because lists have append() and pop().



### Exception handling

```
try:
	#do something here
	#more
except:
	#code to run upon exception
```
* A much more elegant way of doing things

```
try:
	#stuff
except <Specific exception type like IOError, ValueError>:
	#runs upon exception
finally:
	#runs anyway... typically used to store cleanup code like closing files etc.
```


### File handling
1. Opening files f = open("filename.extension","open mode")
eg.
```
 f = open("file.txt","r") # will open file in read mode
 f = open("file.txt", "w") # in write mode
 f = open("file.txt","rw") #we can do both also
 ```
2. File writing

` f.write("whatever we want")`

3. Reading from file
* We typically read line by line

```
for line in f:
	#do more stuff
	print(line)
```
OR
```
all_content = f.readlines()
```
