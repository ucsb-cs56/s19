---
num: "Lecture 7"
desc: "Abstract Methods and Classes"
ready: true
lecture_date: 2019-04-22 17:00:00.00-7:00
---

# Abstract Methods
* Sometimes it doesn’t make sense for a class to provide method definitions
* For example
    * A shape’s area is dependent on the type of shape.
    * ... or an Animal class may make a sound, but that sound is dependent on the Animal.
    * ... or a Student may pay different tuitions (international students, out-of-state, international)... or the Unit limit may differ...
* If it doesn’t make sense for the class to provide a method definition, but know that the method should be implemented somewhere in the subclasses.
    * Abstract methods can be used!
    * A method can be declared as "abstract" if there is no sensible solution to provide an implementation in the base class.

## Example

```
// Person.java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String toString() {
    	return "Name: " + name + ", age: " + age;
    }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getPerm() { return age; }
    public void setPerm(int age) { this.age = age; }
}
```
```
// Student.java
public class Student extends Person {

    private int perm;

    public Student(String name, int age, int perm) {
        super(name, age); // calls Person(name, age);
        this.perm = perm;
    }

    public int getPerm() { return perm; }	
    public void setPerm(int perm) { this.perm = perm; }

    public String toString() {
        System.out.println("In Student.toString()");
        return super.toString() + ", perm: " + perm;
    }
}
```

* What if we wanted to add a method to calculate quarterly fees?
* The type of student (domestic, out-of-state, international, extension, ...) may all pay different amounts.
    * But we know tuition applies to all students.
* We can add "abstract" to Student class.
* and add an abstract method: calculateQuarterlyFees()

<pre>
public <b>abstract</b> class Student extends Person {
// ...
public <b>abstract</b> double calculateQuarterlyFees();
</pre>

Subclasses that extend an abstract class have two choices:
* Fill in the "hole" by overriding and providing an implementation for the abstract methods in the base class.
* Declare itself as abstract and force subclasses to implement its abstract methods.
    * It’s not necessary to redeclare the abstract method in the subclass, but I think it’s good style so people who need to extend the abstract class won’t “dig” through the hierarchy.

## Example

```
// NonResidentStudent.java
public class NonResidentStudent extends Student {

    private String homeState;

    public NonResidentStudent(String name, int age, int perm,
    String homeState) {
        super(name, age, perm);
        this.homeState = homeState;
    }

    public String getHomeState() { return homeState; }
    public void setHomeState(String homeState) {
        this.homeState = homeState;
    }

    public double calculateQuarterlyFees() {
        return 42900 / 3;
    }
}
```
```
// DomesticStudent.java
public class DomesticStudent extends Student {

    private String county;

    public DomesticStudent(String name, int age, int perm, String county) {
        super(name, age, perm);
        this.county = county;
    }

    public String getCounty() { return county; }
    public void setCounty(String county) { this.county = county; }

    public double calculateQuarterlyFees() {
        return 13900 / 3;
    }
}
```

## Example using polymorphism with Abstract Methods

```
// in main
Student[] array = new Student[2];
array[0] = new NonResidentStudent("Richert", 21, 80498567, "Oregon");
array[1] = new DomesticStudent("Zorra", 18, 1234567, "Los Angeles");

for (int i = 0; i < 2; i++) {
    System.out.println(array[i].getName());

    // Abstract methods are visible to Student objects
    // Polymorphism allows the correct calculateQuarterlyFees method to be called.
    System.out.format("%.2f\n",array[i].calculateQuarterlyFees()); // format two decimal places
}
```
