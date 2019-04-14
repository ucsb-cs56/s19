---
num: "Lecture 3"
desc: "Defining Classes"
ready: true
lecture_date: 2019-04-08 17:00:00.00-7:00
---

# Public vs. Private

* Within a class, we can declare class variables with certain access:
    * Public, private, and protected
        * We won’t go into details for protected yet.
* Public means any instantiated object of that class can directly access a public variable / method.
* Private means only the class itself can have direct access to that variable / method.

## Example of Defining a Car Class

```
// Car.java
public class Car {
    public double maxMPH; // class variables should be private
    private String make;
    private String model;
    private int year;
    private String color;

    // constructor (used to initialize class variables)
    public Car(double mph, String make, String model, int year, String color) {
        maxMPH = mph;
        this.make = make;
        this.model = model;
        this.year = year;
        this.color = color;
    }
}
```
```
// Lecture.java
public static void main(String args[]) {
    Car car = new Car(160, "Chevrolet", "Equinox", 2011, "black");
    System.out.println(car.maxMPH); // OK
    System.out.println(car.make); // ERROR: make is private
}
```

# The "this" keyword

* "this" refers to this class
* Used mainly to disambiguate variables within the class
* Without "this" in the example constructor above, Java thinks you’re referring to the parameter name instead of class member (due to scoping).

* It is possible to make everything public (and it’s also possible to write your entire program in the main method!)… but bad style to do so.
    * The object-oriented programming way of doing things is to hide certain functionality from consumers of the class and provide a public interface to use in their programs.
    * Encapsulates the "details" of the class implementation.
    * Allow programmers to provide an API to use the class.
        * Helps prevent unintended side-effects (programmers can’t "accidentally" break functionality within a class).

# Accessor (getter) / Mutator (setter) Methods

* In order for a consumer of the class to modify private fields in the class, public methods are provided that change the private fields.

## Example

```
// Car.java
    // Access / getter methods
    public double getMaxMPH() { return maxMPH; }
    public String getMake() { return make; }
    public String getModel() { return model; }
    public int getYear() { return year; }
    public String getColor() { return color; }

// Mutator / setter methods
    public void setMaxMPH(int maxMPH) { this.maxMPH = maxMPH; }
    public void setMake(String make) { this.make = make; }
    public void setModel(String model) { this.model = model; }
    public void setYear(int year) { this.year = year; }
    public void setColor(String color) { this.color = color; }
```
```
// Lecture.java
// in main
Car car2 = new Car(120, "Pontiac", "GrandAM", 2001, "silver");	
System.out.println(car2.getMaxMPH());
System.out.println(car2.getMake());
System.out.println(car2.getModel());
System.out.println(car2.getYear());
System.out.println(car2.getColor());
car2.setColor("pink");
System.out.println(car2.getColor());
```

# Static Class Variables
* Static class variables belong to the class instead of a specific instance of an object.
* Static variables are shared among ALL instances of the class.

## Example

```
// Car.java
// Additional class fields in Car
    private static int staticCounter = 0;
    private int counter = 0;

// Update Car Constructor to increment the counters when constructed
    public Car(double mph, String make, String model, int year, 
        String color) {
        maxMPH = mph;
        this.make = make;
        this.model = model;
        this.year = year;
        this.color = color;
        staticCounter++;
        counter++;
    }

// Accessor methods for counters
    public int getStaticCounter() { return staticCounter; }	
    public int getCounter() { return counter; }
```
```
// Lecture.java
// in main()
    Car car1 = new Car(140, "Chevrolet", "Equinox", 2011, "black");
    System.out.println(car1.getStaticCounter()); // 1
    System.out.println(car1.getCounter()); // 1
            
    Car car2 = new Car(120, "Pontiac", "GrandAM", 2001, "silver");
    System.out.println(car2.getStaticCounter()); // 2
    System.out.println(car2.getCounter()); // 1
```

# Static Methods
* Methods that do not require an instance of an object (i.e. a constructed object) for it to be called.
* Does it make sense to call a method without an object instantiation?
    * Consider a method convertMilesToKilometers(double miles).
    * The functionality of this is not dependent on the state of the object.

## Example

```
// in Car.java
public static double convertMilesToKilometers(double miles) {
    return miles * 1.60934;
}
```
```
// Lecture.java
// in main()
System.out.println(Car.convertMilesToKilometers(1)); // 1.604
```

# Method Overloading
* Combination between methods with the same name and parameter types is called the method signature.
* Within a class, we can have methods with the same name, but not the same signature, for example:
    * `public void setMaxMPH(int mph)`
    * `public void setMaxMPH(short mph)`
    * `public void setMaxMPH(String mph)`

```
car1.setMaxMPH(1); // calls the int
car1.setMaxMPH(1.0); // calls the double
short x = 3;
car1.setMaxMPH(x); // calls the short
car1.setMaxMPH("1"); // calls the String
```

# Overloading Constructors

* Constructors can be overloaded as well.
* A default constructor is a constructor without any parameters.
    * If no constructor is provided, Java makes one that sets the class variables to default values.
* If any constructor is defined, Java does not supply a default constructor and you must write one.

# Pass by Value

* Pass by value is when a copy of a value is used, not the original variable itself.
* Any changes to this variable will not remain changed when out of scope.

## Example

```
// Lecture.java
public static void addThree(int x) {
	x = x + 3;
	System.out.println(x); // 8
}

// in main
int x = 5;
addThree(x);
System.out.println("in main. x = " + x); // 5
```

# Pass by Reference

* Pass by reference is when the original variable is passed into a method. Any changes to this variable will remain changed when out of scope.
* Can Java pass variables by reference?

```
public static void main(String args[]) {
    Car car1 = new Car(140, "Chevrolet", "Equinox", 2011, "black");
    System.out.println(car1.getColor()); // black
    printColor(car1);
    System.out.println(car1.getColor()); // blue
}	
	
public static void printColor(Car c) {
    c.setColor("blue");
    System.out.println(c.getColor()); // blue
```

* Seems like Java objects are passed by reference, right?
    * Nope! Java passes everything by value.
    * Object <b>references</b> are passed by value.
    * We can see this in the example below:

```
// Lecture.java
public static void printColor(Car c) {
    // new reference
    c = new Car(140, "Pontiac", "GrandAM", 2001, "silver");
    System.out.println(c.getColor()); // silver
}

public static void main(String args[]) {
    Car car1 = new Car(140, "Chevrolet", "Equinox", 2011, "black");
    System.out.println(car1.getColor()); // black
    printColor(car1);
    System.out.println(car1.getColor()); // still black
}
```

# Copy Constructor

* Special type of constructor that takes in an instantiated object of itself and creates a new copy with the same values.
* Be careful with assigning objects – object assignments are simply copying reference values!

## Example - Car class containing an Engine class

```
// Engine.java
public class Engine {
    private String engineType;

    public Engine(String engineType) {
        this.engineType = engineType;
    }

    public String getEngineType() { return engineType; }
    public void setEngineType(String engineType) {
        this.engineType = engineType;
    }
}
```
```
// in Car.java
// add Engine private field
private Engine engine;

// car constructor - add Engine parameter
public Car(double mph, String make, String model, int year,  String color, Engine engine) {
// ...
    this.engine = engine;
}

// car copy constructor - assign engine
public Car(Car c) {
// ...
    this.engine = c.getEngine(); // here is the shallow copy
}

// Getter / Setter for Engine field
public Engine getEngine() { return engine; }
public void setEngine(Engine engine) { this.engine = engine; }
```
```
// Lecture.java
    public static void main(String args[]) {
        Engine e = new Engine("v4");
        Car car1 = new Car(140, "Chevrolet", "Equinox", 2011, "black", e);
        Car car2 = new Car(car1);
        
        System.out.println(car1.getEngine().getEngineType()); // v4
        car2.getEngine().setEngineType("v6");
        System.out.println(car1.getEngine().getEngineType()); // v6 SHALLOW COPY!
    }
```

* In order to do a deep copy, the Car object will need its own memory location for its Engine object.

```
public Car(Car c) {
// ...
        this.engine = new Engine(c.getEngine().getEngineType()); // Deep copy
}
```
