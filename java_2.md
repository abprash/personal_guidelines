# More Stuff on Java

### Other Theoretical stuff about Java
* Swapping in Java is an interesting affair. For simple primitives, the usual swap function **CANNOT** be done. But if you are having a much more complex object, say a Car, then the reference swap can get invalidated after the swap method ends and will not be reflected outside.
* But if you are swapping the internal fields of two complex objects, then they will be reflected outside. But two ints, cannot be swapped by a simple swap method.
* Because, **Java is strictly pass by values for primitives. And it is pass by value of Object references for Objects**. **End of story. Nothing more**. **There is no pass by reference in Java**.
* Eg. its shown as follows
```
class Car{
  p s v main(String[] args){
    Car c1 = new Car(1,11);
    Car c2 = new Car(2,22);
    System.out.println(c1+","+c2);
    swap(c1,c2);
    System.out.println(c1+","+c2
  }

  //this will NOT WORK!!!!!!!!!!!!!

  public void swap(Car a, Car b){
    Car temp = a;
    a = b;
    b = temp;
    //the swap made above will be erased as soon as this method returns
  }

}
```
* So, what is the alternative for this? We should use a wrapper class to make the swap. And the temp variable should be the internal actual object we want to swap instead of the wrapper class.
* Another alternative would be to simply do the swap 3 lines without a method call directly.
* There is a chance for a constructor to be called recursively, if you write object creation code within the constructor or any instance block.
* eg.
```
//will not cause a stack overflow Exception
******************

class Test3{
  Test3()
   {
       System.out.println("Geeksforgeeks");
   }

   static Test3 a = new Test3(); //line 8

   public static void main(String[] args{
     Test3 b; //line 12
     b = new Test3();
   }
}

//************************
//the below code will cause a stack overflow Exception .............. if either of danger lines 1 or 2 are present in the actual code

class Test3{
  Test3()
   {
       System.out.println("Geeksforgeeks");
       new Test3(); // Danger 1
   }

  Test3 a = new Test3(); //Danger 2

   public static void main(String[] args{
     Test3 b; //line 12
     b = new Test3();
   }
}


```
* No exceptions are thrown even if it has exception causing code in the finalize() method.
* Constructors cannot be enclosed in try-catch blocks.
* Access modifier of anything (be it a member variable or a member function) should always be public.
* Constructors allows a sequential access of data between threads.
* static inner classes cannot access non-static fields of the outer class.
* Interfaces can also be nested as well. There can be same methods in both interfaces as well. So the implementing class can implement any of them, no issues.
* You need not even cast the object into a Thread object to make it run. It can just run on its own.  
```
public class Test extends Thread implements Runnable
{
    public void run()
    {
        System.out.printf("GFG ");
    }
    public static void main(String[] args) throws InterruptedException
    {
        Test obj = new Test();
        obj.run();
        obj.start();
    }
}
```
* Static variables must be for the whole class and cannot be declared in any instance method or any static method.
* The following runtime polymorphism example can be tricky.
```
public class RuntimePolymorphism
{
    public static void main(String[] args)
    {
        A a = new B();
        B b = new B();

        System.out.println(a.c + " " + a.getValue()
            + " " + b.getValue() + " " + b.getSuperValue());
    }
}

class A
{
    protected char c = 'A';
    char getValue()
    {
        return c;
    }
}

class B extends A
{
    protected char c = 'B';
    char getValue()
    {
        return c;
    }
    char getSuperValue()
    {
        return super.c;
    }
}

//******************************ANS - A B B A
```
* This can also be tricky --
```
public class RuntimePolymorphism
{
    public static void main(String[] args)
    {
        A a = new B();
        B b = new B();
        System.out.println(a.c + " " + a.getValue() +
            " " + b.getValue() + " " + b.getSuperValue());
    }
}

class A
{
    protected char c = 'A';
    char getValue()
    {
        return c;
    }
}
class B extends A
{
    protected char c = 'B';
    char getSuperValue()
    {
        return super.c;
    }
}

//**************************** OP - A A A A
```
* There are throwable exceptions of type "Error" also. so the following code is perfectly legal,
```
public class Except
{
    public static void main(String[] args)
    {    
        try
        {
            throw new Error();
        }
        catch (Error e)
        {
            try
            {
                throw new RuntimeException();
            }
            catch (Throwable t)
            {

            }
        }
            System.out.println("phew");
    }
}


//********************************* OP - phew

```
* If there is a private final method in the parent class, then if the child class also has another method with the same signature, then its not overriding, because the method in the parent is effectively hidden.
```
class Clidder
{
    private final void flipper()
    {
        System.out.println("Clidder");
    }
}

public class Clidlet extends Clidder
{
    public final void flipper()
    {
        System.out.println("Clidlet");
    }
    public static void main(String[] args)
    {
        new Clidlet().flipper();
    }
}

//******************* OP - Clidlet
```
