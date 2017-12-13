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
