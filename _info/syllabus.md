---
title: "Syllabus, CMPSC 56, Spring 2019"
layout: handout
ready: false
---

<div style="font-size:110%;" markdown="1">

Basic Facts
-----------

* **Course Web Site**: <https://ucsb-cs56.github.io/s19/>
* **Instructor**:  [Richert Wang](http://www.cs.ucsb.edu/~richert)  
   * Email is richert@ucsb.edu, BUT please use [Piazza] (piazza.com/ucsb/spring2019/cs56s19/home) for course related communication.
* **Lecture**: MW 5pm - 6:15pm, PHELPS 3526. ATTENDANCE REQUIRED. 
* **TAs**: {{site.ta_list_full}} (contact via Piazza)
* **Mentors**: {{site.mentors_list}} (contact via Piazza)
* **Lab** (50 minute lab section): Thursdays 4pm, 5pm, 6pm - Phelps 3525. ATTENDANCE REQUIRED.                                         
* **Office Hours**: See <https://ucsb-cs56.github.io/s19/info/office_hours/>

# Required Resources

* Textbooks:
   * "Head First Java" - K. Sierra and B. Bates, 2nd edition
   * "Head First Design Patterns" - Eric and Elisabeth Freeman

Official UCSB Catalog Description
---------------------------------

<div style="background-color:#eee; border: 8px inset #333; font-size:90%; margin:1em; width:45em; padding: 0.5em;" markdown="1">

CMPSC 56: Advanced Applications Programming

**Prerequisite:** Computer Science 24 with a grade of C or better.

**Recommended Preparation:** Students are encouraged to complete Computer Science 32 prior to enrolling in Computer Science 56.

Advanced application programming using a high-level, virtual-machine-based language. Topics include generic programming, exception handling, programming language implementation; automatic memory management, and application development, management, and maintenance tools; event handling, concurrency and threading, and advanced library use.

</div>

## A few course policies in brief

* If you are registered for another UCSB course that overlaps with this one, you MUST HAVE specific written permission from both instructors, or I am within my rights to give you a failing grade on any work you miss as a result, and will NOT make any accommodations for you.
* Collaboration is only permitted when specifically allowed for — otherwise, you must do your own work.
* Attendance is required at all lectures and labs (discussion sections).
  * I recognize that some absences (e.g. minor illnesses, mishaps, etc.) are unavoidable. Litigating whether each of these is "excused" or not isn't a good use of anyone's time, so instead we will drop the lowest two grades from everyone's homework. This way, absences (or failure to submit homework) does not unduly penalize your grade unless it becomes excessive.
* <strong>You</strong> must turn in your homework in lecture the day that it is due.
* We will use the [gradescope system](https://gradescope.com) this quarter. More instructions on gradescope will be given in lecture and lab assignments.
* All regrade requests must be made on Gradescope and we will not consider a regrade request one week after the assignment grades are distributed back to students.

You may NOT: 

* Turn in homework on a day other than when it is due (early or late).
* Have someone else turn in your homework for you (that will be considered academic dishonesty).
* Drop it off with the instructor or TA to be graded later.

## What this course is about

Our goal is to learn Java - but not just to learn Java for the sake of learning Java. After all, some of you already "know Java", at some level.

Our bigger goals are:
* to practice using big APIs to get stuff done - a very relevant real world job skill!
* to learn how to learn a new language or technology - something you'll do a lot in your career
   * to learn about a few specific topics: the JVM, threads, automatic garbage collection, etc..
   * to learn some professional-level, real-world programming practices.

At this point in the curriculum, you have obtained a lot of experience with C++ and dynamic memory management. When using Java, a lot of this manual memory management is done under-the-hood without any explicit action required by the programmer. Whoa! Then why do we bother with C++ and haven't just used Java the entire time? Well, programming languages are like tools - some are better for certain jobs than others. Java does have its cons and we will explore the behavior of Java in this course. In addition to learning the mechanics, we will discuss real-world applications and organizational skills of managing Java applications.

Learning the details of programming requires <strong>A LOT</strong> of practice, like learning any new skill. Making mistakes is an essential part of learning as long as you learn from them! Questions like "I wonder what will happen if I do this..." or "How will Java behave in this case..." is a great way to investigate and observe the functionality and limitations of a programming language (there are many programming languages available to software developers and each have their specific pros and cons that may or may not be the best choice for the problems you are trying to solve).

I find the best way to practice is to <strong>rapid prototype</strong> constantly. Writing simple snippets of code to test and confirm your understanding allows you to 1) practice typing out code, which makes you more comfortable with the language and 2) solidify your understanding of the specific behavior of the programming language functionality.

# Course Grades

Letter grades will be determined by the end of the course after all labs, homeworks, and exams have been computed. I can say that I will not grade harder than a traditional straight scale (90% = A-, 80% = B-, etc.). However, I will adjust the letter grades accordingly based on the overall performance of the class at the end of the course. If you are concerned about your grade in the class, I encourage you to discuss the matter with me during my office hours. Please come talk to me sooner rather than later so there can be some time where we can help you succeed in the course.

Your course grade will be determined as follows:

| Grade Item                        | Percentage of Final Grade |
|-----------------------------------|---------------------------|
| Midterm (Wed 5/1)                 | 25 %                      |
| Final (Tues 6/11 7:30pm - 10:30pm)| 35 %                      |
| Homeworks                         | 10 %                      |
| Labs                              | 30 %                      |

In general, homeworks will be assigned throughout the quarter, and are due in lecture on the due date.

There will be labs assigned throughout the quarter as well. You will work on the labs during your assigned lab section, and most likely on your own time outside of your assigned lab section. Please be sure to check the due dates for all assignments on the course page and calendar.

# Late work

I will consider late submissions <strong>only for medical or family emergencies where legitimate documentation can be provided</strong>. This does not include overwhelming workload from other courses, scheduling conflicts, vacation plans, weddings, etc. 

* There will not be any make ups for examinations.
* Two of the lowest homework scores will be dropped. Late homework submissions will not be accepted. However, even if you know you will not be able to submit a homework on time, I highly encourage you to complete it anyways since the homeworks will help prepare you for the examinations.
* All labs must be submitted by the due date. Depending on the case, your TA may consider grading your lab with a late penalty (and usually for cases where the submission was done very soon after the deadline). However, this is not an official policy and you risk receiving a zero for a late lab submission. Even if we do decide to grade a late submission, some penalty will be applied (the longer past the deadline, the higher the penalty).

Accommodations for disabilities
-------------------------------

Students with disabilities may request academic accommodations for exams online through the UCSB Disabled Students Program at [http://dsp.sa.ucsb.edu/](http://dsp.sa.ucsb.edu/). Please make your requests for exam accommodations through the online system as early in the quarter as possible to ensure proper arrangement.

Managing stress
---------------

Personal concerns such as stress, anxiety, relationships, depression, cultural differences, can interfere with the ability of students to succeed and thrive. For helpful resources, please contact UCSB Counseling & Psychological Services (CAPS) at 805-893-4411 or visit [http://counseling.sa.ucsb.edu/](http://counseling.sa.ucsb.edu/).

Responsible scholarship
-----------------------

Honesty and integrity in all academic work is essential for a valuable educational experience.  The Office of Judicial Affairs has policies, tips, and resources for proper citation use, recognizing actions considered to be cheating or other forms of academic theft, and students’ responsibilities, available on their website at: http://judicialaffairs.sa.ucsb.edu. **Students are responsible for educating themselves on the policies and to abide by them.**

Furthermore, for general academic support, students are encouraged to visit Campus Learning Assistance Services (CLAS) early and often. CLAS offers instructional groups, drop-in tutoring, writing and ESL services, skills workshops and one-on-one consultations. CLAS is located on the third floor of the Student Resource Building, or visit http://clas.sa.ucsb.edu

Standard Disclaimer
-------------------

This syllabus is as accurate as possible, but is subject to change at the instructor's discretion, within the bounds of UC policy.