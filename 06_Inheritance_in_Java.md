# Inheritance in Java

## 1. What is Inheritance?

**Inheritance** is one of the fundamental principles of Object-Oriented Programming (OOP).

Inheritance allows one class to **acquire fields and methods from another class**.

The class that provides the common members is called the:

> **Parent class, superclass, or base class**

The class that inherits from it is called the:

> **Child class, subclass, or derived class**

The basic relationship is:

```text
Parent Class
     ↓
inherits
     ↓
Child Class
```

In Java, inheritance is implemented using the:

```java
extends
```

keyword.

### Basic Example

```java
class Animal {

    void eat() {
        System.out.println("Animal is eating.");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Dog is barking.");
    }
}
```

The `Dog` class inherits the `eat()` method from `Animal`.

We can use:

```java
Dog dog = new Dog();

dog.eat();
dog.bark();
```

Output:

```text
Animal is eating.
Dog is barking.
```

The `Dog` class has its own behavior:

```java
bark()
```

and also receives behavior from:

```java
Animal
```

through inheritance.

---

# 2. Why Do We Use Inheritance?

Inheritance is primarily used to:

- Reuse existing code
- Create relationships between classes
- Avoid unnecessary duplication
- Extend existing functionality
- Support polymorphism
- Represent "is-a" relationships

For example:

```text
Animal
 ├── Dog
 ├── Cat
 └── Bird
```

A dog **is an** animal.

A cat **is an** animal.

A bird **is an** animal.

Therefore, common properties and behaviors can be placed in the `Animal` class.

---

# 3. The `extends` Keyword

Java uses `extends` to establish class inheritance.

Example:

```java
class Person {

    String name;

    void introduce() {
        System.out.println("My name is " + name);
    }
}

class Student extends Person {

    String course;
}
```

`Student` inherits from `Person`.

Therefore, a `Student` object can access the inherited `name` field and `introduce()` method, subject to Java's access rules.

Example:

```java
Student student = new Student();

student.name = "Juan";
student.course = "BSIT";

student.introduce();
```

---

# 4. Parent Class and Child Class

Consider:

```java
class Employee {

    String name;
    double salary;

    void work() {
        System.out.println(name + " is working.");
    }
}

class Developer extends Employee {

    void writeCode() {
        System.out.println(name + " is writing code.");
    }
}
```

Here:

```text
Employee
   ↑
Developer
```

`Employee` is the parent class.

`Developer` is the child class.

The child class receives the accessible members of the parent class and can also define its own members.

---

# 5. What Does a Child Class Inherit?

A child class can use accessible members from its parent class.

For example:

```java
class Person {

    String name;

    void speak() {
        System.out.println("Person is speaking.");
    }
}

class Student extends Person {

    void study() {
        System.out.println(name + " is studying.");
    }
}
```

The `Student` class can use:

```text
name
speak()
```

from `Person`.

It also has:

```text
study()
```

of its own.

Therefore:

```java
Student student = new Student();

student.name = "Juan";
student.speak();
student.study();
```

---

# 6. The "Is-A" Relationship

Inheritance should generally represent an **is-a relationship**.

Examples:

```text
Dog is an Animal
Cat is an Animal
Student is a Person
Manager is an Employee
Car is a Vehicle
```

For example:

```java
class Vehicle {
}

class Car extends Vehicle {
}
```

This represents:

```text
Car IS-A Vehicle
```

However:

```text
Car HAS-A Engine
```

is not inheritance.

That is a **composition/has-a relationship**.

---

# 7. Inheritance vs. Composition

Consider a car.

A car is a vehicle:

```java
class Vehicle {
}

class Car extends Vehicle {
}
```

This is an:

```text
IS-A relationship
```

But a car has an engine:

```java
class Engine {
}

class Car {

    private Engine engine;
}
```

This is a:

```text
HAS-A relationship
```

A useful rule is:

```text
IS-A  → Inheritance
HAS-A → Composition
```

---

# 8. Adding New Features in a Child Class

A child class can add new fields and methods.

```java
class Employee {

    String name;

    void work() {
        System.out.println(name + " is working.");
    }
}

class Developer extends Employee {

    String programmingLanguage;

    void writeCode() {
        System.out.println(
            name + " is writing " +
            programmingLanguage + " code."
        );
    }
}
```

Usage:

```java
Developer developer = new Developer();

developer.name = "Juan";
developer.programmingLanguage = "Java";

developer.work();
developer.writeCode();
```

The developer has:

```text
Inherited:
    name
    work()

Own:
    programmingLanguage
    writeCode()
```

---

# 9. Method Overriding

Inheritance is closely related to **method overriding**.

A child class can provide its own implementation of an inherited method.

Example:

```java
class Animal {

    void sound() {
        System.out.println("Animal makes a sound.");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog barks.");
    }
}
```

Usage:

```java
Dog dog = new Dog();

dog.sound();
```

Output:

```text
Dog barks.
```

The child class has replaced the inherited implementation with its own implementation.

This is called:

> **Method overriding**

---

# 10. The `@Override` Annotation

When overriding a method, it is recommended to use:

```java
@Override
```

Example:

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog barks.");
    }
}
```

The annotation tells the compiler:

> "I intend to override a method from the parent class."

This helps detect mistakes.

For example:

```java
@Override
void sounds() {
}
```

If the parent class has no `sounds()` method, Java will report an error.

Without `@Override`, a spelling mistake could accidentally create a new method instead of overriding the intended one.

---

# 11. Calling the Parent Method with `super`

The `super` keyword can be used to access members of the parent class.

For example:

```java
class Animal {

    void sound() {
        System.out.println("Animal makes a sound.");
    }
}

class Dog extends Animal {

    @Override
    void sound() {

        super.sound();

        System.out.println("Dog barks.");
    }
}
```

Usage:

```java
Dog dog = new Dog();

dog.sound();
```

Output:

```text
Animal makes a sound.
Dog barks.
```

Here:

```java
super.sound();
```

calls the parent version of `sound()`.

---

# 12. `super` and Parent Fields

`super` can also refer to an accessible field in the parent class.

```java
class Person {

    String name = "Person";
}

class Student extends Person {

    String name = "Student";

    void displayNames() {

        System.out.println(name);
        System.out.println(super.name);
    }
}
```

Usage:

```java
Student student = new Student();

student.displayNames();
```

Output:

```text
Student
Person
```

Here:

```java
name
```

refers to the child field.

While:

```java
super.name
```

refers to the parent field.

---

# 13. Constructors and Inheritance

Constructors are **not inherited**.

However, when a child object is created, Java must initialize the parent portion of the object.

Consider:

```java
class Person {

    Person() {
        System.out.println("Person constructor");
    }
}

class Student extends Person {

    Student() {
        System.out.println("Student constructor");
    }
}
```

Creating:

```java
Student student = new Student();
```

produces:

```text
Person constructor
Student constructor
```

The parent constructor runs before the child constructor.

---

# 14. Calling a Parent Constructor with `super()`

A child constructor can explicitly call a parent constructor using:

```java
super();
```

Example:

```java
class Person {

    String name;

    Person(String name) {
        this.name = name;
    }
}

class Student extends Person {

    String course;

    Student(String name, String course) {

        super(name);

        this.course = course;
    }
}
```

Usage:

```java
Student student =
    new Student("Juan", "BSIT");
```

The following happens:

```text
Student constructor
       ↓
super(name)
       ↓
Person constructor
       ↓
name initialized
       ↓
Student constructor continues
       ↓
course initialized
```

---

# 15. Important Rule About `super()`

A call to a parent constructor using:

```java
super();
```

or:

```java
super(parameters);
```

must be the **first statement** in the child constructor.

Correct:

```java
Student(String name) {

    super(name);

    System.out.println("Student created.");
}
```

Incorrect:

```java
Student(String name) {

    System.out.println("Student created.");

    super(name);
}
```

The second version produces a compilation error.

---

# 16. Automatic `super()`

If a child constructor does not explicitly call `super()`, Java attempts to insert:

```java
super();
```

automatically as the first statement.

Example:

```java
class Person {

    Person() {
        System.out.println("Person");
    }
}

class Student extends Person {

    Student() {
        System.out.println("Student");
    }
}
```

Java effectively treats the child constructor as:

```java
Student() {

    super();

    System.out.println("Student");
}
```

Therefore, the parent no-argument constructor is called first.

---

# 17. What Happens If the Parent Has No No-Argument Constructor?

Consider:

```java
class Person {

    Person(String name) {
        System.out.println(name);
    }
}

class Student extends Person {

    Student() {
        System.out.println("Student");
    }
}
```

This produces a compilation error.

Why?

Java attempts to insert:

```java
super();
```

but `Person` does not have a no-argument constructor.

The child must explicitly call the available parent constructor:

```java
class Student extends Person {

    Student() {

        super("Juan");

        System.out.println("Student");
    }
}
```

---

# 18. Single Inheritance

Java supports **single inheritance for classes**.

This means a class can directly extend only one other class.

Valid:

```java
class Animal {
}

class Dog extends Animal {
}
```

Invalid:

```java
class A {
}

class B {
}

class C extends A, B {
}
```

Java does not allow a class to directly extend multiple classes.

---

# 19. Multilevel Inheritance

A class can inherit from a class that already inherits from another class.

Example:

```text
Animal
   ↓
Mammal
   ↓
Dog
```

Java:

```java
class Animal {

    void eat() {
        System.out.println("Eating");
    }
}

class Mammal extends Animal {

    void breathe() {
        System.out.println("Breathing");
    }
}

class Dog extends Mammal {

    void bark() {
        System.out.println("Barking");
    }
}
```

Usage:

```java
Dog dog = new Dog();

dog.eat();
dog.breathe();
dog.bark();
```

The `Dog` object can use accessible inherited behavior from both `Mammal` and `Animal`.

---

# 20. Hierarchical Inheritance

Hierarchical inheritance occurs when multiple child classes inherit from the same parent.

Example:

```text
        Animal
        /    \
       /      \
     Dog      Cat
```

Java:

```java
class Animal {

    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Barking");
    }
}

class Cat extends Animal {

    void meow() {
        System.out.println("Meowing");
    }
}
```

Both `Dog` and `Cat` inherit:

```text
eat()
```

but each has its own behavior:

```text
Dog → bark()
Cat → meow()
```

---

# 21. Multiple Inheritance and Java

Java does not allow multiple inheritance through classes.

This is invalid:

```java
class C extends A, B {
}
```

One reason is that multiple inheritance can create ambiguity.

For example:

```text
       A
      / \
     B   C
      \ /
       D
```

If both `B` and `C` provide the same method, it can become unclear which implementation `D` should inherit.

Java avoids this type of class inheritance ambiguity by allowing a class to extend only one class.

Java can, however, implement multiple interfaces:

```java
class Student implements Person, Learner {
}
```

This is discussed under Java interfaces.

---

# 22. Protected Members

The `protected` access modifier is particularly relevant to inheritance.

Example:

```java
class Person {

    protected String name;
}

class Student extends Person {

    void displayName() {
        System.out.println(name);
    }
}
```

The child class can access the protected member according to Java's `protected` access rules.

However, `protected` should not automatically be used for every field.

For stronger encapsulation, fields are often kept `private`, with protected or public methods used when appropriate.

---

# 23. Private Members and Inheritance

A child class does not have direct access to a parent's `private` fields.

Example:

```java
class Person {

    private String name;

    public String getName() {
        return name;
    }
}

class Student extends Person {

    void display() {

        // name = "Juan"; // Not allowed directly

        System.out.println(getName());
    }
}
```

The `Student` class cannot directly access:

```java
name
```

because it is private to `Person`.

Instead, it can use an accessible method such as:

```java
getName()
```

This demonstrates how inheritance and encapsulation work together.

---

# 24. Inheritance and Encapsulation

Inheritance does not mean that every parent field should be `protected`.

A common design is:

```java
class Person {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

class Student extends Person {

    void display() {
        System.out.println(getName());
    }
}
```

The parent keeps its data private while exposing controlled behavior.

This maintains encapsulation while allowing subclasses to reuse functionality.

---

# 25. Inheritance and Polymorphism

Inheritance is one of the foundations of runtime polymorphism.

Consider:

```java
class Animal {

    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {

    @Override
    void sound() {
        System.out.println("Meow");
    }
}
```

We can write:

```java
Animal animal1 = new Dog();
Animal animal2 = new Cat();

animal1.sound();
animal2.sound();
```

Output:

```text
Bark
Meow
```

The variable type is:

```text
Animal
```

but the actual objects are:

```text
Dog
Cat
```

Java determines the overridden method to execute at runtime.

This is called:

> **Runtime polymorphism**

---

# 26. Real-World Example: Employee System

Inheritance is useful when several classes share common employee information.

```java
class Employee {

    protected String name;
    protected double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public void work() {
        System.out.println(name + " is working.");
    }
}
```

A developer can extend it:

```java
class Developer extends Employee {

    private String programmingLanguage;

    public Developer(
        String name,
        double salary,
        String programmingLanguage
    ) {

        super(name, salary);

        this.programmingLanguage = programmingLanguage;
    }

    public void writeCode() {
        System.out.println(
            name + " is writing " +
            programmingLanguage + " code."
        );
    }
}
```

A manager can also extend it:

```java
class Manager extends Employee {

    public Manager(String name, double salary) {
        super(name, salary);
    }

    public void manageTeam() {
        System.out.println(
            name + " is managing the team."
        );
    }
}
```

Now we have:

```text
             Employee
              /    \
             /      \
      Developer    Manager
```

Common information:

```text
name
salary
work()
```

Specialized behavior:

```text
Developer → writeCode()
Manager   → manageTeam()
```

---

# 27. Real-World Example: School System

Consider:

```text
Person
 /   \
/     \
Student Teacher
```

Parent class:

```java
class Person {

    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

Student:

```java
class Student extends Person {

    private String course;

    public Student(
        String name,
        int age,
        String course
    ) {

        super(name, age);

        this.course = course;
    }

    public void study() {
        System.out.println(
            getName() + " is studying."
        );
    }
}
```

Teacher:

```java
class Teacher extends Person {

    private String subject;

    public Teacher(
        String name,
        int age,
        String subject
    ) {

        super(name, age);

        this.subject = subject;
    }

    public void teach() {
        System.out.println(
            getName() + " is teaching " +
            subject + "."
        );
    }
}
```

Usage:

```java
Student student =
    new Student("Juan", 20, "BSIT");

Teacher teacher =
    new Teacher("Maria", 35, "Java");

student.study();
teacher.teach();
```

Both classes reuse:

```text
name
age
getName()
getAge()
```

from `Person`.

---

# 28. Abstract Classes and Inheritance

Inheritance is commonly used with abstract classes.

An abstract class can define common behavior while requiring subclasses to implement specific behavior.

Example:

```java
abstract class Animal {

    public void eat() {
        System.out.println("Animal is eating.");
    }

    public abstract void sound();
}
```

A child class must provide the abstract method:

```java
class Dog extends Animal {

    @Override
    public void sound() {
        System.out.println("Bark");
    }
}
```

Usage:

```java
Dog dog = new Dog();

dog.eat();
dog.sound();
```

The abstract class provides common functionality, while the child provides specialized behavior.

---

# 29. The `final` Keyword and Inheritance

The `final` keyword can restrict inheritance and overriding.

### Final Class

A final class cannot be extended.

```java
final class SecurityManager {
}
```

This is invalid:

```java
class CustomSecurityManager extends SecurityManager {
}
```

because `SecurityManager` is final.

### Final Method

A final method cannot be overridden.

```java
class Animal {

    public final void breathe() {
        System.out.println("Breathing");
    }
}
```

This is invalid:

```java
class Dog extends Animal {

    @Override
    public void breathe() {
    }
}
```

The parent method is final.

---

# 30. Common Mistakes

## Mistake 1: Using Inheritance for a "Has-A" Relationship

Incorrect design:

```java
class Car extends Engine {
}
```

A car is not an engine.

A car **has an** engine.

Better:

```java
class Engine {
}

class Car {

    private Engine engine;
}
```

---

## Mistake 2: Trying to Extend Multiple Classes

Invalid:

```java
class C extends A, B {
}
```

A Java class can directly extend only one class.

---

## Mistake 3: Forgetting `super()` When the Parent Requires Parameters

Parent:

```java
class Person {

    Person(String name) {
    }
}
```

Child:

```java
class Student extends Person {

    Student() {
    }
}
```

This fails because Java cannot call a non-existent `Person()` constructor.

Correct:

```java
class Student extends Person {

    Student() {
        super("Unknown");
    }
}
```

---

## Mistake 4: Attempting to Access Private Parent Fields Directly

```java
class Person {

    private String name;
}

class Student extends Person {

    void display() {
        // System.out.println(name); // Not allowed
    }
}
```

Use an accessible method:

```java
System.out.println(getName());
```

---

## Mistake 5: Forgetting `@Override`

Instead of:

```java
void sounds() {
}
```

use:

```java
@Override
void sound() {
}
```

The annotation helps the compiler verify that the method actually overrides a parent method.

---

## Mistake 6: Overusing Inheritance

Not every class relationship should use inheritance.

Before using:

```java
extends
```

ask:

> **Is the child truly a type of the parent?**

If not, composition may be more appropriate.

---

# 31. Rules for Implementing Inheritance

Students should remember the following rules when implementing inheritance in Java.

## Rule 1 — Use `extends` for Class Inheritance

Example:

```java
class Dog extends Animal {
}
```

---

## Rule 2 — A Class Can Directly Extend Only One Class

Valid:

```java
class Dog extends Animal {
}
```

Invalid:

```java
class Dog extends Animal, Pet {
}
```

---

## Rule 3 — Inheritance Should Represent an "Is-A" Relationship

Good:

```text
Dog is an Animal
Student is a Person
Manager is an Employee
```

Avoid inheritance when the relationship is:

```text
Car has an Engine
Student has an Address
Order has Products
```

These are usually composition relationships.

---

## Rule 4 — Constructors Are Not Inherited

If a parent has:

```java
Person(String name) {
}
```

the child does not automatically inherit that constructor.

The child must define its own constructor if necessary.

---

## Rule 5 — Use `super()` to Call a Parent Constructor

Example:

```java
Student(String name) {
    super(name);
}
```

---

## Rule 6 — `super()` Must Be the First Statement

Correct:

```java
Student(String name) {

    super(name);

    System.out.println("Created");
}
```

---

## Rule 7 — If No `super()` Is Written, Java Attempts to Call `super()`

Java attempts to insert:

```java
super();
```

at the beginning of the constructor.

Therefore, the parent must have an accessible no-argument constructor unless the child explicitly calls another parent constructor.

---

## Rule 8 — Use `@Override` When Overriding a Parent Method

Example:

```java
@Override
void sound() {
    System.out.println("Bark");
}
```

This makes the intention clear and allows the compiler to detect mistakes.

---

## Rule 9 — A Child Can Add Its Own Fields and Methods

Example:

```java
class Dog extends Animal {

    void bark() {
    }
}
```

---

## Rule 10 — A Child Can Override Inherited Methods

Example:

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

---

## Rule 11 — A `private` Parent Member Cannot Be Accessed Directly by the Child

Example:

```java
class Person {

    private String name;
}
```

The child cannot directly use:

```java
name
```

Instead, the parent can provide an accessible method such as:

```java
getName()
```

---

## Rule 12 — `protected` Members Can Be Accessed by Subclasses According to Java's Access Rules

Example:

```java
class Person {

    protected String name;
}
```

A subclass can access the member within the applicable `protected` access rules.

However, `protected` should be used deliberately rather than simply replacing all `private` fields.

---

## Rule 13 — A `final` Class Cannot Be Extended

Example:

```java
final class Animal {
}
```

This is invalid:

```java
class Dog extends Animal {
}
```

---

## Rule 14 — A `final` Method Cannot Be Overridden

Example:

```java
class Animal {

    final void breathe() {
    }
}
```

The child cannot override `breathe()`.

---

## Rule 15 — Java Supports Multilevel Inheritance

Example:

```text
Animal
   ↓
Mammal
   ↓
Dog
```

This is valid Java.

---

## Rule 16 — Java Supports Hierarchical Inheritance

Example:

```text
       Animal
       /    \
     Dog    Cat
```

Multiple classes can extend the same parent class.

---

## Rule 17 — Java Does Not Support Multiple Inheritance Through Classes

This is invalid:

```java
class C extends A, B {
}
```

Multiple inheritance of type can be achieved through interfaces:

```java
class C implements A, B {
}
```

when `A` and `B` are interfaces.

---

## Rule 18 — A Child Class Can Be Used Where Its Parent Type Is Expected

Example:

```java
Animal animal = new Dog();
```

This is called **upcasting**.

It is useful for polymorphism.

---

## Rule 19 — Overridden Instance Methods Are Resolved at Runtime

Example:

```java
Animal animal = new Dog();

animal.sound();
```

If `Dog` overrides `sound()`, the `Dog` implementation is executed.

---

## Rule 20 — Do Not Use Inheritance Just to Reuse a Small Amount of Code

Inheritance creates a strong relationship between classes.

If the relationship is not genuinely an "is-a" relationship, consider composition instead.

---

# 32. Inheritance and Access Modifiers

Consider:

```java
class Parent {

    private int privateValue;
    int defaultValue;
    protected int protectedValue;
    public int publicValue;
}
```

A child class's access to these members depends on Java's access rules.

A simplified view is:

| Modifier | Directly Accessible by Child? |
|---|---:|
| `private` | No |
| package-private/default | Yes, when in the same package |
| `protected` | Yes, subject to protected access rules |
| `public` | Yes |

Important:

> Inheritance does not make private members directly accessible to the child.

The parent still controls its private data.

---

# 33. Inheritance and Polymorphism Example

Consider:

```java
class Employee {

    void work() {
        System.out.println("Employee is working.");
    }
}

class Developer extends Employee {

    @Override
    void work() {
        System.out.println("Developer is writing code.");
    }
}

class Designer extends Employee {

    @Override
    void work() {
        System.out.println("Designer is creating designs.");
    }
}
```

We can write:

```java
Employee employee1 = new Developer();
Employee employee2 = new Designer();

employee1.work();
employee2.work();
```

Output:

```text
Developer is writing code.
Designer is creating designs.
```

The common type is:

```java
Employee
```

but each object provides its own implementation.

This allows code to work with the parent type while supporting specialized child behavior.

---

# 34. Complete Example: School Management System

```java
class Person {

    private String name;
    private int age;

    public Person(String name, int age) {

        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public void introduce() {
        System.out.println(
            "My name is " + name +
            " and I am " + age + " years old."
        );
    }
}
```

Student:

```java
class Student extends Person {

    private String course;

    public Student(
        String name,
        int age,
        String course
    ) {

        super(name, age);

        this.course = course;
    }

    public void study() {
        System.out.println(
            getName() +
            " is studying " +
            course + "."
        );
    }

    @Override
    public void introduce() {

        System.out.println(
            "I am " + getName() +
            ", a " + course +
            " student."
        );
    }
}
```

Teacher:

```java
class Teacher extends Person {

    private String subject;

    public Teacher(
        String name,
        int age,
        String subject
    ) {

        super(name, age);

        this.subject = subject;
    }

    public void teach() {
        System.out.println(
            getName() +
            " is teaching " +
            subject + "."
        );
    }

    @Override
    public void introduce() {

        System.out.println(
            "I am " + getName() +
            ", and I teach " +
            subject + "."
        );
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        Student student =
            new Student(
                "Juan",
                20,
                "BSIT"
            );

        Teacher teacher =
            new Teacher(
                "Maria",
                35,
                "Java"
            );

        student.introduce();
        student.study();

        teacher.introduce();
        teacher.teach();
    }
}
```

The class relationship is:

```text
             Person
             /    \
            /      \
       Student    Teacher
```

Common functionality:

```text
name
age
getName()
getAge()
introduce()
```

Specialized functionality:

```text
Student → study()
Teacher → teach()
```

---

# 35. Inheritance Checklist

Before using inheritance, ask:

```text
□ Is there a genuine "is-a" relationship?
□ Is there common functionality that belongs in the parent?
□ Should the child specialize the parent?
□ Does the child need to override any behavior?
□ Does the child constructor need to call super(...)?
□ Are parent fields appropriately encapsulated?
□ Should fields remain private?
□ Are overridden methods marked with @Override?
□ Is inheritance actually better than composition?
□ Am I avoiding unnecessary class hierarchy complexity?
```

---

# 36. Quick Comparison of OOP Concepts

Inheritance is related to several other OOP concepts.

| Concept | Main Purpose |
|---|---|
| Encapsulation | Protect and control object data |
| Inheritance | Reuse and extend class behavior |
| Polymorphism | Allow one interface/type to represent different implementations |
| Abstraction | Hide unnecessary implementation details |

A simple way to remember them:

```text
Encapsulation
    ↓
Protect the data

Inheritance
    ↓
Reuse and extend

Polymorphism
    ↓
Different behavior through a common type

Abstraction
    ↓
Hide unnecessary details
```

---

# 37. Key Takeaways

Inheritance allows a class to acquire and reuse accessible behavior from another class.

The basic syntax is:

```java
class Child extends Parent {
}
```

The relationship is:

```text
Parent
   ↓
Child
```

The child can:

- Use inherited accessible members
- Add new fields
- Add new methods
- Override inherited methods
- Call parent constructors using `super()`
- Call parent methods using `super.methodName()`

The parent can provide common functionality while the child provides specialized behavior.

## Most Important Rules

1. Use `extends` for class inheritance.
2. A class can directly extend only one class.
3. Inheritance should represent an **is-a relationship**.
4. Constructors are not inherited.
5. Use `super()` or `super(parameters)` to call a parent constructor.
6. `super()` must be the first statement in a constructor.
7. If no `super()` is written, Java attempts to call `super()`.
8. Use `@Override` when overriding methods.
9. A child can add its own fields and methods.
10. A child can override inherited methods when Java's overriding rules permit it.
11. Private parent fields cannot be directly accessed by the child.
12. `protected` provides subclass access according to Java's access rules.
13. A `final` class cannot be extended.
14. A `final` method cannot be overridden.
15. Java supports single, multilevel, and hierarchical class inheritance.
16. Java does not support multiple inheritance through classes.
17. A child object can be assigned to a parent reference.
18. Overridden instance methods support runtime polymorphism.
19. Use inheritance for genuine "is-a" relationships.
20. Consider composition for "has-a" relationships.

## Final Reminder

> **Inheritance = Create a new class based on an existing class so that common functionality can be reused and specialized.**

The basic pattern is:

```java
class Parent {

    void commonBehavior() {
        System.out.println("Common behavior");
    }
}

class Child extends Parent {

    void specializedBehavior() {
        System.out.println("Specialized behavior");
    }
}
```

Usage:

```java
Child child = new Child();

child.commonBehavior();
child.specializedBehavior();
```

The key idea is:

```text
PARENT CLASS
     ↓
COMMON FEATURES
     ↓
CHILD CLASS
     ↓
SPECIALIZED FEATURES
```

Use inheritance when the child truly **is a type of** the parent.
