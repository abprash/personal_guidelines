##Python

### Major Data structures in Python

### References -> [here](https://docs.python.org/3/tutorial/datastructures.html)

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

### General useful Python BIFs
* BIF - Built In Functions
* locals() = has a set of all variable names used in the current scope
* str(x) = takes in an object as parameter to cast into a string. Usually used for printing anything like numbers, lists etc.
* int(x) = will convert x into an integer. 
* print() = print("Something",file="file.txt") then, it will send contents to file.txt.
* "string".strip() - its like java's trim(). will cut leading and trailing white spaces.
* "string".split(",") - just like Java's split().

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

```
f.write("whatever we want")
```

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

4. A very standard way of working with files is

```
try:
	file_handle = open("file.txt","w")
	data = file_handle.readlines()
except IOError as err:
	print("Something happened " + str(err))
finally:
	if file_handle in locals():
		file_handle.close()
```

An alternate way of doing it is the keyword - with
* with - provides a uniform way of working with context managers (here file is a context).
* Similar to the RAII idiom - Resource Acquired In Initiliazation. or CADRE - Constructor Acquires and Destructor Releases.
* Point being, with keyword provides a context for using a resource. It is guaranteed to be acquired as long as we are in the with block and will be released after leaving it. NO MATTER IF WE LEAVE THE SECTION VIA AN EXCEPTION OR NORMAL EXECUTION. So with **simplifies exception handling by taking care of common preparation and cleanup tasks by the use of so called context managers**.
* So, we need not need an explicit finally statement if we are using with keyword.
* So, in a way it is much cleaner.
* open() also does the same like a context manager.
* **with** - takes advantage of Python's context management and takes care
> A much cleaner way would be
```
try:
	with open("file.txt","r") as fh:
		content = fh.readlines()
		print(content)
except IOError as err:
	print("Error :"+str(err))
```

### Persistence

* Its too much hassle if we have to always store data in files, then implement custom methods to read it. Its not worth it, because if the file format changes then the code should also change. Its brittle.
* So python gives you the PICKLE. (Analogous to serialized Java objects)
* Pickled objects can be persisted in files, and then read back into programs when needed.
* Pickle uses a custom binary format for pickling called protocol. So, the file will be looking like gobbledygook.
* example
```
import pickle

list = []
#some more operations on the list so that there is now lots of elements in the list

#lets dump into a pickle first
with open("file1.txt","wb") as fh1:
	pickle.dump(list, fh1)
	
#then load from it
with open("file1.txt","rb") as fh:
	list_fromfile = fh.load()

#then print it
print(list_fromfile)


```

### Sorting
* Python provides two methods for sorting. In-place sorting and copied sorting.
```
list = [1,2,5,1,4,2,88,2]

list.sort() # will do an in place sorting

list2 = sorted(list) #will do a copy of the data, and return the sorted list

```


### List Comprehensions
* Short hand way of applying some functions on all list elements
* or copying elements from one to another list after some mutators
* List comprehension is preferred when we have to do a simple operation which can usually be expressed in one line.
* Its python's way of providing support for some functional programming concepts

> example of list comprehensions
```
x_list = [1,2,3]
new_list = [x**2 for x in x_list]
```


### Dictionaries
* Hash, mapping, associative arrays - its all dictionaries.
* When you have data with some structure, its always better to use a dict.
> dict() or {}

###Sets
* Chief goal : Eliminate duplicates.
> set()


### Object Oriented Python (Oops Object Oriented Programming with Python)
