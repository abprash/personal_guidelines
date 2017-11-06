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
