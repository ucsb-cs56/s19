---
layout: lab
num: lab03
ready: true
desc: "Roster Manager"
assigned: 2019-04-22 08:00
due: 2019-05-05 23:59
---

# Goals

For this lab, you will focus on dealing with arrays and Exception handling. You will write a simple class roster program that manages courses and students enrolled in those courses. This program will provide a simple command-line interface that allows users to interact with the application and perform actions such as creating a course, removing a course, enrolling students in a course, and dropping students from a course. Once the program is up and running, you will also write a test suite in JUnit covering various types of cases discussed in class.

# Pair Programming

This lab may be done solo, or in pairs.

<b>Before</b> you begin working on the lab, please decide if you will work solo or with a partner.

As stated in previous labs, there are a few requirements you must follow if you decide to work with a partner. I will re-iterate them here:

* You and your partner must agree to work together outside of lab section in case you do not finish the lab during your lab section. You must agree to reserve at least two hours outside of lab section to work together if needed (preferably during an open lab hour where you can work in Phelps 3525 and ask a mentor for help). You are responsible for exchanging contact information in case you need to reach your partner.
* If you choose to work with a partner for a future lab where pair programming is allowed, then you must choose a partner you have not worked with before.
* You MUST add your partner on Gradescope when submitting your work <strong>*<u>EACH TIME</u>*</strong> you submit a file(s). After uploading your file(s) on Gradescope, there is a "Group Members" link at the bottom (or "Add Group Member" under "Groups") where you can select the partner you are working with. Whoever uploaded the submission must make sure your partner is part of your Group. Click on "Group Members" -> "Add Member" and select your partner from the list.
* <b>You must</b> write your Name(s) and Perm number on each file submitted to Gradescope.

Once you and your partner are in agreement, choose an initial driver and navigator, and have the driver log into their account.

# Instructions

There are many ways a class roster program like this can be accomplished (and one would probably not implement a roster program using a command-line interface). Students to work with arrays and manage these data types manually (rather than using predefined objects such as ArrayLists that already provide most of the functionality for you).

## Class Roster Menu

The first thing the user sees is a brief welcome message and a small menu describing commands that the user can make. For example:

```
Welcome to Class Roster Manager!
Select an action based on the following menu: 
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: 
```

Since we’ll be dealing with arrays, we will define the limits for both the maximum number of courses the program will keep track of as well as the maximum number of students each course can have, which are 10 and 50 respectively (obviously this does not scale, but that's not what we're focusing on for this lab).

## Program Behavior

The menu choices are case insensitive and are defined as follows:

`ac: Add Course`
* Adds a course to be managed by the program.
* The program will prompt the user for the course code (i.e. "CS56"), and the course name (i.e. "Advanced Applications Programming").
    * If the program already has the maximum number of courses (i.e. 10), then a brief error is reported and the menu is displayed again.
* There are no restrictions on what the valid strings for a course code or name could be.
* There is no specific order to maintain for the list of courses this program will manage.
* The course codes must be unique. If a duplicate is being added, then a brief error is reported and the menu is displayed again.

`dc: Drop Course`
* Drops a course currently managed by the program.
* The program will prompt the user for the course code. The program will then delete the course with the course code from memory.
    * If the course codes do not match (note – course codes are case insensitive), then a brief error is reported and the menu is displayed again.
    * If there are no courses that were added to the program, then a brief error is reported and the menu is displayed again.

`as: Add Student`
* Adds a student to a specific course in the system.
* The program will prompt the user for the course code that the student will be added to, the PERM number, and the first / last name of the student.
    * Similar to the Drop Course case, if the course code does not match an existing course code, then a brief error is reported and the menu is displayed again.
    * There are no restrictions on what the first and last name strings can be (even empty ones!).
    * The PERM number must be a positive integer greater than 0 (your program can assume a valid integer is entered by the user, but not a positive integer).
    * PERM numbers must be unique. If a duplicate PERM number is found, then a brief error is reported.
    * Students will be ordered lexicographically by last name and you will need to manually sort each student when adding them to the course.
        * In the event of a tie (i.e. two students have the same last name), their PERM number will be used (the smaller PERM number appears before the larger PERM number).
    * If the course enrollment is full (i.e. it contains 50 students), then a brief error is reported and the menu is displayed again.

`ds: Drop Student`
* Drops a student from a specific course in the system.
* The program will prompt the user for the course code that the student will be dropped from and the student’s ID.
    * If the course code does not match an existing course code, then a brief error is reported and the menu is displayed again.
    * If the PERM number does not exist in the course’s list of students, a brief error is reported and the menu is displayed again.
* If the course enrollment has zero students, then a brief error is reported and the menu is displayed again.

`p: Print ClassRoster`
* Prints the entire roster, including all of the courses and students enrolled for each course.
    * This includes the course code, the course name, the number of students currently enrolled in the course, and the list of students including their PERM number, last name, and first name of the student.

`q: Quit Program`
* Quits the program when this option is entered.

# Exceptions
Since there are several scenarios where a brief error is reported, students will practice Exception handling to "fine-tune" the error messages that are displayed to the user. The Exception classes you will need to create are minimal without a body (just to define the Exception type). The Exceptions you will need to create and use in your program are:

* `DuplicateCourseException`: Thrown when a course already exists when trying to add one.
* `CourseLimitException`: Thrown when a course is trying to be added and the number of courses is at the max.
* `CourseNotFoundException`: Thrown when trying to add a student to a course that does not exist.
* `EmptyCourseListException`: Thrown when trying to remove a course when the course list is empty.
* `EmptyStudentListException`: Thrown when trying to drop a student from an empty course.
* `StudentLimitException`: Thrown when a student is added to a full course.
* `StudentNotFoundException`: Thrown when trying to remove a student in a course that they’re not enrolled in.
* `DuplicateStudentException`: Thrown when trying to add a student to a course and a duplicate PERM number exists.

# Code Organization

Some requirements on the code structure that students will need to follow. Please be sure to follow these naming conventions (do not rename files / classes / methods to something other than what is listed below):

`Lab03.java`.
* Contains the main method. All it does is construct a RosterManager and calls .run().

`RosterManager.java`
* A class representing the highest layer of managing the class roster containing:
    * An array of Courses
    * Total number of current courses.
* Public Methods
    * `run()`
        * Runs the main loop of the program. This prints the menu, accepts user input, and handles each of the user commands.
        * Handles the custom Exceptions and prints the messages to the user.
    * `addCourse(Course c) throws DuplicateCourseException, CourseLimitException`
        * Adds a Course to the array of Courses
    * `deleteCourse(String courseCode) throws CourseNotFoundException, EmptyCourseListException`
        * Deletes a course from the array of Courses.
    * `addStudent(String courseCode, Student s) throws StudentLimitException, DuplicateStudentException, CourseNotFoundException`
        * Adds a student to an already existing courseCode.
    * `deleteStudent(int id, String courseCode) throws EmptyStudentListException, StudentNotFoundException, CourseNotFoundException`
        * Deletes a student to an already existing courseCode.
    * `printRoster()`
        * Prints the information for all courses and their enrolled students.

`Course.java`
* Class that represents a course containing:
    * A String representing the course code.
    * A String representing the course name.
    * An int representing the current enrollment in the course.
    * An array of Students enrolled in the course.
* Public Methods
    * `addStudent(Student s) throws StudentLimitException, DuplicateStudentException`
        * Adds a student to the course. The student array order will need to be maintained (see requirements above).
    * `removeStudent(int studentId) throws EmptyStudentListException, StudentNotFoundException`
        * Removes a student from the course based on their PERM number. 
    * Any setters / getters you need to use in your program.

`Student.java`
* Class representing a student containing:
    * An int representing the PERM number.
    * A String representing the student’s first name.
    * A String representing the student’s last name.
* Public Methods
    * Any setters / getters you need to use in your program.

`ClassRosterUI.java`
* Class representing the I/O between the program and user.
* There really isn’t a "state" to keep when using this object, so all of its methods can be static.
* Public Methods
    * `static void printMenu()`
        * Prints the menu options to the console.
    * `static String getCommand()`
        * Gets the command from the user. Loops until the user enters a valid command and returns the valid command.
    * Any method to help obtain information from the user including:
        * Getting the course information when adding a new course.
        * Getting the course code when adding a student to a course or trying to remove a course.
        * Getting the student information when adding a student to a course.
        * Getting the PERM number when trying to remove a student from a course.

# Testing
Unlike the previous labs, this one will have a large testing component. You will write a test suite in JUnit (as discussed in lecture) going through various cases and confirming your program handles them as expected. To use JUnit, review the notes from [Lecture 5](https://ucsb-cs56.github.io/s19/lectures/lect05/).

Write your test suite in a file named `ClassRosterTester.java`. At the very minimum, you will:

* Test the `RosterManager`’s public methods that add / delete courses and students. Since these methods accept constructed objects, you can manually create them in your tests, call the appropriate methods, and write assert statements (or throw Exceptions) to confirm it’s behaving the way you expect for all cases listed in the specifications.
* You should test normal and boundary cases such as "does my course list contain Students in the proper order when inserting them?" or "is the correct Exception thrown when I try to add a student to a class at max capacity?" There are many things to keep in mind, but it's important to be thorough and cover the normal and boundary cases according to the specifications!

# Examples

The following is an example of adding some courses and students to our Roster as well as some (but not all) error cases your program should handle:

<pre>
Welcome to Class Roster Manager!
Select an action based on the following menu: 
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>ac</b>
Enter Course Code: <b>CS56</b>
Enter Course Name: <b>Advanced Applications Programming</b>
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>as</b>
Enter course code for Student: <b>CS56</b>
Enter PERM: <b>12345678</b>
Enter last name: <b>Wang</b>
Enter first name: <b>Richert</b>
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>as</b>
Enter course code for Student: <b>CS56</b>
Enter PERM: <b>87654321</b>
Enter last name: <b>Doe</b>
Enter first name: <b>John</b>
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>p</b>
********************
CS56: Advanced Applications Programming
Enrolled: 2
	87654321 | Doe, John
	12345678 | Wang, Richert
********************
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>ac</b>
Enter Course Code: <b>CS32</b>
Enter Course Name: <b>Object Oriented Design and Implementation</b>
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>as</b>
Enter course code for Student: <b>CS32</b>
Enter PERM: <b>55555555</b>
Enter last name: <b>Doe</b>
Enter first name: <b>Jane</b>
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>as</b>
Enter course code for Student: <b>fjdskalf</b>
ERROR: Could not find course.
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>p</b>
********************
CS56: Advanced Applications Programming
Enrolled: 2
	87654321 | Doe, John
	12345678 | Wang, Richert
CS32: Object Oriented Design and Implementation
Enrolled: 1
	55555555 | Doe, Jane
********************
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>ds</b>
Enter course code for Student: <b>CS56</b>
Enter PERM: <b>8765432</b>
ERROR: Student could not be found.
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>ds</b>
Enter course code for Student: <b>CS56</b>
Enter PERM: <b>87654321</b>
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>p</b>
********************
CS56: Advanced Applications Programming
Enrolled: 1
	12345678 | Wang, Richert
CS32: Object Oriented Design and Implementation
Enrolled: 1
	55555555 | Doe, Jane
********************
----------
ac: Add Course
dc: Drop Course
as: Add Student
ds: Drop Student
 p: Print ClassRoster
 q: Quit Program
----------
Enter Command: <b>q</b>
</pre>

# Submitting to Gradescope

The lab assignment "Lab03" should appear in your Gradescope dashboard in CMPSC 56. If you haven't submitted anything for this assignment yet, Gradescope will prompt you to upload your files.

**Remember to add your partner to Groups Members for this submission on Gradescope if applicable. Please include both names and PERM numbers for each submitted file. At this point, if you worked in a pair, it is a good idea for both partners to log into Gradescope and check if you can see the uploaded files for Lab03.**

For this lab, you will need to upload your source files:

**NOTE: Please be sure to ONLY submit these files in order to help our TAs streamline grading. DO NOT include additional files (such as .class files). Also, DO NOT put your files into any subdirectories or define specific packages when submitting your files to Gradescope.**:

* `ClassRosterUI.java`
* `Course.java`
* `Lab03.java`
* `RosterManager.java`
* `Student.java`
* `CourseLimitException.java`
* `CourseNotFoundException.java`
* `DuplicateCourseException.java`
* `DuplicateStudentException.java`
* `EmptyCourseListException.java`
* `EmptyStudentListException.java`
* `StudentLimitException.java`
* `StudentNotFoundException.java`
* `ClassRosterTester.java`

You either can navigate to your files, "drag-and-drop" them into the "Submit Programming Assignment" window, or use your private GitHub repo to submit your work (if you do this, be sure that your repo ONLY contains the .java files and doesn't have any .java files in sub directories).

If you already submitted something on Gradescope, it will take you to their "Autograder Results" page. There is a "Resubmit" button on the bottom right that will allow you to update the files for your submission.

Most of the programming assignments for this class will not be autograded in the same way you may have seen in earlier courses. We will use Gradescope mainly as a "dropbox" for all of your assignments. Our staff will manually grade your assignments and assign a score to your submission.

Since Gradescope autograding is disabled, it is <b>very</b> important for you to test and compile your code locally according to the specifications of this lab. As a software developer, it's an important skill to think of correct functionality and test cases on your own. Be sure to do this before submitting your code to Gradescope. If our staff cannot run your code, then you will receive a 0 for this assignment (and this will not be apparent when submitting your code since Gradescope will not actually compile your code for this assignment).

Your submission will initially have a score of `0.0/100`. Don't worry - this is normal. Once the assignment has been graded by our staff, your actual score will be updated.

