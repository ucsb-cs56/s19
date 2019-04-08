---
layout: lab
num: lab01
ready: true
desc: "Defining Java Classes"
assigned: 2019-04-08 08:00
due: 2019-04-14 23:59
---

# Goals

* This lab will help familiarize yourself with defining your own Java classes and using them in your program as illustrated in lecture.
* You will work on writing classes using getter / setter methods, constructors, and modifying the state of objects in your program.

# Overview

Be sure to complete all parts of Lab00. You should have access to Gradescope and also be able to create private repos in our [github organization](https://github.com/ucsb-cs32-s19-wang).

You will write a simple program that can represent the properties of a typical hardbound book for bookstore. The structure of your program will consist of the following components:

* A Book class consisting of the following private attributes:
    * int - number of pages in the book
    * int - year the book was published
    * double - price of the book in USD
    * String - title of the book
    * Author – an object representing the Author of the book (for this assignment, you may assume a book only has one author).

* An Author class consisting of the following private attributes:
    * String - first name of author
    * String – last name of author
    * int – birth year of author
    * int – number of publications the author has

* A `Lab01` class consisting of:
    * The main method that starts your Java program.

You will write the appropriate accessor / mutator methods for all attributes in the Book and Author classes, as well as their default and copy constructors. The copy constructor for the Book class must perform a <b>deep copy</b> of the Author object (i.e. the copied author has its own memory location and is not shared). For the Book class, you will override the method `.toString()` which prints out all of the information of the Book including the details of the Author. When writing the setter method for the Author attribute in the Book class, you will need to pass in the appropriate fields so the Author object can be constructed. For example:

```
public void setAuthor(String firstName, String lastName, int birthYear,
int numOfPublications) { ... }
```

Your main method should contain code that illustrates the following functionality:
* Construct a Book object using the default constructor.
* Set the appropriate fields using the Book’s setter methods.
* Print out the Book’s information (including the Author’s information) by
calling the Book’s .toString() method.

Test that your copy constructor works by doing the following:
* Create another book object that is a copy of your original Book object using the copy constructor.
* Print out the fields of the copied object to the console and confirm that all of the fields are the same as the original object.

Change the state of the object by:
* Incrementing the number of publications for the Author of the Book.
* Printing out the copied object with the updated number of publications.

In your main method, you can "hard-code" the values in the parameters when
initially constructing your book object (we will explore various ways of interacting with our program in Java soon). For example, to construct a Book object in your main method, you can do the following:

```
// Create a new book with default values for all attributes
Book book = new Book();

// Set the appropriate fields for the book’s objects
book.setTitle("Harry Potter and the Goblet of Fire");
book.setPrice(12.99);
book.setYearPublished(2000);
book.setNumOfPages(734);
book.setAuthor("J.K.", "Rowling", 1965, 7);

// Prints out the state of the book.
System.out.println(book.toString());
```

The format of `book.toString()` for the above code is shown below:

```
Title: Harry Potter and the Goblet of Fire
Published in: 2000
Number of Pages: 734
Price: $12.99
Written by J.K. Rowling, who was born in 1965 and has 7 publications
```

<b>Note:</b> If the author only has one publication, then the "Written by" output line should state 1 <b>publication</b> (singular) instead of publications (plural).

This is not the only way to construct your Java program (there are MANY ways to design your code to solve a problem). As a side-exercise, think of other ways you can structure your program to accomplish the same thing. For example, is it possible to allow the main method to construct an Author object and pass this object when setting the Author attribute in the Book class? What are the advantages / disadvantages of this? Can you accomplish copying an object without copy constructors? If so, why would you want copy constructors?

# Submitting to Gradescope

## -----IMPORTANT! PLEASE READ BEFORE SUBMITTING-----

The lab assignment "Lab01" should appear in your Gradescope dashboard in CMPSC 56. If you haven't submitted anything for this assignment yet, Gradescope will prompt you to upload your files.

For this lab, you will need to upload your source files (`Book.java`, `Author.java`, and `Lab01.java`). You either can navigate to your files, "drag-and-drop" them into the "Submit Programming Assignment" window, or  use your private GitHub repo to submit your work.

If you already submitted something on Gradescope, it will take you to their "Autograder Results" page. There is a "Resubmit" button on the bottom right that will allow you to update the files for your submission.

Most of the programming assignments for this class will not be autograded in the same way you may have seen in earlier courses. We will use Gradescope mainly as a "dropbox" for all of your assignments. Our staff will manually grade your assignments and assign a score to your submission.

Since Gradescope autograding is disabled, it is <b>very</b> important for you to test and compile your code locally according to the specifications for this lab. As a software developer, it's an important skill to think of correct functionality and test cases on your own. Be sure to do this before submitting your code to Gradescope. If our staff cannot run your code, then you will receive a 0 for this assignment (and this will not be apparent when submitting since Gradescope will not actually compile your code for this assignment).

Your submission will initially have a score of `0.0/100`. Don't worry - this is normal. Once the assignment has been graded by our staff, your actual score will be updated.
