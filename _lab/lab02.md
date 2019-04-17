---
layout: lab
num: lab02
ready: true
desc: "Craps!"
assigned: 2019-04-15 08:00
due: 2019-04-21 23:59
---

# Goals

* For this lab, you will write a small simulator on the simplest version of the dice game called Craps: [http://casinogambling.about.com/od/craps/a/craps101.htm](http://casinogambling.about.com/od/craps/a/craps101.htm)

# Rules

Specifically, you will implement the "Passline Bet" version of craps using two six-sided die. The rules are as follows:

<i> "You place your bet on the passline before a new shooter begins his roll. This is known as the come out roll. If the shooter rolls a 7 or 11 you win. If the shooter rolls a 2, 3 or 12, you lose. If the shooter rolls any other number, that number becomes the point number.
The shooter must roll that number again before a seven is rolled. If that happens, you win even money for your passline bet. If a seven is rolled before the point number is rolled again, you lose." </i>

When the user rolls a 7 or 11 on the first roll, this is known as "Natural." When the user rolls a 2, 3, or 12 on the first roll, this is known as "Craps."

# Requirements

Your simulation will prompt the user for their name, the amount of money the user brings to the table, and the amount of money the user wants to bet. The simulation will continuously run by placing the bet amount continuously until the user’s balance is $0. A sample output (with some error cases) for this is as follows (bold text indicates user input):

<pre>
Welcome to SimCraps! Enter your user name: <b>Richert</b>
Hello Richert!
Enter the amount of money you will bring to the table: <b>1000</b>
Enter the bet amount between $1 and $1000: <b>1001</b>
Invalid bet! Please enter a bet between $1 and $1000: <b>-2</b>
Invalid bet! Please enter a bet between $1 and $1000: <b>100</b>
</pre>

The username can be any String (even an empty one) without any whitespaces. You can assume that the user will only enter a valid Integer value for the initial balance brought to the table. However, your program must check that the bet amount is in a valid range between $1 and the user’s balance. You must also check and make sure that the bet amount throughout the simulation never exceeds the user’s remaining balance. For example, if the original bet was $100 and you only have a balance $50, then the bet for that game will be $50. If you win, and your balance is back up to $100, the next bet will be your original bet of $100.

During the games played in the simulation, you will need to print out the actions to the console (i.e. what the user is rolling, if the user won, if the user lost, what the user is betting, and what the user’s current balance is). A sample output for a few games of craps is as follows:

```
Richert bets $100
Rolled a 5
Rolled a 7
*****Crap out! You loose.*****
Richert's balance: 900. Playing a new game...
Richert bets $100
Rolled a 8
Rolled a 6
Rolled a 8
*****Rolled the point! You win!*****
Richert's balance: 1000. Playing a new game...
Richert bets $100
Rolled a 3
*****Craps! You loose.*****
Richert's balance: 900. Playing a new game...
Richert bets $100
Rolled a 7
*****Natural! You win!*****
```

Once the simulation is over, your program will print out statistics of the entire simulation. The stats you need to keep track of are:
* Number of games played
* Number of games won
* Number of games lost
* Maximum number of rolls in a single game
* "Natural" roll count
* "Craps" roll count
* Maximum winning streak
* Maximum loosing streak
* Maximum balance throughout simulation
* The game number when the Maximum balance was obtained

After the statistics are printed out, the program will prompt the user to run another simulation or not by entering 'y' or 'n'. If the user chooses not to, your program should terminate. If the user wants to run another simulation, then the simulation is rerun and starts by asking the user for their name, balance, and bet. Your program must check to see if the user input is valid (lower-case / upper-case responses are valid). 

<pre>
Richert's balance: 100. Playing a new game...
Richert bets $100
Rolled a 9
Rolled a 8
Rolled a 6
Rolled a 7
*****Crap out! You loose.*****
Richert's balance: $0

*****************************
*** SIMULATION STATISTICS ***
*****************************
Games played: 230
Games won: 110
Games lost: 120
Maximum Rolls in a single game: 29
Natural Count: 44
Craps Count: 25
Maximum Winning Streak: 9
Maximum Loosing Streak: 7
Maximum balance: 2100 during game 97

Replay? Enter 'y' or 'n': <b>n</b>
</pre>

# Structure

There are many ways to structure / organize your code and I will provide some requirements for you to follow. Some objects to implement are:

<b>Lab2.java</b>
* Contains the main method. All it does is construct a CrapsSimulation object and calls .start() on it.

<b>CrapsSimulation.java</b>
* A class representing all the information for the Simulation. This includes:
    * A CrapsGame object
    * A CrapsMetricsMonitor object
    * The user’s name
    * The user’s balance
    * The user’s bet
    * The current win streak
    * The current lose streak
* Public methods that this class must implement are:
    * CrapsSimulation()
        * Constructor that initializes all fields to default values and constructs any objects used (i.e. Scanner, CrapsMetricsMonitor, …)
    * void start()
        * Main loop of a single simulation run. This is where the user inputs their name, balance, and bet, runs the simulation, and continues to do so if the user wants to run it again.

<b>CrapsGame.java</b>
* A class representing all the information for a single craps game. This includes:
    * Number of rolls that happened in a game
    * A CrapsMetricsMonitor object
        * Public methods that this class must implement are:
        * CrapsGame(CrapsMetricsMonitor monitor)
            * Constructor that initializes the class fields. A CrapsMetricsMonitor is passed into the constructor since there are specific stats within a single game that must be updated (and the same object should be used for all metrics in the simulation).
    * boolean playGame()
        * Contains the algorithm for an actual game.
        * Returns true if the game was won, false otherwise.

<b>CrapsMetricsMonitor.java</b>
* A class representing all of the statistics gathered during a single simulation. 
* Public methods:
    * Any methods you need to update / increment / decrement the class fields. These should be called throughout your simulation.
    * void printStatistics()
        * Prints all of the statistics for the simulation (see sample output).
    * void reset()
        * A method to reset the state of the CrapsMetricsMonitor object. This will be called if the user wants to start a new simulation after one was just completed.

# Submitting to Gradescope

The lab assignment "Lab02" should appear in your Gradescope dashboard in CMPSC 56. If you haven't submitted anything for this assignment yet, Gradescope will prompt you to upload your files.

For this lab, you will need to upload your source files (`CrapsGame.java`, `CrapsMetricsMonitor.java`, `CrapsSimulation.java`, and `Lab02.java`). You either can navigate to your files, "drag-and-drop" them into the "Submit Programming Assignment" window, or  use your private GitHub repo to submit your work.

If you already submitted something on Gradescope, it will take you to their "Autograder Results" page. There is a "Resubmit" button on the bottom right that will allow you to update the files for your submission.

Most of the programming assignments for this class will not be autograded in the same way you may have seen in earlier courses. We will use Gradescope mainly as a "dropbox" for all of your assignments. Our staff will manually grade your assignments and assign a score to your submission.

Since Gradescope autograding is disabled, it is <b>very</b> important for you to test and compile your code locally according to the specifications of this lab. As a software developer, it's an important skill to think of correct functionality and test cases on your own. Be sure to do this before submitting your code to Gradescope. If our staff cannot run your code, then you will receive a 0 for this assignment (and this will not be apparent when submitting your code since Gradescope will not actually compile your code for this assignment).

Your submission will initially have a score of `0.0/100`. Don't worry - this is normal. Once the assignment has been graded by our staff, your actual score will be updated.
