---
num: "Lecture 5"
desc: "Exceptions, JUnit Testing"
ready: true
lecture_date: 2019-04-15 17:00:00.00-7:00
---

# Exception handling

* Allows programmers to catch and handle error conditions that occur throughout a program.

## Exception Object
* A java object that represents a certain error condition.
* Many types of Exceptions can occur (IOException, NullPointerException, ArithmeticException, ...)
    * Java will create and "throw" an exception when it occurs for certain cases.
    * All exceptions are inherited from the Exception Java object.
    * Programmers can manually create and throw their own Exception object by extending from the Exception class.
        * Can contain information or perform certain actions.
    * All Java Exceptions carry information, such as the name of the Exception and stack trace.

## Try / Catch
* Exceptions that are thrown need to be handled somewhere in your code.
    * An exception "bubbles up" your code
    * If it’s unhandled in the method where it occurs, it has to be thrown to the caller. If the caller doesn’t handle it, then the Exception gets thrown to its caller, and so on.
    * If the Exception reaches main and is not handled there, then your program will crash.
* Try / catch blocks are used to handle an Exception.
    * If an exception occurs within a try block, you can catch the Exception and execute code in a corresponding catch block.
    * If a catch block is executed, it resumes functionality after the try/catch block.

## Example

```
public class SomeClass {
    private Scanner s;
    public SomeClass() {
        s = new Scanner(System.in);
    }

    public int getInt() {
        int value = 0;
        System.out.print("Enter a positive number: ");
        try {
            value = Integer.parseInt(s.nextLine());
        } catch (Exception e) {
            // What exceptions can happen based on the user?
            System.out.println("Caught Exception: " + e);
        }
        s.close();
        return value;
    } 
}
```
```
public static void main(String args[]) {
    SomeClass x = new SomeClass();
    System.out.println("x = " + x.getInt());
}
```

* Some cases:

<pre>
User Enters Valid Integer: <b>10</b>
x = 10

User Enters Invalid Integer: <b>abcd</b>
Caught Exception: java.lang.NumberFormatException: For input string: "abcd"
x = 0

User Enters an Integer not in range: <b>9999999999</b>
Caught Exception: java.lang.NumberFormatException: For input string: "9999999999"
x = 0
</pre>

## Catching any Exception in Main
* ... is generally not a good idea.
    * Not common to have the same action taken for all types of possible Exceptions that can occur in your program.

## Handling Multiple Exceptions
* When catching an Exception, you can execute different actions based on what Exception was caught.
* Java  goes through each catch block one-by-one until a compatible Exception is matched.
* To catch any Exceptions, you can catch an Exception type (since all Exceptions are inherited from the Exception class).

## Example

```
// In SomeClass.java
private int[] array;

public SomeClass() {
    // Comment this out see a NullPointerException
    array = new int[100];
}

// ...

public void updateArray(int pos, int value) {
    try {
        array[pos] = value;
    } catch (IndexOutOfBoundsException e) {
        System.out.println("In IndexOutOfBoundsException");
            System.out.println(e);
    } catch (NullPointerException e) {
        System.out.println("In NullPointerException");
        System.out.println(e);
    }
}
```
```
// Lecture.java
public static void main(String args[]) {
    SomeClass x = new SomeClass();
    x.updateArray(100, 100);
}
```

Some Cases:
* If you call x.updateArray(100,100)
    * IndexOutOfBoundsException is thrown and handled in the corresponding catch block.
* If you comment out the line allocating the array...
    * NullPointerException is thrown and handled in the corresponding catch block.

## Finally Block
* Regardless of what happens in a try/catch block, the code in “finally” will always execute.

## Example

```
// Change updateArray in SomeClass.java to take in user input
public void updateArray() {
        int pos = 0;
        int value = 0;
        try {
            System.out.print("Enter position: ");
            pos = Integer.parseInt(s.nextLine());
            System.out.print("Enter value: ");
            value = Integer.parseInt(s.nextLine());
            array[pos] = value;
            System.out.println("Updated array[" + pos + "] = " + value);
        } catch (NumberFormatException e) {
            System.out.println("In NumberFormatException block");
            return;
        } catch (IndexOutOfBoundsException e) {
            System.out.println("In IndexOutOfBoundsException block");
            return;
        } catch (NullPointerException e) {
            System.out.println("In NullPointerException block");
            return;
        } catch (Exception e) {
            System.out.println("In Exception block");
            return;
        } finally {
            System.out.println("In finally block");
            s.close();
        }
        System.out.println("exiting method");
    }
```
```
// in main
x.updateArray();
```

## Combining Exceptions into one block
* In Java versions < 7, each type of exception had to have its own catch block
* In Java versions >= 7, multiple exceptions can have the same catch block

```
public static void main(String args[]) {
    SomeClass x = new SomeClass();

    try {
        x.updateArray();
    } catch (IndexOutOfBoundsException | NullPointerException e) {
        System.out.println("In multiple exception catch block");
    }
}
```

## Creating Your Own Custom Exception
* An Exception type can be declared by defining the Class that extends the Exception class.
* You can create, throw, catch this type in your code depending on some action.
* You can also maintain additional information or provide your own methods if needed.

```
// MyException.java
// Create an Exception called MyException extending the Exception class
public class MyException extends Exception {}
```
```
// Catch or throw your exception whenever you want.
// Example of throwing the Exception to the caller.
public void someMethod(int x) throws MyException {
    if (x == 0) {
        throw new MyException();
    }
}
```

# Testing

* Testing every possible path in every possible situation.
* Complete tests are infeasible!
* The best we can hope for is trying to approximate a Complete Test by testing various types of cases.
* The more rigorous testing a program can "pass", the more confidence is gained that the program "bulletproof".

## Test Suite
* A program containing various tests confirming certain behavior.

## Example of building our own test function

```
public static int getBiggestInt(int a, int b, int c, int d) {
    if (a >= b && a >= c && a >= d) {
        return a;
    } else if (b >= a && b >= c && b >= d) {
        return b;
    } else if (c >= a && c >= b && c >= d) {
        return c;
    } else {
        return d;
    }
}

public static void runTestCase(int a, int b, int c, int d, int expected) {
    int result = getBiggestInt(a, b, c, d);
    if (result != expected) {
    System.out.println("result: " + result + " != expected: " + expected);
    } else {
        System.out.println("result: " + result + " == expected: " + expected);
    }
}

public static void main(String args[]) {
    // normal test cases
    runTestCase(1,2,3,4,4);
    runTestCase(1,2,4,3,4);
    runTestCase(1,1,3,2,3);
    runTestCase(-1,-2,-3,-1,-1);
        
    // Boundary Cases
    runTestCase(Integer.MAX_VALUE, 3,4,5,Integer.MAX_VALUE);
    runTestCase(3,Integer.MAX_VALUE,4,5,Integer.MAX_VALUE);
    runTestCase(Integer.MIN_VALUE,Integer.MIN_VALUE, -1, 0, 0);
    //...
}
```

## Some Types of Test Cases
* Normal Cases: Any expected or “normal” input cases that you can reasonably expect from the user.
* Error Cases: Any case where it’s not expected, but is possible
    * Passing in a null object, having a bad file name, disabling network connectivity for something that requires it…
* Boundary Cases: Testing the borders of the input.
    * Testing both max / min value for int parameters.
* There are other types of test cases as well (integration, scalability, end-to-end, etc).

## JUnit
* JUnit allows a programmer to perform automated tests.
* Runs one or more methods on the objects you are testing and checks the results using assertions.
* Tests are run one after another and will produce a graphical report on the cases that passed, the tests that encountered an error (i.e. crashed), and the tests that didn’t crash, but failed an assertion.

## Some JUnit Methods

```
assertEquals(expected_value, result_of_test);
assertFalse(boolean_result);
assertTrue(boolean_result);
assertNull(some_object);
assertArrayEquals(array, expected_array);
.. And many more!
```

* You can read the JUnit4 API to see what's available:

[https://junit.org/junit4/javadoc/latest/](https://junit.org/junit4/javadoc/latest/)

## Using JUnit Example

```
// Example.java
public class Example {

    public int getBiggestInt(int a, int b, int c, int d) {
        if (a >= b && a >= c && a >= d) {
            return a;
        } else if (b >= a && b >= c && b >= d) {
            return b;
        } else if (c >= a && c >= b && c >= d) {
            return c;
        } else {
            return d;
        }
    }
}
```
```
// Tester.java
import org.junit.Test;
import org.junit.Before;
import org.junit.After;
import org.junit.AfterClass;
import org.junit.BeforeClass;

import static org.junit.Assert.assertEquals;

public class Tester {

    private int[] array;
    private Example b; // contains getBiggestInt method defined above

    @Before // Executed before each test in this class
    public void executeBeforeEachTest() {
        System.out.println("@Before: see before every test");
        array = new int[10];
        b = new Example();
    }

    @Test
    public void testInsertItem() {
        array[0] = 0;
        array[1] = 1;
        array[2] = 2;
        array[3] = 3;
        
        assertEquals(array[0], 0);
        assertEquals(array[1], 1);
        assertEquals(array[2], 2);
        assertEquals(array[3], 3);
    }

    @Test
    public void getBiggestTest() {
        assertEquals(b.getBiggestInt(1,2,3,4), 4);
        assertEquals(b.getBiggestInt(1,2,4,3), 4);
    }

    @Test
    public void getBiggestBoundaryTest() {
        assertEquals(b.getBiggestInt(Integer.MAX_VALUE, 3,4,5), Integer.MAX_VALUE);

        assertEquals(b.getBiggestInt(3, Integer.MAX_VALUE,4,5), Integer.MAX_VALUE);
    }

    // Testing an Expected Exception was thrown
    @Test(expected=ArithmeticException.class)
    public void testExceptionThrown() {
        int x = 0;
        int y = 1;
        int z = y / x;
        // Should crash, but doesn't since we're telling the test
    // that an exception should happen.
    }

    @After
    public void executeAfterTest() {
        System.out.println("@After: See this after every test");
    }

    @AfterClass
    public static void executeAfterAllTests() {
        System.out.println("@AfterClass: See this once after all tests");
    }

    @BeforeClass
    public static void executeBeforeAllTests() {
        System.out.println("@BeforeClass: See this once before all tests");
    }
}
```

## Executing JUnit from command line

* Note: A `classpath` tells Java where to look for files in order to compile the Java application.

1.	Get the JUnit4 jar from [http://cs.ucsb.edu/~richert/cs56/lib/junit-4.8.2.jar](http://cs.ucsb.edu/~richert/cs56/lib/junit-4.8.2.jar)
2.	Put it in your directory or a sub folder where your java files are (`lib` folder for example).
3.	Compile all .java files and set the classpath to the current directory and where the junit jar exists.

`javac -cp .:lib/* Tester.java Example.java`

* `-cp` tells the compiler to use '.' And `lib/*` as part of the classpath.
* Tester.java contains the JUnit tests
* Example.java contains code the JUnit tests will use

`java -cp .:lib/* org.junit.runner.JUnitCore Tester`

* Executes the JUnit Tester file with Junit (note this is necessary since there isn’t a "main" method.

## Using an IDE (such as Eclipse)

* Encourage everyone to explore an IDE (Eclipse was demo'd in class).
    * IntelliJ is also a popular Java IDE.
    * Much easier to "plumb" everything together with an IDE.
    * Once your project is setup, right click tester file (Tester.java) and "run as" JUnit 4.
        * May ask if you want to add JUnit4 to the classpath if this hasn't been done already.
