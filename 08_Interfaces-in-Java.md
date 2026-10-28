# Interfaces in Java

## 1. What Is an Interface?

An **interface** in Java defines a contract that classes can agree to follow. It specifies methods that an implementing class must provide, along with certain kinds of methods that can have implementations.

Interfaces help define **what an object can do** without requiring every implementing class to use the same inheritance hierarchy or internal implementation.

For example, different classes may be able to print, move, or send notifications:

```text
             Printable
             (interface)
             /        \
            /          \
      Report          Photo
      prints          prints
      report          photo
```

Both `Report` and `Photo` can implement `Printable`, even though they are different types of objects.

The keyword used to declare an interface is:

```java
interface
```

A class uses the keyword:

```java
implements
```

to implement an interface.

Example:

```java
interface Printable {
    void print();
}

class Report implements Printable {

    @Override
    public void print() {
        System.out.println("Printing report...");
    }
}
```

Usage:

```java
Report report = new Report();
report.print();
```

Output:

```text
Printing report...
```

The interface declares `print()`. The `Report` class supplies its implementation.

---

## 2. Why Do We Use Interfaces?

Interfaces are useful when different classes need to provide the same capability or follow the same contract.

They help us:

- Define common behavior across different classes.
- Reduce dependency on a particular implementation.
- Support polymorphism.
- Allow a class to implement multiple interfaces.
- Make code easier to extend and test.
- Separate a contract from the implementation.
- Design systems where implementations can be changed without changing the code that uses the interface.

For example, an application may send notifications through email or SMS. Both can follow the same `NotificationService` interface, even though they send messages differently.

---

## 3. Basic Interface Syntax

```java
interface InterfaceName {

    void requiredMethod();
}
```

A class implements the interface like this:

```java
class ClassName implements InterfaceName {

    @Override
    public void requiredMethod() {
        System.out.println("Implementation goes here.");
    }
}
```

A simple example:

```java
interface Greeter {
    void greet();
}

class EnglishGreeter implements Greeter {

    @Override
    public void greet() {
        System.out.println("Hello!");
    }
}

class FilipinoGreeter implements Greeter {

    @Override
    public void greet() {
        System.out.println("Kumusta!");
    }
}
```

Both classes follow the same contract but provide different behavior.

---

# 4. Rules in Defining an Interface

## Rule 1 — Use the `interface` Keyword

Declare an interface using `interface`.

```java
interface Drivable {
    void drive();
}
```

Do not declare it as a class:

```java
// Incorrect for an interface declaration:
// class Drivable {
//     void drive();
// }
```

## Rule 2 — Interface Methods Are Usually Public and Abstract by Default

A method declared in an interface without a body is implicitly `public abstract`.

These declarations are equivalent:

```java
interface Drivable {
    void drive();
}
```

and:

```java
interface Drivable {
    public abstract void drive();
}
```

The shorter form is common in Java code.

## Rule 3 — Ordinary Abstract Interface Methods Have No Method Body

Correct:

```java
interface Drawable {
    void draw();
}
```

A regular abstract interface method ends with a semicolon.

Java interfaces can also contain methods with bodies, such as `default`, `static`, and private helper methods, discussed below.

## Rule 4 — Interface Fields Are Constants

Fields declared in an interface are implicitly:

- `public`
- `static`
- `final`

Example:

```java
interface AppSettings {
    int MAX_LOGIN_ATTEMPTS = 3;
}
```

This is equivalent to:

```java
interface AppSettings {
    public static final int MAX_LOGIN_ATTEMPTS = 3;
}
```

Because the field is `final`, it cannot be reassigned.

```java
// AppSettings.MAX_LOGIN_ATTEMPTS = 5; // Compilation error
```

Interfaces are not intended to store per-object instance state.

## Rule 5 — Interfaces Do Not Have Constructors

An interface cannot define a constructor because it cannot be instantiated directly.

```java
interface Flyable {
    void fly();
}
```

A class that implements `Flyable` can have constructors.

```java
class Bird implements Flyable {

    public Bird() {
        System.out.println("Bird created.");
    }

    @Override
    public void fly() {
        System.out.println("Bird is flying.");
    }
}
```

## Rule 6 — An Interface Cannot Be Instantiated Directly

This is invalid:

```java
// Flyable object = new Flyable();
```

Create an object of a concrete implementing class instead:

```java
Flyable object = new Bird();
object.fly();
```

The interface can still be used as a variable, parameter, or return type.

## Rule 7 — An Interface Can Contain `default` Methods

A `default` method has an implementation and is available to implementing classes unless overridden.

```java
interface Logger {

    default void logInfo(String message) {
        System.out.println("INFO: " + message);
    }
}
```

A class can use the method without implementing it:

```java
class FileLogger implements Logger {
}

FileLogger logger = new FileLogger();
logger.logInfo("Application started.");
```

Output:

```text
INFO: Application started.
```

Default methods allow interfaces to provide reusable behavior while preserving compatibility as interfaces evolve.

## Rule 8 — An Interface Can Contain Static Methods

A static method belongs to the interface itself and is called using the interface name.

```java
interface Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Usage:

```java
int result = Calculator.add(10, 5);
System.out.println(result);
```

Output:

```text
15
```

Static interface methods are not inherited as instance methods by implementing classes. Call them through the interface name.

## Rule 9 — An Interface Can Contain Private Helper Methods

Since Java 9, interfaces can define private methods to share code between their own default and static methods.

```java
interface MessageFormatter {

    default String format(String message) {
        return addPrefix(message);
    }

    private String addPrefix(String message) {
        return "[MESSAGE] " + message;
    }
}
```

The private helper method is used internally by the interface. Implementing classes cannot call it directly.

## Rule 10 — Interfaces Cannot Be Declared `final`

An interface is designed to be implemented or extended. The `final` modifier is not permitted for interfaces.

```java
// Invalid Java:
// final interface Movable {
//     void move();
// }
```

## Rule 11 — An Interface Can Extend Another Interface

An interface can inherit the contract of another interface by using `extends`.

```java
interface Readable {
    void read();
}

interface Writable extends Readable {
    void write();
}
```

`Writable` includes the inherited `read()` requirement as well as its own `write()` requirement.

## Rule 12 — An Interface Can Extend Multiple Interfaces

Unlike classes, an interface can extend more than one interface.

```java
interface Printable {
    void print();
}

interface Savable {
    void save();
}

interface Document extends Printable, Savable {
    void close();
}
```

A concrete class implementing `Document` must provide implementations for `print()`, `save()`, and `close()`, unless suitable implementations are inherited.

## Rule 13 — An Interface Can Be Used as a Type

An interface can be used as a variable type, method parameter type, or return type.

```java
interface Playable {
    void play();
}

class MusicPlayer implements Playable {

    @Override
    public void play() {
        System.out.println("Playing audio.");
    }
}
```

Usage:

```java
Playable player = new MusicPlayer();
player.play();
```

Code can work with the `Playable` contract without depending on the concrete class name.

---

# 5. Rules in Inheriting an Interface

**Interface inheritance** means creating a new interface that extends an existing interface. The keyword used is `extends`, not `implements`.

## Rule 1 — Use `extends` to Inherit an Interface

```java
interface Animal {
    void eat();
}

interface Pet extends Animal {
    void play();
}
```

`Pet` inherits the `eat()` contract and adds `play()`.

## Rule 2 — An Interface Can Extend Multiple Interfaces

```java
interface Printable {
    void print();
}

interface Shareable {
    void share();
}

interface DigitalDocument extends Printable, Shareable {
    void save();
}
```

`DigitalDocument` inherits the method requirements from both parent interfaces.

## Rule 3 — An Extending Interface Inherits Abstract Method Requirements

```java
interface Vehicle {
    void start();
}

interface ElectricVehicle extends Vehicle {
    void charge();
}
```

A concrete class implementing `ElectricVehicle` must implement both `start()` and `charge()`.

## Rule 4 — An Abstract Method May Be Declared Again in a Child Interface

A child interface may redeclare an inherited method.

```java
interface Vehicle {
    void start();
}

interface Car extends Vehicle {
    @Override
    void start();
}
```

The method remains abstract in `Car`; a concrete implementing class still needs to implement it.

## Rule 5 — A Child Interface Can Add New Methods

```java
interface Vehicle {
    void start();
}

interface SmartVehicle extends Vehicle {
    void connectToApp();
}
```

A class implementing `SmartVehicle` must satisfy both contracts.

## Rule 6 — A Child Interface Can Inherit Default Methods

```java
interface Greeter {

    default void greet() {
        System.out.println("Hello!");
    }
}

interface FriendlyGreeter extends Greeter {
}
```

A class implementing `FriendlyGreeter` can use the inherited default `greet()` method.

## Rule 7 — Conflicting Default Methods Must Be Resolved

If an interface extends two interfaces that provide unrelated default implementations with the same signature, the child interface must resolve the conflict.

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}

interface B {

    default void show() {
        System.out.println("B");
    }
}

interface C extends A, B {

    @Override
    default void show() {
        A.super.show();
        B.super.show();
        System.out.println("C");
    }
}
```

The child interface supplies its own implementation and can explicitly call the parent defaults using `A.super.show()` and `B.super.show()`.

## Rule 8 — An Interface Does Not Extend a Class

An interface can extend interfaces, not classes.

```java
interface Movable {
    void move();
}
```

This is invalid:

```java
// interface Car extends VehicleClass { }
```

A class may extend a class and implement interfaces:

```java
class ElectricCar extends VehicleClass implements Movable {
    // Implement required methods.
}
```

## Rule 9 — An Interface Can Inherit Constants, but Avoid Name Conflicts

Interfaces may declare constants. When multiple parent interfaces provide fields with the same name, refer to the intended constant using its declaring interface name to avoid ambiguity.

```java
interface First {
    int LIMIT = 10;
}

interface Second {
    int LIMIT = 20;
}

interface Combined extends First, Second {
}
```

Use explicit qualification where appropriate:

```java
System.out.println(First.LIMIT);
System.out.println(Second.LIMIT);
```

## Rule 10 — A Class Does Not Use `extends` to Implement an Interface

Incorrect:

```java
// class Dog extends Pet { }
```

If `Pet` is an interface, a class uses `implements`:

```java
class Dog implements Pet {

    @Override
    public void eat() {
        System.out.println("Dog is eating.");
    }

    @Override
    public void play() {
        System.out.println("Dog is playing.");
    }
}
```

---

# 6. Rules in Implementing an Interface

A class **implements** an interface by promising to provide the required behavior. Use `implements` in the class declaration.

## Rule 1 — Use `implements`

```java
interface Flyable {
    void fly();
}

class Bird implements Flyable {

    @Override
    public void fly() {
        System.out.println("Bird is flying.");
    }
}
```

## Rule 2 — Implement All Required Abstract Methods

A concrete class must implement all inherited abstract interface methods unless suitable implementations are inherited from a superclass.

```java
interface Device {
    void turnOn();
    void turnOff();
}

class Television implements Device {

    @Override
    public void turnOn() {
        System.out.println("TV turned on.");
    }

    @Override
    public void turnOff() {
        System.out.println("TV turned off.");
    }
}
```

If a class does not implement all required methods, it must be declared `abstract`.

## Rule 3 — Implemented Methods Must Be `public`

Interface abstract methods are implicitly `public`. An implementation cannot reduce their visibility.

Correct:

```java
@Override
public void turnOn() {
    System.out.println("On");
}
```

Incorrect:

```java
// @Override
// protected void turnOn() { }
```

A `protected` or `private` implementation would have weaker access than the interface contract permits.

## Rule 4 — Use a Compatible Method Signature

The implementing method must match the interface method's name and parameter types, and it must use a compatible return type.

```java
interface Printer {
    void print(String text);
}

class ConsolePrinter implements Printer {

    @Override
    public void print(String text) {
        System.out.println(text);
    }
}
```

This does not implement `print(String)`:

```java
// public void print(int number) { }
```

That is a different overload.

## Rule 5 — Use `@Override`

It is recommended to mark interface implementations with `@Override`.

```java
@Override
public void print(String text) {
    System.out.println(text);
}
```

This allows the compiler to catch spelling or signature mistakes.

## Rule 6 — A Class Can Implement Multiple Interfaces

A class can implement multiple interfaces separated by commas.

```java
interface Printable {
    void print();
}

interface Savable {
    void save();
}

class Report implements Printable, Savable {

    @Override
    public void print() {
        System.out.println("Printing report.");
    }

    @Override
    public void save() {
        System.out.println("Saving report.");
    }
}
```

This is one of the important differences between class inheritance and interface implementation.

## Rule 7 — A Class Can Extend One Class and Implement Multiple Interfaces

The order is:

```java
class ClassName extends ParentClass
        implements InterfaceOne, InterfaceTwo {
}
```

Example:

```java
class Person {
    public void introduce() {
        System.out.println("Hello.");
    }
}

interface Learner {
    void study();
}

interface Athlete {
    void train();
}

class Student extends Person implements Learner, Athlete {

    @Override
    public void study() {
        System.out.println("Student is studying.");
    }

    @Override
    public void train() {
        System.out.println("Student is training.");
    }
}
```

A class can extend only one class, but it can implement multiple interfaces.

## Rule 8 — An Abstract Class May Implement an Interface Without Implementing Every Method

An abstract class can leave some interface methods for a later concrete subclass.

```java
interface Operable {
    void operate();
}

abstract class Machine implements Operable {
    // operate() is not implemented here.
}

class PrinterMachine extends Machine {

    @Override
    public void operate() {
        System.out.println("Printer machine is operating.");
    }
}
```

## Rule 9 — A Class Can Inherit an Implementation from Its Parent Class

If the superclass already provides a suitable public implementation of an interface method, the subclass may satisfy the interface through that inherited implementation.

```java
interface Greeting {
    void greet();
}

class BaseGreeter {
    public void greet() {
        System.out.println("Hello!");
    }
}

class FriendlyGreeter extends BaseGreeter implements Greeting {
}
```

The public `greet()` method inherited from `BaseGreeter` satisfies the interface method.

## Rule 10 — Resolve Conflicting Default Methods

If a class implements two unrelated interfaces that provide conflicting default methods with the same signature, the class must override the method to resolve the conflict.

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}

interface B {

    default void show() {
        System.out.println("B");
    }
}

class C implements A, B {

    @Override
    public void show() {
        A.super.show();
        B.super.show();
        System.out.println("C");
    }
}
```

## Rule 11 — A Class Can Be Used Through an Interface Reference

```java
interface Playable {
    void play();
}

class VideoPlayer implements Playable {

    @Override
    public void play() {
        System.out.println("Playing video.");
    }
}
```

Usage:

```java
Playable player = new VideoPlayer();
player.play();
```

The variable depends on the `Playable` contract rather than the concrete class.

---

# 7. Example: Multiple Classes Implementing One Interface

Suppose a system supports different ways of making a payment. Each payment type should follow the same contract.

```java
interface PaymentMethod {
    void pay(double amount);
}
```

Credit card:

```java
class CreditCard implements PaymentMethod {

    @Override
    public void pay(double amount) {
        System.out.println(
            "Paid " + amount + " using a credit card."
        );
    }
}
```

Cash:

```java
class Cash implements PaymentMethod {

    @Override
    public void pay(double amount) {
        System.out.println(
            "Paid " + amount + " in cash."
        );
    }
}
```

Bank transfer:

```java
class BankTransfer implements PaymentMethod {

    @Override
    public void pay(double amount) {
        System.out.println(
            "Transferred " + amount + " through a bank."
        );
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        PaymentMethod payment1 = new CreditCard();
        PaymentMethod payment2 = new Cash();
        PaymentMethod payment3 = new BankTransfer();

        payment1.pay(1500);
        payment2.pay(500);
        payment3.pay(2500);
    }
}
```

All payment classes implement `PaymentMethod`, but each class provides its own implementation of `pay()`.

This is a practical example of **polymorphism**.

---

# 8. Example: Notification Service

An application may need to send messages through different channels.

```java
interface NotificationService {
    void send(String recipient, String message);
}
```

Email implementation:

```java
class EmailNotificationService implements NotificationService {

    @Override
    public void send(String recipient, String message) {
        System.out.println(
            "Email to " + recipient + ": " + message
        );
    }
}
```

SMS implementation:

```java
class SmsNotificationService implements NotificationService {

    @Override
    public void send(String recipient, String message) {
        System.out.println(
            "SMS to " + recipient + ": " + message
        );
    }
}
```

A class can depend on the interface:

```java
class NotificationManager {

    private NotificationService notificationService;

    public NotificationManager(
        NotificationService notificationService
    ) {
        this.notificationService = notificationService;
    }

    public void notifyUser(String recipient, String message) {
        notificationService.send(recipient, message);
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        NotificationService emailService =
            new EmailNotificationService();

        NotificationManager manager =
            new NotificationManager(emailService);

        manager.notifyUser(
            "student@example.com",
            "Your class starts at 8 AM."
        );
    }
}
```

The `NotificationManager` does not need to know the internal details of email delivery. It uses the common interface contract.

A real service would connect the implementations to actual email or SMS providers.

---

# 9. Example: Interface Inheritance

This example shows an interface extending another interface.

```java
interface Workable {
    void work();
}

interface Reportable extends Workable {
    void generateReport();
}
```

A concrete class implements `Reportable`:

```java
class OfficeEmployee implements Reportable {

    @Override
    public void work() {
        System.out.println("Employee is working.");
    }

    @Override
    public void generateReport() {
        System.out.println("Employee is generating a report.");
    }
}
```

Usage:

```java
OfficeEmployee employee = new OfficeEmployee();

employee.work();
employee.generateReport();
```

Because `Reportable` extends `Workable`, implementing `Reportable` also requires the `work()` behavior.

---

# 10. Example: Interface with a Default Method

Use a default method when an interface should provide a common implementation that classes can reuse or override.

```java
interface Device {

    void turnOn();

    default void showStatus() {
        System.out.println("Device status: ready.");
    }
}
```

Implementation:

```java
class Laptop implements Device {

    @Override
    public void turnOn() {
        System.out.println("Laptop is starting.");
    }
}
```

Usage:

```java
Laptop laptop = new Laptop();

laptop.turnOn();
laptop.showStatus();
```

`Laptop` must implement `turnOn()`, but it can inherit the `showStatus()` default method.

A class may override a default method when specialized behavior is needed.

---

# 11. Interface vs. Abstract Class

| Feature | Interface | Abstract Class |
|---|---|---|
| Declared using | `interface` | `abstract class` |
| Class relationship | A class `implements` it | A class `extends` it |
| Multiple can be used by a class? | Yes, a class can implement multiple interfaces | A class can extend only one class |
| Constructors | No | Yes |
| Per-instance fields | No | Yes |
| Constants | Fields are implicitly `public static final` | Can have ordinary instance and static fields |
| Abstract methods | Yes | Yes |
| Concrete methods | `default`, `static`, and private methods can have bodies | Yes |
| Typical purpose | Define a capability or contract | Share state and implementation among related classes |

A practical guideline:

- Use an **interface** when different classes need to follow the same contract.
- Use an **abstract class** when related classes need a shared base implementation or instance state.
- A class can extend an abstract class and implement one or more interfaces.

---

# 12. Common Mistakes

## Mistake 1 — Using `extends` Instead of `implements`

Incorrect:

```java
// class Dog extends Runnable { }
```

If `Runnable` is an interface, use:

```java
class Dog implements Runnable {

    @Override
    public void run() {
        System.out.println("Dog is running.");
    }
}
```

## Mistake 2 — Forgetting to Implement Required Methods

```java
interface Drawable {
    void draw();
}
```

A concrete class must implement `draw()`:

```java
class Circle implements Drawable {

    @Override
    public void draw() {
        System.out.println("Drawing circle.");
    }
}
```

Otherwise, the class must be declared abstract.

## Mistake 3 — Using Weaker Access Than `public`

Incorrect:

```java
// @Override
// void draw() { }
```

A method implementing an interface method must be `public`.

## Mistake 4 — Trying to Create an Interface Object

Incorrect:

```java
// Drawable drawable = new Drawable();
```

Correct:

```java
Drawable drawable = new Circle();
```

## Mistake 5 — Trying to Add Instance State to an Interface

Interface fields are constants, not per-object fields.

If every object needs its own `name`, `balance`, or `radius`, that state belongs in a class.

## Mistake 6 — Confusing Interface Inheritance with Class Implementation

- Interface extends interface: `extends`
- Class implements interface: `implements`
- Class extends class: `extends`

## Mistake 7 — Forgetting to Resolve Conflicting Default Methods

When unrelated interfaces provide conflicting defaults, the implementing class must override the conflicting method.

---

# 13. Rules Checklist for Implementation

## A. Rules in Defining an Interface

1. Use the `interface` keyword to declare an interface.
2. An interface cannot be instantiated directly.
3. Ordinary abstract interface methods have no body and end with a semicolon.
4. Interface abstract methods are implicitly `public abstract`.
5. Fields declared in an interface are implicitly `public static final`.
6. Interfaces do not have constructors.
7. Interfaces can contain `default` methods with implementations.
8. Interfaces can contain `static` methods with implementations.
9. Since Java 9, interfaces can contain private helper methods.
10. An interface can extend one or more interfaces.
11. An interface can be used as a variable, parameter, or return type.
12. An interface cannot be declared `final`.

## B. Rules in Inheriting an Interface

1. Use `extends` when one interface inherits from another interface.
2. An interface can extend multiple interfaces, separated by commas.
3. A child interface inherits the method requirements of its parent interfaces.
4. A child interface can declare additional methods.
5. A child interface can redeclare an inherited abstract method.
6. An abstract method can remain unimplemented in an interface because interfaces are not instantiated directly.
7. A class implementing the child interface must satisfy all inherited abstract method requirements.
8. A child interface can inherit default methods.
9. Conflicting inherited default methods must be resolved in the child interface or implementing class as required by Java's rules.
10. An interface cannot extend a class.
11. Use interface qualification when constants with the same name could be ambiguous.
12. Do not use `implements` to make one interface inherit another interface.

## C. Rules in Implementing an Interface

1. Use `implements` in a class declaration to implement an interface.
2. A concrete class must implement all inherited abstract interface methods unless suitable implementations are inherited.
3. A class that does not implement every required abstract method must be declared `abstract`.
4. Implemented interface methods must be `public`.
5. The method name and parameter types must match the interface method to implement it.
6. The return type must be compatible with the interface declaration.
7. Use `@Override` to help the compiler verify method implementations.
8. A class can implement multiple interfaces separated by commas.
9. A class can extend one class and implement multiple interfaces.
10. An abstract class can implement an interface while leaving some methods for a concrete subclass.
11. A suitable public method inherited from a superclass can satisfy an interface method.
12. A class must resolve conflicting default methods from unrelated interfaces.
13. An interface reference can refer to an object of any class implementing that interface.
14. Depend on interface types when code should be flexible and independent of concrete implementations.

## D. Design Rules

1. Use interfaces to define capabilities and contracts.
2. Keep implementation details inside implementing classes.
3. Prefer meaningful interface names, often adjectives such as `Runnable` or capability names such as `Printable`.
4. Keep each interface focused on a coherent responsibility.
5. Avoid adding unrelated methods to a single large interface.
6. Use default methods when a shared implementation is appropriate and compatible with the interface's purpose.
7. Use abstract classes when related classes need shared instance state, constructors, or common implementation.
8. Use dependency injection with interface types when implementations should be replaceable or testable.
9. Use polymorphism to write code against an interface rather than a specific implementation when practical.
10. Resolve method conflicts explicitly and clearly.

---

# 14. Quick Reference: `extends` vs. `implements`

| Situation | Correct Keyword | Example |
|---|---|---|
| Class extends a class | `extends` | `class Dog extends Animal` |
| Interface extends an interface | `extends` | `interface Pet extends Animal` |
| Class implements an interface | `implements` | `class Bird implements Flyable` |
| Class extends a class and implements interfaces | Both | `class Car extends Vehicle implements Drivable, Insurable` |

Remember:

```java
class ClassName extends ParentClass
        implements InterfaceOne, InterfaceTwo {
}
```

If a class extends a parent class, `extends` must appear before `implements`.

---

# 15. Complete Example: School System

This example demonstrates interfaces, multiple interface implementation, and polymorphism.

```java
interface Learner {
    void study();
}

interface Presenter {
    void present();
}

class Student implements Learner, Presenter {

    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void study() {
        System.out.println(name + " is studying.");
    }

    @Override
    public void present() {
        System.out.println(name + " is presenting a project.");
    }
}
```

Main program:

```java
public class Main {

    public static void main(String[] args) {

        Student student = new Student("Juan");

        Learner learner = student;
        Presenter presenter = student;

        learner.study();
        presenter.present();
    }
}
```

Output:

```text
Juan is studying.
Juan is presenting a project.
```

The `Student` class implements two interfaces, so the same object can be used through either capability.

---

# 16. Key Takeaways

An **interface** defines a contract that classes agree to follow. It allows code to depend on required behavior rather than a particular implementation.

Remember these three relationships:

```text
Interface defines a contract
         ↓
Interface extends interface
         ↓
Class implements interface
```

The basic syntax is:

```java
interface Printable {
    void print();
}

class Report implements Printable {

    @Override
    public void print() {
        System.out.println("Printing report.");
    }
}
```

Usage:

```java
Printable printable = new Report();
printable.print();
```

## Most Important Rules

1. Use `interface` to declare an interface.
2. Use `extends` when an interface inherits from another interface.
3. Use `implements` when a class implements an interface.
4. An interface cannot be instantiated directly.
5. Ordinary abstract interface methods are implicitly `public abstract`.
6. Interface fields are implicitly `public static final`.
7. Interfaces do not have constructors.
8. A concrete class must implement all required abstract methods unless implementations are inherited.
9. Implemented interface methods must be `public`.
10. A class can implement multiple interfaces.
11. A class can extend one class and implement multiple interfaces.
12. Use `@Override` to catch method signature mistakes.
13. Resolve conflicting default methods.
14. Use interface references to support polymorphism and flexible design.
15. Use interfaces to define capabilities and contracts, and abstract classes when shared state or implementation is needed.

## Final Reminder

> **An interface defines what a class must be able to do; the implementing class defines how it does it.**
