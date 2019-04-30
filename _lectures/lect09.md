---
num: "Lecture 9"
desc: "Midterm Review, File IO"
ready: true
lecture_date: 2019-04-29 17:00:00.00-7:00
---

```
Midterm Review

Logistics
- Bring studentID and writing utensil
- No electronic devices
- No notes, no books

Format
- Mix of questions on lectures, homeworks, readings,
and labs
- Short Answers
    - Briefly define, describe, state, ...
- Write code
- Fill in the blank, complete the table, select answers, ....
- Given code, write the output
- Given a statement, tell me if it's true or false
    - If false, state why
- ...
- Expect it to take ~1 hour, but you have the entire class period (5 - 6:15pm)
- Will cover a broad range of topics since the start of class, but probably not everything.

Topics
- Will cover everything up to Wednesday's lecture (4/24)

Java Basics
- Variable names
- Primitive Types
    - Sizes
    - Typecasting
- Objects
    - Creating new Objects
    - Object variables (references)
    - .equals (==) operator
    - Object class (Base class for all objects)
- Overflow
- Operator conversions for certain Types
    - Example: (2/3) vs. (2.0 / 3)

Strings
- Multiple ways to create new Strings
- Escape characters
    - not all, but at least the ones mentioned in lecture
- Methods
    - .length, .substring, .toUpperCase, .toLowerCase
    .charAt, .equals (how does this differ from Object.equals), .equalsIgnoreCase, .compareTo
- Concatenation
- Conversion from String to Int and vice-versa
    - Can also convert to doubles, floats, etc., but will only expect knowledge for String <-> ints
- String pool

Control Flow
- Similar to C++
- if statements
- Relational operators (==, !=, <, ...)
- Logical operators (!, &&, ||, ...)
- While, do-while, for loops
    - Break and continue

Classes
- public vs. private
- Defining our own Classes
- "this" keyword
- Accessor (getter) vs. Mutator (setter) Methods
- static variables
- static Methods
- defining Methods
- overloading methods and constructors
- Pass by value vs. Pass by reference
    - What does java do?
- Copy constructors
    - Shallow vs. Deep Copy

Program Input
- Command line arguments
- Console Input
    - Scanner object using System.in

Random Number Generation
- java.util.Random
- .nextInt(), nextInt(int), .nextDouble()

Arrays
- Objects in Java
- Array methods (.clone) and fields (.length)

ArrayLists
- Constructing ArrayLists
- .add, .set, .get, .remove
- Exceptions thrown in certain Classes
- ArrayList of ArrayLists structure

Memory Concepts
- Stack Overflow
- Recursion
- Garbage Collection
- Object references and the heap

Exception Handling
- Base Exception Object
- Try / Catch
- Exception flow through Stack
- Handling multiple exception Types
- Finally block
- Multiple Exceptions in a single catch block
- Creating exceptions (extending from Base class)

Testing
- Error cases: Normal vs. Error vs. Boundary
- Test Suite
- JUnit
    - .assertEquals, .assertFalse, .assertTrue, ...
    - JUnit assertions
        - @Test, @Before, @BeforeClass, @After, @AfterClass,
    - Testing if Exceptions were thrown correctly
        - @Test(expected=Exception.class)

Inheritance
- Extending from Base class
- Accessibility of variables and methods
- Constructor chaining
    - super (also used to call base class methods)
- Instanceof operator
- .getClass methods

Polymorphism (Dynamic Binding)
- Java Behavior
- Type assignment (sub class assigned to base class)
- Overwriting methods in subclasses
- Final methods and Classes
- Memory concepts using inheritance
- Casting object rules
    - What this looks like on the heap and what is legal

Abstract Methods
- Defining and any implications

Abstract Classes
- Defining and implications
- Can we instantiate an object of an Abstract Class?

Interfaces
- Implementing Interfaces
- Interface types and Polymorphism

Switch statements
- cases, break, default
- Using multiple values in a single case
```

