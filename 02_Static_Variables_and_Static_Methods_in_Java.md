# Static Variables and Static Methods in Java

## 1. Introduction to `static`

In Java, the keyword `static` means that a member belongs to the **class
itself**, rather than to a particular object.

Normally, when we create objects:

``` java
Student student1 = new Student();
Student student2 = new Student();
```

each object can have its own copy of instance variables.

When a variable or method is declared `static`, it is associated with
the **class**.

### Simple Comparison

  Type                Belongs to   Accessed Through
  ------------------- ------------ ------------------
  Instance variable   Object       Object
  Instance method     Object       Object
  Static variable     Class        Class
  Static method       Class        Class

Example:

``` java
class Student {
    String name;                         // Instance variable
    static String school = "ABC University"; // Static variable
}
```

Every `Student` object has its own `name`, but all students share the
same `school`.

------------------------------------------------------------------------

# 2. Static Variables

A **static variable** is a variable that is shared by all objects of a
class.

It is also called a **class variable** or **static field**.

Example:

``` java
class Student {
    String name;
    static String school = "ABC University";
}
```

Create several students:

``` java
Student student1 = new Student();
Student student2 = new Student();

student1.name = "Juan";
student2.name = "Maria";
```

The `name` values are different:

``` text
student1.name → Juan
student2.name → Maria
```

But the `school` variable is shared:

``` text
Student.school → ABC University
```

We can change it through the class:

``` java
Student.school = "XYZ University";
```

Now:

``` java
System.out.println(student1.school);
System.out.println(student2.school);
```

Both display:

``` text
XYZ University
XYZ University
```

### Why?

There is only **one shared copy** of the static variable.

------------------------------------------------------------------------

# 3. Static Variable Example: Counting Objects

One of the most useful applications of a static variable is counting how
many objects have been created.

``` java
class Student {
    String name;
    static int studentCount = 0;

    Student(String name) {
        this.name = name;
        studentCount++;
    }
}
```

Then:

``` java
Student s1 = new Student("Juan");
Student s2 = new Student("Maria");
Student s3 = new Student("Pedro");

System.out.println(Student.studentCount);
```

Output:

``` text
3
```

### What happens?

When the first object is created:

``` text
studentCount: 0 → 1
```

When the second object is created:

``` text
studentCount: 1 → 2
```

When the third object is created:

``` text
studentCount: 2 → 3
```

Because `studentCount` is static, **all Student objects use the same
variable**.

------------------------------------------------------------------------

# 4. Static Methods

A **static method** is a method that belongs to the class instead of an
individual object.

Example:

``` java
class Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

We do not need to create a `Calculator` object.

We can simply write:

``` java
int result = Calculator.add(10, 20);

System.out.println(result);
```

Output:

``` text
30
```

The normal pattern is:

``` text
ClassName.methodName()
```

For example:

``` java
Calculator.add(10, 20);
```

------------------------------------------------------------------------

# 5. Why Are Static Methods Useful?

Static methods are useful when an operation **does not depend on a
specific object's data**.

Example:

``` java
class Calculator {

    static int add(int a, int b) {
        return a + b;
    }

    static int subtract(int a, int b) {
        return a - b;
    }

    static int multiply(int a, int b) {
        return a * b;
    }
}
```

We can use:

``` java
System.out.println(Calculator.add(10, 5));
System.out.println(Calculator.subtract(10, 5));
System.out.println(Calculator.multiply(10, 5));
```

Output:

``` text
15
5
50
```

There is no reason to create a `Calculator` object because the
calculations do not depend on a particular calculator object's state.

------------------------------------------------------------------------

# 6. `Math` Class: A Real Java Example

Java already provides many static methods through the `Math` class.

For example:

``` java
double result = Math.sqrt(25);

System.out.println(result);
```

Output:

``` text
5.0
```

Other examples:

``` java
Math.pow(2, 3);
Math.abs(-10);
Math.max(10, 20);
Math.min(10, 20);
```

The pattern is:

``` text
ClassName.methodName()
```

For example:

``` java
Math.sqrt(25);
```

------------------------------------------------------------------------

# 7. `static` and the `main()` Method

One of the most important examples is the Java `main()` method:

``` java
public static void main(String[] args)
```

Why is `main()` static?

Java needs to execute the `main()` method as the entry point of the
program without first requiring an object of that class.

Example:

``` java
class Demo {

    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
```

The program can begin execution through the static `main()` method.

------------------------------------------------------------------------

# 8. Static vs. Instance Members

This distinction is extremely important.

Consider:

``` java
class Student {

    String name;
    static String school = "ABC University";

    void displayStudent() {
        System.out.println(name);
        System.out.println(school);
    }
}
```

Here:

``` java
String name;
```

is an **instance variable**.

Every student has a different name.

``` java
static String school;
```

is a **static variable**.

All students share the same school.

We can visualize it like this:

``` text
Student Class
│
├── static school
│       │
│       └── "ABC University"
│
├── Student Object 1
│       └── name = "Juan"
│
├── Student Object 2
│       └── name = "Maria"
│
└── Student Object 3
        └── name = "Pedro"
```

There is **one `school`**, but each object has its **own `name`**.

------------------------------------------------------------------------

# 9. Important Rule: Static Methods Cannot Directly Access Instance Variables

Consider:

``` java
class Student {

    String name;

    static void displayName() {
        System.out.println(name);
    }
}
```

This produces an error.

### Why?

The variable `name` belongs to an **object**, while `displayName()`
belongs to the **class**.

Java does not know which object's `name` should be accessed.

For example, there could be:

``` text
Juan
Maria
Pedro
```

Which one should the static method display?

There is no specific object.

### Correct Approach

Pass the value to the method:

``` java
class Student {

    static void displayName(String name) {
        System.out.println(name);
    }
}
```

Then:

``` java
Student.displayName("Juan");
```

------------------------------------------------------------------------

# 10. Static Methods Can Access Static Variables

This is allowed:

``` java
class Student {

    static int studentCount = 0;

    static void displayCount() {
        System.out.println(studentCount);
    }
}
```

We can call:

``` java
Student.displayCount();
```

Why does this work?

Both members belong to the class:

``` text
Student
│
├── static studentCount
│
└── static displayCount()
```

------------------------------------------------------------------------

# 11. Static Methods Can Call Other Static Methods

For example:

``` java
class Calculator {

    static int add(int a, int b) {
        return a + b;
    }

    static int calculate() {
        return add(10, 20);
    }
}
```

Then:

``` java
System.out.println(Calculator.calculate());
```

Output:

``` text
30
```

Since both methods are static, they can directly interact.

------------------------------------------------------------------------

# 12. Can an Instance Method Access Static Members?

**Yes.**

An instance method can access static variables and static methods.

Example:

``` java
class Student {

    static String school = "ABC University";
    String name;

    void display() {
        System.out.println(name);
        System.out.println(school);
    }
}
```

This is valid.

The instance method `display()` can access:

``` text
name   → instance variable
school → static variable
```

------------------------------------------------------------------------

# 13. Static Constants

Static variables are commonly combined with `final` to create constants.

Example:

``` java
class Constants {

    static final double PI = 3.14159;
    static final int MAX_STUDENTS = 50;
}
```

We can access them using:

``` java
System.out.println(Constants.PI);
System.out.println(Constants.MAX_STUDENTS);
```

The combination:

``` java
static final
```

means that the value is:

-   shared by the class
-   cannot be changed after initialization

This is a common Java pattern.

------------------------------------------------------------------------

# 14. Common Real-World Uses of `static`

## 1. Shared Information

``` java
static String schoolName;
```

Useful when every object belongs to the same school.

## 2. Object Counters

``` java
static int count;
```

Useful for tracking how many objects have been created.

## 3. Utility Methods

``` java
Math.sqrt(25);
```

Useful for operations that do not depend on object-specific data.

## 4. Constants

``` java
static final double PI = 3.14159;
```

Useful for values that should be shared throughout a program and should
not change.

## 5. Program Entry Point

``` java
public static void main(String[] args)
```

Used as the starting point of a Java application.

------------------------------------------------------------------------

# 15. Rules for Implementing Static Variables

## Rule 1 --- Declare the Variable Using `static`

``` java
static int count;
```

------------------------------------------------------------------------

## Rule 2 --- A Static Variable Belongs to the Class

It is associated with the class rather than with a specific object.

Preferred access:

``` java
Student.school
```

------------------------------------------------------------------------

## Rule 3 --- Static Variables Are Shared

If 100 `Student` objects exist, they still share the same:

``` java
static String school;
```

------------------------------------------------------------------------

## Rule 4 --- Prefer Accessing Static Variables Through the Class Name

Preferred:

``` java
Student.school
```

Although Java permits accessing a static member through an object:

``` java
student1.school
```

this is discouraged because it may incorrectly suggest that the variable
belongs to `student1`.

------------------------------------------------------------------------

## Rule 5 --- Use Static Variables for Shared Data

Good example:

``` java
static String schoolName;
```

Poor example:

``` java
static String studentName;
```

if every student needs a different name.

------------------------------------------------------------------------

# 16. Rules for Implementing Static Methods

## Rule 1 --- Declare the Method Using `static`

``` java
static void display() {
}
```

------------------------------------------------------------------------

## Rule 2 --- Call Static Methods Using the Class Name

``` java
Student.display();
```

------------------------------------------------------------------------

## Rule 3 --- Static Methods Can Directly Access Static Variables

Example:

``` java
static int count;

static void displayCount() {
    System.out.println(count);
}
```

------------------------------------------------------------------------

## Rule 4 --- Static Methods Cannot Directly Access Instance Variables

This is invalid:

``` java
class Student {

    String name;

    static void display() {
        System.out.println(name); // Error
    }
}
```

The reason is that `name` belongs to an object.

------------------------------------------------------------------------

## Rule 5 --- Static Methods Cannot Directly Call Instance Methods

This is invalid:

``` java
class Student {

    void displayName() {
        System.out.println("Student");
    }

    static void test() {
        displayName(); // Error
    }
}
```

The static method does not have a specific object through which it can
call the instance method.

------------------------------------------------------------------------

## Rule 6 --- Instance Methods Can Access Static Members

This is valid:

``` java
class Student {

    static int count;

    void display() {
        System.out.println(count);
    }
}
```

------------------------------------------------------------------------

## Rule 7 --- Static Methods Cannot Use `this`

This is invalid:

``` java
static void display() {
    System.out.println(this);
}
```

Why?

Because `this` refers to the **current object**, while a static method
does not belong to a particular object.

------------------------------------------------------------------------

## Rule 8 --- Static Methods Can Receive Objects as Parameters

Example:

``` java
class Student {

    String name;

    static void displayStudent(Student student) {
        System.out.println(student.name);
    }
}
```

Usage:

``` java
Student s1 = new Student();
s1.name = "Juan";

Student.displayStudent(s1);
```

The static method can access instance data because we explicitly
provided the object.

------------------------------------------------------------------------

# 17. Complete Example

The following example combines instance variables, static variables,
instance methods, static methods, and a constructor:

``` java
class Student {

    // Instance variable
    String name;

    // Static variable shared by all Student objects
    static String school = "ABC University";

    // Static variable used as a counter
    static int studentCount = 0;

    // Constructor
    Student(String name) {
        this.name = name;
        studentCount++;
    }

    // Instance method
    void displayStudent() {
        System.out.println("Name: " + name);
        System.out.println("School: " + school);
    }

    // Static method
    static void displayStudentCount() {
        System.out.println("Number of students: " + studentCount);
    }
}
```

Main class:

``` java
public class Main {

    public static void main(String[] args) {

        Student s1 = new Student("Juan");
        Student s2 = new Student("Maria");
        Student s3 = new Student("Pedro");

        s1.displayStudent();
        s2.displayStudent();
        s3.displayStudent();

        Student.displayStudentCount();
    }
}
```

Output:

``` text
Name: Juan
School: ABC University

Name: Maria
School: ABC University

Name: Pedro
School: ABC University

Number of students: 3
```

------------------------------------------------------------------------

# 18. Visualizing the Complete Example

``` text
                    Student Class
                         │
             ┌───────────┴───────────┐
             │                       │
       Static Members          Instance Members
             │                       │
     ┌───────┴────────┐        ┌─────┴─────┐
     │                │        │           │
 school          studentCount  name    displayStudent()
     │                │
     └────────────────┘
       Shared by all
          objects


        Student Objects

    s1                    s2                    s3
    │                     │                     │
 name = Juan          name = Maria         name = Pedro
    │                     │                     │
    └─────────────────────┴─────────────────────┘
                 Same static school
```

The key idea is:

``` text
Static member
    ↓
Belongs to the class
    ↓
Shared by objects

Instance member
    ↓
Belongs to an object
    ↓
Each object has its own copy
```

------------------------------------------------------------------------

# 19. Quick Decision Guide

When deciding whether to use `static`, ask these questions.

### Question 1

**Does this data belong to one specific object?**

If **yes**, use an instance variable:

``` java
String name;
```

If **no**, and the data should be shared, use a static variable:

``` java
static String school;
```

------------------------------------------------------------------------

### Question 2

**Does this operation need information from a particular object?**

If **yes**, use an instance method:

``` java
void displayName() {
    System.out.println(name);
}
```

If **no**, use a static method:

``` java
static int add(int a, int b) {
    return a + b;
}
```

------------------------------------------------------------------------

# 20. Key Takeaways

Remember these core principles:

1.  **`static` means the member belongs to the class.**
2.  **Static variables are shared by all objects of the class.**
3.  **Static methods can be called without creating an object.**
4.  **Static methods can directly access static members.**
5.  **Static methods cannot directly access instance variables.**
6.  **Static methods cannot directly call instance methods.**
7.  **Static methods cannot use `this`.**
8.  **Instance methods can access static members.**
9.  **Use `static` when the data or behavior does not depend on a
    particular object.**
10. **Use `static final` for class-level constants.**

## The Most Important Rule

> **Instance members belong to an object. Static members belong to the
> class.**

Think of it as:

``` text
INSTANCE
    ↓
Object-specific data and behavior

STATIC
    ↓
Class-wide data and behavior
```

This distinction is the foundation for understanding more advanced Java
concepts such as static initialization blocks, nested static classes,
inheritance, and object-oriented design.
