---
num: "Lecture 6"
desc: "Inheritance / Polymorphism"
ready: true
lecture_date: 2019-04-17 17:00:00.00-7:00
---

# Inheritance

* Inheritance is a way of extending functionality and properties of an existing class.
    * and allows you to add new features and overwrite existing ones.

## Example

```
// Vehicle.java
public class Vehicle {

    private double maxMPH;
    private String color;

    public Vehicle() {
        System.out.println("Vehicle default constructor");
    }

    public double getMaxMPH() { return maxMPH; }
    public void setMaxMPH(double maxMPH) { this.maxMPH = maxMPH; }
    public String getColor() { return color; }
    public void setColor(String color) { this.color = color; }
}
```

* Every Vehicle should have a max speed (in MPH) and a color
* There are many types of classes that can inherit from this.
    * Car, Plane, Motorcycle, Boat, ...

```
// Car.java
public class Car extends Vehicle {

    private String make;
    private String model;

    public Car() {
        System.out.println("Car default constructor");
    }

    public String getMake() { return make; }
    public void setMake(String make) { this.make = make; }
    public String getModel() { return model; }
    public void setModel(String model) { this.model = model; } 
}
```
```
// Lecture.java

public class Lecture {

    public static void main(String[] args) {
        Car c = new Car();
    }
}
```
```
// Output:
Vehicle default constructor
Car default constructor
```

* Note that Java calls our default constructor from the base class first, and then subclasses (top-down).

## Default Constructors in Java

* Comment out Vehicle default constructor

```
    // public Vehicle() {
    //     System.out.println("Vehicle default constructor");
    // }
```

* Add another Vehicle constructor (not the default)

```
    public Vehicle(double maxMPH, String color) {
        System.out.println("in Vehicle(double maxMPH, String color)");
        this.maxMPH = maxMPH;
        this.color = color;
    }
```

* Since we don’t have a default constructor in Vehicle anymore, Java can’t automatically call the default constructor in the Car class and doesn’t know how to construct the Vehicle base class.
    * Java won’t compile this code in this case.
* We would need to call the base class’ constructor using super in Car's constructor

```
public Car() {
        super(100, "black"); // must be first line
        System.out.println("Car default constructor");
    }

    // Add another Car constructor
    public Car(double maxMPH, String color, String make, String model) {
        // Need super() since we are extending from a Base class.
        // super implicitly gets called with Vehicle default constructor.
        super(maxMPH, color); // calls Vehicle(maxMPH, color)
        System.out.println("Car constructor");
        this.make = make;
        this.model = model;
    }
```
```
// in main
        Car c = new Car(120, "black", "Pontiac", "GrandAM");

        System.out.println(c.getMaxMPH()); // 120.0
        System.out.println(c.getColor());  // black
        System.out.println(c.getMake());   // Pontiac 
        System.out.println(c.getModel());  // GrandAM
```

* We say that Vehicle is the base class (or parent class) of Car
* Car is a derived class (or subclass) of Vehicle
* We can further derive subclasses from Car if we wished.
    * MiniVan, Truck, Coupe, …
* Each of the subclasses that extend Car will inherit everything from Vehicle AND Car as well as defining their own unique features within the class.

## Example with Truck class inheriting from Car

```
// Truck.java
public class Truck extends Car {

    private double bedVolume;

    public Truck(double maxMPH, String color, String make, String model, double bedVolume) {
        super(maxMPH, color, make, model);
        System.out.println("Truck Constructor");
        this.bedVolume = bedVolume;
    }

    public double getBedVolume() { return bedVolume; }
    public void setBedVolume(double bedVolume) { this.bedVolume = bedVolume; }
}
```
```
// in main
Truck t = new Truck(120, "black", "Pontiac", "GrandAM", 400);
System.out.println(t.getMaxMPH()); // 120.0
System.out.println(t.getColor());  // black
System.out.println(t.getMake());   // Pontiac 
System.out.println(t.getModel());  // GrandAM
System.out.println(t.getBedVolume()); // 400.0
```

## Constructor Chaining
* Truck uses its super constructor and calls Car’s constructor
* Car uses its super constructor and calls Vehicle’s constructor
* ... and so on.
    * This is known as <b>constructor chaining</b>

## Instanceof operator
* We can check if an object reference is a legal type using the `instanceof` operator.
* This includes the current class and all base classes as well.
* `instanceof` works only for objects in the same hierarchy.
* Recall Exceptions
    * All Exceptions are of type Exception since they are all inheriting from that class
    * That’s why we can catch ALL exceptions by catching an Exception type
* In order to actually check the specific class, you can use `.getClass().equals()`

## Example

```
Car c = new Car(120, "black", "Pontiac", "GrandAM");
if (c instanceof Vehicle)
    System.out.println("instance of Vehicle"); // valid
if (c instanceof Car)
    System.out.println("instance of Car"); // valid
if (c instanceof Truck)
    System.out.println("instance of Truck"); // invalid
if (c instanceof Object)
    System.out.println("instance of Object"); // valid
```

* In order to actually check the specific class, you can use .getClass().equals()

```
if (c.getClass().equals(Vehicle.class))
    System.out.println("getClass.equals.Vehicle");
if (c.getClass().equals(Car.class))
    System.out.println("getClass.equals.Car"); // prints this
if (c.getClass().equals(Truck.class))
    System.out.println("getClass.equals.Truck");
```

# Polymorphism (Dynamic Binding)

* Recall, all classes in Java extends the Object class.
* Which means that any object can be assigned to the Object class

```
Object o = new Car(120, "black", "Pontiac", "GrandAM");
Object o2 = new int[100];
Object o3 = new Scanner(System.in);
```

* But we can’t have it the other way around

```
Car c = new Object(); // illegal!
```

* Notice that only the Object methods are available if assigning a subclass to an Object class type.
* This includes `.toString()`, `.equals()`, ...
    * `.toString()` defined in the object class is some sort of representation of the class itself.
    * In most cases, this is probably not what you want `.toString()` to do.
    * `.equals()` in the Object class pretty much compares object references such as `==`
        * but recall that `String.equals(String)` compares the content of two Strings.

## How come String .equals() works differently if all objects inherit from Object.class?
* The String class provides its own implementation of how .equals() should work.
* The String class overrides the .equals() method and uses its own definition.
* A subclass can override inherited methods to provide its own specific definition.

## Example

```
// in main
Vehicle v = new Vehicle(100, "blue");
System.out.println(v.toString());
```
```
// Output
in Vehicle(double maxMPH, String color)
Vehicle@1e81f4dc
```

* Unfortunately, `.toString` in this case doesn’t make much sense.
* and it may make sense for `.toString()` to provide some information on the state of the Class.
* We can override the `.toString()` method in our Vehicle class:

```
// Vehicle.java

    public String toString() {
        System.out.println("In Vehicle.toString()");
        return "maxMPH = " + maxMPH + ", color = " + color;
    }
```
```
// in main
Vehicle v = new Vehicle(100, "blue");
System.out.println(v.toString());
```
```
// Output
in Vehicle(double maxMPH, String color)
In Vehicle.toString()
maxMPH = 100.0, color = blue
```

* Java recognizes that Vehicle is the constructed object.
    * we said new Vehicle(), not new Object().
    * Java will use the constructed type’s methods if it exists
        * If not, it continues to look up the chain of inherited methods until it finds a match.
        * If it doesn’t find a match, a compiler error occurs.
    * The feature of matching the type of Object instantiated with the appropriate method definition is called <b><i>dynamic binding</i></b> or <b><i>polymporphism</i></b>.

## Example

```
// in Car.java
    public String toString() {
        System.out.println("in Car.toString()");
        return super.toString() + ", make = " + make + ", model = " + model;
    }
```
```
// in Truck.java
    public String toString() {
        System.out.println("in Truck.toString()");
        return super.toString() + ", bedVolume = " + bedVolume;
    }
```
```
// in main
Car c = new Car(120, "black", "Pontiac", "GrandAM");
System.out.println(c.toString()); // calls Car.toString()

Truck t = new Truck(120, "black", "Pontiac", "GrandAM", 400);
System.out.println(t.toString()); // calls Truck.toString()
```

## Final methods and classes
* We can declare methods and classes as final.
    * Declaring a class as final prevents other classes from extending it.
    * Declaring a method as final prevents other classes from overriding it.

## Example
* Change `.toString()` in Vehicle to: `public final String toString() {`
    * See the compiler error saying it cannot override a final method.
* Change the class definition in Vehicle to: `public final class Vehicle {`
    * See the compiler error stating that it cannot inherit from Vehicle.

## A polymorphism Example

```
// in main
vehicleArray[0] = new Vehicle(100, "blue");
vehicleArray[1] = new Car(120, "black", "Pontiac", "GrandAM");
vehicleArray[2] = new Truck(120, "black", "Pontiac", "GrandAM", 400);

for (int i = 0;  i < 3; i++) {
    // call .toString() for each type of object (polymorphism)
    System.out.println(vehicleArray[i].toString());
    System.out.println("---");
}
```

* Note that the constructed type's `.toString` method is called.

## Memory Slicing

* Primitive type Example:

```
double x = 2.2;
int y = (int) x; // type casted
```

* Technically, primitive types are stored on the stack
* Memory slicing does occur in this case (i.e. only can represent the data within the size of the int).
* Note that Objects on the heap aren’t sliced...

```
Object o = new Vehicle(100, "blue");
System.out.println(o.toString());
```

* Technically, Vehicle is created on the "heap" in Java
* All Objects are created on the heap using "new"
* This includes all memory associated with the type that was constructed
* Object o is only able to call methods that are defined in the Object class
* .toString() in Vehicle is used due to polymorphism, which contains all memory for a Vehicle (no slicing).

## Casting Objects

* Several rules about this, see: [http://www.wideskills.com/java-tutorial/java-object-typecasting](http://www.wideskills.com/java-tutorial/java-object-typecasting)
* Example:

```
Vehicle v1 = (Vehicle) new Object(); // runtime error, ClassCastException
Vehicle v2 = (Vehicle) new Car(120, "black", "Pontiac", "GrandAM"); // OK

Car c = (Car) v2;
System.out.println(e.getModel()); // still OK. Object exists fully on heap

Object o = new Vehicle(100, "blue");
Vehicle v = (Vehicle) o; // OK
```

* As long as you’re casting compatible objects, then it's OK.
