---
num: "Lecture 13"
desc: "Design Patterns - Decorator"
ready: true
lecture_date: 2019-05-15 17:00:00.00-7:00
---

# Decorator Pattern

* Imagine a situation where you must keep track of various attributes for objects, where all attributes can be used in combination with each other.
    * Textbook’s example: Coffee with condiments
    * Can have an infinite number of condiments!
    * Dark Rost, mocha x 2, whip cream x 3, …
* A silly approach is to have classes represent every possible combination...
    * Hopefully we know better designs than that at this point.
* Another approach is to have each attribute baked into the base class.
    * This is much better and probably acceptable for certain situations.
* The problem with this is what if we need to add more condiments or change prices.
    * We’ll have to make modifications to an existing class that may have already been tested.
* The <b>Open-Close Principle</b>
    * Classes should be open for extension, but closed for modification.
        * Basically, once a class is written and tested, we don’t want to modify this.
        * But we want to extend functionality and we can do this by creating additional classes and test those.
        * If specifications change, then we can modify a single class instead of modifying the change in multiple places.

* The Decorator Pattern
    * "attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality"

## Idea

* Separate Components that contain multiple Behaviors or Attributes (Decorators).
* Components and Behaviors will extend from a common type. 
    * This is important for wrapping components with the necessary decorators.
* Let's implement various Pizzas with toppings using the decorator pattern:

![PizzaDecorator](PizzaDecorator.png)

![DecoratorUML](DecoratorUML.png)

![PizzaUMLDecorator](PizzaUMLDecorator.png)

## Implementation

```
/`/ Pizza.java
public abstract class Pizza {

    private String size = "";
    private String type = "";
    
    public String getSize() { return size; }
    public String getType() { return type; }
    public void setSize(String size) { this.size = size; }
    public void setType(String type) { this.type = type; }
    
    public abstract double cost();
}
```
```
// ToppingDecorator.java
public abstract class ToppingDecorator extends Pizza {
    public abstract String getType();
}
```
```
// Regular.java
public class Regular extends Pizza {

    public Regular(String size) {
        setSize(size);
        setType("Regular Crust");
    }
    
    @Override
    public double cost() {
        switch (getSize()) {
            case "small":
                return 10;
            case "medium":
                return 15;
            case "large":
                return 20;
        }
        return 0;
    }
}
```
```
// Pan.java
public class Pan extends Pizza {

    public Pan(String size) {
        setSize(size);
        setType("Pan Crust");
    }
    
    @Override
    public double cost() {
        switch (getSize()) {
            case "small":
                return 12;
            case "medium":
                return 17;
            case "large":
                return 22;
        }
        return 0;
    }
}
```
```
// Cheese.java
public class Cheese extends ToppingDecorator {
    
    private Pizza pizza;
    
    public Cheese(Pizza pizza) {
        this.pizza = pizza;
        setSize(pizza.getSize());
        setType(pizza.getType());
    }

    @Override
    public String getType() {
        return pizza.getType() + " + Cheese";
    }
    
    public double cost() {
        switch (pizza.getSize()) {
            case "small":
                return 1 + pizza.cost();
            case "medium":
                return 2 + pizza.cost();
            case "large":
                return 4 + pizza.cost();
        }
        return 0;
    }
}
```
```
// Pepperoni.java
public class Pepperoni extends ToppingDecorator {

    private Pizza pizza;
    
    public Pepperoni(Pizza pizza) {
        this.pizza = pizza;
        setSize(pizza.getSize());
        setType(pizza.getType());
    }

    @Override
    public String getType() {
        return pizza.getType() + " + Pepperoni";
    }
    
    public double cost() {
        switch (pizza.getSize()) {
            case "small":
                return 1.5 + pizza.cost();
            case "medium":
                return 2.5 + pizza.cost();
            case "large":
                return 4.5  + pizza.cost();
        }
        return 0;
    }
}
```
```
// Pineapple.java
public class Pineapple extends ToppingDecorator {

    private Pizza pizza;
    
    public Pineapple(Pizza pizza) {
        this.pizza = pizza;
        setSize(pizza.getSize());
        setType(pizza.getType());
    }

    @Override
    public String getType() {
        return pizza.getType() + " + Pineapple";
    }
    
    public double cost() {
        switch (pizza.getSize()) {
            case "small":
                return 0.75 + pizza.cost();
            case "medium":
                return 1.5 + pizza.cost();
            case "large":
                return 2.5 + pizza.cost();
        }
        return 0;
    }
}
```
```
// Lecture.java
public class Lecture {
    
    public static void printPizza(Pizza pizza) {
        System.out.format("%s %s: $%.2f\n", pizza.getSize(), pizza.getType(), pizza.cost());
        System.out.println("---");
    }
    
    public static void main(String[] args) {
        Pizza pizza = new Regular("large");
        pizza = new Cheese(pizza);
        pizza = new Pineapple(pizza);
        printPizza(pizza);
    
        Pizza pizza2 = new Pan("small");
        pizza2 = new Cheese(pizza2);
        pizza2 = new Cheese(pizza2);
        pizza2 = new Pepperoni(pizza2);
        printPizza(pizza2);	
    }
}
```

