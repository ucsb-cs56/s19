---
layout: lab
num: lab05
ready: true
desc: "Message Board"
assigned: 2019-05-20 08:00
due: 2019-06-02 23:59
---

# Goals

For this lab, we will focus on writing a Message Board application utilizing the Observer design pattern and various collection libraries for keeping track of users and their message posts. Unlike previous labs, we will focus on implementing the management of the posts without a User Interface - it's common to have teams work on application components in parallel. In addition to JUnit tests, you should be able to test your application by making method calls to the message board to ensure your code is working as expected.

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

Message board applications generally allow users to post messages and reply to messages that have already been posted. Unlike previous labs, you will not be required to write a User Interface. Rather, it's your job to test the public methods for the message board by writing a test program and observe that the correct output is displayed, as well as writing JUnit tests that cover various cases ensuring functionality works as expected.

Each message board post will contain zero or more <b>tags</b> indicating the category that the post is associated with. You may assume that inputed tags are alpha-numeric Strings that do not contain spaces. Your program should treat the tags as case insensitive (no difference between upper and lower case).

Users can subscribe themselves to receive updates for certain tags if desired, and the Message Board application will <b>push</b> notifications to users if the user has subscribed themselves to receive updates for certain tags. Your code must follow the observer pattern and receive an update whenever a post containing a subscribed tag is added to the message board. If a User is subscribed to multiple tags for a post that was just added to the system, the user should only receive one update for the post that was just added. The user who just added a post to the system should not receive an update for their own post.

A post may also be considered a reply to other posts and the system must keep track of post / reply relationships. Posts that are replies to other posts may also have their own replies and you may assume there is no limit to the depth of replies the system can manage (A post may have a reply, and that post may have a reply, and that post may have replies, etc.).

Each Post object should contain the following information:

* A list of Strings representing the post's tags.
* A String containing the message of the post.
* A list of posts that are replies for this post. 
* An int representing a unique Post ID. The Post IDs are unique positive numbers starting at 1 and incrementally increases every time a new Post in the system is created.
* An int representing a Parent Post ID. If the post is not a reply to an existing post, then this is set to -1.
    * If the post is a reply to an existing post, then this Parent Post ID is the Post ID of the message this post is replying to.
    * If the post is a reply to an existing post, no additional tags will be added to this post. However, the reply post will contain the same tags as the post it is replying to.
* A reference to a User object who made the post.
* Any accessor / mutator methods or other fields that your code needs in order to manage posts in the system.

A User object contains the following information:

* An int representing a unique User ID. Similar to Post objects, the User IDs are unique positive numbers starting at 1 and incrementally increases every time a new User is created.
* A list of String tags that this user is subscribed to.
* A list of Post objects that the user has made to the system.
* Any accessor / mutator methods or other fields that your code needs in order to manage posts in the system.

An example of what a Post (that is not a reply) will look like when printed to the console is shown below:

```
--
Post ID: 1
Tags: food cooking 
Posted by UserID: 1
Message: Cooking is fun!
--
```

If the Post is in reply to a message, it will contain the Post ID it is replying to as follows:

<pre>
--
Post ID: 2
Tags: food cooking 
Posted by UserID: 3
<b>Re: to Post ID: 1</b>
Message: I agree!
--
</pre>

Once a message board is created, the system will print out a "play-by-play" log of what happens when certain events are created (such as users subscribing to tags, or when posts are added to the system, etc.).

# Code Organization

This lab will provide a lot of flexibility on how you design and maintain the message board. The only restrictions are:

* You must use the observer design pattern to allow Users to receive push updates whenever a post containing a subscribed tag is added to the message board.
* `Subject.java` is an interface with the following methods:

    * `public void registerUserTag(String tag, User user)`
        * Subscribes a user to receive updates for any posts containing the specific tag.

    * `public void removeUserTag(String tag, User user)`
        * Unsubscribes a user from receiving updates for any posts containing the specific tag.

    * `public void notifyUsers(Post p)`
        * Whenever a post is added, notifyUsers is called to send updates (if any) to subscribed users for the specific tags in the post.

* User objects should implement the Observer interface whenever the system sends an update to the user with a recently added post. 
* `Observer.java` is an interface with the following methods:

    * `public void update(Post post)`
        * Updates the user by simply printing out the posted message. Your console output should state that the displayed post is a result of updating a subscribed user.

* `Post.java`
    * Should have the following constructor when creating Posts to add to the system:

        * `public Post(ArrayList<String> tags, String message, User user, int parentID)`

* `User.java`
    * This class should implement the `Observer` interface since User objects are the ones receiving updates from the system.
    * Should have the following constructor when creating Users in the message board application:

        * `public User()`


* `MessageBoardManager.java`
    * This class will implement the `Subject` interface since it is expected to send updates whenever a post is made to the system.
    * The data structure(s) used in this class to manage posts is up to you. Think about what may be needed to efficiently perform the following operations:

    * `public void addPost(Post p)`
        * Adds a Post to the system. Sends an update to any subscribed users that have registered to receive updates containing the tags in this post.
         * If the postID already exists, then an ERROR stating so is displayed (no update is made).
         * The order of the displayed user updates does not matter as long as they exist and are correct.

    * `public void addReply(Post reply)`
        * Adds a Reply to the message board. For our purposes, a Reply message is a Post with a Parent Post ID != -1 (Parent Post ID should be the Post ID of the Post object that it is replying to).
        * If the reply's Parent Post ID does not exist in the system, then an ERROR stating so is displayed (no update is made).

    * `public void displayTagMessages(String tag)`
        * Displays all Posts containing the String tag in the message board.

    * `public void displayKeywordMessages(String keyword)`
        * Displays all posts containing the String keyword in the message portion of a post.

    * `public void displayThread(int postID)`
        * Displays the entire thread that the postID belongs to.
            * Note that it is possible that the postID may not be the "root" post in the thread hierarchy.
        * The order of the displayed post must be a depth first traversal that starts with the original post (i.e. Parent Post ID == -1).
            * Then the reply posts are displayed (if any).
            * If a reply also contains replies, this is displayed first before moving on with the rest of the replies for the parent post.
                * An example of this will be shown in the sample console output below.
        * If the postID does not exist, then an ERROR stating so is displayed (nothing is displayed).

    * `public void displayUserPosts(User user)`
        * Displays all posts (including replies) the user made.

* `Lab05.java` will contain your main method. You should write various scenarios to observe the expected behavior. Note, you will also need to write JUnit tests for various scenarios testing the public methods for `MessageBoardManager` stated above.

# Console UI Example

An example of a "play-by-play" console log for the following scenario is provided below. You will write similar scenarios in your `Lab05.java` file.

```
// Lab05.java
public class Lab05 {

    public static void main(String[] args) {
        MessageBoardManager messageBoard = new MessageBoardManager();
        
        // Create users and register tags for them.
        User u1 = new User();
        messageBoard.registerUserTag("food", u1);
        User u2 = new User();
        messageBoard.registerUserTag("cooking", u2);
        User u3 = new User();
        messageBoard.registerUserTag("FOOD", u3);
        
        // Constructing a Post with tags
        ArrayList<String> tags = new ArrayList<String>();
        tags.add("food");
        tags.add("cooking");
        Post p1 = new Post(tags, "Cooking is fun!", u1, -1);
        messageBoard.addPost(p1);
        
        // Removing u2 from the "cooking" tag
        messageBoard.removeUserTag("cooking", u2);
        
        // Creating a reply to p1
        Post p1_1 = new Post(p1.getTags(), "I agree!", u3, p1.getPostID());
        messageBoard.addReply(p1_1);
        
        // Creating another reply to p1
        Post p1_2 = new Post(p1.getTags(), "and delicious!", u1, p1.getPostID());
        messageBoard.addReply(p1_2);
        
        // Creating a reply to reply p1_1
        Post p1_1_1 = new Post(p1_1.getTags(), "Mayo is gross.", u2, p1_1.getPostID());
        messageBoard.addReply(p1_1_1);
        
        // Displays entire thread for p1_1_1's hierarchy
        messageBoard.displayThread(p1_1_1.getPostID());
        
        // Displays all posts for user u1
        messageBoard.displayUserPosts(u1);
        
        // Creates a new post with new tags
        ArrayList<String> tags2 = new ArrayList<String>();
        tags2.add("sports");
        tags2.add("hockey");
        Post p2 = new Post(tags2, "Go Kings Go!", u3, -1);
        messageBoard.addPost(p2);
        
        // Displays all posts containing the tag "cooking"
        messageBoard.displayTagMessages("cooking");
        
        // Displays all posts containing the keyword "fun"
        messageBoard.displayKeywordMessages("fun");
        
        // Displays all posts
        messageBoard.displayKeywordMessages("AgrEE");
    }
}
```
```
// Console Output
^^^^^ Adding tag: food for User ID: 1 ^^^^^

^^^^^ Adding tag: cooking for User ID: 2 ^^^^^

^^^^^ Adding tag: FOOD for User ID: 3 ^^^^^

+++ Adding Post to MessageBoard +++
--
Post ID: 1
Tags: food cooking 
Posted by UserID: 1
Message: Cooking is fun!
--
++++++++++++++++++++++++++++++++++++

***** Updating UserID: 3 *****
--
Post ID: 1
Tags: food cooking 
Posted by UserID: 1
Message: Cooking is fun!
--
******************************

***** Updating UserID: 2 *****
--
Post ID: 1
Tags: food cooking 
Posted by UserID: 1
Message: Cooking is fun!
--
******************************

^^^^^ Removing tag: cooking for User ID: 2 ^^^^^

+++ Adding Post to MessageBoard +++
--
Post ID: 2
Tags: food cooking 
Posted by UserID: 3
Re: to Post ID: 1
Message: I agree!
--
++++++++++++++++++++++++++++++++++++

***** Updating UserID: 1 *****
--
Post ID: 2
Tags: food cooking 
Posted by UserID: 3
Re: to Post ID: 1
Message: I agree!
--
******************************

+++ Adding Post to MessageBoard +++
--
Post ID: 3
Tags: food cooking 
Posted by UserID: 1
Re: to Post ID: 1
Message: and delicious!
--
++++++++++++++++++++++++++++++++++++

***** Updating UserID: 3 *****
--
Post ID: 3
Tags: food cooking 
Posted by UserID: 1
Re: to Post ID: 1
Message: and delicious!
--
******************************

+++ Adding Post to MessageBoard +++
--
Post ID: 4
Tags: food cooking 
Posted by UserID: 2
Re: to Post ID: 2
Message: Mayo is gross.
--
++++++++++++++++++++++++++++++++++++

***** Updating UserID: 1 *****
--
Post ID: 4
Tags: food cooking 
Posted by UserID: 2
Re: to Post ID: 2
Message: Mayo is gross.
--
******************************

***** Updating UserID: 3 *****
--
Post ID: 4
Tags: food cooking 
Posted by UserID: 2
Re: to Post ID: 2
Message: Mayo is gross.
--
******************************

##### Displaying thread for PostID: 4 #####
--
Post ID: 1
Tags: food cooking 
Posted by UserID: 1
Message: Cooking is fun!
--
--
Post ID: 2
Tags: food cooking 
Posted by UserID: 3
Re: to Post ID: 1
Message: I agree!
--
--
Post ID: 4
Tags: food cooking 
Posted by UserID: 2
Re: to Post ID: 2
Message: Mayo is gross.
--
--
Post ID: 3
Tags: food cooking 
Posted by UserID: 1
Re: to Post ID: 1
Message: and delicious!
--
##############################

##### Displaying all posts for User ID: 1 #####
--
Post ID: 1
Tags: food cooking 
Posted by UserID: 1
Message: Cooking is fun!
--
--
Post ID: 3
Tags: food cooking 
Posted by UserID: 1
Re: to Post ID: 1
Message: and delicious!
--
##############################

+++ Adding Post to MessageBoard +++
--
Post ID: 5
Tags: sports hockey 
Posted by UserID: 3
Message: Go Kings Go!
--
++++++++++++++++++++++++++++++++++++

##### Displaying posts with tag: cooking #####
--
Post ID: 1
Tags: food cooking 
Posted by UserID: 1
Message: Cooking is fun!
--
--
Post ID: 2
Tags: food cooking 
Posted by UserID: 3
Re: to Post ID: 1
Message: I agree!
--
--
Post ID: 3
Tags: food cooking 
Posted by UserID: 1
Re: to Post ID: 1
Message: and delicious!
--
--
Post ID: 4
Tags: food cooking 
Posted by UserID: 2
Re: to Post ID: 2
Message: Mayo is gross.
--
##############################
##### Displaying posts with keyword: fun #####
--
Post ID: 1
Tags: food cooking 
Posted by UserID: 1
Message: Cooking is fun!
--
##############################

##### Displaying posts with keyword: AgrEE #####
--
Post ID: 2
Tags: food cooking 
Posted by UserID: 3
Re: to Post ID: 1
Message: I agree!
--
##############################
```

## Some hints and useful tools

I would recommend looking into some Java collections (or similar) such as:

* `java.util.HashSet`
    * May help ensure multiple updates are not sent to the same user when a post is added.

* `java.util.LinkedHashMap`
    * May help organize key value pair relationships that will make your algorithms (and life) easier when executing certain methods in `MessageBoardManager`.

* Using `static` fields
    * May help ensure User ID and Post ID are unique when constructing them.

* In general, careful book-keeping will be important.

# Testing

In addition to displaying events as shown above, you will need to write JUnit tests in a test file named `MessageBoardTester.java`. This file will test various scenarios for the public MessageBoard methods stated above.

# Code Organization and Design

In order to help our TAs understand the overall design approach of your code, please submit a image containing a UML diagram with your code organization. Name this file `UML(.jpg, .jpeg, .pdf, or .png). 

# Submitting to Gradescope

The lab assignment "Lab05" should appear in your Gradescope dashboard in CMPSC 56. If you haven't submitted anything for this assignment yet, Gradescope will prompt you to upload your files.

**Remember to add your partner to Groups Members for this submission on Gradescope if applicable. Please include both names and PERM numbers for each submitted file. At this point, if you worked in a pair, it is a good idea for both partners to log into Gradescope and check if you can see the uploaded files for Lab05.**

For this lab, you will need to upload all of your .java source files and your UML class diagram file.

**NOTE: Please be sure to ONLY submit the .java files in order to help our TAs streamline grading. DO NOT include additional files (such as .class files). Also, DO NOT put your files into any subdirectories or define specific packages when submitting your files to Gradescope**:

You either can navigate to your files, "drag-and-drop" them into the "Submit Programming Assignment" window, or use your private GitHub repo to submit your work (if you do this, be sure that your repo ONLY contains the .java files and doesn't have any .java files in sub directories).

If you already submitted something on Gradescope, it will take you to their "Autograder Results" page. There is a "Resubmit" button on the bottom right that will allow you to update the files for your submission.

Most of the programming assignments for this class will not be autograded in the same way you may have seen in earlier courses. We will use Gradescope mainly as a "dropbox" for all of your assignments. Our staff will manually grade your assignments and assign a score to your submission.

Since Gradescope autograding is disabled, it is <b>very</b> important for you to test and compile your code locally according to the specifications of this lab. As a software developer, it's an important skill to think of correct functionality and test cases on your own. Be sure to do this before submitting your code to Gradescope. If our staff cannot run your code, then you will receive a 0 for this assignment (and this will not be apparent when submitting your code since Gradescope will not actually compile your code for this assignment).

Your submission will initially have a score of `0.0/100`. Don't worry - this is normal. Once the assignment has been graded by our staff, your actual score will be updated.
