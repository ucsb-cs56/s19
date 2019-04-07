---
num: "Lecture 2"
desc: "Strings, Control Flow"
ready: true
lecture_date: 2019-04-03 17:00:00.00-7:00
---

# Strings

* Strings are objects of class String (not a primitive type like "int")
* Strings are characters stored as an array
* Note: we do not have to use the keyword `new` when creating strings (but we can).

```
String name = "Some string";
String name = new String("Some string");
```

## Escape characters
* `\t`	insert tab
* `\n`	insert newline
* `\’`	insert single quote
* `\"`	insert double quote
* `\\` 	insert backslash

Examples:

```
System.out.println("a\tb\tc");
System.out.println("d\ne\nf");
System.out.println("g\'h\'i");
System.out.println("j\"k\"l");
System.out.println("m\\n\\o");
```

## Some common String methods
* `.length()` – returns number of characters in the string

```
String name = "Richert";
System.out.println(name.length()); // 7
```

* `.substring(int start, int end)` – returns the substring starting at position start up to and not including position end.

```
String name = "Richert"
System.out.println(name.substring(1, 5)); // iche
```

* `.toUpperCase(), .toLowerCase()` - converts String to upper / lower case

```
String s = new String("Hello");
System.out.println(s.toUpperCase()); // HELLO
System.out.println(s.toLowerCase()); // hello
```

* `.charAt(pos)` - returns the character at index pos

```
System.out.println(s.charAt(2)); // ‘l’
System.out.println(s.charAt(100)); // ERROR – StringIndexOutOfBoundsException
```

* `.equals(String s)` - Checks if two strings have the same value, not reference.

```
System.out.println(s.equals(“Hello”)); // true
System.out.println(s.equals(“hello”)); // false
```

* `.equalsIgnoreCase(String s)` - checks content of string ignoring case.

```
System.out.println(s.equals(“Hello”)); // true
System.out.println(s.equals(“hello”)); // false
```

* `s1.compareTo(String s2)` - Lexicographical comparison
    * == 0 if same
    * > 0 if s1 > s2
    * < 0 if s1 < s2

```
String a = "a";	
String b = "b";
String x = "A";
		
System.out.println(a.compareTo(a)); // == 0
System.out.println(a.compareTo(b)); // < 0
System.out.println(a.compareTo(x)); // > 0
```

## Concatenation

* Combine strings with `+`

```
System.out.println(3 + 4 + "Hi"); // 7Hi
System.out.println(3 + (4 + "Hi")); //34Hi
System.out.println("3" + 4 + "Hi"); //34Hi
```

* Converting a String to an Integer

```
String s = "3";
int i = Integer.parseInt(s);
System.out.println(i + 5); // 8
```

* Converting an Integer to a String

```
String s = Integer.toString(3);
System.out.println(s); // 3
// OR	
String t = String.valueOf(4);
System.out.println(t); // 4
// OR	
String u = "" + 3;
System.out.println(u); // 3 (kinda hacky)
```

## String pool

Example:

```
String s = "Hello";
String t = "Hello";
		
System.out.println(s == t); // prints true
		
t = new String("Hello");
System.out.println(s == t); // prints false
```

* If `==` compares references and not values (as mentioned in lecture 1), then why does the first print statement return true?
    * Java does some optimization by storing identical Strings that are declared in code to reference the same location in memory.
        * These strings are stored in a <b>String pool</b>
    * If the String changes, then Java will allocate another section in memory for the updated string.
        * Makes sense since Java Strings are <b>immutable</b> (i.e. they can’t be changed in memory once created).
    * The `new` keyword bypasses the String pool and creates a separate memory location for the initialized String.
* For a more detailed explanation of this behavior and String pools, refer to:
    * [http://stackoverflow.com/questions/2486191/what-is-the-java-string-pool-and-how-is-s-different-from-new-strings](http://stackoverflow.com/questions/2486191/what-is-the-java-string-pool-and-how-is-s-different-from-new-strings)
    * [http://www.programcreek.com/2013/04/why-string-is-immutable-in-java](http://www.programcreek.com/2013/04/why-string-is-immutable-in-java)

# Control Flow

## Conditional Statements

Allows the execution of code if some boolean value is true.
    * Examples of boolean experessions: `true`, `false`, `i < 10`, `s.equals("Hi");`

## If statement

```
if (boolean_expression) {
	statements
} else {
	statements
}
```
* Note: If there is only one statement per block, you can remove the `{ }`.

```
if (boolean_expression)
    statement
else
    statement
```

* Java does allow "null bodies."
* Generally bad practice since condition statements can be written where null bodies are not needed.

```
if (boolean_expression)
    ; // or { }
else {
    statements
}
```

## Relational Operators

* Examples: `==`, `!=`, `<`, `<=`, `>`, `>=`
* Left to right precedence.
* Can change order of operations using `( )`
    * Recall: == for objects refer to the same object’s memory location.

Example:

```
Object x = new Object(); // just a plain ol’ object.
Object y = x;
if (x == y) {
    System.out.println("x == y"); // y refers to x
} else {
    System.out.println("x != y");
}
```

## Logical Operators

* Examples: `!`, `&&` , `||`
    * short-circuit: && - right expression is evaluated only if left expression is true.
    * short-circuit: || - right expression is evaluated only if left expression is false.
* Can change order of operations using `( )`
* Note: Java has logical bit operators `&` and `|` . These perform AND / OR operations on the bits of the value.

Example:

```
int z = 5;

if (!((z – 3) > 0)) {
    System.out.println("true");
} else {
    System.out.println(“false"); // Prints False
}
```

## Unreachable Code

```
public static int f() {
    int a = 0, b = 0;
    if (a < b) {
        System.out.println("a<b");
        return a;
    } else {
        System.out.println("a>=b");
        return b;
    } 
    System.out.println("after if/else"); // Compile error, code is unreachable
}
```

## While Loop

General format:

```
while (boolean_expression) {
    statements;
}
```

* Statements can be executed anywhere from 0 to infinite times.
* Boolean expression is checked at the start of the loop.
    * Executes statements if the boolean expression evaluates to true.
    * To guarantee executing something at least once, that code will have to exist before while loop. (redundant code).
    * Similar to if statements, `{ }` required for more than one statement.

Example:

```
int i = 0;
while(i < 10) {
    i++;
    System.out.println(i);
} // Prints 1 -> 10
```

## Do-while Loop

General format:

```
do {
    statements;
} while (boolean expression); // note the ';' here
```

* Statements can be executed anywhere from 1 to infinite times.
* Boolean expression is checked at the end of the first (and subsequent) iterations.
    * Executes statements if the boolean expression evaluates to true.

Example:

```
int i = 0;
do {
i++;
    System.out.println(i);
} while(i < 10); // Prints 1 -> 10
```

## For Loop

General format:

```
for (initial action; boolean expression; update action) {
    statements;
}
```

* Statements can be executed anywhere from 0 to infinite times.
* Initial action is executed first (regardless if boolean expression evaluates to true or not).
* Boolean expression is checked before each iteration of the loop.
    * Executes statements if the boolean expression evaluates to true.
* Update action is executed after the statements are executed.
    * Usually does something to change initial assignment.
* Boolean expression is checked after update action is performed.
* Note: Any while loop can be written as a for loop (and vice versa).

Some looping examples:

```
for (int i = 0; i < 10; i++) {
    System.out.println(i); // prints 0 – 9
}

int i = 0;
while (i < 10) {
    System.out.println(i); // prints 0 - 9
    i++;
}

i = 0;
for (; i < 10;) { // null initial and update statements 
    System.out.println(i); // prints 0 - 9
    i++;
}

for (int j = 0;;j++) { // null boolean expression statement
    System.out.println(j * j); // infinite loop, overflow values
```

## Scoping with Loops

* Similar to scoping with conditional statements, variables declared within `{ }` are only valid in that block.

Examples:

```
int sum = 0;
for (int i = 1; i <= 10; i++)
{
	sum = sum + i;
}
System.out.println(i);  // ERROR: no i here!

int i = 3; 
int sum = 0;
for (int i = 1; i <= 10; i++)   // ERROR: i is already declared
{	}			
	
int sum = 0;
do {
int count = i + 1;
} while (count != 999); // ERROR: no count here!
```

## Break and Continue

* `break` – breaks out of the current loop.
* `continue` – breaks out of the current iteration of the loop and performs the next iteration.

Example:

```
int i = 0;
while (true) {
	i++;
	System.out.println(i);
		
	if (i < 10) {
		continue;
	} else {
		break;
	}
}
System.out.println("Outta the loop");
// Prints 1->10
// Outta the loop
```
