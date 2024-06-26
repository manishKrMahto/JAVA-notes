# Inheritance and interfaces:

## <u>Types of inheritance </u>
In Java, inheritance is a fundamental concept of object-oriented programming (OOP) that allows one class (subclass or derived class) to inherit properties and behaviors from another class (superclass or base class). This promotes code reuse and allows for hierarchical relationships between classes. Java supports several types of inheritance, each serving different purposes and offering varying degrees of flexibility. Let's explore the types of inheritance in Java:

### 1. Single Inheritance

**Single inheritance** involves one subclass inheriting from one superclass. In Java, a class can extend only one superclass, forming a single chain of inheritance.

- **Example**:
  
```java
// Superclass
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

// Subclass inheriting from Animal
class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}
```

In this example, `Dog` inherits the `eat()` method from `Animal`. Java supports single inheritance to avoid the complexities and ambiguities that can arise from multiple inheritance.

### 2. Multilevel Inheritance

**Multilevel inheritance** involves a chain of inheritance where one class extends another class, and then another class extends the second class, forming a parent-child-grandchild relationship.

- **Example**:

```java
// Superclass
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

// Subclass inheriting from Animal
class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}

// Subclass inheriting from Dog
class Labrador extends Dog {
    void color() {
        System.out.println("Labrador is golden in color");
    }
}
```

Here, `Labrador` inherits both `eat()` and `bark()` methods from `Animal` and `Dog`, respectively. `Labrador` demonstrates multilevel inheritance by extending `Dog`.

### 3. Hierarchical Inheritance

**Hierarchical inheritance** involves one superclass being inherited by multiple subclasses. This creates a tree-like structure where multiple subclasses inherit common behaviors and attributes from a single superclass.

- **Example**:

```java
// Superclass
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

// Subclass inheriting from Animal
class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}

// Another subclass inheriting from Animal
class Cat extends Animal {
    void meow() {
        System.out.println("Cat is meowing");
    }
}
```

In this example, both `Dog` and `Cat` inherit the `eat()` method from `Animal`. They demonstrate hierarchical inheritance as they extend the common superclass `Animal`.

### 4. Multiple Inheritance (through Interfaces)

Java does not support multiple inheritance through classes (where a class extends more than one class). However, Java supports **multiple inheritance through interfaces**, where a class can implement multiple interfaces.

- **Example**:

```java
// Interface with abstract methods
interface Flyable {
    void fly();
}

// Interface with default method
interface Swimmable {
    default void swim() {
        System.out.println("Swimming");
    }
}

// Class implementing multiple interfaces
class Bird implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Flying");
    }
}
```

Here, `Bird` implements both `Flyable` and `Swimmable` interfaces. This demonstrates multiple inheritance through interfaces, allowing `Bird` to inherit behaviors from both interfaces.

### Important Points

- **Java Inheritance Rules**: In Java, classes cannot extend more than one class (single inheritance). However, a class can implement multiple interfaces, enabling multiple inheritance of type.

- **Superclass and Subclass Relationship**: Inheritance establishes an "is-a" relationship between superclass and subclass. For example, `Dog` "is-an" `Animal`.

- **Access Modifiers**: Inherited members (fields and methods) can have their access modified using access specifiers (`public`, `protected`, `default`, `private`) depending on their visibility requirements.

### Summary

Understanding the types of inheritance in Java helps in designing and implementing hierarchical relationships between classes effectively. Each type of inheritance offers different advantages and is suited for different scenarios in software development, promoting code reuse, abstraction, and polymorphism in object-oriented programming.

## <u>Single inheritance </u> 
already covered above   

## <u>interface </u>
In Java, an interface is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. It provides a way to achieve abstraction and multiple inheritance of type. Let's explore the characteristics, usage, and examples of interfaces in Java.

### Characteristics of Interfaces

1. **Constants**: Interfaces can declare constants, which are implicitly `public`, `static`, and `final`. These constants can be accessed directly using the interface name.

2. **Method Signatures**: Interfaces can declare method signatures without providing implementation details. These methods are implicitly `public` and `abstract`.

3. **Default Methods**: Starting from Java 8, interfaces can have default methods, which provide a default implementation. Classes that implement the interface can use or override the default method.

4. **Static Methods**: Starting from Java 8, interfaces can also contain static methods, which can be called directly using the interface name.

5. **Multiple Inheritance**: Unlike classes, a Java class can implement multiple interfaces. This allows for achieving multiple inheritance of type in Java.

### Declaring an Interface

An interface in Java is declared using the `interface` keyword:

```java
public interface Animal {
    // Constant declaration
    int legs = 4;

    // Method signature (implicitly public and abstract)
    void eat();

    // Default method (introduced in Java 8)
    default void breathe() {
        System.out.println("Breathing");
    }

    // Static method (introduced in Java 8)
    static void move() {
        System.out.println("Moving");
    }
}
```

### Implementing an Interface

To use an interface in Java, a class implements the interface using the `implements` keyword. The class then provides concrete implementations of all the abstract methods declared in the interface.

```java
public class Dog implements Animal {
    @Override
    public void eat() {
        System.out.println("Dog is eating");
    }

    // Optional: Override default method
    @Override
    public void breathe() {
        System.out.println("Dog is breathing");
    }
}
```

### Example of Using Interfaces

```java
public class InterfaceExample {
    public static void main(String[] args) {
        Animal dog = new Dog();
        dog.eat();       // Output: Dog is eating
        dog.breathe();   // Output: Dog is breathing (overridden method)

        Animal.move();   // Output: Moving (calling static method from interface)
    }
}
```

### Benefits of Interfaces

- **Abstraction**: Interfaces allow you to define a contract without specifying the implementation details. This promotes loose coupling and enhances code maintainability.

- **Multiple Inheritance**: Interfaces facilitate multiple inheritance of type, allowing a class to inherit behaviors from multiple sources.

- **Polymorphism**: Interfaces support polymorphism, where objects can be treated as instances of their interface type, enabling flexibility in object-oriented design.

- **Default and Static Methods**: Default and static methods in interfaces provide backward compatibility and utility methods, respectively, enhancing the functionality of interfaces.

### When to Use Interfaces

- Use interfaces when you want to provide a contract for a group of classes to implement common behavior.
- Use interfaces to achieve loose coupling and enhance flexibility in your design.
- Use interfaces when you need to support multiple inheritance of type.

### Summary

Interfaces in Java provide a powerful mechanism for defining contracts and achieving abstraction, multiple inheritance of type, and polymorphism. They play a crucial role in modern Java programming, facilitating code reuse, flexibility, and maintainability. Understanding how to declare, implement, and use interfaces is essential for effective object-oriented design and development in Java.
