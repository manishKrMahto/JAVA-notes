# Object Oriented Concepts

## <u>Recapitulate concepts of Object Oriented Programming </u>
Already covered

## <u>Object </u>
Already covered

## <u>Class </u>
Already covered

## <u>Method </u>
Already covered

## <u>Abstraction </u>
### Abstraction in Java

Abstraction is one of the fundamental concepts of object-oriented programming (OOP) in Java. It allows us to focus on what an object does instead of how it does it. In Java, abstraction is achieved using abstract classes and interfaces. Let's explore these concepts in detail.

#### 1. **Abstract Classes**

An abstract class is a class that cannot be instantiated on its own and is meant to be subclassed. It can contain abstract methods (methods without a body) as well as concrete methods (methods with a body). The purpose of an abstract class is to provide a common interface and share code among related classes.

**Features of Abstract Classes:**
- An abstract class can have both abstract and non-abstract methods.
- It can have instance variables and constructors.
- It can have final methods, which cannot be overridden by subclasses.
- Abstract classes cannot be instantiated directly.

**Syntax:**
```java
abstract class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    abstract void sound(); // Abstract method

    void eat() { // Concrete method
        System.out.println(name + " is eating.");
    }
}
```

**Example:**
```java
abstract class Animal {
    abstract void sound(); // Abstract method

    void eat() {
        System.out.println("This animal eats food."); // Concrete method
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("The dog barks.");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("The cat meows.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();
        Animal myCat = new Cat();
        myDog.sound(); // Output: The dog barks.
        myCat.sound(); // Output: The cat meows.
        myDog.eat();   // Output: This animal eats food.
        myCat.eat();   // Output: This animal eats food.
    }
}
```

#### 2. **Interfaces**

An interface in Java is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. Interfaces cannot contain instance fields. Methods in interfaces are implicitly abstract and public if not explicitly defined as default or static.

**Features of Interfaces:**
- Interfaces provide a way to achieve abstraction and multiple inheritance.
- All methods in an interface are abstract unless they are defined as default or static.
- A class can implement multiple interfaces, which allows for a form of multiple inheritance.
- Interfaces cannot have constructors.

**Syntax:**
```java
interface Animal {
    void sound(); // Abstract method
    void eat();   // Abstract method
}
```

**Example:**
```java
interface Animal {
    void sound(); // Abstract method
    void eat();   // Abstract method
}

class Dog implements Animal {
    @Override
    public void sound() {
        System.out.println("The dog barks.");
    }

    @Override
    public void eat() {
        System.out.println("The dog eats bones.");
    }
}

class Cat implements Animal {
    @Override
    public void sound() {
        System.out.println("The cat meows.");
    }

    @Override
    public void eat() {
        System.out.println("The cat eats fish.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();
        Animal myCat = new Cat();
        myDog.sound(); // Output: The dog barks.
        myCat.sound(); // Output: The cat meows.
        myDog.eat();   // Output: The dog eats bones.
        myCat.eat();   // Output: The cat eats fish.
    }
}
```

#### 3. **Differences Between Abstract Classes and Interfaces**

- **Abstract Class:**
  - Can have both abstract and concrete methods.
  - Can have instance variables.
  - Can have constructors.
  - Supports single inheritance (a class can inherit only one abstract class).

- **Interface:**
  - All methods are abstract by default (until Java 8, where default and static methods were introduced).
  - Cannot have instance variables (only constants).
  - Cannot have constructors.
  - Supports multiple inheritance (a class can implement multiple interfaces).

### Summary

Abstraction in Java is achieved using abstract classes and interfaces, which provide a way to define methods that must be implemented by derived classes, thus hiding the implementation details and showing only the necessary features. This helps in designing more flexible and maintainable code by focusing on the essentials and allowing specific implementations to vary.

## <u>Encapsulation </u>
### Encapsulation in Java

Encapsulation is one of the four fundamental principles of Object-Oriented Programming (OOP) in Java. It refers to the bundling of data (variables) and methods (functions) that operate on the data into a single unit, known as a class. Encapsulation restricts direct access to some of the object's components, which can prevent the accidental modification of data.

The main objectives of encapsulation are to:
- **Protect an object's internal state:** The internal state of an object should only be modified through controlled methods.
- **Reduce code complexity:** By hiding the complex implementation details, encapsulation makes the code easier to maintain and understand.
- **Improve code modularity:** Encapsulation helps in organizing the code into logical units.

### Key Concepts

#### 1. **Access Modifiers**

Access modifiers in Java define the scope of a class, constructor, method, or variable. There are four types of access modifiers:

- **private:** The member is accessible only within the same class.
- **default** (no modifier): The member is accessible only within the same package.
- **protected:** The member is accessible within the same package and by subclasses.
- **public:** The member is accessible from any other class.

#### 2. **Getters and Setters**

Getters and setters are methods used to access and modify the private fields of a class. They provide a controlled way to access and update the data.

**Example:**

```java
public class Person {
    // Private fields
    private String name;
    private int age;

    // Public getter method for name
    public String getName() {
        return name;
    }

    // Public setter method for name
    public void setName(String name) {
        this.name = name;
    }

    // Public getter method for age
    public int getAge() {
        return age;
    }

    // Public setter method for age
    public void setAge(int age) {
        if (age > 0) {
            this.age = age;
        }
    }
}
```

In the above example, the `name` and `age` fields are private, so they cannot be accessed directly from outside the `Person` class. Instead, they are accessed and modified through the `getName`, `setName`, `getAge`, and `setAge` methods.

### Advantages of Encapsulation

1. **Control of the Data:** The class can control what is stored in its fields. This control allows validation and the enforcement of certain conditions when setting values.
2. **Increased Flexibility:** The internal representation of an object can be changed without affecting the code that uses the object.
3. **Reusability:** Encapsulated code is easier to manage, understand, and reuse.
4. **Modularity:** Encapsulation helps in organizing the code into logical units, which improves modularity and code maintenance.
5. **Data Hiding:** By restricting access to certain components of an object, encapsulation provides data hiding, which enhances security.

### Example of Encapsulation

Let's look at a more detailed example to understand encapsulation better.

```java
public class BankAccount {
    // Private fields
    private String accountNumber;
    private double balance;

    // Constructor
    public BankAccount(String accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }

    // Public getter for accountNumber
    public String getAccountNumber() {
        return accountNumber;
    }

    // Public getter for balance
    public double getBalance() {
        return balance;
    }

    // Public method to deposit money
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    // Public method to withdraw money
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // Create a new BankAccount object
        BankAccount myAccount = new BankAccount("123456789", 500.00);

        // Access the account number
        System.out.println("Account Number: " + myAccount.getAccountNumber());

        // Deposit money
        myAccount.deposit(200.00);
        System.out.println("Balance after deposit: " + myAccount.getBalance());

        // Withdraw money
        myAccount.withdraw(100.00);
        System.out.println("Balance after withdrawal: " + myAccount.getBalance());
    }
}
```

In this example, the `BankAccount` class encapsulates the `accountNumber` and `balance` fields. These fields are private, and they can only be accessed and modified through the public methods `getAccountNumber`, `getBalance`, `deposit`, and `withdraw`. This ensures that the balance can only be changed through controlled operations, preventing accidental or unauthorized modifications.

### Summary

Encapsulation in Java is a powerful mechanism for bundling the data and methods that operate on the data within a single unit (a class). By using private fields and providing public getter and setter methods, encapsulation provides controlled access to the internal state of an object. This approach helps protect the object's integrity, improves code modularity and maintainability, and enhances security by hiding implementation details from the outside world.

## <u>Polymorphism </u>
### Polymorphism in Java

Polymorphism is one of the core concepts of Object-Oriented Programming (OOP) in Java. It refers to the ability of a single interface or method to operate on different types or classes. The term "polymorphism" means "many shapes" or "many forms." In Java, polymorphism allows methods to perform different tasks based on the object they are acting upon. There are two main types of polymorphism in Java:

1. **Compile-Time Polymorphism (Method Overloading)**
2. **Run-Time Polymorphism (Method Overriding)**

### 1. Compile-Time Polymorphism (Method Overloading)

Method overloading is a form of compile-time polymorphism where multiple methods have the same name but different parameters (different type, number, or both). The method to be invoked is determined at compile time based on the method signature.

**Example:**

```java
public class MathUtils {
    // Method to add two integers
    public int add(int a, int b) {
        return a + b;
    }

    // Method to add three integers
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    // Method to add two double values
    public double add(double a, double b) {
        return a + b;
    }

    public static void main(String[] args) {
        MathUtils math = new MathUtils();
        System.out.println(math.add(5, 3));           // Output: 8
        System.out.println(math.add(5, 3, 2));        // Output: 10
        System.out.println(math.add(5.5, 3.5));       // Output: 9.0
    }
}
```

In the above example, the `add` method is overloaded with different parameter lists. The appropriate method is chosen at compile time based on the arguments passed to the method.

### 2. Run-Time Polymorphism (Method Overriding)

Method overriding is a form of run-time polymorphism where a subclass provides a specific implementation for a method that is already defined in its superclass. The method to be invoked is determined at run time based on the actual object type.

**Example:**

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal;

        myAnimal = new Dog();
        myAnimal.sound();  // Output: Dog barks

        myAnimal = new Cat();
        myAnimal.sound();  // Output: Cat meows
    }
}
```

In the above example, the `sound` method is overridden in the `Dog` and `Cat` classes. The method to be called is determined at runtime based on the actual object type.

### Advantages of Polymorphism

1. **Flexibility:** Polymorphism allows one interface to be used for a general class of actions. It helps in designing systems that are easily extensible and maintainable.
2. **Reusability:** Methods can be reused without modification in subclasses, promoting code reuse.
3. **Maintainability:** It enhances the ability to manage and maintain the code by allowing a single method to work with different types of objects.
4. **Scalability:** New subclasses can be added with minimal changes to the existing code, making the system more scalable.

### Example of Polymorphism in a Real-World Scenario

Consider a scenario where you have different types of employees in a company, and each type of employee has a different way of calculating their salary.

```java
class Employee {
    String name;
    double baseSalary;

    Employee(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }

    double calculateSalary() {
        return baseSalary;
    }
}

class Manager extends Employee {
    double bonus;

    Manager(String name, double baseSalary, double bonus) {
        super(name, baseSalary);
        this.bonus = bonus;
    }

    @Override
    double calculateSalary() {
        return baseSalary + bonus;
    }
}

class Developer extends Employee {
    double stockOptions;

    Developer(String name, double baseSalary, double stockOptions) {
        super(name, baseSalary);
        this.stockOptions = stockOptions;
    }

    @Override
    double calculateSalary() {
        return baseSalary + stockOptions;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee emp1 = new Manager("Alice", 70000, 10000);
        Employee emp2 = new Developer("Bob", 80000, 5000);

        System.out.println(emp1.name + "'s Salary: " + emp1.calculateSalary()); // Output: Alice's Salary: 80000.0
        System.out.println(emp2.name + "'s Salary: " + emp2.calculateSalary()); // Output: Bob's Salary: 85000.0
    }
}
```

In this example, the `calculateSalary` method is overridden in the `Manager` and `Developer` classes to provide specific implementations. At runtime, the appropriate method is called based on the actual object type.

### Summary

Polymorphism in Java allows methods to do different things based on the object they are acting upon. There are two types of polymorphism:

- **Compile-Time Polymorphism (Method Overloading):** Multiple methods with the same name but different parameter lists.
- **Run-Time Polymorphism (Method Overriding):** A subclass provides a specific implementation for a method that is already defined in its superclass.

Polymorphism enhances flexibility, reusability, maintainability, and scalability of code, making it a powerful feature of OOP in Java.

## <u>Inheritence </u>
Already Covered 

## <u> Dynamic Binding and Message Passing </u>
### Dynamic Binding in Java

Dynamic binding (also known as late binding) is a concept in Java where the method to be called is resolved at runtime, not at compile time. This is in contrast to static binding (early binding), where method calls are resolved at compile time.

#### How Dynamic Binding Works

In Java, dynamic binding occurs when a method call to an overridden method is resolved at runtime rather than compile time. This is facilitated by Java’s ability to use polymorphism, where a reference of a superclass type can point to an object of a subclass.

#### Example of Dynamic Binding:

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal;

        myAnimal = new Dog();
        myAnimal.sound();  // Output: Dog barks

        myAnimal = new Cat();
        myAnimal.sound();  // Output: Cat meows
    }
}
```

In this example, the `sound` method is overridden in the `Dog` and `Cat` classes. When `myAnimal.sound()` is called, the actual method that gets executed is determined at runtime based on the actual object type that `myAnimal` refers to. This is dynamic binding.

#### Benefits of Dynamic Binding:

1. **Flexibility:** Allows for more flexible and reusable code.
2. **Runtime Decision Making:** Enables decision-making at runtime, making programs more dynamic and adaptable.
3. **Polymorphism:** It is a fundamental aspect of polymorphism in OOP, allowing methods to behave differently based on the object that invokes them.

### Message Passing in Java

Message passing is a form of communication used in object-oriented programming where objects communicate by sending and receiving information to each other. In Java, message passing typically occurs through method calls.

#### Key Points of Message Passing:

1. **Objects and Methods:** Objects in Java interact by invoking methods on other objects.
2. **Encapsulation:** Methods allow encapsulated data to be accessed or modified, adhering to the principle of encapsulation.
3. **Method Invocation:** The actual communication is done through method invocation, where an object calls a method on another object, possibly passing arguments and expecting a return value.

#### Example of Message Passing:

```java
class Printer {
    void print(String message) {
        System.out.println(message);
    }
}

class User {
    Printer printer;

    User(Printer printer) {
        this.printer = printer;
    }

    void sendPrintRequest(String message) {
        printer.print(message);
    }
}

public class Main {
    public static void main(String[] args) {
        Printer printer = new Printer();
        User user = new User(printer);

        user.sendPrintRequest("Hello, World!");  // Output: Hello, World!
    }
}
```

In this example, the `User` object communicates with the `Printer` object by sending a print request. The `sendPrintRequest` method in the `User` class acts as a message passing mechanism to invoke the `print` method of the `Printer` class.

#### Benefits of Message Passing:

1. **Decoupling:** Objects can interact without being tightly coupled, making the system more modular.
2. **Maintainability:** Easier to maintain and update the code as changes in one part do not necessarily impact others.
3. **Reusability:** Objects and methods can be reused across different parts of a program or in different programs.

### Summary

**Dynamic Binding:**
- Dynamic binding in Java refers to the process where the method to be invoked is determined at runtime.
- It is a key feature enabling polymorphism, allowing methods to be overridden in subclasses and the appropriate method to be called based on the actual object type at runtime.

**Message Passing:**
- Message passing is the process by which objects communicate and interact in Java.
- Objects send messages to each other through method calls, adhering to encapsulation principles and promoting modular, maintainable, and reusable code.

These concepts are fundamental to object-oriented programming in Java, facilitating flexible, dynamic, and efficient code design.
