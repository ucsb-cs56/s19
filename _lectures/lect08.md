---
num: "Lecture 8"
desc: " Interfaces, Switch Statements"
ready: true
lecture_date: 2019-04-24 17:00:00.00-7:00
---

# Interfaces
* In Java, a class may not extend multiple classes (unlike C++).
    * Though multiple inheritance is "kinda" possible by implementing many Interfaces.
* An interface is a mechanism for defining a "purely abstract class"
    * Similar to classes, Interfaces are considered as a type
    * Similar to abstract classes, Interface objects cannot be instantiated.
    * Interfaces may contain only public non-static methods and public static final fields (i.e. constants).
* It’s used when you want to specify a behavior that a class (and subclasses) need to support without specifying any implementation details.
* A class may implement multiple Interfaces.

## Example implementing the comparable interface

```
public interface Comparable
{
	// compares two objects: x.compareTo(y) such that
	// if x == y, return 0
	// if x < y, return -1
	// if x > y, return 1
    int compareTo(Object obj);
}
```

* Link to Comparable Interface in Java's API: [https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Comparable.html](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Comparable.html)

* For each method in an interface, you have to provide a contract explaining what the meaning of the method means.
    * What does the returned int mean when `compareTo` is called?
    * You have to specify this in writing. For example, in the API specs:

<b>int compareTo​(T o)</b>

<i>Compares this object with the specified object for order. Returns a negative integer, zero, or a positive integer as this object is less than, equal to, or greater than the specified object.</i>

<pre>
public abstract class Student extends Person <b>implements Comparable</b> {

// Implement the Interface’s method in the Student class
// We can compare students based on their PERM 
public int compareTo(Object o) {
    Student x = (Student) o; 
    if (studentID < x.perm) {
        return -1;
    } else if (perm > x.perm) {
        return 1;
    } else {
        return 0;
    }
}
</pre>

## Example Using compareTo with Student Objects

```
public static Student findMinStudent(Comparable[] c) {
    Student minStudent = (Student) c[0];
    
    for (int i = 1; i < c.length; i++) {
        if (c[i].compareTo(minStudent) < 0) {
            minStudent = (Student) c[i];
        }
    }
    return minStudent;
}
```
```
// in main
Student[] array = new Student[2];
array[0] = new NonResidentStudent("Richert", 21, 80498567, "Oregon");
array[1] = new DomesticStudent("Zorra", 18, 1234567, "Los Angeles");

Student minStudent = findMinStudent(array);

System.out.println(minStudent.toString());

System.out.println(array[0].compareTo(array[1]));
System.out.println(array[1].compareTo(array[0]));
```

# Switch Statements
* In many cases, you want to perform specific actions if a variable equals a certain value.
* A shorthanded way to write a multi-way if statement
* Can work with primitive types (byte, short, char, int), Strings (JDK 7), and some other cases (primitive wrappers (Integer, Byte, ...)).

## Example

```
int studentYear = 2;
String status;
switch (studentYear) { // switch block
	case 1: status = "Freshman";
		break;
	case 2: status = "Sophomore";
		break; // if this was removed, it will evaluate case 3.
	case 3: status = "Junior";
		break;
	case 4: status = "Senior";
		break;
	default: status = "Undefined";
		break;
} // end switch block
System.out.println(status);
```

* Good for testing specific equality. For ranges, consider using if blocks
    * "break" means to exit out of the switch block.
        * Without it, all cases will be evaluated until a break is encountered.
    * "default" means if there are no matching cases, then the code in that block is executed.
    * "default" can be put anywhere in the switch block, but it’s good practice to leave it at the end.

* Can also check for multiple values in a single case:

## Example with multiple values for a single case block

```
switch (studentYear) { // switch block
    case 1: case 2:
        status = "LowerDivision";
        break;

    case 3: case 4:
        status = "UpperDivision";
        break;
    default: status = “Undefined”;
        break;
} // end switch block
System.out.println(status);
```

## Example using String values in switch cases

```
String studentYear = "junior";
String status;

switch (studentYear) { // switch block
case "freshman": case "sophomore":
        status = "LowerDivision";
        break;

case "junior": case "senior":
        status = "UpperDivision";
        break;
default: status = "Undefined";
        break;
} // end switch block
System.out.println(status);
```
