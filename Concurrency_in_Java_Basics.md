# Concurrency in Java - Basics
## [Credits](https://docs.oracle.com/javase/tutorial/essential/concurrency)

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
* Now for the fun part.. How they work..
* Basically, how the interrupts in thread can be thought of is like - giving a thread a gentle nudge thereby giving them the opportunity to exit cleanly, as opposed to calling thread_name.stop(), which (can lead to unpredictable behaviour) can be thought of as shooting the thread in the face with an assault rifle. [Credits to this SO answer](https://stackoverflow.com/questions/3590000/what-does-java-lang-thread-interrupt-do)
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
* Thread.interrupt() method sets the flag, and Thread.interrupted() will clear it.


## Join
* Join is used when we need to wait for another thread to finish execution. Only after that thread finishes execution, will join() return, and then the main thread will continue execution.
* [See this](https://stackoverflow.com/questions/18479771/java-multithreading-concept-and-join-method)

```
public static void main(String[] args){
	//
	//
	//
 	thread_name.join();
 	//some more
}
```
* main() will actually halt at join(). Until thread_name finishes execution, and only then, will main continue with "some more".


## Thread - Gotchas
* thread_name.start() (a class which extends Thread) actually starts a new method. But, thread_name.run() (a class which implements Runnable)
* start() actually creates a new thread and starts it. But, run() behaves like a method call.
* [Look at this](https://stackoverflow.com/questions/8579657/whats-the-difference-between-thread-start-and-runnable-run)
* [Difference between extending Thread and impl Runnable](https://stackoverflow.com/questions/541487/implements-runnable-vs-extends-thread)
* [Runnable vs Thread](http://javarevisited.blogspot.com/2012/01/difference-thread-vs-runnable-interface.html)
* Take care to never call the class's constructor, within the class definition. Because it would result in a stack overflow exception, because it just keeps getting called.
eg.
```
public class Sad{
	String a ;
	int b ;
	Sad sadFellow = new Sad(); //line 4 //will cause SO exception
	static Sad oneSadFellow = new Sad(); //will **NOT CAUSE** the SO exception

	public Sad(){
		//init the obj
		//**TAKE CARE NOT TO INVOKE THE constructor here again inside as well**
		//that is
		// DO not invoke Sad() inside the constructor.
		//this is plain stupid
	}

	public static void main(String [] args){

		Sad s = new Sad(); //this line will cause the StackOverflow exception.
		//because it will load Sad class again, where it will be again invoking Sad again and again, which is on line 4
		//thats it .. we are done

	}
}
```
* [A very overflowing error](https://stackoverflow.com/questions/9126122/stackoverflow-error-when-creating-object-outside-the-main-method), [Also this](https://stackoverflow.com/questions/38966213/why-do-i-get-infinite-loopstackoverflow-error-while-creating-an-object-in-a-cl)
* The following code shows peculiar behavior,
```
Integer a = 128, b=128;
System.out.println(a == b); // will print false


Integer c = 100,  d = 100;
System.out.println(c == d); // will print true
//WHY!!!!??
//because it will work well from -128 to 127. Beyond that, we have to use equals to get the correct comparison.

```
[Look at this](https://stackoverflow.com/questions/29139274/how-equal-operator-works-with-primitive-and-object-type-data), [also](https://stackoverflow.com/questions/26742042/how-operator-compare-references-mean-internal-working-of-operator)

* **Explanation for the above code** Integer caches values between -128 to 127. So when we do, the first declaration, they are cached. But, beyond that they are not cached.
```
Integer a = new Integer(12), b=new Integer(12);
System.out.println(a == b); // will print false
```
[Credits](https://stackoverflow.com/questions/8427416/why-is-true-for-some-integer-objects)

* We can invoke the Java GC by the statement,
` System.gc();`

* When the Garbage collector is called, the corresponding class' finalize method is invoked upon it to clear up. But when we just simply orphan some other class' object, it will never be invoked. eg.
```
public class GC_demo {

	public static void main(String[] args) throws InterruptedException{
		GC_demo obj = new GC_demo();
		String s = new String();
		obj = null; // if this line is commented, finalize won't be invoked.
		//if its NOT commented, then the overridden finalize will be invoked, just before the GC destroys the obj.

		s = null;
		//or the GC can also be invoked by Runtime.runtime.gc();
		System.gc();
		Thread.sleep(1000);
		//if the above thhread sleep line is commented, then both threads (GC and current main thread) will carry on executing.
		System.out.println("main ends here");
	}

	@Override
	public void finalize(){
		System.out.println("finalize called");
		//if there are any exception causing code here, then the JVM will **ignore** it.
		//WHY??
		//because, at this stage, who really cares whether there is any exception when clearing up stuff. You just destroy everything.

	}

}
```
* GC can also run in the background after main returns.
* GC will be called on the obj reference as many times as needed. If we initialize an obj ref variable to a to 100 different objects, then the first 99 objects will be orphaned for the GC to collect them.
* You can call a static method with a null instance (when the instance is NULL), but if you try with a non static method, it will throw a run time NullPointerException. (Because, the JVM will look at the class name, and directly invoke the static method.)
* A method can have **varying** arguments (var args also called variadic functions) if we denote it with 3 dots.
```
public int getData(int... value)
    {
    	//System.out.println(value.length);
        //****
        //Always treat "value" as an array
        return  (data+2);
    }

//can be invoked as
System.out.println(temp.getData(7,2));
System.out.println(temp.getData(new int[]{7,8,9}));
System.out.println(temp.getData(new int[]{7,8,9}));

```
* A nested (or inner) static class cannot access instance variables. It can access only static variables.
* The class name of the file and the class name containing the main method should be the same. Else the JVM will complain that there is no main methood while **RUNNING.**
* Calling thread's run() is simply like a method call, whereas, **start() is starting a new thread whose entry point is contents of method run()**.
* Constructors cannot be enclosed in a try-catch block. It will be a compilation error.
* The following [link](https://stackoverflow.com/questions/2536692/a-simple-scenario-using-wait-and-notify-in-java) explains the use of wait() and notify() well.
```

```
### Synchronization
* There are internal locks called monitor locks. Or simply monitors. These help in Synchronization.
* There are two kinds of synchronized code,
	* Synchronized methods
	* Synchronized statements
* Example of synchronized methods
```
public class SynchronizedCounter {
    private int c = 0;

    public synchronized void increment() {
        c++;
    }

    public synchronized void decrement() {
        c--;
    }

    public synchronized int value() {
        return c;
    }
}

```
* Synchronized statements
```
public class MsLunch {
    private long c1 = 0;
    private long c2 = 0;
    private Object lock1 = new Object();
    private Object lock2 = new Object();

    public void inc1() {
        synchronized(lock1) {
            c1++;
        }
    }

    public void inc2() {
        synchronized(lock2) {
            c2++;
        }
    }
}
```
* Every object has an intrinsic lock associated with it. If that object will be used by concurrent threads, then before modifying any of the fields, we have to acquire the lock and then do the operations on the object. And once done, releases the intrinsic lock. As long as one thread holds the intrinsic lock, another thread cannot acquire it without the first thread releasing it.
* **Reentrant synchronization** - But a thread can acquire a lock that it already owns. Allowing a thread to acquire the same lock more than once enables reentrant synchronization. This describes a situation where synchronized code, directly or indirectly, invokes a method that also contains synchronized code, and both sets of code use the same lock. Without reentrant synchronization, synchronized code would have to take many additional precautions to avoid having a thread cause itself to block.
* **Volatile keyword** - Think of this like a synchronization for variables. We cannot use synchronized with member variables, so it has to be with volatile. It will not cache it and always fetch it from main memory, so it is guaranteed that it will always be using the most fresh copy of that variable.
	* Reads and writes are atomic for all variables - both objects and primitives. (except long and double)
	* Reads and writes are atomic for all volatile declared variables. (including obj, and primitives - long and double )
	* **LIveness** - A concurrent application's ability to execute in a timely manner is known as its liveness.
* Deadlock - Where one process(P2) is waiting for another resource(R2) which in turn is held by the 2 nd process (P2). No useful work. endless waiting.
* Livelock - The processes are running but are not doing any useful work.
