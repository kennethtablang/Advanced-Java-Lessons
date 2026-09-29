# Constructors in Java

## 1. What is a Constructor?

A **constructor** is a special member of a Java class that is used to **initialize an object when the object is created**.

A constructor looks similar to a method, but there are important differences:

- A constructor has the **same name as the class**.
- A constructor **does not have a return type**, not even `void`.
- A constructor is automatically called when an object is created using `new`.
- Constructors are commonly used to assign initial values to an object's fields.

### Basic Example

```java
class Student {

    String name;
    int age;

    Student() {
        name = "Unknown";
        age = 0;
    }
}
```

When an object is created:

```java
Student student = new Student();
```

Java automatically calls:

```java
Student()
```

The object is initialized with:

```text
name = "Unknown"
age = 0
```

---

# 2. Why Do We Use Constructors?

Constructors are useful because they allow an object to start in a valid and predictable state.

Without a constructor:

```java
class Student {

    String name;
    int age;
}
```

We might have to initialize the object manually:

```java
Student student = new Student();

student.name = "Juan";
student.age = 20;
```

With a constructor:

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

We can initialize the object immediately:

```java
Student student = new Student("Juan", 20);
```

This makes object creation more organized and can help ensure that required information is supplied.

---

# 3. Basic Constructor Syntax

The general syntax is:

```java
ClassName(parameters) {
    // initialization statements
}
```

Example:

```java
class Car {

    String brand;
    String model;

    Car(String brand, String model) {
        this.brand = brand;
        this.model = model;
    }
}
```

Creating an object:

```java
Car car = new Car("Toyota", "Vios");
```

The constructor receives:

```text
brand = "Toyota"
model = "Vios"
```

and assigns those values to the object's fields.

---

# 4. Constructor vs. Method

Constructors and methods may look similar, but they have different purposes.

| Constructor | Method |
|---|---|
| Initializes an object | Performs an operation |
| Same name as the class | Can have any valid name |
| No return type | Has a return type or `void` |
| Automatically called when an object is created | Usually called explicitly |
| Cannot be inherited | Methods can be inherited |
| Used during object creation | Used after or during object use |

### Constructor

```java
class Student {

    Student(String name) {
        System.out.println("Student created: " + name);
    }
}
```

### Method

```java
class Student {

    void displayName(String name) {
        System.out.println("Student: " + name);
    }
}
```

Creating the object:

```java
Student student = new Student("Juan");
```

Calling the method:

```java
student.displayName("Juan");
```

---

# 5. Types of Constructors

There are two commonly discussed categories of constructors:

1. **No-argument constructor**
2. **Parameterized constructor**

Constructors can also be overloaded, allowing a class to have multiple constructors with different parameter lists.

---

# 6. No-Argument Constructor

A **no-argument constructor** is a constructor that does not accept parameters.

```java
class Student {

    String name;
    int age;

    Student() {
        name = "Unknown";
        age = 0;
    }
}
```

Usage:

```java
Student student = new Student();

System.out.println(student.name);
System.out.println(student.age);
```

Output:

```text
Unknown
0
```

A no-argument constructor is useful when an object can start with default or predefined values.

---

# 7. Parameterized Constructor

A **parameterized constructor** accepts values when an object is created.

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Usage:

```java
Student student = new Student("Juan", 20);

System.out.println(student.name);
System.out.println(student.age);
```

Output:

```text
Juan
20
```

The values supplied during object creation are passed to the constructor.

---

# 8. Understanding `this`

The keyword `this` refers to the **current object**.

It is commonly used when constructor parameters have the same names as instance variables.

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Here:

```java
this.name
```

refers to the object's instance variable.

While:

```java
name
```

refers to the constructor parameter.

Therefore:

```java
this.name = name;
```

means:

```text
current object's name = parameter name
```

Similarly:

```java
this.age = age;
```

means:

```text
current object's age = parameter age
```

---

# 9. Constructor Overloading

A class can have multiple constructors as long as their parameter lists are different.

This is called **constructor overloading**.

```java
class Student {

    String name;
    int age;
    String course;

    Student() {
        name = "Unknown";
        age = 0;
        course = "Undeclared";
    }

    Student(String name) {
        this.name = name;
        age = 0;
        course = "Undeclared";
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
        course = "Undeclared";
    }

    Student(String name, int age, String course) {
        this.name = name;
        this.age = age;
        this.course = course;
    }
}
```

We can create objects in different ways:

```java
Student s1 = new Student();

Student s2 = new Student("Juan");

Student s3 = new Student("Maria", 20);

Student s4 = new Student("Pedro", 21, "BSIT");
```

Java determines which constructor to execute based on the arguments supplied.

---

# 10. Constructor Chaining with `this()`

One constructor can call another constructor in the same class using:

```java
this();
```

or:

```java
this(parameters);
```

Example:

```java
class Student {

    String name;
    int age;
    String course;

    Student() {
        this("Unknown", 0, "Undeclared");
    }

    Student(String name, int age, String course) {
        this.name = name;
        this.age = age;
        this.course = course;
    }
}
```

Now:

```java
Student student = new Student();
```

calls:

```java
Student()
```

which then calls:

```java
Student("Unknown", 0, "Undeclared")
```

This technique is called **constructor chaining**.

It helps prevent duplicate initialization code.

---

# 11. Important Rule About `this()`

When using `this()` to call another constructor, it must be the **first statement** inside the constructor.

Correct:

```java
Student() {
    this("Unknown", 0);
}
```

Incorrect:

```java
Student() {
    System.out.println("Creating student");
    this("Unknown", 0);
}
```

The second example produces a compilation error because `this()` is not the first statement.

---

# 12. Calling a Parent Constructor with `super()`

When inheritance is involved, a child class can call a constructor of its parent class using:

```java
super();
```

or:

```java
super(parameters);
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

When we create:

```java
Student student = new Student("Juan", "BSIT");
```

the child constructor calls:

```java
super(name);
```

which executes the parent constructor:

```java
Person(String name)
```

---

# 13. Important Rule About `super()`

Like `this()`, a `super()` constructor call must appear as the **first statement** of a constructor.

Correct:

```java
Student(String name) {
    super(name);
    System.out.println("Student created");
}
```

Incorrect:

```java
Student(String name) {
    System.out.println("Creating student");
    super(name);
}
```

The second version produces a compilation error.

---

# 14. What Happens If We Do Not Write a Constructor?

If a class has **no constructor declared**, Java can provide a default constructor automatically.

Example:

```java
class Student {

    String name;
    int age;
}
```

Java effectively provides a no-argument constructor similar to:

```java
Student() {
    super();
}
```

Therefore:

```java
Student student = new Student();
```

is valid.

The fields receive Java's default field values:

```text
String → null
int    → 0
double → 0.0
boolean → false
```

---

# 15. Important Rule: The Default Constructor Is Not Always Automatically Available

Consider:

```java
class Student {

    String name;

    Student(String name) {
        this.name = name;
    }
}
```

Now this is valid:

```java
Student student = new Student("Juan");
```

But this is not:

```java
Student student = new Student();
```

Why?

Because once we declare a constructor ourselves, Java does **not** automatically provide a no-argument constructor.

If we want both, we must declare both:

```java
class Student {

    String name;

    Student() {
        name = "Unknown";
    }

    Student(String name) {
        this.name = name;
    }
}
```

---

# 16. Constructor Access Modifiers

Constructors can use access modifiers such as:

- `public`
- `protected`
- `private`
- package-private/default

### Public Constructor

```java
public Student() {
}
```

A public constructor can be accessed from other classes, subject to normal Java access rules.

### Private Constructor

```java
class Database {

    private Database() {
    }
}
```

A private constructor prevents other classes from directly creating objects using `new Database()`.

Private constructors are commonly used in certain design patterns and utility classes.

---

# 17. Real-World Example: Bank Account

Constructors can ensure that a bank account starts with required information.

```java
class BankAccount {

    String accountNumber;
    String owner;
    double balance;

    BankAccount(String accountNumber, String owner) {
        this.accountNumber = accountNumber;
        this.owner = owner;
        this.balance = 0;
    }

    BankAccount(String accountNumber, String owner, double initialBalance) {
        this.accountNumber = accountNumber;
        this.owner = owner;
        this.balance = initialBalance;
    }
}
```

Usage:

```java
BankAccount account1 =
    new BankAccount("ACC001", "Juan");

BankAccount account2 =
    new BankAccount("ACC002", "Maria", 5000);
```

The first account starts with:

```text
Balance = 0
```

The second account starts with:

```text
Balance = 5000
```

---

# 18. Real-World Example: Product

Constructors are also useful in inventory or POS systems.

```java
class Product {

    int productId;
    String name;
    double price;

    Product(int productId, String name, double price) {
        this.productId = productId;
        this.name = name;
        this.price = price;
    }

    void displayProduct() {
        System.out.println("ID: " + productId);
        System.out.println("Name: " + name);
        System.out.println("Price: " + price);
    }
}
```

Creating an object:

```java
Product product =
    new Product(101, "Keyboard", 1500.00);

product.displayProduct();
```

Output:

```text
ID: 101
Name: Keyboard
Price: 1500.0
```

The constructor ensures that the product receives its initial information when it is created.

---

# 19. Real-World Example: Student Information System

```java
class Student {

    int studentId;
    String name;
    String course;
    int yearLevel;

    Student(int studentId, String name, String course, int yearLevel) {
        this.studentId = studentId;
        this.name = name;
        this.course = course;
        this.yearLevel = yearLevel;
    }

    void displayInfo() {
        System.out.println("Student ID: " + studentId);
        System.out.println("Name: " + name);
        System.out.println("Course: " + course);
        System.out.println("Year Level: " + yearLevel);
    }
}
```

Usage:

```java
Student student =
    new Student(1001, "Juan Dela Cruz", "BSIT", 2);

student.displayInfo();
```

Instead of creating an empty student and assigning every field separately, the constructor initializes the object immediately.

---

# 20. Constructor Rules

Students should remember the following rules when implementing constructors.

### Rule 1 — Constructor Name Must Match the Class Name

Correct:

```java
class Student {

    Student() {
    }
}
```

Incorrect:

```java
class Student {

    Person() {
    }
}
```

`Person()` is not a constructor for `Student`.

---

### Rule 2 — A Constructor Has No Return Type

Correct:

```java
Student() {
}
```

Incorrect:

```java
void Student() {
}
```

The second example is actually a method, not a constructor.

---

### Rule 3 — Constructors Are Called When Objects Are Created

```java
Student student = new Student();
```

The `new` keyword creates the object and invokes the appropriate constructor.

---

### Rule 4 — Constructors Can Have Parameters

```java
Student(String name, int age) {
    this.name = name;
    this.age = age;
}
```

---

### Rule 5 — Constructors Can Be Overloaded

```java
Student() {
}

Student(String name) {
}

Student(String name, int age) {
}
```

The parameter lists must be different.

---

### Rule 6 — Constructors Cannot Be Overloaded by Return Type

Constructors do not have return types.

Therefore, this is invalid:

```java
Student() {
}

void Student() {
}
```

The second declaration is a method, not a constructor.

---

### Rule 7 — `this()` Calls Another Constructor in the Same Class

```java
Student() {
    this("Unknown");
}

Student(String name) {
    this.name = name;
}
```

---

### Rule 8 — `this()` Must Be the First Statement

Correct:

```java
Student() {
    this("Unknown");
}
```

Incorrect:

```java
Student() {
    System.out.println("Creating student");
    this("Unknown");
}
```

---

### Rule 9 — `super()` Calls a Parent Constructor

```java
Student(String name) {
    super(name);
}
```

---

### Rule 10 — `super()` Must Be the First Statement

Correct:

```java
Student(String name) {
    super(name);
    this.name = name;
}
```

---

### Rule 11 — A Constructor Cannot Be `static`

Constructors belong to object creation and are associated with instances. They cannot be declared as:

```java
static Student() {
}
```

This is invalid Java syntax.

---

### Rule 12 — A Constructor Cannot Be `abstract`

Constructors cannot be abstract because constructors must contain an implementation for initializing an object.

---

### Rule 13 — A Constructor Cannot Be Overridden

Constructors are not inherited by subclasses, so they cannot be overridden.

A child class can define its own constructors and can invoke a parent constructor using `super()`.

---

### Rule 14 — If No Constructor Is Declared, Java Provides a Default Constructor

For example:

```java
class Student {
}
```

allows:

```java
Student student = new Student();
```

However, this automatic constructor is only provided when the class does not declare any constructor.

---

### Rule 15 — Declaring a Constructor Removes the Automatically Provided Default Constructor

If we declare:

```java
Student(String name) {
    this.name = name;
}
```

then:

```java
new Student();
```

is no longer automatically available.

If we need it, we must explicitly declare:

```java
Student() {
}
```

---

### Rule 16 — Constructors Can Have Access Modifiers

Examples:

```java
public Student() {
}

protected Student() {
}

private Student() {
}
```

The access modifier controls where the constructor can be accessed.

---

### Rule 17 — Constructors Are Not Inherited

If:

```java
class Person {

    Person(String name) {
    }
}
```

and:

```java
class Student extends Person {
}
```

the `Student` class does not inherit the `Person(String)` constructor.

The child class must define an appropriate constructor and call the parent constructor when necessary.

---

### Rule 18 — Use Constructors to Establish a Valid Initial State

A constructor should initialize the fields necessary for the object to operate correctly.

For example:

```java
class Product {

    int productId;
    String name;
    double price;

    Product(int productId, String name, double price) {
        this.productId = productId;
        this.name = name;
        this.price = price;
    }
}
```

This ensures that a `Product` object is created with its basic information.

---

# 21. Constructor vs. Default Values

Java automatically assigns default values to instance variables when an object is created.

| Data Type | Default Value |
|---|---|
| `byte` | `0` |
| `short` | `0` |
| `int` | `0` |
| `long` | `0L` |
| `float` | `0.0f` |
| `double` | `0.0d` |
| `char` | `'\u0000'` |
| `boolean` | `false` |
| Reference types | `null` |

Example:

```java
class Student {

    String name;
    int age;
    boolean enrolled;
}
```

Creating:

```java
Student student = new Student();
```

initially gives:

```text
name     = null
age      = 0
enrolled = false
```

A constructor allows us to replace these default values with meaningful initial values.

---

# 22. Constructor Chaining Example

Constructor chaining can reduce duplicate code.

Without constructor chaining:

```java
class Student {

    String name;
    int age;
    String course;

    Student() {
        name = "Unknown";
        age = 0;
        course = "Undeclared";
    }

    Student(String name) {
        this.name = name;
        age = 0;
        course = "Undeclared";
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
        course = "Undeclared";
    }
}
```

There is repeated initialization code.

With constructor chaining:

```java
class Student {

    String name;
    int age;
    String course;

    Student() {
        this("Unknown", 0, "Undeclared");
    }

    Student(String name) {
        this(name, 0, "Undeclared");
    }

    Student(String name, int age) {
        this(name, age, "Undeclared");
    }

    Student(String name, int age, String course) {
        this.name = name;
        this.age = age;
        this.course = course;
    }
}
```

Now the initialization logic is centralized in one constructor.

---

# 23. Common Mistakes

## Mistake 1: Adding `void`

Incorrect:

```java
void Student() {
}
```

This is a method.

Correct:

```java
Student() {
}
```

---

## Mistake 2: Using the Wrong Name

Incorrect:

```java
class Student {

    student() {
    }
}
```

Java is case-sensitive.

Correct:

```java
class Student {

    Student() {
    }
}
```

---

## Mistake 3: Expecting a Default Constructor After Creating a Parameterized Constructor

```java
class Student {

    Student(String name) {
    }
}
```

This does not automatically provide:

```java
Student()
```

Therefore:

```java
new Student();
```

causes a compilation error.

---

## Mistake 4: Putting `this()` After Another Statement

Incorrect:

```java
Student() {
    System.out.println("Hello");
    this("Unknown");
}
```

Correct:

```java
Student() {
    this("Unknown");
}
```

---

## Mistake 5: Putting `super()` After Another Statement

Incorrect:

```java
Student(String name) {
    System.out.println("Creating student");
    super(name);
}
```

Correct:

```java
Student(String name) {
    super(name);
    System.out.println("Creating student");
}
```

---

# 24. Constructor vs. Method: Quick Comparison

| Feature | Constructor | Method |
|---|---|---|
| Purpose | Initializes object | Performs an operation |
| Name | Same as class | Any valid identifier |
| Return type | None | Required, including `void` |
| Invocation | During object creation | Explicit method call |
| Can be overloaded? | Yes | Yes |
| Can be overridden? | No | Yes |
| Can be `static`? | No | Yes |
| Can be `abstract`? | No | Yes |
| Inherited? | No | Methods may be inherited |
| Uses `this()`? | Yes | No, `this()` is for constructor chaining |
| Uses `super()`? | Yes | `super()` constructor calls are only made from constructors |

---

# 25. Key Takeaways

A constructor is a special part of a Java class used to **initialize an object when it is created**.

Remember these core ideas:

```text
Constructor
    ↓
Same name as class
    ↓
No return type
    ↓
Called when object is created
    ↓
Initializes object fields
```

### Most Important Rules

1. Constructor name must match the class name.
2. Constructors do not have return types.
3. Constructors are called when an object is created.
4. Constructors can accept parameters.
5. Constructors can be overloaded.
6. `this()` calls another constructor in the same class.
7. `this()` must be the first statement.
8. `super()` calls a parent constructor.
9. `super()` must be the first statement.
10. Constructors cannot be `static`.
11. Constructors cannot be `abstract`.
12. Constructors cannot be overridden.
13. Constructors are not inherited.
14. If no constructor is declared, Java provides a default no-argument constructor.
15. Once a constructor is explicitly declared, the automatic default constructor is no longer provided.
16. Constructors can use access modifiers.
17. Constructors should be used to establish a valid initial state for an object.

## Final Reminder

> **Constructor = A special class member used to initialize an object when it is created.**

The simplest pattern to remember is:

```java
class ClassName {

    ClassName(parameters) {
        // initialize object
    }
}
```

Example:

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Creating the object:

```java
Student student = new Student("Juan", 20);
```

The constructor runs automatically and initializes the new `Student` object.
