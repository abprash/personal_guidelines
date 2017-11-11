# Concurrency in Java - Basics
##[Credits](https://docs.oracle.com/javase/tutorial/essential/concurrency)

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
		public void run(){
			System.out.println("Hello.... I am thread!!");
		}
		public static void main(String[] args){
			(new IWillRun()).start();
		}
	}
	```
	2. Or extending Threads
	```
	public class IWillRun2 extends Thread{
		public void run(){
			System.out.println("Hello .. I am Bob!!");

		}

		public static void main(String[] args){
			(new IWillRun2()).start();//will invoke the run method
		}
	}
	```

* Out of these two approaches, implementing Runnable is preferred because, it will keep the class more general. Otherwise we have to extend it from Thread. It may not work well with a subclass which is already extending from another base class.


## Pausing Threads with sleep
* We can make the threads sleep, there are two values it can take, it can take nanoseconds or milliseconds values.
```
ClassExtThread t = new ClassExtThread();
t.sleep(long millis);
t.sleep(long millis, int nanosec);
```
* We cannot guarantee the precise timing though. It will depend on the scheduler in the OS, because that is what will be putting the thread on the ready queue.


## Interrupts
* Basically, how the interrupts in thread can be thought of is like - giving a thread a gentle nudge thereby giving them the opportunity to exit cleanly, as opposed to calling thread_name.stop(), which (can lead to unpredictable behaviour) can be thought of as shooting the thread in the face with an assault rifle. [Credits to this SO answer](https://stackoverflow.com/questions/3590000/what-does-java-lang-thread-interrupt-do)
* Now for the fun part.. How they work..
	1. When a thread t1 wants to interrupt t2,
	2. t1 will go and invoke t2.interrupt(). (Assuming that t1 has permissions to interrupt it, else t2 will throw a SecurityException)
	3. The code of t2 will keep checking if there is an interrupt, if there is, it will throw an InterruptedException and exit out of the run() method.
		1. There are various scenarios that can happen here, what if t2 is waiting on an IO operation, or blocked in sleep() or blocked in join(),
		2. Then, if it is blocked in an IO operation -> ClosedByInterruptException is thrown.
		3. If it is blocked by wait() or by join(), then t2 will invoke Thread.interrupted() method - (which will actually check if it is interrupted, **AND** clear the interrupt bit and return true else return false) and finally throws InterruptedException.

```
public class SomeThread extends Thread{
	
	public void run(){
		//we will be doing some op here
		String[] s = {"a","b","c","d"};
		for(int i=0; i<s.length; i++){
			if(s[i].equals("somoeting")){
				//do something
			}
			else{
				//do nothing
			}
			//also keep checking if any rogue thread will disturb us
			if(Thread.interrupted()){
				//option 1
				//we are done. someone interrupted us.. i am gonna return
				return;
				//option 2
				//we can either do that or even throw an InterruptedException
				//or even
				//throw new InterruptedException();
				//or we can even have a try catch block to handle the interrupted exception here itself.

			}
		}
	}
}
```