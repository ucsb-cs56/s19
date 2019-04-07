---
num: "Lecture 1"
desc: "Introduction"
ready: true
lecture_date: 2019-04-01 17:00:00.00-7:00
---

# Course Syllabus

Be sure to read the course syllabus for information on the logistics of this course.

https://ucsb-cs56.github.io/s19/info/syllabus/

# Why use Java?
* Large developer support and 3rd party libraries
* Garbage collection
* Object Oriented Framework
* Optimized language (“Just in Time” Compiling)
* Portability (can write once, run anywhere)

# Hello World Program

```
// Lecture.java
public class HelloWorld {
    public static void main(String[] args) { // or String args[]
        System.out.println(“Hello World!”);
    }
}
```

## Executing the program via the command line

```
$ javac Lecture.java
$ java Lecture
Hello World!
$
```

Note that the `javac Lecture.java` command will generate a `Lecture.class` file.

# Compiling Java Code

* One of the big benefits of a Java program is that it is platform independent.
* We can write java code on any platform and then the java compiler (or JVM compiler) creates a lower-level representation in a .class file (bytecode)
    * This .class file is portable and can be transferred to any platform running Java’s Virtual Machine.
    * You can’t do this with C/C++ - the .o and executable files are platform dependent and are not portable.
    * The Java Virtual Machine (JVM) is a piece of software that executes the bytecode and also manages memory for Java applications.

## Main Stepts of Executing a Java Program
1.	.java source code (the java code we write)
2. javac converts .java source code into bytecode
3. Execute bytecode on any device with the JVM installed

# Some Java Basics

## Comments
    * Can comment blocks of code with `/* … */`
    * Can comment single lines of code with `//`

## Identifiers
* Can start with an underscore, $, or letter and can contain letters, digits, or underscore.
    * Examples: `var1, _temp, $total`
    * Normally, Java programmers do not use `_` or `$` in variable names.
        * Though there are a few exceptions such as defining constants.
    * Identifiers are case sensitive.
        * `Var` != `var` – these are two different variable names.
    * Cannot use predefined keywords
        * if, else, for, do, new, public, private, final, byte, short, int, ...
        * For an entire list of Java keywords, see [http://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html)

## Java Primitive Types

* Integers
    * `byte` (1 byte)
    * `short` (2 bytes)
    * `int` (4 bytes)
    * `long` (8 bytes)

* Floating Point Numbers
    * `float` (4 bytes)
    * `double` (8 bytes)

* Characters
    * `char` (2 bytes)

* Boolean
    * `boolean` (1 byte)

Note that primitive types in Java are stored on the stack.

## Declaring Variables
* You can declare variables on single lines (one per line):
    * `int x;`
    * `int y;`
* You can combine multiple declarations in the same line if the variables are of the same type:
    * `int x, y;`
* Usually variable names are created with camelCasing without underscores.
    * Example: `isEmpty`, `numOfStudents`
* Every variable must be initialized before it is used.
* The life of a variable usually is valid within the block it was declared in between `{ }` (in general).
    * Also known as the <b>scope</b> of the variable.

## Assignment Statements

Example:

```
float sum = 0.0;
int a = 1, b = 2, c = 3;
```

* You cannot assign variables with different types
    * `int a = “a”; // illegal!`
    * `int x = 1; double b = x; // legal`
    * `double c = 1.1; int y = c; // illegal!`
* Java will not compile if assigning a type will result in a loss of precision, unless we specifically type cast the type.

```
int x = 10;
double y = x;
y += 0.1; // y = y + 0.1
System.out.println(y);
x = y; // ERROR (if loss of precision occurs, Java won't compile)
x = (int) y; // type casting y into an int
System.out.println(x);
```

## Objects
* The `new` keyword is used to allocate memory for a new object.
* All objects in Java are dynamically allocated on the heap.
* The only values on the stack are Java primitive types and object <b>references</b>.
    * You can think of Java similar to C++ where all objects are dynamically allocated on the heap and all object variables are pointers.
    * However, Java doesn't use pointers in the same sense that C++ does.
        * For example, you cannot do pointer arithmetic on a reference.

Example:

```
Object x = new Object();
Object y  = x; // y and x refer to the same Object
y = new Object(); // y refers to a new object
x = y; 	// x and y refer to the same object.

// Nothing points to the original object. Java’s 
// garbage collection automatically removes it from
// memory.
// Note: `==` compares object references, not values.
```

## Overflow

* Java does not check for overflow

Example:

```
int i = Integer.MAX_VALUE;
System.out.println(i); // 2147483647
System.out.println(i + 1); // -2147483648
System.out.println(Integer.MIN_VALUE); // -2147483648
// No compilation / runtime error. Programmer must check these cases
```

## Increment / Decrement
* `x++;` // post-increment – increments x by 1 after the expression is evaluated.
* `++x;` // pre-increment – increments x by 1 before expression is evaluated.
* `x--;` // post-decrement – decrements x by 1 after the expression is evaluated.
* `--x;` // pre-decrement – decrements x by 1 before expression is evaluated.

Example:

```
int x = 1;
System.out.println(x++); // prints 1
System.out.println(x); // prints 2

x = 1;
System.out.println(++x); // prints 2
System.out.println(x); // prints 2
```

## Shortcut Expressions

Similar to C++. Some Examples:

```
v1 += v2; // v1 = v1 + v2
v1 -= v2; // v1 = v1 – v2
v1 *= v2; // v1 = v1 * v2
v1 /= v2; // v1 = v1 / v2
```

## Arithmetic Conversions

* If two operands of different types are used, result is converted to the "highest" type.

```
System.out.println(5 / 3); // 1
System.out.println(5.0 / 2); // 2.5
```

## Typecasting

* Changing the type of a variable during an operation.

Example:

```
int x = 5;
int y = 2;
double answer = (double) x / y; // 2.5 even though two ints used
```
