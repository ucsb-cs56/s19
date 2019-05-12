---
num: "Lecture 10"
desc: "Generics, Multi-dimensional Arrays"
ready: true
lecture_date: 2019-05-06 17:00:00.00-7:00
---

# Generics
* We’ve seen how Generics can be used with ArrayLists.
* A programmer can create their own Generic representations.
* Similar concept to C++ template types, but…
    * In Java, generic types cannot be primitive (though we have use object wrappers for primitive types).

# Generic Methods
* Assume we want to write a method that takes an array, gets the last item of an array, and returns that item.
* We could write the same method for all different parameter types?
    * Crazy ...
* We can write a generic method to account for ALL types of possible arrays and return that specific type.

## Example

```
// defining a method to be generic
public static <T> T getLastItem(T[] array) {
    if (array.length > 0) {
        return array[array.length - 1];
    }
    return null;
}
```
```
// in main
Integer[] intArray = {1,2,3}; // note: int[] error, requires Integer object
Double[] doubleArray = {1.1, 2.2, 3.3};
String[] stringArray = {"I", "<3", "CS56"};

System.out.println(getLastItem(intArray));
System.out.println(getLastItem(doubleArray));
System.out.println(getLastItem(stringArray));
```

# Generic Classes
* We can also create entire classes that are generic.
* Assume we want to write a simple class storing a pair of values (both of the same type).

## Example Pair class with one generic type

```
public class Pair<T> {
    private T first;
    private T second;

    public Pair(T first, T second) {
        this.first = first;
        this.second = second;
    }

    public T getFirst() {
        return first;
    }

    public T getSecond() {
        return second;
    }

    public void print() {
        System.out.println(first + ", " + second);
    }
}
```
```
// in main
Pair<Integer> pair = new Pair<Integer>(1,2);
System.out.println(pair.getFirst() + pair.getSecond());
pair.print();
```

* The above class assumes first and second values are of the same type.
* We may not want to make that restriction and attempt to store a Pair where first and second can be of any Object type.
* When the Pair object is constructed, the appropriate types for first and second are inferred.

## Example Pair class with two generic types

```
public class Pair<T,U> {
    private T first;
    private U second;

    public Pair(T first, U second) {
        this.first = first;
        this.second = second;
    }

    public T getFirst() {
        return first;
    }

    public U getSecond() {
        return second;
    }

    public void print() {
        System.out.println(first + ", " + second);
    }
}
```
```
// in main
Pair<Integer, String> pair = new Pair<Integer, String>(1,"2");
System.out.println(pair.getFirst() + pair.getSecond());
pair.print();
```

# 2D Arrays
* Arrays we’ve been talking about so far have been a one dimensional (i.e. a simple list).
    * We kinda saw 2D Arrays before with an ArrayList of ArrayLists example
* We can actually organize data into multiple dimensions with multi-dimensional arrays.
*  good way to think of it is an array of arrays...

## Example 4x5 Grid of int values

```
// in main
int[][] int2d = new int[4][5];

// traverse the entire 2D array structure and print it out in a matrix
for (int i = 0; i < int2d.length; i++) {
    for (int j = 0; j < int2d[i].length; j++) {
        System.out.print(int2d[i][j] + " ");
    }
    System.out.println();
}
```

* Under-the-hood, this is organized like 4 one-dimensional arrays of size 5 in memory.
* We can set / get values using [x][y] notation.
* x represents the row while y represents column.
* Remember the game battleship?

```
int2d[1][3] = 8;

0 0 0 0 0
0 0 0 8 0
0 0 0 0 0
0 0 0 0 0
```

* Another way to visualize the structure of 4 one-dimensional arrays of size 5:


```
for (int i = 0; i < int2d.length; i++) {
    System.out.print("|");
    for (int j = 0; j < int2d[i].length; j++) {
        System.out.print(";");
        System.out.print(int2d[i][j] + " ");
    }
}
```

# Multi-dimensional Arrays

* We can expand the dimensions of arrays. For example, a 3D array can be initialized:

```
int [][][] int3d = new int[2][3][2];
```

* Under-the-hood, this is organized like 2 two-dimensional arrays of size 3 x 2 in memory.
* One way to visualize a 3D array structure:

```
int3d[1][1][1] = 1;

for (int i = 0; i < int3d.length; i++) {
    System.out.print("|");
    for (int j = 0; j < int3d[0].length;j++) {
        System.out.print(";");
        for (int k = 0; k < int3d[0][0].length;k++) {
            System.out.print(int3d[i][j][k]);
        }
    }
}
```

