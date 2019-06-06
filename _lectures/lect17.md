---
num: "Lecture 17"
desc: "Final Review"
ready: true
lecture_date: 2019-06-05 17:00:00.00-7:00
---

# Final Review

```
Logistics
- Bring studentID and writing utensil
- No electronic devices
- No notes, no books
- Tuesday 6/11 @ 7:30pm
    - Expect it to take ~ 2 hours

Format
- Mix of questions covering lectures, homeworks, readings, and labs
- Short Answers
    - Briefly define, describe, state, ...
- Write code
- Fill in the blank, complete the table, select answers, draw diagrams, ...
- Given code, write the output
- Given a statement, tell me if it's true or false
    - if false, state why

Topics
- Will cover everything up to Monday's lecture (6/3)
- Will be cummulative with an emphasis on post midterm material

(Post Midterm)
File IO
- Using Scanner object for File IO
- Absolute vs Relative file paths
- Reading files from file system and URL
- Delimiters
    - How do we break data into "tokens" using Delimiters
    - .useDelimiter, .hasNext, .next, ...
- Writing / appending data to a file
    - PrintWriter, FileWriter
- String formatting
    - .printf , format
    - left / right justify
    - format specifiers
    - floating point precision

Generics
- Similar to C++ Templates
- Generic methods
- Generic classes

MultiDimensional Arrays
- 2D / 3D (ND)
- Memory organization
- Syntax

UML
- Know the basic structure
    - Not all the details, but at least enough covered in lecture notes and readings
    - fields of classes (attributes)
    - methods (operations)
    - relationships (inheritance, ...)

Strategy (Behavior) Design Pattern
- Understand general use cases, structure, and extensibility
- Break out behaviors into interfaces
- Supports the "open-close" principle

Observer Design Pattern
- Publication / Subscription (pub / sub)
- Subjects (publishers) update observers (subscribers) whenever relevant information is updated.
- Understand design / organization.

Decorator Pattern
- Components containing multiple behaviors or attributes (Decorators)
    - Components and Decorators extend from a common type
    - Important to "wrap" decorators around components and other decorators
    - Understand the overall relationships of components / decorators

Interfaces extending interfaces
    - and how classes implement multiple interfaces
    - understanding the hierarchy and type structure

Collections
- Understand Java API hierarchy with Collection interface
    - and how other collections (list / set) extends Collection
- HashSet, HashMap, TreeMap
- Understanding commonly used methods
    - HashSet: .add, .size, .contains, .remove
    - HashMap: .put, .containsKey, .containsValue, .get, .keySet, .values, .remove, .size

Multithreads
- How concurrency is acheived with multi-core architectures
- OS Scheduler behavior (context switching) between processes and threads
- Creating threads
    - Thread class
    - Runnable interface
    - .run method
- Thread.join()
- .interrupt method
- Thread.sleep (in milliseconds)
    - InterruptedException handling

Race Conditions
- Example of multiple threads updating a bank account
- Using locks to support atomic operations
    - Lock (ReentrantLock) and synchronized

Anonymous classes
- Good for implementing an interface on the fly

Functional Interface
- Special type of interface that has only one abstract method

Lambda Expressions
- Allows functionality to be defined outside of a class
- Implements functional interfaces
- Know syntax of providing definition of function interface's abstract method
- Examples of using Lambda Expressions to implement functional interaces
    - Defining lambda expression for Comparator interface and Runnable.
```
