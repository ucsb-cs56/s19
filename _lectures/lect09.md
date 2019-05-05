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

# File IO

* Data from input source or output destination is represented in a "stream"
    * A stream is a sequence of binary data (0’s and 1’s).
* Since bytes are usually the smallest piece of data, Java has Stream objects that read / write data in 8-bit (1 byte) pieces.

* American Standard Code for Information Interchange (ASCII)
    * Each character is represented as 1 byte
    * This standard was only keeping English in mind…
* Unicode
    * Can represent characters from a larger set
    * We need more than 8 bits to represent characters from different languages.
* UTF-8
    * can store anywhere between 1 – 4 bytes per character
        * Most common standard for the web
    * UTF-16 – can store characters with 2 or 4 bytes
* ... and others!
* We can get Java’s encoding scheme by using:

```
System.out.println(System.getProperty("file.encoding"));
```

## Scanner Object
* In Java, reading from Files and user input from the command line can be done in several different ways.
* Scanner is a Java Object we’ve used to collect user input
    * and this can be used for File I/O
* We can also use Scanners to read data from a file into the application.

```
// If the file does not exist, a FileNotFoundException is thrown
Scanner inFile = new Scanner(new File(“file.txt”));
```

* Assume a file called `file.txt` exists in the project root directory containing:

```
This is line 1
This is line 2
This is line 3
```

* Can use an absolute path, but this differs based on the File System.
* Can use a relative path, which looks within the project directory.
* What happens if we pass in the filename String, not a file object?
    * The scanner will use the string (i.e. "file.txt") to parse through!

## Example

```
// get all lines in the file and print them one-by-one
String line;
while (inFile.hasNextLine()) {
    line = inFile.nextLine();
    System.out.println(line);
}

// get all words in the file and print them one-by-one
// assuming words are separated by the white space character(s)
while (inFile.hasNext()) {
    word = inFile.next();
    System.out.println(word);
}
```

## Tokens
* Tokens are the next “piece” of data you can expect when scanning through the stream.
* .hasNext() checks if the next token is a String
    * but we can directly check certain types
    * .hasNextInt(), .hasNextLong(), .hasNextByte(), .hasNextDouble() …

## A note about absolute file paths
* If our file wasn’t in our project and in our local hard drive, we can use the file path.	
    * Example: /Users/Richert/Desktop/JavaWorkspace/CS56Lecture/file.txt
* For this class, you should always use a relative path, and not the full file path.
* Since the file systems for users of an application vary (not everyone has the same folders / directories / operating systems, ...), you can imagine how absolute file paths are not compatible with everyone.

## Reading a file using a URL Example

```
try {
Scanner inFile = new Scanner(new File("file.txt"));

    String line;
    Scanner remoteIn = null;
    try {
        URL remoteFileLocation =
        new URL("https://sites.cs.ucsb.edu/~richert/file.txt");

        URLConnection connection = remoteFileLocation.openConnection();
        remoteIn = new Scanner(connection.getInputStream());

        while (remoteIn.hasNextLine()) {
            line = remoteIn.nextLine();
            System.out.println(line);
        }
    } catch (IOException e) {
        System.out.println(e.toString());
    } finally {
        if (remoteIn != null) {
            remoteIn.close();
        }
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```

## Delimiters
* Scanners use delimiters to distinguish what is "next" to read on the input stream.
* By default, Scanners have a delimiter set to any number of whitespace characters.
* If our first line was written as "This is line          1", the tokens will be the same.
* Scanner breaks the pieces into non-whitespace tokens.
* We can set Scanner to use a specific delimiter.
    * When inFile.next() is called, it will read the token and put it into a String up to the delimiter or \n character.
    * inFile.useDelimiter("l");

## Example using "l" as a delimiter

```
try {
    Scanner inFile = new Scanner(new File("file.txt"));
    inFile.useDelimiter("l");

    String line;
    while(inFile.hasNext()) {
        line = inFile.next();
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
///
This is 
ine 1
This is 
ine 2
This is 
ine 3
```

* inFile.useDelimiter(“”); is a special case.
    * This is used to parse individual characters.
    * Each inFile.next() call returns a string consisting of the next character.

## Example
* Assume we have a fixed format for data in an input file separated by ";"
* `[FirstName;LastName;StreetAddress;City;Zip]`
* You can obtain "pieces" of data and write general parsing algorithms if there is some fixed format to follow.

```
// file2.txt
Richert,Wang,CS56,Goleta,93117
```
```
String s;
Scanner inFile = null;
try {
    inFile = new Scanner(new File("file2.txt"));
} catch (FileNotFoundException e) {
    System.out.println(e);
}
String [] stringArray;
// Get the entire line one-at-a-time
while (inFile.hasNextLine()) {
    s = inFile.nextLine();
    stringArray = s.split(",");
    for (int i = 0; i < stringArray.length; i++) {
        System.out.println(stringArray[i]);
    }
}
```

## Writing data to a File
* We’ve talked about reading data from the console and files (remotely and locally). Let’s talk about writing data to a file.

```
PrintWriter out = null;
try {
    out = new PrintWriter("output.txt");
    out.println("First Line!");
} catch (FileNotFoundException e) {
    // Non existent path, write access denied, …
    System.out.println(e.toString());
} finally {
    if (out != null) {
        out.close();
    }
}
```

## Formatting Data using printf
* System.out.printf() is used to display formatted text.
* System.out.format() behaves the same as System.out.printf().
* Example

```
String course = "CS 56";
System.out.println("I <3 " + course);
```

vs.

```
String course = "CS 56";
System.out.printf("I <3 %s", course);
```

* `%s` is known as a format specifier
    * s is known as a conversion character
* `%d` – decimal integer (or just integer)
* `%f` – floating point
* `%e` – floating point in exponential notation
* `%c` – Characters
* `%s` – Strings
* `%b` – boolean
* `%%` - ‘%’ sign
* ...

## Example

<pre>
String month = "May";
int day = 6;
int year = 2019;
boolean b = true;
System.out.format("It is %b that today is the %dth of %s in the year %d.", b, day, month, year);

char grade = 'A';
double percentage = 99.8;
System.out.format("In order to get a %c, I need to score a %f%% on the final", grade, percentage);
// nobody falls in this category :)
<b>Output:</b> In order to get an A, I need to score 99.800000% on the final
</pre>

* `%f` has a default precision of 6 decimal places.
* We can define precision using %.(decimal_precision)f
* We can define the precision as follows:

```
System.out.format("In order to get a %c, I need to score %.2f%% on the final", grade, percentage);
```

## Left/Right Justify
* We can define the minimum field width for the amount of space we want a string to consume.
    * Good for aligning output.
* Negative values indicate that the characters should start at the left-most character in the space allotted.
* Positive values indicate that the characters should end at the right-most character in the space allotted.
* If the characters exceed the allotted space, it simply fills in all of the characters (no truncation).
* For any unused characters in the allotted space, a whitespace is inserted.

```
String first = "Richert";
String last = "Wang";
System.out.format("Hello, my name is %-10s %5s", first, last);
//Output: Hello, my name is Richert     Wang

System.out.format("Hello, my name is %-1s %5s", first, last);
//Output: Hello, my name is Richert  Wang
```

## Writing to a File
* A simple object called PrintWriter exists, which allows you to write content to a file.
* PrintWriter is very simple and doesn’t allow you to append to an existing file using the object directly

```
PrintWriter out = null;

try {
    out = new PrintWriter("output2.txt");
    String first = "Richert";
    String last = "Wang";
    out.format("Hello, my name is %-10s %5s\n", first, last);
    out.format("Another line");
} catch (Exception e) {
    System.out.println(e.toString());
} finally {
    if (out != null) {
        out.close();
    }
}
```

## Append to a File
* Another object, FileWriter, provides more functionality than PrintWriter, including the ability to append to an existing file.

```
FileWriter out = null;
try {
    out = new FileWriter("output", true);
    out.write("First Line!!!");
} catch (FileNotFoundException e) {
// Non existent path, write access denied, …
S   ystem.out.println(e.toString());
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (out != null) {
        try {
            out.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## Example Using Filewriter with printWriter to format

```
FileWriter out = null;
try {
    out = new FileWriter("output", true);
    String first = "Richert";
    String last = "Wang";
    PrintWriter writer = new PrintWriter(out);
    writer.format("Hello, my name is %-10s %5s", first, last);
} catch (FileNotFoundException e) {
    System.out.println(e.toString());
}
```
