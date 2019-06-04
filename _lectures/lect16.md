---
num: "Lecture 16"
desc: "Lambda Expressions"
ready: true
lecture_date: 2019-06-03 17:00:00.00-7:00
---

# Lambda Expressions

* A good video tutorial explaining Lambdas in more detail: [https://youtu.be/gpIUfj3KaOc](https://youtu.be/gpIUfj3KaOc)

* So far, we’ve seen that every unit of functionality we want in our Java programs, we need to define them in a class (via methods).
* In general, this works but there are times where we may not want to create specific classes for simple units of functionality and may want to create them "on-the-fly."
* For example, if we wanted to write some code that allowed us to call animal sounds, we can implement this in many ways:
        * We could have all the logic for all animal sounds in a single method and pass in an animal identifier to switch which sound to make...

```
public class Lecture {
    public static void makeSound(String animal) {
        switch(animal) {
        case "dog":
            System.out.println("BARK!");
            break;
        case "cat":
            System.out.println("MEOW!");
            break;
        // ... and so on ...
        default:
            System.out.println("??");
        }
        return;
    }

    public static void main(String[] args) {
        makeSound("dog");
    }
}
```

* This makes code maintenance difficult.
* Also breaks the "open-close" principle.
* We could use polymorphism to generate our behavior by passing in an Animal interface with a `sound()` method...

```
// Animal.java
public interface Animal {
    public void sound();
}
```
```
// Dog.java
public class Dog implements Animal {
    @Override
    public void sound() {
        System.out.println("BARK!");
    }
}
```
```
// Cat.java
public class Cat implements Animal {
    @Override
    public void sound() {
        System.out.println("MEOW!");
    }
}
```
```
// Lecture.java
public class Lecture {
    public static void makeSound(Animal animal) {
        animal.sound();
    }

    public static void main(String[] args) {
        makeSound(new Cat()); // Example of an anonymous Object
        makeSound(new Dog());
    }
}
```

## Anonymous Classes
* Anonymous classes can be used to define classes extending abstract classes or implementing interfaces.
* Could be useful for code organization
    * If we only want to use the class once and define specific behavior, we ...
        * Could implement a class file
        * Or just define the behavior and move on.
    * If a class is used in multiple parts in your code, then providing its own implementation may be better for readability / reusability reasons.

## Example

<pre>
// Animal.java

<b>// @FunctionalInterface is optional, but is used for
// protection. Will not compile if the interface has more
// than one abstract method.</b>

<b>@FunctionalInterface</b>
public interface Animal {
    public void sound();
}
</pre>
```
import java.util.ArrayList;

public class Lecture {
    public static void makeSound(Animal animal) {
        animal.sound();
    }

    public static void main(String[] args) {
        //Animal a = new Animal(); // ERROR!
        
        // Anonymous class representing a dog
        Animal dog = new Animal() {
            public void sound() {
                System.out.println("BARK!");
            }
        };
        // Anonymous class representing a cat
        Animal cat = new Animal() {
            public void sound() {
                System.out.println("MEOW!");
            }
        };
        // Anonymous class representing a cow
        Animal cow = new Animal() {
            public void sound() {
                System.out.println("MOO!");
            }
        };
        
        ArrayList<Animal> animalList = new ArrayList<Animal>();
        animalList.add(dog);
        animalList.add(cat);
        animalList.add(cow);
        
        for (Animal a : animalList) {
            makeSound(a);
        }
    }
}
```

* Can even use anonymous objects and classes together:

```
animalList.add(new Animal() {
    public void sound() {
        System.out.println("CAW!");
    }
});
```

## Lambda Expressions

* Allows pieces of functionality to exist without it being defined in a class.
* Functions are defined inline whenever we need it.
* Lambda functions are used with <b>Functional Interfaces</b>
* There are already a lot of functional interfaces in the Java API
    * Comparator: `compare`
    * Runnable: `run`
    * ...

## Example of converting sound() to a lambda expression

`public void sound();`

* Remove `public` since this is not associated with a class.
* Remove `void` since the return type of the function is inferred by the compiler.
* Remove the function name `sound` since lambda expressions are associated with a functional interface containing only one method name.
    * The compiler can infer the name of the functional interface method since there's only one.
* Only `()` remains.
* We  associate the body of the function using `->` and the function definitions will exist between `{ }`
* For example

```
Animal dog = () -> { System.out.println("BARK!"); };
Animal cat = () -> { System.out.println("MEOW!"); };
Animal cow = () -> { System.out.println("MOO!"); };
```

* Note that lambda functions with a single statement do not require `{ }`

```
Animal dog = () -> System.out.println("BARK!");
Animal cat = () -> System.out.println("MEOW!");
Animal cow = () -> System.out.println("MOO!");
```

## Example of Lambda Expressions with parameter

* We can modify our implementation to have an Animal's weight affects the sound it makes.

<pre>
// @FunctionalInterface is optional, but is used for
// protection. Will not compile if the interface has more
// than one abstract method.

@FunctionalInterface
public interface Animal {
    public void sound(<b>double weight</b>);
}
</pre>

<pre>
// Lecture.java
public class Lecture {
    public static void makeSound(Animal animal) {
        animal.sound(<b>60</b>);
    }

    public static void main(String[] args) {
        Animal dog = (double weight) -> {
            if (weight > 50)
                System.out.println("BARK!!!!!");
            else
                System.out.println("BARK!");
        };
        Animal cat = (double weight) -> {
            if (weight > 50)
                System.out.println("MEOW!!!!!");
            else
                System.out.println("MEOW!");
        };	
        Animal cow = (double weight) -> {
            if (weight > 50)
                System.out.println("MOO!!!!!");
            else
                System.out.println("MOO!");
        };

        ArrayList&lt;Animal&gt; animalList = new ArrayList&lt;Animal&gt;();
        animalList.add(dog);
        animalList.add(cat);
        animalList.add(cow);
        
        for (Animal a : animalList) {
            makeSound(a);
        }
    }
}
</pre>

* Note: since the parameter types are inferred by the compiler, we can don’t need the types in the lambda expression's parameter list and just use the parameters' variable name(s).

<pre>
Animal dog = (<b>weight</b>) -> {
    if (weight > 50) System.out.println("BARK!!!!!");
    else System.out.println("BARK!");
};

Animal cat = (<b>weight</b>) -> {
    if (weight > 50) System.out.println("MEOW!!!!!");
    else System.out.println("MEOW!");
};	

Animal cow = (<b>weight</b>) -> {
    if (weight > 50) System.out.println("MOO!!!!!");
    else System.out.println("MOO!");
};
</pre>

## Implementing Collections.Sort with Lambda Expressions

* Collections ([java.util.Collections](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Collections.html)) has a method `.sort` to automatically sort lists.
    * `sort(List<T> list, Comparator<? super T> c)`
        * `sort` takes a `List` and the functional interface [`Comparator`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Comparator.html)

* We can implement the Comparator interface to sort `Student` objects by perm number using anonymous classes

```
// Student.java
public class Student {
    private String name;
    private int perm;

    public Student(String name, int perm) {
        this.name = name;
        this.perm = perm;
    }

    public String getName() { return name; }
    public int getPerm() { return perm; }
}
```
```
// Lecture.java
    public static void main(String[] args) {
        ArrayList<Student> a = new ArrayList<Student>();
        a.add(new Student("RICHERT", 80498567));
        a.add(new Student("MIKE", 1234567));
        a.add(new Student("TANYA", 3333333));
        a.add(new Student("ZORRA", 5555555));

        // Anonymous class
        Collections.sort(a, new Comparator<Object>() {
            public int compare(Object o1, Object o2) {
                Student x = (Student) o1;
                Student y = (Student) o2;
                if (x.getPerm() > y.getPerm())
                    return 1;
                else if (x.getPerm() == y.getPerm())
                    return 0;
                else 
                    return -1;
            }
        });
        
        for (Student s : a) {
            System.out.println(s.getName() + ": " + s.getPerm());
        }
    }
```

* and we can implement it with lambda expressions

```
Collections.sort(a, (s1, s2) -> {
    if (s1.getPerm() > s2.getPerm())
        return 1;
    else if (s1.getPerm() == s2.getPerm())
        return 0;
    else 
        return -1;
});
```

## Example using lambda expressions implementing the Runnable Interface

```
public class Lecture {
    public static void main(String[] args) {
        
        Thread t1 = new Thread(() -> {
            int i = 0;
            while (true) {
                ++i;
                System.out.println(i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        t1.start();
    }
}
```
