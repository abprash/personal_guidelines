# Concurrency in Java - Basics

* Concurrency is even possible in systems without multiple cores.
* [Look at this](https://stackoverflow.com/questions/17669159/what-is-the-relation-between-the-main-method-and-main-thread-in-java)
* Basic working of a Java program - (from process and thread view)
	1. JVM is created as a new process and runs.
	2. JVM now creates a main thread.
	3. the main thread looks for the "static void main method(String[] args)" to run. Now this is run as a thread inside the JVM
	4. JVM will create a new thread as programmer requests.
	5. JVM will also additionally kickstart new internal threads for garbage collection.

## Processes and Threads
* Processes
	1. Processes are generally self contained execution environment.
	2. They usually contain their own address space and private basic set of run time resources (like open files and file handles).
	3. Most implementations of the JVM run as a single process. More threads are created as needed by the programmer.
	3. IPC is carried out via pipes and sockets.

* Threads
	1. Threads are **sometimes** (this is the most important word) called as lightweight threads.
	2. Like processes, they also provide their own runtime environment.
	1. A process contains at least one thread and may extend to multiple.
	1. It can be easier to create newer threads as they require lesser resources than processes.
	3. Communication is often a problem, (Whyyyyy!!??) 

## Defining Threads and Starting them
* There are two ways of starting and defining threads - which can be 
	1. Implementing Runnable
	```
	public class IWillRun implements Runnable{
		public void start(){
			System.out.println("Hello.... I am thread!!");
	}
		public static void main(String[] args){
			(new IWillRun()).start();
		}
	}
	```
	2. Or extending Threads