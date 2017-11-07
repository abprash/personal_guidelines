## Java OOPS - Basics

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


### Overriding
* When you override a parent's method, you are agreeing to the contract.
* The contract here being, the method. 
	* The parent method's same arguments must be used. Because, from the point of the compiler, it will only be able to see the reference method and not the actual object's methods even though it may be a valid overload.
	* It should not be less restrictive than the parent method.
	* It should not throw more broader errors than the parent method.


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


### When should you use concrete class, abstract class or interface?
* Make a subclass only when you want to have a more spcific behaviour for a class, with overridden behaviour.
* Make an abstract class, if you want to have a common template for many subclasses to inherit from. But an instance of the super class would not make sense.
* Make an interface when you want to define a role that the other classes can play regardless of where they are in the inheritance tree. Or if you want to introduce selective behaviour.
* [More info](https://stackoverflow.com/questions/479142/when-to-use-an-interface-instead-of-an-abstract-class-and-vice-versa)

