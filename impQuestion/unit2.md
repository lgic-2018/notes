## Difference between abstract class concrete class and interface class with example java?



In Java, abstract classes, concrete classes, and interfaces are different constructs used for defining classes and their behavior. Here's an explanation of each along with an example:

#### Abstract Class:

<!-- An abstract class is a class that cannot be instantiated on its own and serves as a blueprint for other classes.
It can contain both abstract and non-abstract methods.
Abstract methods are declared without an implementation and must be implemented by any non-abstract subclass.
It can also have regular methods with implementations.
Abstract classes are useful when you want to provide a common interface for a group of related classes. -->
An abstract class is a class that cannot be instantiated on its own and serves as a blueprint for other classes. It can contain both abstract and non-abstract methods. Abstract methods are declared without an implementation and must be implemented by any non-abstract subclass. It can also have regular methods with implementations. Abstract classes are useful when you want to provide a common interface for a group of related classes.

#### Example:

```
abstract class Animal {
    public abstract void makeSound();
    
    public void sleep() {
        System.out.println("Zzz");
    }
}

class Dog extends Animal {
    public void makeSound() {
        System.out.println("Woof");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog();
        animal.makeSound();
        animal.sleep();
    }
}
```
#### Output:
```
Woof
Zzz
```

#### Concrete Class:

A concrete class is a regular class that can be instantiated directly. It can have both abstract and non-abstract methods, but all the abstract methods defined in its parent abstract class must be implemented. Concrete classes provide the actual implementation of the methods defined in their parent classes. They are used to create objects that have specific behavior and attributes.

#### Example:


```
abstract class Vehicle {
    public abstract void start();
}

class Car extends Vehicle {
    public void start() {
        System.out.println("Car started");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle vehicle = new Car();
        vehicle.start();
    }
}

```
#### Output:
```
Car started

```

#### Interface

An interface is a collection of abstract methods that defines a contract for classes to adhere to. It cannot be instantiated directly; instead, classes implement interfaces to provide their own implementation of the interface methods. An interface can contain constant fields (public static final) and default methods (with implementations) since Java 8. It is useful when you want to enforce certain behavior across unrelated classes.

#### Example:



```
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() {
        System.out.println("Drawing a circle");
    }
}

public class Main {
    public static void main(String[] args) {
        Shape shape = new Circle();
        shape.draw();
    }
}


```
#### Output:
```
Drawing a circle
```
In summary, abstract classes provide a common interface and may have implemented methods, concrete classes are regular classes that provide the actual implementation, and interfaces define contracts for classes to implement.
