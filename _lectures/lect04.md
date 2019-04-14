---
num: "Lecture 4"
desc: "User Input, Random, ArrayLists"
ready: true
lecture_date: 2019-04-10 17:00:00.00-7:00
---

# Command Line Arguments

* Similar to C++, we can pass in content into our program when executing it using command line arguments.
* This is what the String[] args portion in the main method is.
* All of the command line arguments are of String types and we need to convert them to numbers if needed.

## Example

```
$ java Lecture 10 20
```
```
public class Lecture {

    public static void main(String args[]) {
        System.out.println(args[0]); // 10
        System.out.println(args[1]); // 20
        System.out.println(args[0] + args[1]); // 1020
        System.out.println(Integer.parseInt(args[0]) + 
        Integer.parseInt(args[1])); // 30
    }
}
```

# Console Input

* A way for users to interact with their programs is by using the console for input.
    * Your program can prompt users for information and users can enter information into the program by typing it in the console.
    * There are several ways to do this, but we will use the `Scanner` object for this purpose.
    * The Scanner object can be used for several types of I/O interaction including files (more on this later).

## Example

```
// Lecture.java
import java.util.Scanner; // import Scanner object

Scanner s = new Scanner(System.in);
String t = s.nextLine();
System.out.println(t);
s.close(); // important so resources aren’t wasted.
```

* The program execution stalls until the user enters something. Then the program simply prints out what was entered.
* Another example where user enters a name:

```
Scanner s = new Scanner(System.in);
System.out.print("Enter your name: ");
String first = s.next(); // returns a value up to a newline or space
String last = s.next();

System.out.println(first);
System.out.println(last);

s.close();
```

* Note: There are many other ways you can finagle with console input (i.e. set delimiters, etc.).
* There are also ways to safeguard and check for user the input being correct
    * We’ll get to Exception handling soon.

# Random Number Generation
* Creating Random behavior / values in programming is  useful.
* Java’s API gives us the Random object (java.util.Random).

## Example

```
// Lecture.java

import java.util.Random;

// in main
Random generator = new Random();
int randomInt = generator.nextInt(4); // 0 -> 3
double randomDouble = generator.nextDouble(); // 0 -> 0.9999999...
System.out.println(randomInt); 
System.out.println(randomDouble);
int x = generator.nextInt(3) – 1; // returns [-1, 0, 1]
int y = generator.nextInt(6) + 5; // returns [5, 6, 7, 8, 9, 10]
```

# Arrays

* You can allocate memory to store items of the same type with arrays.
* Recall, arrays are indexed from 0 to n – 1.
* The items in a Java array must be of the same type. 
* Arrays are objects in Java (i.e. they’re not primitive types like in C++).

## Example

```
// Lecture.java

// in main

// declare array
int[] array;

// assign array of size 100 ints (i.e. 100 * 4 bytes)
array = new int[100]; // initializes ints to 0 

// initialize elements
array[0] = 10;
array[1] = 20;

// extract elements
System.out.println(array[1]); // prints 20

// initialization shortcut
int[] array2 = {10, 20, 30}; // creates an array of size 3 ints

System.out.println(array2[2]); // prints 30
System.out.println(array[4]); // returns 0
System.out.println(array2[4]); // ERROR, ArrayIndexOutOfBounds
```

* Note that since an Array is an object, we can access things like the array size using the `.length` field.
    * `int x = array.length; // .length is not a method, but a value`
* `.clone()` may be useful to copy the contents of the array.
    * `int[] array3 = array2.clone();`

## Example

```
int[] array3 = array2.clone();
if (array3 == array2) {
    System.out.println("array3 == array2");
} else {
    System.out.println("array3 != array2"); // Two separate arrays
}

// loop through array (print out values).
for (int i = 0; i < array3.length; i++) {
    System.out.println(array3[i]);
} // 10, 20, 30

System.out.println("--");

// Note the "for-each" loop syntax
for (int x : array3) {
    System.out.println(x);
}
```

## ArrayLists

* Arrays need to be defined with a predetermined size.
* If you try and access / add to a position past the size, an error occurs.
* Sometimes we don’t know what the size limit is and we want to expand our array automatically.
* ArrayList is the solution to this!
    * ArrayLists are similar to C++ std::vector

```
import java.util.ArrayList;

// constructs an ArrayList object containing Integer objects
// initially, the list is empty.
ArrayList<Integer> x = new ArrayList<Integer>();
```

* The <Integer> is known as a <b>Generic</b> type.
    * Similar to templates in C++
* Basically, we don’t know what an ArrayList can hold and we want to make it general enough where it can hold any type the programmer wants.
    * In this case, we want to hold Integers.
    * But we can also hold Booleans, Doubles, Objects, Scanners, Students, etc.
    * Without Generics, we would write a generic ArrayList implementation for every possible type...
        * ... crazy.

## Example: Adding to an ArrayList

```
ArrayList<Integer> x = new ArrayList<Integer>();

x.add(5);
x.add(10);
x.add(15);
x.add(20);

for (int i = 0; i < x.size(); i++) {
	System.out.println(x.get(i));
}
```

## Example: Setting items in an ArrayList

* We can change an existing item in the ArrayList using `.set()`
    * `x.set(3,100);`
        * Note, `[ ]` operator won’t work for ArrayList.
    * Though if we try to set something past the size, we get an error.
        * `x.set(100,100); // out of bounds`

## Example: Removing from an ArrayList

* We can remove existing elements by index.
    * `x.remove(1); // Removes 2nd item in list`
* We can remove existing elements by Object value.
    * `x.remove(10); // ERROR? But can’t we remove objects?`
    * `x.remove((Integer) 10) // or x.remove(new Integer(10))`
        * In this case, the first occurrence of the object is removed from the ArrayList.

## Some more examples using ArrayLists

```
// Sums up the integers in the ArrayList
public static int sum(ArrayList<Integer> x) {
    int sum = 0;
    for (int i = 0; i < x.size(); i++) {
        sum += x.get(i);
    }
    return sum;
}

// Swaps two values in the arrayList based on index
public static void swap(ArrayList<Integer> x, int i, int j) {
    int temp = x.get(i);
    x.set(i, x.get(j));
    x.set(j, temp);
}
	
// Returns the position of the first occurence of the value.
public static int getFirstPosition(ArrayList<Integer> x, int value) {
    int i = 0;
    while (i < x.size()) {
        if (x.get(i) == value)
            return i;
        else
            i++;
    }
    return -1;
}

// ---
// in main
ArrayList<Integer> x = new ArrayList<Integer>();

x.add(5);
x.add(10);
x.add(15);
x.add(20);

System.out.println(sum(x)); // 50
swap(x, 0, 3);
printArrayList(x); // 20, 10, 15, 5
System.out.println(getFirstPosition(x, 15)); // 2
```

# ArrayLists containing ArrayLists

* ArrayLists can maintain a list of any Object.
    * ... including other ArrayLists
    * We’ll talk more about this kind of structure when talking about 2D arrays.

```
// Lecture.java

// in main
ArrayList<ArrayList<Integer>> listOfLists = new 
ArrayList<ArrayList<Integer>>();

ArrayList<Integer> arrayInt1 = new ArrayList<Integer>();
arrayInt1.add(100);
arrayInt1.add(200);
arrayInt1.add(300);	

ArrayList<Integer> arrayInt2 = new ArrayList<Integer>();
arrayInt2.add(-100);
arrayInt2.add(-200);	
listOfLists.add(arrayInt1);
listOfLists.add(arrayInt2);

// Print all ints in the listOfLists
for (int i = 0; i < listOfLists.size(); i++) {
    ArrayList<Integer> intList = listOfLists.get(i);
    for (int j = 0; j < intList.size(); j++) {
        System.out.println(intList.get(j));
    }
}
```

# Stack Overflow
* When a program crashes due to running out of memory in the call stack.
* The `java.lang.StackOverflowError` Exception is thrown.

## Example: Factorial

* Example of stack overflow with infinite recursion

```
public static int factorial(int n) {
    // No base case
//  if (n == 1) {
//      return 1;
//  }
    return n * factorial(n - 1);
}
```

* Results in a stack overflow. Memory runs out due to increasing stack size.
* Will the following code produce a stack overflow error?

```
public static int factorial(int n) {
String s;
    Runtime runtime = Runtime.getRuntime();
    while (true) {
        s = new String();
        System.out.println("Used Memory:" + (runtime.totalMemory() - runtime.freeMemory()) / (1024 * 1024)); // measured in bytes
    }
}
```

* Stack is not indefinitely growing.
* Heap is growing (new allocations are made continuously), but garbage collection eventually cleans up unreferenced objects.
* Stack overflow doesn't occur.
