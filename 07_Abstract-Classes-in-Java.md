# Abstract Classes in Java

## 1. What Is an Abstract Class?

An **abstract class** is a class declared with the `abstract` keyword. It is designed to serve as a base class for other classes.

An abstract class can define common fields and methods for its subclasses. It can also declare **abstract methods**—methods that have a declaration but no implementation in the abstract class.

Subclasses can then provide the implementations of those abstract methods.

```text
             Abstract Class
             /            \
       Subclass A        Subclass B
       implements        implements
       behavior A        behavior B
```

Example:

```java
abstract class Animal {
    public abstract void makeSound();
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Bark!");
    }
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}
```

Usage:

```java
Dog dog = new Dog();
Cat cat = new Cat();

dog.makeSound();
cat.makeSound();
```

Output:

```text
Bark!
Meow!
```

The `Animal` class defines a required behavior, `makeSound()`, but leaves its implementation to each concrete subclass.

---

## 2. Why Do We Use Abstract Classes?

Abstract classes are useful when multiple related classes share some common features but need different implementations for certain behaviors.

They help us:

- Define a common base for related classes.
- Reuse fields, constructors, and implemented methods.
- Require subclasses to implement specific behaviors.
- Provide a consistent structure across subclasses.
- Support inheritance and runtime polymorphism.
- Reduce repeated code.

For example, `Dog`, `Cat`, and `Bird` are all animals. They may share common behavior such as eating, but they make different sounds.

```java
abstract class Animal {
    public void eat() {
        System.out.println("The animal is eating.");
    }

    public abstract void makeSound();
}
```

The `eat()` method is implemented once, while `makeSound()` is left for each subclass.

---

## 3. The `abstract` Keyword

Java uses the `abstract` keyword to declare abstract classes and abstract methods.

Abstract class:

```java
abstract class Shape {
}
```

Abstract method:

```java
abstract void draw();
```

An abstract method has no method body. It ends with a semicolon.

A concrete method, in contrast, has a body:

```java
void display() {
    System.out.println("Displaying information.");
}
```

A class may contain both abstract and concrete methods if the class itself is declared abstract.

---

## 4. Rules in Defining Abstract Classes

### Rule 1 — Use the `abstract` Keyword

An abstract class must be declared using `abstract`.

```java
abstract class Vehicle {
}
```

### Rule 2 — You Cannot Instantiate an Abstract Class Directly

This is invalid:

```java
abstract class Animal {
}

// Animal animal = new Animal(); // Compilation error
```

Instead, instantiate a concrete subclass:

```java
class Dog extends Animal {
}

Dog dog = new Dog();
```

The abstract class can still be used as a reference type:

```java
Animal animal = new Dog();
```

### Rule 3 — An Abstract Class Can Contain Abstract and Concrete Methods

```java
abstract class Animal {
    public abstract void makeSound();

    public void sleep() {
        System.out.println("Sleeping...");
    }
}
```

`makeSound()` is abstract. `sleep()` has an implementation.

### Rule 4 — An Abstract Class Can Have Fields

```java
abstract class Employee {
    protected String name;
    protected double salary;
}
```

Fields can store state shared by subclasses.

### Rule 5 — An Abstract Class Can Have Constructors

An abstract class can define constructors to initialize its fields.

```java
abstract class Employee {
    protected String name;

    public Employee(String name) {
        this.name = name;
    }
}
```

The constructor is called when a concrete subclass object is created.

```java
class Developer extends Employee {
    public Developer(String name) {
        super(name);
    }
}
```

Usage:

```java
Developer developer = new Developer("Juan");
```

You cannot create an `Employee` object directly if `Employee` is abstract, but its constructor initializes the parent portion of a `Developer` object.

### Rule 6 — An Abstract Class Can Have Static Methods

Abstract classes may declare static methods because static methods belong to the class, not to an instance.

```java
abstract class Utility {
    public static void showMessage() {
        System.out.println("Hello!");
    }
}
```

Usage:

```java
Utility.showMessage();
```

An abstract class still cannot be instantiated directly.

### Rule 7 — An Abstract Class Can Extend Another Class

```java
abstract class Animal {
    public abstract void makeSound();
}

abstract class Mammal extends Animal {
    public void breathe() {
        System.out.println("Breathing...");
    }
}
```

`Mammal` remains abstract and does not need to implement `makeSound()` yet.

### Rule 8 — An Abstract Class Can Implement Interfaces

```java
interface Movable {
    void move();
}

abstract class Vehicle implements Movable {
    public void start() {
        System.out.println("Vehicle starting.");
    }
}
```

Because `Vehicle` is abstract, it can leave `move()` unimplemented. A concrete subclass must implement it unless the method is implemented in an intermediate class.

### Rule 9 — An Abstract Class Cannot Be `final`

These modifiers conflict:

```java
// Invalid Java:
// final abstract class Animal {
// }
```

A `final` class cannot be extended, while an abstract class is intended to be subclassed.

### Rule 10 — An Abstract Class May Have No Abstract Methods

An abstract class can be declared abstract even if every method has an implementation.

```java
abstract class Report {
    public void printHeader() {
        System.out.println("Annual Report");
    }
}
```

This may be useful when the class is intended only as a base class and should not be instantiated directly.

---

## 5. Rules in Defining Abstract Methods

An **abstract method** declares a behavior that must be implemented by a concrete subclass, unless a suitable implementation is inherited.

### Rule 1 — Declare the Method with `abstract`

```java
public abstract void makeSound();
```

### Rule 2 — An Abstract Method Has No Body

Correct:

```java
abstract void draw();
```

Incorrect:

```java
// Invalid: an abstract method cannot have a method body.
// abstract void draw() {
//     System.out.println("Drawing");
// }
```

If a method has a body, it is a concrete method, not an abstract method.

### Rule 3 — End the Declaration with a Semicolon

```java
public abstract void calculateArea();
```

Because there is no body, the declaration ends with `;`.

### Rule 4 — A Class Containing an Abstract Method Must Be Abstract

Invalid:

```java
// Invalid Java:
// class Shape {
//     abstract void draw();
// }
```

Correct:

```java
abstract class Shape {
    public abstract void draw();
}
```

A non-abstract class cannot contain an abstract method.

### Rule 5 — A Concrete Subclass Must Implement Inherited Abstract Methods

```java
abstract class Shape {
    public abstract void draw();
}

class Circle extends Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a circle.");
    }
}
```

If `Circle` does not implement `draw()`, it must also be declared abstract.

### Rule 6 — An Abstract Subclass May Leave Abstract Methods Unimplemented

```java
abstract class Shape {
    public abstract void draw();
}

abstract class TwoDimensionalShape extends Shape {
    // draw() is not implemented here.
}
```

A later concrete subclass must implement `draw()`.

### Rule 7 — An Abstract Method Cannot Be `private`

This is invalid:

```java
// Invalid Java:
// abstract class Example {
//     private abstract void run();
// }
```

A private method cannot be overridden by a subclass, so a private abstract method would not make sense.

### Rule 8 — An Abstract Method Cannot Be `final`

This is invalid:

```java
// Invalid Java:
// abstract class Example {
//     public final abstract void run();
// }
```

An abstract method requires an implementation in a concrete subclass, but a final method cannot be overridden.

### Rule 9 — An Abstract Method Cannot Be `static`

This is invalid:

```java
// Invalid Java:
// abstract class Example {
//     public static abstract void run();
// }
```

Static methods belong to the class and are not overridden as instance methods. Abstract methods rely on implementation by subclasses through overriding.

### Rule 10 — Use a Compatible Access Level When Overriding

An overriding method cannot reduce the visibility of the inherited method.

```java
abstract class Animal {
    public abstract void makeSound();
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Bark!");
    }
}
```

This is invalid because it reduces visibility:

```java
// Invalid Java:
// class Dog extends Animal {
//     @Override
//     protected void makeSound() {
//     }
// }
```

A `public` method must remain `public` when overridden.

### Rule 11 — The Overriding Method Must Have a Compatible Signature

The method name and parameter types must match the abstract method's signature for overriding.

```java
abstract class Printer {
    public abstract void print(String message);
}

class ConsolePrinter extends Printer {
    @Override
    public void print(String message) {
        System.out.println(message);
    }
}
```

A method with different parameters does not implement the required method:

```java
// This overload does not implement print(String):
// public void print(int number) { }
```

### Rule 12 — The Return Type Must Be Compatible

The overriding method must use the same return type or a permitted covariant return type for reference types.

```java
abstract class AnimalFactory {
    public abstract Animal create();
}

class DogFactory extends AnimalFactory {
    @Override
    public Dog create() {
        return new Dog();
    }
}
```

Here, `Dog` is a subtype of `Animal`, so the covariant return type is valid.

---

## 6. Example: Abstract Shape Class

A common use of abstract classes is defining a family of shapes.

```java
abstract class Shape {
    public abstract double calculateArea();

    public void display() {
        System.out.println("This is a shape.");
    }
}
```

Circle:

```java
class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```

Rectangle:

```java
class Rectangle extends Shape {
    private double length;
    private double width;

    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    public double calculateArea() {
        return length * width;
    }
}
```

Usage:

```java
public class Main {
    public static void main(String[] args) {
        Shape circle = new Circle(5);
        Shape rectangle = new Rectangle(4, 6);

        circle.display();
        System.out.println("Circle area: " + circle.calculateArea());

        rectangle.display();
        System.out.println("Rectangle area: " + rectangle.calculateArea());
    }
}
```

The `Shape` class defines a common contract: every concrete shape must calculate its area. Each subclass supplies its own formula.

---

## 7. Example: Abstract Employee Class

An employee may have common information such as name and employee ID, while different employee types calculate compensation differently.

```java
abstract class Employee {
    private String name;
    private int employeeId;

    public Employee(String name, int employeeId) {
        this.name = name;
        this.employeeId = employeeId;
    }

    public String getName() {
        return name;
    }

    public int getEmployeeId() {
        return employeeId;
    }

    public void displayInformation() {
        System.out.println("Name: " + name);
        System.out.println("Employee ID: " + employeeId);
    }

    public abstract double calculatePay();
}
```

Salaried employee:

```java
class SalariedEmployee extends Employee {
    private double monthlySalary;

    public SalariedEmployee(
        String name,
        int employeeId,
        double monthlySalary
    ) {
        super(name, employeeId);
        this.monthlySalary = monthlySalary;
    }

    @Override
    public double calculatePay() {
        return monthlySalary;
    }
}
```

Hourly employee:

```java
class HourlyEmployee extends Employee {
    private double hourlyRate;
    private double hoursWorked;

    public HourlyEmployee(
        String name,
        int employeeId,
        double hourlyRate,
        double hoursWorked
    ) {
        super(name, employeeId);
        this.hourlyRate = hourlyRate;
        this.hoursWorked = hoursWorked;
    }

    @Override
    public double calculatePay() {
        return hourlyRate * hoursWorked;
    }
}
```

Usage:

```java
public class Main {
    public static void main(String[] args) {
        Employee salaried =
            new SalariedEmployee("Maria", 1001, 30000);

        Employee hourly =
            new HourlyEmployee("Juan", 1002, 150, 160);

        salaried.displayInformation();
        System.out.println("Pay: " + salaried.calculatePay());

        hourly.displayInformation();
        System.out.println("Pay: " + hourly.calculatePay());
    }
}
```

The shared information and behavior live in `Employee`. The subclasses implement their own `calculatePay()` logic.

---

## 8. Example: Abstract Payment Class

Abstract classes can define a common contract for different payment types.

```java
abstract class Payment {
    protected double amount;

    public Payment(double amount) {
        this.amount = amount;
    }

    public void printReceipt() {
        System.out.println("Payment amount: " + amount);
    }

    public abstract void processPayment();
}
```

Credit card payment:

```java
class CreditCardPayment extends Payment {
    public CreditCardPayment(double amount) {
        super(amount);
    }

    @Override
    public void processPayment() {
        System.out.println("Processing credit card payment.");
    }
}
```

Cash payment:

```java
class CashPayment extends Payment {
    public CashPayment(double amount) {
        super(amount);
    }

    @Override
    public void processPayment() {
        System.out.println("Processing cash payment.");
    }
}
```

Usage:

```java
Payment payment = new CreditCardPayment(1500);

payment.processPayment();
payment.printReceipt();
```

This structure allows code to use the common `Payment` type while different subclasses process payments in different ways.

*Note:* A real payment system would also require security controls, transaction validation, and integration with a payment provider.

---

## 9. Abstract Classes and Polymorphism

An abstract class can be used as a reference type even though it cannot be instantiated directly.

```java
abstract class Animal {
    public abstract void makeSound();
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Bark!");
    }
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}
```

A method can accept any concrete `Animal`:

```java
public static void makeAnimalSound(Animal animal) {
    animal.makeSound();
}
```

Usage:

```java
makeAnimalSound(new Dog());
makeAnimalSound(new Cat());
```

Output:

```text
Bark!
Meow!
```

The same method works with different subclasses. The actual object's implementation runs at runtime.

---

## 10. Abstract Class vs. Concrete Class

| Feature | Abstract Class | Concrete Class |
|---|---|---|
| Can be instantiated directly? | No | Yes |
| Can contain abstract methods? | Yes | No |
| Can contain implemented methods? | Yes | Yes |
| Can have fields? | Yes | Yes |
| Can have constructors? | Yes | Yes |
| Typical purpose | Shared base and common contract | Fully usable object type |

---

## 11. Abstract Class vs. Interface

Both abstract classes and interfaces can define requirements for other classes, but they serve somewhat different purposes.

| Feature | Abstract Class | Interface |
|---|---|---|
| Declaration | `abstract class` | `interface` |
| Inheritance keyword | `extends` | `implements` |
| Can have instance fields? | Yes | No ordinary instance fields; fields are implicitly `public static final` |
| Constructors? | Yes | No |
| Can have concrete methods? | Yes | Yes, using permitted forms such as `default`, `static`, and private helper methods |
| A class can use multiple? | Extends only one class | Can implement multiple interfaces |
| Typical use | Shared state and implementation among related classes | A capability or contract that different classes can implement |

Example of an abstract class:

```java
abstract class Vehicle {
    protected String brand;

    public Vehicle(String brand) {
        this.brand = brand;
    }

    public abstract void start();
}
```

Example of an interface:

```java
interface Rechargeable {
    void recharge();
}
```

A class can extend a base class and implement an interface:

```java
class ElectricCar extends Vehicle implements Rechargeable {
    public ElectricCar(String brand) {
        super(brand);
    }

    @Override
    public void start() {
        System.out.println(brand + " electric car started.");
    }

    @Override
    public void recharge() {
        System.out.println("Battery charging.");
    }
}
```

---

## 12. Common Mistakes

### Mistake 1 — Instantiating an Abstract Class

Incorrect:

```java
// Shape shape = new Shape();
```

Correct:

```java
Shape shape = new Circle(3);
```

### Mistake 2 — Declaring an Abstract Method in a Concrete Class

Incorrect:

```java
// class Shape {
//     abstract void draw();
// }
```

Correct:

```java
abstract class Shape {
    abstract void draw();
}
```

### Mistake 3 — Giving an Abstract Method a Body

Incorrect:

```java
// abstract void draw() {
//     System.out.println("Draw");
// }
```

Correct:

```java
abstract void draw();
```

If a body is needed, remove the `abstract` keyword.

### Mistake 4 — Forgetting to Implement an Abstract Method

A concrete subclass must implement inherited abstract methods unless an implementation is already inherited. Otherwise, the subclass itself must be declared abstract.

### Mistake 5 — Reducing Method Visibility

If the abstract method is `public`, the implementation must not be `protected` or `private`.

### Mistake 6 — Using Incompatible Modifiers

An abstract method cannot be `private`, `static`, or `final`.

### Mistake 7 — Forgetting the Parent Constructor

If the abstract parent class only has a parameterized constructor, the child must call it using `super(arguments)`.

```java
abstract class Animal {
    public Animal(String name) {
    }
}

class Dog extends Animal {
    public Dog() {
        super("Dog");
    }
}
```

---

## 13. Rules Checklist for Implementation

### Abstract Class Rules

1. Declare an abstract class using the `abstract` keyword.
2. Do not instantiate an abstract class directly.
3. Use a concrete subclass to create an object.
4. An abstract class can contain abstract methods and concrete methods.
5. An abstract class can contain fields and constructors.
6. An abstract class can contain static methods.
7. An abstract class can extend another class.
8. An abstract class can implement one or more interfaces.
9. An abstract class cannot be `final`.
10. An abstract class may contain no abstract methods.
11. A subclass of an abstract class must either implement all inherited abstract methods or also be abstract.
12. Use abstract classes when related classes share state or implementation.

### Abstract Method Rules

1. Declare an abstract method with the `abstract` keyword.
2. Do not provide a method body.
3. End the declaration with a semicolon.
4. A class containing an abstract method must be declared abstract.
5. A concrete subclass must implement all inherited abstract methods unless an implementation is already inherited.
6. An abstract subclass may leave abstract methods unimplemented.
7. An abstract method cannot be `private`.
8. An abstract method cannot be `static`.
9. An abstract method cannot be `final`.
10. An overriding method must have a compatible signature.
11. An overriding method must have a compatible return type.
12. An overriding method cannot reduce the original method's visibility.
13. Use `@Override` to make the intent clear and catch signature mistakes.
14. Abstract methods are intended for instance-method overriding, not static method hiding.

### Design Rules

1. Put common state and reusable behavior in the abstract parent class.
2. Leave behavior abstract when subclasses must provide different implementations.
3. Avoid making a method abstract if a useful common implementation is appropriate.
4. Keep fields encapsulated and expose controlled methods.
5. Use constructors to initialize common state.
6. Use `super(...)` to initialize the abstract parent portion of a child object.
7. Use an abstract class when a shared base implementation or state is needed.
8. Consider an interface when you need a contract that unrelated classes can implement.
9. Prefer clear, meaningful class hierarchies.
10. Use `@Override` consistently for methods that implement or override inherited instance methods.

---

## 14. Complete Example: Notification System

This example combines abstract classes, constructors, concrete methods, abstract methods, inheritance, and polymorphism.

```java
abstract class Notification {
    private String recipient;

    public Notification(String recipient) {
        this.recipient = recipient;
    }

    public String getRecipient() {
        return recipient;
    }

    public void showRecipient() {
        System.out.println("Recipient: " + recipient);
    }

    public abstract void send();
}
```

Email notification:

```java
class EmailNotification extends Notification {
    private String subject;

    public EmailNotification(String recipient, String subject) {
        super(recipient);
        this.subject = subject;
    }

    @Override
    public void send() {
        System.out.println(
            "Sending email to " + getRecipient() +
            " with subject: " + subject
        );
    }
}
```

SMS notification:

```java
class SMSNotification extends Notification {
    private String message;

    public SMSNotification(String recipient, String message) {
        super(recipient);
        this.message = message;
    }

    @Override
    public void send() {
        System.out.println(
            "Sending SMS to " + getRecipient() +
            ": " + message
        );
    }
}
```

Main program:

```java
public class Main {
    public static void main(String[] args) {
        Notification email =
            new EmailNotification(
                "student@example.com",
                "Class Announcement"
            );

        Notification sms =
            new SMSNotification(
                "09171234567",
                "Your class starts at 8 AM."
            );

        email.showRecipient();
        email.send();

        sms.showRecipient();
        sms.send();
    }
}
```

The abstract `Notification` class contains the common recipient data and `showRecipient()` method. The subclasses implement the required `send()` behavior differently.

---

## 15. Key Takeaways

An **abstract class** provides a shared base for related classes. It can contain data, constructors, implemented methods, and abstract methods.

An **abstract method** declares required behavior without providing its implementation.

Remember:

```text
ABSTRACT CLASS
    ├── Fields
    ├── Constructors
    ├── Concrete methods
    └── Abstract methods
             ↓
       Concrete subclass
             ↓
    Implements required methods
             ↓
       Object can be created
```

The core syntax is:

```java
abstract class Parent {
    public void commonMethod() {
        System.out.println("Shared behavior");
    }

    public abstract void requiredMethod();
}
```

A concrete child implements the required method:

```java
class Child extends Parent {
    @Override
    public void requiredMethod() {
        System.out.println("Specialized behavior");
    }
}
```

Usage:

```java
Parent object = new Child();

object.commonMethod();
object.requiredMethod();
```

## Final Reminder

> **An abstract class defines a common foundation. An abstract method defines behavior that a concrete subclass must provide.**

Use abstract classes when you need related classes to share common state or behavior while requiring each concrete subclass to supply particular implementations.
