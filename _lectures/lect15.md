---
num: "Lecture 15"
desc: "Multithreads"
ready: true
lecture_date: 2019-05-22 17:00:00.00-7:00
---

# Concurrency

* Concurrency is an important part of computer science.
    * For example, working on "parts of a proble" concurrently if the parts can be done independently.
        * Some things need to be done sequentially (imagine drilling for oil – you can’t concurrently drill 100 meters in order to get to the center 1000 meters deep).
    * Many applications use concurrency for performance reasons.
        * Web servers process requests concurrently.
        * Web browsers fetch / render images concurrently. 
    * <b>Embarrassingly Parallel</b> problems are usually a good fit for concurrent computing.
        * SETI@Home is a project that sends work to computers for processing and the data is sent back. The data sets in this case are independent (embarrassingly parallel).
        * Monte-carlo simulations are embarrassingly parallel (take the average of many simulations).

* Concurrency also allows efficient use of hardware.
    * For multi-core processors, you can design your program such that several parts of it work concurrently.
    * Each processor (core) in this case runs instructions in parallel.
    * For single-core architectures, it provides the illusion that something is working concurrently, but the processor is shared among all programs.
        * The processor does <b>context switching</b> between the various programs, but it cannot process instructions concurrently.

## Threads

* A thread is a program unit that is executed independently of other parts of your program.
    * Every Java application has a single process called the <b>main process</b> or <b>main thread</b>.
    * Every thread that the main process creates is called a <b>subprocess</b> or <b>subthread</b>.
    * A sub-process shares the memory space of the main process.

## Example: Creating threads

```
public class MyThread extends Thread {
    public void run() {
        // This is the code that will be executed on this thread...
        int i = 0;
        while(true) {
            i++;
            if (i % 100000 == 0)
                System.out.println(i);
        }
    }
}
```
```
// main method
        SomeThread t = new SomeThread();
        t.start();
//      SomeThread t2 = new SomeThread();
//      t2.start();
//      SomeThread t3 = new SomeThread();
//      t3.start();
//      SomeThread t4 = new SomeThread();
//      t4.start();
//      SomeThread t5 = new SomeThread();
//      t5.start();
```

* Note: Take a look at your task manager / activity monitor CPU utilization.
    * Technically, a thread only runs on a single core (with 100% utilization for a single core).

## Implementing the Runnable Interface
* We can define and manage threads by implementing the Runnable Interface (rather than creating a class that extends Thread).
* Will need to create thread objects that pass in the Runnable classes.

## Example

```
// RunnableExample.java
public class RunnableExample implements Runnable {
    private String id;
    public RunnableExample(String id) {
        this.id = id;
    }
    public void run() {
        int i = 0;
        while(true) {
            i++;
            if (i % 100000 == 0) {
                System.out.println(i + ": " + id);
            }
        }
    }
}
```
```
// main method
    public static void main(String[] args) {
        RunnableExample r1 = new RunnableExample("r1");
        RunnableExample r2 = new RunnableExample("r2");
        RunnableExample r3 = new RunnableExample("r3");
        Thread t1 = new Thread(r1);
        Thread t2 = new Thread(r2);
        Thread t3 = new Thread(r3);
        t1.start();
//      t1.join(); this main thread waits for t1 to finish
        t2.start();
        t3.start();
```

* Q: When should you extend Thread or implement the Runnable Interface?
    * Recall: A class cannot extend multiple classes.
        * If your class wants to define functionality for a thread and want to extend another class, then it should implement the runnable interface since a class can implement multiple interfaces.

## Sleep
* You can put threads to sleep if you want it to periodically continue its task.
    * Otherwise, CPU resources are consumed and your system can get boggled down.
        * This is known as <b>busy waiting</b>.
    * `Thread.sleep(long milliseconds)` allows the programmer to tell a thread to sleep for a certain period of time.
    * While the thread is sleeping, no CPU resources are dedicated to executing its statements.
    * `sleep` throws the InterruptedException since a thread may be interrupted while sleeping. 

## Example

```
// update run() to the following:
public void run() {
    int i = 0;
    Date now = null;
    try {
        while(true && !Thread.interrupted()) {
            now = new Date();
            i++;
            if (i % 100000 == 0) {
                System.out.println(now.toString() + i + ": " + id);
                Thread.sleep(1000);
            }
        }
    } catch (InterruptedException e) {
        System.out.println(now.toString() + " Thread " + id + " interrupted");
    }
}
```
```
// in main.
try {
    Thread.sleep(3000);
    t1.interrupt();
    Thread.sleep(3000);
    t2.interrupt();
    Thread.sleep(3000);
    t3.interrupt();
} catch (InterruptedException e) {
    // if this happens ... something wonky is going on!
}
// watch how the threads stop one-by-one.
```

# Race Conditions

* If threads share some memory (for example, an object), read / write order conflicts can happen.
* For example, a multi-threaded ATM machine depends on having atomic transactions (either everything is done or nothing is).
    * A joint bank account contains a $100 balance.
    * Person x deposits $10 and Person y withdraws $10 at the same time.
    * Person y obtains current balance of $100.
    * The thread switches to Person x, reads current balance of $100 and deposits $10 (balance = $110)
    * Thread switches back to Person y finishes withdraw and updates balance to ($100 - $10 = $90).

## Example

```
// Bank.java
public class Bank {
    private double totalBalance;	
    public Bank() { totalBalance = 0; }
    public void deposit(double amount) { totalBalance += amount; }
    public double getBalance() { return totalBalance; }
}
```
```
// RunnableExample.java

public class RunnableExample implements Runnable {
    private String id;
    private Bank bank;
    
    public RunnableExample(String id, Bank bank) {
        this.id = id;
        this.bank = bank;
    }
    
    @Override
    public void run() {
        System.out.println("In thread " + Thread.currentThread().getId());
        for (int i = 0; i < 1000; i++) {
            bank.deposit(10);
        }
    }
}
```
```
// in main
Bank b = new Bank();
RunnableExample r1 = new RunnableExample("r1", b);
RunnableExample r2 = new RunnableExample("r2", b);
Thread t1 = new Thread(r1);
Thread t2 = new Thread(r2);
t1.start();
t2.start();		
try {
    Thread.sleep(5000);
} catch (InterruptedException e) { }

System.out.println(b.getBalance()); // != $20,000
```

* A race condition happens here since `totalBalance += amount;` may reflect an older balance when updating the current totalBalance.

# Locks
A solution to a race condition is to block thread access to a shared object.
    * We can surround code manipulating a shared resource with Lock objects (specifically ReentrantLock).
    * There are several other ways to do this (including the keyword synchronized).

## Example
```
// in Bank class, add some locks to prevent multiple thread access.
public class Bank {
    private double totalBalance;
    private Lock balanceLock;
    public Bank() {
        totalBalance = 0;
        balanceLock = new ReentrantLock();
    }
    public void deposit(double amount) {
        balanceLock.lock();
        totalBalance += amount;
        balanceLock.unlock();
    }
    public double getBalance() { return totalBalance; }
}
```
* Note: This change will make the balance $20,000 as expected!

## Example: using synchronized
* Synchronized can be used on a method level to prevent multiple threads from executing this concurrently.
* A little less flexible than the lock mechanism, but works in a similar way.

<pre>
public class Bank {
    private double totalBalance;	
    public Bank() { totalBalance = 0; }
    public <b>synchronized</b> void deposit(double amount) {
        totalBalance += amount;
        return;
    }
    public double getBalance() { return totalBalance; }
}
</pre>
