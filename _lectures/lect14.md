---
num: "Lecture 14"
desc: "More on Interface Types, Java Collections"
ready: true
lecture_date: 2019-05-20 17:00:00.00-7:00
---

# Extending Interfaces

* In Java, you can have interfaces extending other existing Interfaces
    * Usually done when you want to make an Interface with more specific functionality.
    * Polymorphism allows the correct method to be called on a specific object.

## Example

```
public interface InterfaceA {
    public abstract void method1();
    public abstract void method2();
}
```
```
public interface InterfaceB extends InterfaceA {
    public abstract void method3();
}
```
```
public interface InterfaceC {
    public abstract void method4();
}
```
```
public class ClassA implements InterfaceA {
    public void method1() { System.out.println("ClassA.method1"); }
    public void method2() { System.out.println("ClassA.method2"); }
}
```
```
public class ClassB implements InterfaceB {
    public void method1() { System.out.println("ClassB.method1"); }
    public void method2() { System.out.println("ClassB.method2"); }
    public void method3() { System.out.println("ClassB.method3"); }
}
```
```
public class ClassC implements InterfaceB, InterfaceC  {
    public void method1() { System.out.println("ClassC.method1"); }
    public void method2() { System.out.println("ClassC.method2"); }
    public void method3() { System.out.println("ClassC.method3"); }
    public void method4() { System.out.println("ClassC.method4"); }
}
```
```
// Lecture.java

public class Lecture {

    public static void main(String[] args) {
    ClassA a = new ClassA();
    ClassB b = new ClassB();
    ClassC c = new ClassC();

    ArrayList<InterfaceA> listA = new ArrayList<InterfaceA>();
    ArrayList<InterfaceB> listB = new ArrayList<InterfaceB>();
    ArrayList<InterfaceC> listC = new ArrayList<InterfaceC>();

    listA.add(a);
    listA.add(b);
    listA.add(c);

    //listB.add(a); // compilation error
    listB.add(b);
    listB.add(c);

    //listC.add(a); // Compilation error!
    //listC.add(b); // Compilation error!
    listC.add(c);

    for (InterfaceA item : listA) {
        item.method1();
        item.method2();
    }
/*
Output:
ClassA.method1
ClassA.method2
ClassB.method1
ClassB.method2
ClassC.method1
ClassC.method2
*/
    for (InterfaceB item : listB) {
        item.method1();
        item.method2();
        item.method3();
    }
/*
Output:
ClassB.method1
ClassB.method2
ClassB.method3
ClassC.method1
ClassC.method2
ClassC.method3
*/
    for (InterfaceC item : listC) {
        item.method4();
    }
/*
Output:
ClassC.method4
*/
    }
}
```

# Collections

* Collections provide mechanisms for inserting and retrieving various types of data.
    * For example, ArrayLists and LinkedLists are collections that the Java API provides us.

## Collection Interface

* [https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collection.html](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collection.html)
* An unordered group of objects allowing duplicate entries
* Common methods:
    * `int .size()`
    * `boolean .add(item)`
    * `boolean .remove(item)`
    * `boolean .isEmpty()`
    * etc.

## List Interface

* [https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html)
* An unordered group of objects allowing duplicate entries that are indexed.
* Common methods:
    * `Object .get(index)`
    * `int .indexOf(item)`
    * etc.
* Examples: LinkedList, ArrayList

## Set

* [https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Set.html](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Set.html)
* A collection containing no duplicate elements
* Ensures that no pair of objects are equal to each other
* For every object in the set, `.equals(item)` should return false

## Map

* [https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Map.html](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Map.html)
* Technically doesn’t extend the Collection interface, but it can be considered a Collection of values.
* A collection of items stored as (key, value) pairs.
* Keys are unique and can only map to one Object (but the Object can contain multiple values)
* Keys are represented as any object (normally strings or ints)
* Values can be represented as any Object
* Common methods:
    * `boolean .containsKey(key)`
    * `boolean containsValue(value)`
    * `Object .get(key)`
    * `Object .put(key, value)` // returns previous value for key or null
    * `boolean .remove(key, value)` // returns true if the key mapped to the value and was removed
    * `Object .remove(key)` // returns  previous value for key or null
* There are a lot of interfaces defining the functionality for specific types of Collections. For example:
    * [HashMap](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/HashMap.html): Implemented with a hash table.
    * [TreeMap](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/TreeMap.html): Implemented with a balanced tree.

## Examples

```
// HashSet implements Set Interface (no guaranteed order)
HashSet<String> s = new HashSet<String>();
System.out.println(s.add("S1")); //true - successful 
System.out.println(s.add("S2")); // true - successful
System.out.println(s.add("S2")); // false - unsuccessful
System.out.println(s.size()); // 2
System.out.println(s.contains("S1")); // true
System.out.println(s.remove("S1")); // returns true AND removes
System.out.println(s.contains("S1")); // false

// HashMap
HashMap<Integer, String> s = new HashMap<Integer,String>();
System.out.println(s.put(0, "Richert")); // null
System.out.println(s.put(1, "Wang")); // null
System.out.println(s.put(0, "RichARD")); // Richert - returns old value	

System.out.println(s.containsKey(1)); // true
System.out.println(s.containsKey(10)); // false
System.out.println(s.containsValue("Richert")); // false
System.out.println(s.containsValue("RichARD")); // true

// Get Value for specific key
System.out.println(s.get(1)); // Wang

// Traverse Keys
for (Integer i : s.keySet()) {
    System.out.println(i);
}

// Traverse values
for (String i : s.values()) {
    System.out.println(i);
}

System.out.println(s.remove(0)); // RichARD
System.out.println(s.containsValue("RichARD")); // false
System.out.println(s.remove(1, "fjskj")); // false
System.out.println(s.remove(1, "Wang")); // true
System.out.println(s.size()); // 0
```
