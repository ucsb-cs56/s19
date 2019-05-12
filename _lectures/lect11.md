---
num: "Lecture 11"
desc: "UML, Strategy Design Pattern"
ready: true
lecture_date: 2019-05-08 17:00:00.00-7:00
---

# Unified Modeling Language (UML)

* Pictorial "language" used to visualize software architecture and design.
* Can be used in many forms (ex, process management), but is commonly used for Object Oriented (OO) Design.
    * Identifies OO components (such as classes / interfaces)
    * Identifies the relationships between these components
* For a high-level tutorial on UML, see [https://www.tutorialspoint.com/uml](https://www.tutorialspoint.com/uml)

# Design Scenario 1: Animal Interface and Subclasses

* Assume we wanted to model Animals in an OO way.
* We could do it the way we've done it earlier in class:

![AnimalScenario1](AnimalScenario1.png)

* And implement it as follows:

```
// Animal.java
public abstract class Animal {

    private double weight; // in lbs
    private double age; // in years

    public Animal(double weight, double age) {
        this.weight = weight;
        this.age = age;
    }

    public abstract void makeSound();
    public abstract void fly();
    public abstract void swim();
}
```
```
// Dog.java
public class Dog extends Animal {

    public Dog(double weight, double age) {
        super(weight, age);
    }
    @Override
    public void makeSound() {
        System.out.println("BARK!");
    }
    @Override
    public void fly() {
        System.out.println("I believe I can fly ... but I can't :(");
        // or NULL body if there is no sensible definition for this.
    }
    @Override
    public void swim() {
        System.out.println("I can swin!");
        
    }	
}
```
```
// Duck.java
public class Duck extends Animal {

    public Duck(double weight, double age) {
        super(weight, age);
    }
    @Override
    public void makeSound() {
        System.out.println("QUACK!");
    }
    @Override
    public void fly() {
        System.out.println("I can fly!");
    }
    @Override
    public void swim() {
        System.out.println("I can swin!");
        
    }
}
```
```
// Crow.java
public class Crow extends Animal {

    public Crow(double weight, double age) {
        super(weight, age);
    }
    @Override
    public void makeSound() {
        System.out.println("CAW!");
    }
    @Override
    public void fly() {
        System.out.println("I can fly!");
    }
    @Override
    public void swim() {
        System.out.println("I cannot swin!");
        // or NULL body if there is no sensible definition for this.
    }
}
```

* Here we have to provide an implementation on things that we don’t need.
    * Or have unused or null methods in the subclass implementations.
* We could have a lot of null definitions for all animals for no reason.

# Design Scenario 2: Make each differing functionality into its own Interface

![AnimalScenario2](AnimalScenario2.png)

* Solves the problem of avoiding null methods in subclass definitions.
* However, what if we want to change a specific way an animal makes a sound, flys, or swims?
    * We would need to go into each subclass and modify the method definitions that overwrite the interface methods.
    * Can be really cumbersome if there are MANY animals.


# Design Scenario 3: Creating Interfaces for Behaviors of Animals

* What if we break out the animal behavior implementation out of the subclasses and create Interfaces and subclasses for the behaviors.

![AnimalScenario3](AnimalScenario3.png)

* And implement everything as follows:

```
// SoundBehavior.java
public interface SoundBehavior {
    public void makeSound();
}
```
```
// FlyBehavior.java
public interface FlyBehavior {
    public void fly();
}
```
```
// SwimBehavior.java
public interface SwimBehavior {
    public void swim();
}
```
```
// QuackSound.java
public class QuackSound implements SoundBehavior {
    @Override
    public void makeSound() {
        System.out.println("QUACK!");
    }
}
```
```
// BarkSound.java
public class BarkSound implements SoundBehavior {
    @Override
    public void makeSound() {
        System.out.println("BARK!");
    }
}
```
```
// CawSound.java
public class CawSound implements SoundBehavior {
    @Override
    public void makeSound() {
        System.out.println("CAW!");
    }
}
```
```
// NoFlying.java
public class NoFlying implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("I can believe I can fly...but I can't :(");
    }
}
```
```
// Swimming.java
public class Swimming implements SwimBehavior {
    @Override
    public void swim() {
        System.out.println("I can swim!");
    }
}
```
```
// NoSwimming.java
public class NoSwimming implements SwimBehavior {
    @Override
    public void swim() {
        System.out.println("I'm drowning!!!");
    }
}
```
```
// Animal.java
public class Animal {
    private double weight; // in lbs
    private double age; // in years
    
    // Animal Behaviors
    private SoundBehavior sound;
    private FlyBehavior fly;
    private SwimBehavior swim;

    public Animal(double weight, double age) {
        this.weight = weight;
        this.age = age;
    }
    public void setSoundBehavior(SoundBehavior sound) {
        this.sound = sound;
    }
    public void setFlyBehavior(FlyBehavior fly) {
        this.fly = fly;
    }
    public void setSwimBehavior(SwimBehavior swim) {
        this.swim = swim;
    }
    public void makeSound() {
        sound.makeSound();
    }
    public void fly() {
        fly.fly();
    }
    public void swim() {
        swim.swim();
    }
}
```
```
// Dog.java
public class Dog extends Animal {
    public Dog(double weight, double age) {
        super(weight, age);
        
        // can dynamically set behaviors
        this.setFlyBehavior(new NoFlying());
        this.setSoundBehavior(new BarkSound());
        this.setSwimBehavior(new Swimming());
    }
}
```
```
// Duck.java
public class Duck extends Animal {
    public Duck(double weight, double age) {
        super(weight, age);
        this.setSoundBehavior(new QuackSound());
        this.setFlyBehavior(new Flying());
        this.setSwimBehavior(new Swimming());
    }
}
```
```
// Crow.java
public class Crow extends Animal {
    public Crow(double weight, double age) {
        super(weight, age);
        this.setFlyBehavior(new Flying());
        this.setSoundBehavior(new CawSound());
        this.setSwimBehavior(new NoSwimming());
    }
}
```
```
// Lecture.java
public class Lecture {
    public static void main(String[] args) {
        System.out.println("---DUCK---");
        Animal duck = new Duck(5, 10);
        // Polymorphically use the correct animal behavior
        duck.swim();
        duck.fly();
        duck.makeSound();

        System.out.println("---CROW---");
        Animal crow = new Crow(3,100);
        crow.swim();
        crow.fly();
        crow.makeSound();

        System.out.println("---DOG---");
        Animal dog = new Dog(20, 5);
        dog.swim();
        dog.makeSound();
        dog.fly();

        System.out.println("Obtained SUPER POWERS!!!");
        dog.setFlyBehavior(new Flying());
        dog.fly();

        ArrayList<Animal> animals = new ArrayList<Animal>();
        animals.add(duck);
        animals.add(crow);
        animals.add(dog);

        System.out.println("---");
        System.out.println("Sounds of all animals!");
        for (int i = 0; i < animals.size(); i++) {
            animals.get(i).makeSound();
        }
    }
}
```

* Breaking out the varying functionality is known as the Strategy (Behavior) Design Pattern.
* In case we want to make a change for a behavior, we only need to change the Behavior class (not all subclasses implementing the behavior).
* Can be useful to solve certain maintenance issues as described above.
* However, be careful in "over-engineering" solutions and think if Design Patterns in general are the best tools to improve your design.
