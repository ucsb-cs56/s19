---
num: "Lecture 12"
desc: "Design Patterns - Observer"
ready: true
lecture_date: 2019-05-13 17:00:00.00-7:00
---

# Observer Pattern

* When creating applications with real-time updates, there are several things to consider...
* Mainly, how to get updated information consistently?
* One obvious approach is to periodically probe for updated information.
    * Not ideal since we may perform many update calls unnecessarily.
    * What would be a good probing period?
        * Too often is a waste.
        * Too little results in stale information.

* The <b>Observer</b> pattern is a publication/subscription (pub/sub) design that updates information whenever necessary without having to worry about periodic probing.
    * Object state can be updated whenever something changes.
* In general, there needs to be a:
    * Subject (Publisher): Object(s) that maintains data and sends updated information whenever a change occurs
    * Observer (Subscribers): Object(s) that subscribe to certain Subjects and obtain updated information whenever it occurs.
* This pattern supports the ability for Observers to subscribe and unsubscribe to Subjects whenever they want.
* The typical pattern is defined by having a "one-to-many" dependency.
    * When one Subject object changes state, then all of its Observer objects are notified and updated automatically.
    * In general though, a Subject can also be an Observer and vice versa.

![ObserverOverview](ObserverOverview.png)

* and a UML diagram illustrating the general observer design pattern:

![ObserverUML](ObserverUML.png)

# Example: Live updates for a hockey game

* Let’s apply a Observer pattern so an object observing a game's data will be updated whenever a change of state happens.
* In a hockey game, we can keep track of a lot of information, but in this scenario let’s keep track of the team names, score, assists, and time the update happened.

![HockeyUMLObserver](HockeyUMLObserver.png)

# Implementation

```
// Subject.java
public interface Subject {
    public void registerObserver(Observer o);
    public void removeObserver(Observer o);
    public void notifyObservers();
}
```
```
// Observer.java
public interface Observer {
    public void update(String homeTeam, String awayTeam, 
                Pair<Integer, Integer> score,
                Pair<Integer, Integer> assists,
                String time);
}
```
```
// Display.java
public interface Display {
    public void display(); // displays the data
}
```
```
// Pair.java
// From generics lecture...
public class Pair<T,U> {
    private T first;
    private U second;

    public Pair(T first, U second) {
        this.first = first;
        this.second = second;
    }

    public T getFirst() { return first; }

    public U getSecond() { return second; }

    public void print() {
        System.out.println(first + ", " + second);
    }
}
```
```
// GameData.java
import java.util.ArrayList;

// Subject other observers will subscribe to
public class GameData implements Subject {

    private ArrayList<Observer> observers;
    private String homeTeam;
    private String awayTeam;
    private Pair<Integer, Integer> goals;
    private Pair<Integer, Integer> assists;
    private String time;
    
    public GameData(String homeTeam, String awayTeam) {
        observers = new ArrayList<Observer>();
        this.homeTeam = homeTeam;
        this.awayTeam = awayTeam;
        this.goals = new Pair<Integer, Integer>(0,0);
        this.assists = new Pair<Integer, Integer>(0,0);
        this.time = "";
    }
    
    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        int i = observers.indexOf(o);
        if (i >= 0) { // it exists
            observers.remove(i);
        }
    }
}
    @Override
    public void notifyObservers() {
        for (int i = 0; i < observers.size(); i++) {
            Observer observer = observers.get(i);
            observer.update(homeTeam, awayTeam, goals, assists, time);
        }
    }
    
    public void updateGameState(Pair<Integer, Integer> goals,
            Pair<Integer, Integer> assists, String time) {
        this.goals = goals;
        this.assists = assists;
        this.time = time;
        notifyObservers();
    }
}
```
```
// GameObserver.java
public class GameObserver implements Observer, Display {

    private String homeTeam;
    private String awayTeam;
    private Pair<Integer, Integer> goals;
    private Pair<Integer, Integer> assists;
    private String time;
    
    public GameObserver(Subject gameData) {
        gameData = gameData;
        gameData.registerObserver(this);
    }
    
    @Override
    public void update(String homeTeam, String awayTeam,
            Pair<Integer, Integer> goals, Pair<Integer, Integer> assists,
            String time) {
        this.homeTeam = homeTeam;
        this.awayTeam = awayTeam;
        this.goals = goals;
        this.assists = assists;
        this.time = time;
        
        display();
    }

    @Override
    public void display() {
        System.out.format("%8s %-10s %-10s\n", "", homeTeam, awayTeam);
        System.out.format("%8s %-10d %-10d\n", "GOALS:", goals.getFirst(), goals.getSecond());
        System.out.format("%8s %-10d %-10d\n", "ASSISTS:", assists.getFirst(), assists.getSecond());
        System.out.format("%8s %-10s\n", "TIME:", time);
        System.out.println("----");
    }
}
```
```
// Lecture.java
import java.time.format.DateTimeFormatter;  
import java.time.LocalDateTime;  

public class Lecture {
    
    public static void main(String[] args) throws InterruptedException {

        GameData gameData = new GameData("Sharks", "Blues");
        
        // register Observer in constructor
        GameObserver gameObserver = new GameObserver(gameData);
        
        // set current time format
        DateTimeFormatter currentTime = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm:ss");
        LocalDateTime now = LocalDateTime.now();
        
        // Update game state
        gameData.updateGameState(new Pair<Integer, Integer>(0,1),
                                new Pair<Integer, Integer>(0,0),
                                currentTime.format(now));
        
        Thread.sleep(10000); // sleep for 10 seconds
        now = LocalDateTime.now();
        
        // Update game state
        gameData.updateGameState(new Pair<Integer, Integer>(1,1),
                new Pair<Integer, Integer>(1,0),
                currentTime.format(now));
        
        // remove observer
        gameData.removeObserver(gameObserver);
        
        Thread.sleep(5000); // sleep for 5 seconds
        now = LocalDateTime.now();
        
        gameData.updateGameState(new Pair<Integer, Integer>(1,2),
                new Pair<Integer, Integer>(1,1),
                currentTime.format(now));
        
        // re-register observer
        gameData.registerObserver(gameObserver);
        
        Thread.sleep(5000); // sleep for 5 seconds
        now = LocalDateTime.now();
        
        gameData.updateGameState(new Pair<Integer, Integer>(2,2),
                new Pair<Integer, Integer>(2,1),
                currentTime.format(now));
        
        System.out.println("Exiting");
    }
}
```

* This is an example of a "push" design (i.e. the updates occur whenever they happen without asking for the updates.).
    * We can also modify update to react to "pull" relevant info.
