## Java OOPS - Basics


### JRE and JDK
* JRE is the run time environment used by Java. In other words, if you are going to be running Java programs on your machine, you will want this. JRE includes the runtime environment ie. the JVM and the other associated libraries.
* JDK - On the other hand, if you are going to be developing Java applications, you do need the entire Java Development Kit, which encompasses everything ie the (JRE and the associated libraries). JDK contains the main part i.e the javac, the compiler.
* See [this](https://stackoverflow.com/questions/1906445/what-is-the-difference-between-jdk-and-jre) for more info.


### Basic data types and their ranges
* boolean - true, false
* char - 1 Byte 
* byte - 1 byte - range = -128 to 127
* short - -2^15 to 2^15 - 1
* int - (-2^31) to (2^31 - 1)
* long - -2^63 to 2^63 - 1
* float -  
* double - 
* For bigger numbers, it is better to use BigDecimal classes.
* Enumerations can serve a purpose of representing a group of constants belonging to a set/group in a programming language. You can think of them like constants.
eg. Notice that there is no class keyword. This declaration should not be inside a method, but can be in a class or outside it.

```
enum Color
{
    RED, GREEN, BLUE;
} 
///or 
enum Card{
    SPADE, CLUB, HEART, DIAMOND 
}
```
* Every enum constant is always implicitly **public static final**. Since it is static, we can access it by using enum Name. Since it is final, we can’t create child enums.


### Encapsulation
* We cannot have our instance variables exposed to just plain changes. We should protect it.
* Eg. we have a cat object with an instance variable is height, what is to prevent someone from changing that height to 0. So we have to protect it using **gtters and setters**.
* Encapsulation is enforced via **private instance variables and getters/setters**.
* Think of private access specifiers as something like a **force field for the instance fields**. So that nobody from outside can touch it.
* **Encapsulation basically puts a force field around the variables so that nobody can do something inappropriate with them**.
* **One can prevent setting stupid and/or unreal values for certain instance variables like null SSN, negative heights, floating point number of bathrooms in a hotel etc.**
* With setters, we can do certain checks before actually setting the values for the data.

* eg.
```
Cat c = new Cat();
c.height = 0; //Yikes!! should not happen..
//so we should protect the instance variables
//setter for height

public void setHeight(int h){
	// validate the input and then set
	if(h>9){
		this.height = h;
	}
	else
		this.height = 9;
}
```


### Other Java Gotchas
* **Instance variables can be declared without any initializations**. No issues. Compiler gives them default values like 0, 0.0, false or null depending on the type. **But local variables must always be initialized before using.**
* *We can actually have uninitialized variables. But when we try to use them the compiler freaks out.**
* While evaluating boolean expressions, there are two kinds of boolean operators which we can use,
	1. Short circuit operators. (&& and ||)
		* These are like fully logical and convenience methods. If the left side of the || operator is true, it does not even evaluate the right side. Similarly for &&, if the LHS is false, it doesn't even check the RHS.
	1. Non short circuit operators. (& and |)
		* Forces the JVM to evaluate both sides of the expressions.
		* In another context, they are used to do bitwise operations.
	3. Now the question. Why are they useful?
		* They're useful if the right-hand side is a function with side-effects that you want to execute regardless (eg. logging). However, I would suggest that that's a bit of a code smell and will certainly be unintuitive to the next guy. [Source](https://stackoverflow.com/questions/9540718/are-there-good-uses-for-non-short-circuiting-logical-boolean-operators-in-java)


### Inheritance and Polymorphism
* **Remember that inheritance is used to change behavior. So methods represent behavior and instance variables represent state.**
* You can hide a field, but **not override** it.
* **It is extremely bad practice to hide fields, but the example shows it.**
```
public class HideField {

    public static class A
    {   
        String name = "a";

        public void doIt1() { System.out.println( name ); };
        public void doIt2() { System.out.println( name ); };   
    }

    public static class B extends A
    {
        String name = "b";

        public void doIt2() { System.out.println( name ); };
    }

    public static void main(String[] args)
    {
        A a = new A();
        B b = new B();

        a.doIt1(); // print a
        b.doIt1(); // print a
        a.doIt2(); // print a
        b.doIt2(); // print b <-- B.name hides A.name
    }
}
```
* IS-A and HAS-A relationship. If there is an IS-A relationship, then it is ok to have an inheritance. Or else, if there is a HAS-A relationship, then we are better off using it as an instance variable instead of a separate class.
* eg.
```
Cat **IS A** feline. -> So Cat can inherit from Feline.
Dog **IS A** Canine. -> So Dog can inherit from Canine.
Tub **IS NOT A** Bathroom (or) Bathroom **IS NOT A** tub. Neither of them work
So, Bathroom **HAS A** tub works. So we are better off using tub as an instance variable for Bathroom class.
```
* If the inheritance is done right, then it should always pass the IS A test.
* Here, the subclasses can choose to go with the parent class' implementation or **OVERRIDE** them. JVM always calls the lowest method implementation it finds. It starts with the lowest class and goes upwards until it gets the method.
* if you want to call both the parent's method and child method as well and not completely replace, then simply have 
```
public void method_name(){
	super.method_name();
	//do more
	//further more too
}
```
as the first line of the child method's implementation.

* Polymorphism -> The object reference variable and the actual object are different types. Eg. Animal object reference variable can refer a Dog object or Cat object so forth.
* Why are methods or classes declared final? It is because that marks the end of the inheritance tree. We cannot subclass it further. That is why most methods in the Java lang are marked final.
* Private instance variables and private methods are not inherited.
* final classes are not inherited. Non public classes cannot be inherited outside the package.
* 

### Overriding
* When you override a parent's method, you are agreeing to the contract.
* The contract here being, the method. 
	* The parent method's same arguments must be used. Because, from the point of the compiler, it will only be able to see the reference method and not the actual object's methods even though it may be a valid overload.
	* It should not be less restrictive than the parent method.
	* It should not throw more broader errors than the parent method.
* The child's return types can vary from the parent's method only if they are in an inheritance tree, and the return type is a sub type of the parent method's return type. This is called **covariant return type.**


### Overloading
* Overloading a method is simply a different method which happens to have the same name but different argument sets. It has nothing to do with polymorphism or inheritance.
* An overloaded method is not the same as the overridden method.
	* The access modifier can be changed in any direction.
	* There can be different arguments.
	* Should not change the return type alone. (Can change both arguments and return type both) The compiler will think you are trying to override it.

### Interfaces and Abstract Classes
* Some classes **SHOULD** not be instantiated. For eg. from Animal class, Canines -> Dog and Wolf, Hippo and Feline -> Cat, Tiger classes are created.
* How does an Animal object look like? The truth is we do not need an Animal object ever. So its better we simply declare it as an abstract class or an interface.
* **If a class is declared abstract, then the compiler will prevent any code from ever instantiating the class.**
* An abstract class' only purpose is to just simply have children. **It has no use unless it is extended**.
* **An abstract class has to be extended. And an abstract method must be overridden.**
* ONLY an abstract class can contain abstract methods. But however, an abstract class can contain a mixture of both non abstract and abstract methods.
* **You must implement (or override them.. both are the same almost) all abstract methods.**
* Abstract methods have no body.
* **An interface is nothing but 100% pure abstract class**.
* It will only have abstract methods.
* When will an interface be required? When we need selective functionality in some of the child classes. So at that time, we can just have two parents. One will be an abstract class and the other will be an interface.
* What does interface really get you???
	* Polymorphism, Polymorphism and Polymorphism!!!
	* A class can implement multiple interfaces.
	* it need not be a part of the inheritance tree.
	* They are much much more flexible than abstract classes.
	* If you use interfaces instead of concrete sub classes, then the methods in the interface, can take any class which implement the interface.
* The access specifiers allowed for an interface are only abstract and public.


### When should you use concrete class, abstract class or interface?
* Make a subclass only when you want to have a more specific behaviour for a class, with overridden behaviour.
* Make an abstract class, if you want to have a common template for many subclasses to inherit from. But an instance of the super class would not make sense.
* Make an interface when you want to define a role that the other classes can play regardless of where they are in the inheritance tree. Or if you want to introduce selective behaviour.
* [More info](https://stackoverflow.com/questions/479142/when-to-use-an-interface-instead-of-an-abstract-class-and-vice-versa) and [Here](https://www.javaworld.com/article/2077421/learn-java/abstract-classes-vs-interfaces.html)


### Constructors - Life and Death of Objects
* Local variables live on the stack. Instance variables live on the heap.
* Local variables if they are complex objects, then the object reference variable will be on the stack, but the actual object will still be on heap. And once the stack frame is done executing, it will be GCed.
* If you don't write a Constructor, the compiler will give a default constructor for you.
* Constructors have no return types. Constructors can have access modifiers associated with them though. Public/ private or protected or default (which means no access specifier). If private, then no outer class can instantiate the class.
* The compiler will provide you a default constructor as long as you don't put any constructors. But when you stick in an arg constructor, then we have to take care of every constructor.
* If there is a big inheritance chain, then all the constructors in the inheritance chain must be invoked from top most to bottom class. So it is like a constructor chain reaction. Called as **constructor chaining.**
* So, if the sub class object's constructor does not have a call to super() in its first line, then the compiler does it for us. (Take care, even if there is an arg version of super class constructor, it will ONLY invoke the no-arg one.)
* But we can make a change to the super call, so that it accepts arguments, no problem at all.
* A constructor's first call can be to either **this()** or **super()**, but not both. (So, ultimately the constructor which is invoked using this() may have the call to super())
* this() -> Used when we have another constructor which may have the bulk of the initialization code inside it.
* **this() CAN ONLY BE USED INSIDE A CONSTRUCTOR. Never outside it.** 
* So, the general rule is, the parents come before the children. No way that can be circumvented. Its just plain wrong.
* There are 4 ways of creating objects. 
    * Using new,  -> ` Test t = new Test();`
    * using Class.forName("FUlly qualified Class_Name").newInstance(); -> `eg. Test t = (Test) Class.forName("com.pack.Test").newInstance();`
    * Using clone() -> ` Test t2 = (Test) t1.clone();`
    * Deserialization -> 
    ```
    FileInputStream file = new FileInputStream(filename);
    ObjectInputStream in = new ObjectInputStream(file);
    Object obj = in.readObject();
    ```
    * For more look here [This](https://stackoverflow.com/questions/95419/what-are-all-the-different-ways-to-create-an-object-in-java) and [this](http://www.geeksforgeeks.org/classes-objects-java/)
* There are also anonymous objects - where a new instance is created, but it is not assigned to any object reference variable. It will be GC'ed as soon as its work is done. eg.
```
btn.setOnAction(new EventHandler()
{
    public void handle(ActionEvent event)
    {
        System.out.println("Hello World!");
    }
});
```

### Exceptions and their handling
* Checked Exceptions must be declared (using the throws keyword) or caught. 
	* Now the question comes, why not make all exceptions as checked.
	* Ans. Because they are runtime exceptions. The compiler does not care about this. We cannot guarantee that the file will be present at the location/ or the server will be up. But, we can most certainly not exceed the array's length, or check if the character is numeric before parsing it.
	* So, runtime exceptions can be prevented, and are most certainly due to the logic while programming.
* A try-catch is for handling exceptional scenarios and not for handling flaws in the programs.
* An exception is always of the type object -> **Exception**.
* Compiler only cares about the checked exceptions and not the run time exceptions.
* Usual blocks are 
```
try{
	//risky code goes here
}
catch(Exception x){
	//handling and recovering if possible
	//or simply collect debug info and exit
}
finally(){
	//for any cleanup code
}
```
* Finally block always executes **except in cases where, the JVM exits. OR if the thread executing the try or catch block is interrupted or killed.e**
* eg.
```
//bye does not get printed
 try {
    		        System.out.println("hello");
    		        System.exit(0);
    		    }
    		    finally {
    		        System.out.println("bye");
    		    } 
```
* Exceptions are polymorphic in nature. Its the place where polymorphism is exercised to its max.
* One can use the super class Exception to catch all the exceptions Just because, we can do that doesn't mean we should. Ideally we should write the catch blocks in increasing order of scope and catch. They should be ordered small to large.
* You can simply duck when an exception arrives, and simply avoid it. Let the method calling you handle it. (There will always be a method calling you. Worst case, it will be main()).
* So, if your method, declares an exception, then the method calling you must handle the exception. And when an exception occurs, the stack frame just gets popped off. and exception is passed to the calling method.
* **HANDLE OR DECLARE.** 



### Inner Classes
* They are used a lot in event handling. Responding to certain events.
* They just make a lot of sense in GUI event handling.
* [Here too](https://stackoverflow.com/questions/18396016/when-to-use-inner-classes-in-java-for-helper-classes)
* Basically we need multiple implementations of the same method while handling GUI events.


### Other wacky stuff
* There can be overloaded main methods in a program, no problem. But there absolutely must be one "public static void main(String[] args)"
* Why is a constructor public static void?
	* Because, if it is not static, it introduces other contracts which the class must fulfill, including, which constructor to use when creating an instance of the class, what arguments should be passed for the class' constructor, and also ensure the class is not abstract (because abstract classes cannot be instantiated.) etc. It introduces a lot of additional brittle contracts. Which is not good programming. But if it were static, we just have to ensure only one thing. There should be a static main present.
	* It has to be void. No other go. Compiler asks for the main method to return void. However if we want to return something, we can use System.exit(int) to return some error code after main terminates. But we cannot see it on console without having some shell scipt to read the exit code. (in languages like C, it is a means of indicating the error code upon exit. C/C++ progs can have their main return int values.)
	* Or if we want some values from main, we can either store them in String[] args.
	* As to why it is public? No idea.
* How a java prog runs?
	* **When you type java.exe <prog_name>, it will use JNI, basically the DLL loads the JVM. JVM is not the java.exe. So the java.exe is a C program which parses the command line, and takes in the arguments passed in, creates a String array and stores them in it, and passes it to the main method of the program. Then it runs. Basically its all convention. So, if we write our own java.exe code, we can make it start from a method called StartFromThis() with arguments of our choice.** **Full credits to this answer [here](https://stackoverflow.com/questions/146576/why-is-the-java-main-method-static)**
* There can be different blocks in a Java program. Always remember the order in which they are executed....
	1. Static blocks in the order they are written
	2. Instance blocks in the order they are written
	3. at last main() method is invoked
```
public class Boot 
{
    static String s;
    

    //static block below
    static
    {
        s = "";
    }
    

    //instance block below.... will get called after static blocks are done
    {
        System.out.println("GeeksforGeeks ");
    }
    

    //another static block
    static
    {
        System.out.println(s.concat("practice.GeeksforGeeks "));
    } 
    
    //normal method ... do not worry
    Boot() 
    { 
        System.out.println(s.concat("Quiz.GeeksforGeeks"));
    }

    //after static, instance blocks, finally this executes.
    public static void main(String[] args) 
    {
        new Boot();
        System.out.println("Videos.GeeksforGeeks");
    }
}
```
* Static blocks are run only once. Initializer (or instance) blocks are run every time before the constructor.
* 


### Important Java predicting output questions and explanations
* [instanceof](https://stackoverflow.com/questions/7526817/use-of-instance-of-in-java) [this too](http://www.geeksforgeeks.org/java-instanceof-and-its-applications/)
* [radix](https://stackoverflow.com/questions/35521278/printing-an-integer-in-java-that-have-zero-in-front-of-it) [radix_more](https://stackoverflow.com/questions/37532421/how-can-i-put-my-my-binary-octal-and-hexadecimal-in-one-loop)
* [static methods overriding](https://stackoverflow.com/questions/2223386/why-doesnt-java-allow-overriding-of-static-methods)
* **Any variable declared in the static block is block local. END of STORY**. Example below
```
public class Test1 {
    int x = 10;
    
public
    static void main(String[] args)
    {
        System.out.println(new Test1().x);
        //System.out.println(Test1.x); *********This will cause a compilation error because, x variable in the static block is local to that block alone. It is not visible to the entire class. So, yes. Compilation error.
    }
    static
    {
    	//x here is local to this block. it is not static for the whole class
    	int x = 20;

        System.out.print(x + " ");
    }
}
```
* **When we implement an interface, it is necessary that all methods be public.**
* **If there is a return statement in the try block, and there is a catch-finally block, the finally block still executes.**
* **Java allows constructor and a method to have the same name.**[here](https://stackoverflow.com/questions/3401444/methods-with-same-name-as-constructor-why)
```
class Example {
private int x;
public static void main(String args[])
    {
        Example obj = new Example();
    }
public void Example(int x)
    {
        System.out.println(x);
    }
}
```
* Aggregation and Composition - [this](https://stackoverflow.com/questions/734891/aggregation-versus-composition)
* Difference between ArrayList and Vector. [this](https://stackoverflow.com/questions/2986296/what-are-the-differences-between-arraylist-and-vector)
    * Primary differences are
        * Vectors say that they are thread safe, BUT its flawed. So not advisable to use them. 
        * Slow. Even get(), set() methods are synch.d
        * Arraylists grow by 50%, when capacity exceeded, whereas vectors simply double their capacity.
* Jagged arrays are possible in Java.
```
int[][] j = new int[10][];
j[0] = new int[10];
j[1] = new int[100];
j[2] = new int[5];

```
* Sorting
```
Collections.sort(list_name);
Collections.sort(list_name, Collections.reverseOrder());

```

* Do we really need the public static void main() for a program to run?? Ans - Yes, we do. Prior to Java 5, we did not need them, but we need them now. Note that however, content in static block executes prior to main() method.
* THere are two blocks static blocks and instance blocks. Static blocks get called only once - when the class is loaded, before the main method's invocation. But, instance blocks run, upon the creation of each instance. Every time an instance of that class is created, the instance block gets invoked.
* What about multiple static and instance blocks? or Nested static and instance blocks?? Ans - If you think about it, nested static blocks do not make sense - as static blocks belong to the class, and they should be in the class level and no deeper. However, a class can have multiple instance blocks and static blocks. They will be invoked in the order they have been written in the program. There can be nested instance blocks as well. There can be plain nested blocks inside the static block as well, but there **should NOT be nested static blocks**.
```
class Test2 
{
    // static block
    static
    {
        System.out.println("Hello from static block 1 ");
    }
    static
    {
        System.out.println("Hello from s block 2");
    }
    static
    {
        System.out.println("Hello from s block 3");
    }
    {
        System.out.println("Hello from instance block 1");
    }
    {
        System.out.println("Hello from instance block 2");
    }
    {
        System.out.println("Hello from instance block 3");
    }
     public static void main(String[] args){
        System.out.println("Hello from main");
        Test2 t = new Test2();
        Test2 t1 = new Test2();
    }
}

//OP

Hello from static block 1 
Hello from s block 2
Hello from s block 3
Hello from main
Hello from instance block 1
Hello from instance block 2
Hello from instance block 3
Hello from instance block 1
Hello from instance block 2
Hello from instance block 3

```
* The class name and the file name ** should absolutely be the same.** They must match. However, if you use `javac Filename.java` and run it, with a different class name inside (say Trial), that particular class name would be generated. Then we can do `java Trial` to run the class file.
* For the following program, there will be no compilation errors, but it will fail in runtime.
```
//Runtime error
public class String
{
    public static void main (String[] args)
    {
        System.out.println("I got confused");
    }
}

//will work perfectly

public class String
{
    public static void main (java.lang.String[] args)
    {
        System.out.println("I got confused");
    }
}

```
- JVM will complain that, it did not find method main() in the String class. Also, since String is an inbuilt class of Java. Because, the JVM was expecting a main method, with java.lang.String[] args, but it got a user defined class with user defined String[] argument. Because, the order in which JVM will search for the class is the current package first, then the classpath. But if we replace the String in the main args with java.lang.String, it will run perfectly.
* Widening primitives -> This is used to convert chars into ints when used with operators like +/-. Java automatically promotes any byte/ short/ char to int when evaluating any arithmetic expression. 
* In an expression, if there is even one long/double the whole expression will get promoted to that.
* A bit more on ClassCastException and when it is thrown. It is thrown when an object is casted to something which is not its superclass. eg.
```
class A{}
class B extends A{}
class C extends A{}
// You can cast any object to Object, because every class inherits from Object.
// You can cast any B/C object to A, because, it is a type of A.
// You can use A's reference to point to a B/C object.
// You cannot type cast an actual B object into C or vice versa even though they are A's.
// You can cast A's reference to B ONLY IF the actual object is a B.
```
* We can also use underscores to improve readability of numbers. They are like the equivalent of commas used to separate nos. This was from JDK7.
* 

### Interesting stuff about null in Java
* null is case sensitive. null, Null and NULL are different.
* We can invoke static methods even though the actual object is null. This is because, the compiler will see that its a static method, and will invoke using the class' name instead of the object. But, it will throw a runtime null pointer exception if we attempt to invoke an instance method using a null object reference.
* Any reference variable will have the default value to be null.
* We can type case null to any object type.
* When we use the instance of operator, even though it may be an instance, if the object is null, it will return false.
```
Integer a = 2;
Integer b = null;
System.out.println(a instanceof Integer);
System.out.println(b instanceof Integer);

//OP 
//true
//false
```
* break anc continue: - This snippet explains it beautifully.
```
outer:                   //should be placed exactly before the loop
loopingConstructOne  {   //we can have statements before the outer but not inbetween the label and the loop          
    inner:
    loopingConstructTwo {
        continue;       //goes to the top of loopingConstructTwo and continue
        break;          //breaks out of loopingConstructTwo
        continue outer; //goes to the outer label and reenters loopingConstructOne
        break outer;    //breaks out of the loopingConstructOne
        continue inner; //this will behave similar to continue
    }
}
```
* goto in Java is a reserved keyword, but it is not used. I think it is for future support in case they were to add in the future. If it is not used then why is it marked as a reserved keyword? Because, if it were added in the future, then codebases which have used goto as an identifier name will break and will probably be a messy affair albeit fixable. But yeah, to prevent things from breaking in the future. [here](https://stackoverflow.com/questions/2545103/is-there-a-goto-statement-in-java)
* 


### Other data structures and collections
* TreeSet will sort the elements when adding in ascending order, and eliminate duplicates. (because, set.)


### Best practices in software designing
* **SOLID**
* [I](https://stackoverflow.com/questions/9249832/interface-segregation-principle-program-to-an-interface)



### Runtime
* Every Java application has a single instance of the Runtime, which is used to interface the application with the run time environment.
* The run time instance can be obtained with the getRuntime() method.
* An appln. cannot create an own instance of this class.
* A java appln. will shutdown in response to two different kinds of events
    * When the program itself terminates normally, / or last non daemon thread returns/ or when System.exit() is invoked
    * When the program, is killed because the system is being shutdown/logged off./ or Ctrl + C is invoked.
* We can actually start a new separate process in Java, using exec(). An example is as shown.
```
public static void main(String[] args){
    Process process = Runtime.getRuntime().exec("google-chrome");
    System.out.println("Chrome started successfully, but this main thread will exit.");
    //chrome will be started as a separate process
}
```
* We can get lots of cool nifty stuff from the Runtime object, using its super helpful methods like 
```
Runtime.getRuntime().freeMemory(); // -> will give JVM's avail free mem
Runtime.getRuntime().totalMemory();
Runtime.getRuntime().availableProcessors();
Runtime.getRuntime().traceInstructions(false); // will give trace instructions as and when each instruction runs. It is highly system dependent. can be toggled
Runtime.getRuntime().gc(); // will run the GC for us

```


### Difference between exit() and halt() method in Java
* System.exit() is used by the VM when it wants to exit in a normal orderly manner. It will call all its shutdown hooks first, then it will call its finalizers, if finalization on exit is enabled and then halt.
* Runtime.getRuntime().halt() will forcibly quit the VM. None of the shutdown hooks will even run. It always returns abnormally.
* exit() may not always exit the program, but halt will always do.



### Shutdown hook in java
* It is typically executed right after the last line in the program runs.
* There can be multiple shutdown hooks as well, but we cannot guarantee the order, as they are separate threads typically.
* So, when a program terminates, this happens, the VM begins its shutdown sequence,  where it will start all its registered shutdown hooks, in some unspecified order - but they are all executed concurrently. 
* After all the hooks have been finished, VM will then invoke uninvoked finalizers (finalizers are nothing but the finalize methods) if finalization-on-exit has been enabled.
* And then the VM halts.
* Generally an OOP language will have either finalizers or destructors. Occasionally there are some languages like C++ which has both. Both almost do the same job. Finalize methods are called upon by the GC, whereas the destructors are deterministic and called by the programmers.


* Eg.
```
public class ShutdownhookDemo{
    public static void main(String[] args){
        //adding a thread hook to the runtime
        Runtime.getRuntime().addShutdownHook(new Thread(){
            public void run(){
                //lets run it
                System.out.println("inside the hook");
            }
        });



        //print something
        System.out.println("last line");
    }
}
```