---
layout: lab
num: lab04
ready: true
desc: "Music Library"
assigned: 2019-05-06 08:00
due: 2019-05-19 23:59
---

# Goals

For this lab, the focus will be on writing an music organization application utilizing Interfaces, Inheritance, and Polymorphism concepts. The application will read music data from a text file into the program, organize the information, and output the information to files according to the specification.

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

Music Library applications (i.e. iTunes, Windows Media Player, etc.) organize music data that allows users to search, sort, and play songs in their library. This application is used more to organize and archive a music library of various music track formats. We obviously won't cover all functionality in a full-featured music library, but we will write an application that is able to read music data from a text file (either on your local hard drive or on the web), and organize the data such that it will be easy to display the entire contents of the imported library based on artist and song names.

You will implement a simple User Interface that prompts the user to read a predefined file containing information on various Music Tracks (`MusicList.txt`) on your local hard drive or from the web. Your program will use inheritance and polymorphism to organize your code in order to differentiate two types of music tracks: Digital or Vinyl (we can obviously extend this to many types of formats, but for this lab we will assume these are the only two formats that will be imported), and write the music track data to specific output files.

A `MusicList.txt` file will contain multiple music track data in a fixed format. All pieces of the music track data will be separated by a `;`. For a digital music track, the format will look as follows:

`(Track_title);(Track_length);(Artist);(Album);(Release_date);(Format_Type);(BitRate)`

For a vinyl music track, the format will look as follows:

`(Track_title);(Track_length);(Artist);(Album);(Release_date);(Format_Type);(RPM)`

The only difference between a digital music track will be the `Format_Type`- a `D` value implies the track is in a digital format and a `V` value imples the track is in Vinyl format. For digital music tracks, the `BitRate` value is the encoding used for the track. For vinyl music tracks, the `RPM` is the rotations per minute that the track was designed to be played on.

The overall flow of execution in your program will:

* Display a welcome message and prompt the user to either read the `MusicList.txt` file from your hard drive or the web. You may assume `MusicList.txt` exists in the root directory of your java project if you are reading the text file from your hard drive. If you are reading the `MusicList.txt` file from the web, you may assume that the file can be accessed at the URL: [https://sites.cs.ucsb.edu/~richert/cs56/misc/lab04/MusicList.txt](https://sites.cs.ucsb.edu/~richert/cs56/misc/lab04/MusicList.txt).
    * The program will prompt the user if they want to read the `MusicList.txt` file from disk by entering the value `D` or `d`.
    * The program will prompt the user if they want to read the `MusicList.txt` file from the web by entering the value `W` or `w`.
    * Your program will continue to prompt the user for the correct input if the valued entered is incorrect.
* When reading the data and importing the music track information into your application, you will organize the data in two ArrayList of ArrayLists ("buckets") - one of these structures will organize the music tracks according to the Artist name, and the other structure will organize the music tracks according to the Track Title.
    * The index of your ArrayList will determine where a Music Track object will be placed. We will sort the music track objects into appropriate "buckets". For example, when organizing tracks by Artists, arrayList[0] will contain an ArrayList of Music Tracks where each index in this inner-list will refer to Music Tracks where the Artist name starts with an `A` or `a` sorted in lexicographical order (arrayList[1] contains Artist names starting with `B` or `b` and so on). In the event of a tie, the order will be determined by the Track name in lexicographical order.
    * The same logic applies when organizing Music Tracks by Track Title - in the event of a tie in this case, the order will be determined by the Artist name in lexicographical order.
        * The idea here is it will be easy to simply traverse the ArrayList of ArrayList of Music Tracks and have everything already in sorted order based on Artist names and Track titles.
    * Your implementation should only construct one object for each Music Track on the heap, and your ArrayLists will reference these objects on the heap appropriately.
    * You may assume that the Artist and Track Titles will always start with an Alpha character [A-Za-z], but the organization / sorting of the Music Tracks will be case insensitive (there is no difference between `A` and `a`). 
* Once all of the music track data is imported and organized in your application, the application will prompt the user to write the information in a specific format. The user may enter `A` or `a` to write the Music Library to a file sorted by Artist, `T` or `t` to write the Music Library to a file sorted by Track title, or `Q` or `q` to quit the application.
    * Your program will continue to prompt the user for the correct input if the valued entered is incorrect or until the user enters `Q` or `q`.
    * The program will write to a file named `artistOutput.txt` if the user wants to sort the music library based on the Artist name.
    * The program will write to a file named `titleOutput.txt` if the user wants to sort the music library based on the Track name.
    * Examples of the resulting output for each file can be seen at [artistOutput.txt](https://sites.cs.ucsb.edu/~richert/cs56/misc/lab04/artistOutput.txt) and [titleOutput.txt](https://sites.cs.ucsb.edu/~richert/cs56/misc/lab04/titleOutput.txt).
* There will be a fixed format and specific ordering when writing the Music Library to a file defined as follows and illustrated in the sample output files:
    * The Track Title field will be left-justified with 40 spaces.
    * The Artist field will be left-justified with 40 spaces.
    * The Album field will be left-justified with 40 spaces.
    * The Track Length field will be left-justified with 7 spaces.
    * The year field will be left-justified with 5 spaces.
    * The specific additional Track format information will be left-justified with 40 spaces.

# Console UI Example

The following output is an example of how a user will interact with your application (user input is in bold) and comments are not part of the output but used for context:

<pre>
Welcome to the Music Library Application!
Enter `D` to read the music file from your local disk or `W` to read the music file from the web: <b>Q</b>
Invalid Input.
Enter `D` to read the music file from your local disk or `W` to read the music file from the web: <b>wrong</b>
Invalid Input.
Enter `D` to read the music file from your local disk or `W` to read the music file from the web: <b>w</b>
<b>// reads MusicList.txt from the web.</b>
Enter `A` to output tracks by Artists or `T` to output tracks by Title. Enter `Q` to quit: <b>wrong</b>
Invalid Input.
Enter `A` to output tracks by Artists or `T` to output tracks by Title. Enter `Q` to quit: <b>A</b>
<b>// Generates artistOutput.txt</b>
Enter `A` to output tracks by Artists or `T` to output tracks by Title. Enter `Q` to quit: <b>t</b>
<b>// Generates titleOutput.txt</b>
Enter `A` to output tracks by Artists or `T` to output tracks by Title. Enter `Q` to quit: <b>q</b>
<b>// Quits application</b>
</pre>

# Code Organization

You will be able to implement your program satisfying the specifications above, but there will be some requirements you must follow:

`Lab04.java`
* Contains the main method which constructs a `MusicManager` object and starts the program.

`MusicManager.java`
* Constructs the class objects maintining the two different ArrayLists of ArrayLists: `ArtistBucket` and `TitleBucket` (described below).
* `start()`
    * Prompts the user to read the file from the hard drive or web.
    * Prompts the user to write to the `artistOutput.txt` or `titleOutput.txt` files according to the specifications above.
    * The output files will be written to the root directory of your Java Project.
* Provide any accessor / getter methods you need in your program.
* You may consider breaking the UI functionality into a separate `MusicLibraryUI` class.

`MusicTrackInterface.java`
* An Interface with the following fields and abstract methods that need to be overwritten by classes implementing this Interface:
    * `public abstract String getTitle()`
        * Returns the Music Track title.
    * `public abstract String getLength()`
        * Returns the Music Track length.
	* `public abstract String getArtist()`
        * Returns the Music Track Artist name.
    * `public abstract String getAlbum()`
        * Returns the Music Track Album name.
    * `public abstract int getYear()`
        * Returns the Music Track year.
    * `public abstract String getAdditionalInfo()`
        * Returns a String containing additional information based on the type (Digital or Vinyl) of the Music Tracks.

`MusicTrack.java`
* An abstract class that contains the common information between both Digital and Vinyl Music Track formats.
* The constructor should set the common fields between both Digital adn Vinyl Music Track formats.
* This class is abstract since we cannot define the `getAdditionalInfo()` here - this must be done in further subclasses.

`DigitalTrack.java`
* A class that extends `MusicTrack`
* Contains the specific information (bitRate) all digital tracks contain.
* Overwrites the `getAdditionalInfo()` method containing a string containing additional information specific to a digital Music Track.

`VinylTrack.java`
* A class that extends `MusicTrack`
* Contains the specific information (RPM) all Vinyl tracks contain.
* Overwrites the `getAdditionalInfo()` method containing a string containing additional information specific to a Vinyl Music Track.

`BucketInterface.java`
* An Interface with the following abstract methods that need to be overwritten by classes implementing this Interface:
    * `public abstract void addItem(MusicTrack itemToAdd)`
        * Adds the Music Track into the appropriate `ArtistBucket` or `TitleBucket`.
    * `public abstract ArrayList<ArrayList<MusicTrack>> getBuckets()`
        * Getter method that returns the ArrayList of ArrayList structure.

`ArtistBucket.java`
* Implements the `BucketInterface` methods.
* The data structure to use (as stated in the specification) is an ArrayList containing ArrayList<MusicItem> where Music Track information is placed in the appropriate index based on the Artist name.

`TitleBucket.java`
* Implements the `BucketInterface` methods.
* The data structure to use (as stated in the specification) is an ArrayList containing ArrayList<MusicItem> where Music Track information is placed in the appropriate index based on the Track title.

`OutputFileInterface.java`
* An Interface with the following abstract methods that need to be overwritten by classes implementing this Interface:
    * `void open(String outputFileName)`
        * Uses a Scanner object to write Music Track information to a file named `outputFileName`.
        * You may assume that `outputFileName` is formatted correctly.
    * `public void writeItem(MusicTrack trackToWrite)`
        * Writes the information for a single Music Track item to the file.
    * `public void close()`
        * Closes the Scanner object used.

`OutputFile.java`
* Implements the `OutputFileInterface` methods.
* All information (including the field titles and each Music Track information) will be written to the appropriate output file.

# Possibly some useful tools

You will probably find the String method `compareTo()` quite useful. s.compareTo(t), with s and t being Strings, returns 0 if s and t have the same value, a number less than 0 if s comes before t in alphanumeric order, and a number greater than 0 if s comes after t. This method comes in handy when figuring out where to place a music item in the music list so that ordering by title is maintained.

You will also probably find the String method `split(delimiter) `quite useful. split returns an array (of type String) with each cell containing the substring that is terminated by the given delimiter, or by the end of the string; they are placed in order in the array. So, if you have, say, a string "This is a sentence." stored in aSentence, String[] words = aSentence.split(" ") will return words[0] = "This", word[1] = "is", word[2] = "a" and word[3] = "sentence." - a handy method for breaking up the Music Track lines into appropriate values.

Also, note that characters are actually numerical values that can be subtracted from other characters. One handy way to calculate which ArrayList index a specific Artist name or Track Title should go into is by extracting the first character from the name is:

`Character.toUpperCase(string.charAt(0)) - 'A'`

For the example, `a` or `A` will return an index 0, `b` or `B` will return an index 1, etc. 

# Testing

For this lab, you may use the given `MusicList.txt` file and the resulting output to confirm the correctness of your program. You may also create your own `MusicList.txt` files to test the edge cases (duplicate artist / title names) to make sure your ArrayLists are sorted properly.

# Submitting to Gradescope

The lab assignment "Lab04" should appear in your Gradescope dashboard in CMPSC 56. If you haven't submitted anything for this assignment yet, Gradescope will prompt you to upload your files.

**Remember to add your partner to Groups Members for this submission on Gradescope if applicable. Please include both names and PERM numbers for each submitted file. At this point, if you worked in a pair, it is a good idea for both partners to log into Gradescope and check if you can see the uploaded files for Lab03.**

For this lab, you will need to upload all of your .java source files as well as `MusicList.txt`:

**NOTE: Please be sure to ONLY submit the .java files and MusicList.txt in order to help our TAs streamline grading. DO NOT include additional files (such as .class files). Also, DO NOT put your files into any subdirectories or define specific packages when submitting your files to Gradescope.**:

You either can navigate to your files, "drag-and-drop" them into the "Submit Programming Assignment" window, or use your private GitHub repo to submit your work (if you do this, be sure that your repo ONLY contains the .java files and doesn't have any .java files in sub directories).

If you already submitted something on Gradescope, it will take you to their "Autograder Results" page. There is a "Resubmit" button on the bottom right that will allow you to update the files for your submission.

Most of the programming assignments for this class will not be autograded in the same way you may have seen in earlier courses. We will use Gradescope mainly as a "dropbox" for all of your assignments. Our staff will manually grade your assignments and assign a score to your submission.

Since Gradescope autograding is disabled, it is <b>very</b> important for you to test and compile your code locally according to the specifications of this lab. As a software developer, it's an important skill to think of correct functionality and test cases on your own. Be sure to do this before submitting your code to Gradescope. If our staff cannot run your code, then you will receive a 0 for this assignment (and this will not be apparent when submitting your code since Gradescope will not actually compile your code for this assignment).

Your submission will initially have a score of `0.0/100`. Don't worry - this is normal. Once the assignment has been graded by our staff, your actual score will be updated.

<sub> Adapted and modifed from Norm Jacobson's A Donation to the Music Archive ICS45J lab for CS56 S19. </sub>