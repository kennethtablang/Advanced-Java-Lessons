# Designing Methods in Java

## 1. Introduction to Methods

A **method** in Java is a named block of code designed to perform a
specific task.

Methods help programmers organize code into smaller, reusable units.

Instead of writing the same instructions repeatedly, we can place them
inside a method and call that method whenever we need it.

### Example

``` java
static void displayMessage() {
    System.out.println("Hello, Java!");
}
```

We can call the method:

``` java
displayMessage();
```

Output:

``` text
Hello, Java!
```

### Why Use Methods?

Methods help us:

-   Organize code
-   Reuse code
-   Reduce duplicate code
-   Make programs easier to understand
-   Make programs easier to test and maintain
-   Divide a large problem into smaller problems

------------------------------------------------------------------------

# 2. Anatomy of a Method

Consider the following method:

``` java
public static int add(int a, int b) {
    return a + b;
}
```

A method can be broken into several parts:

``` text
public     static     int     add     (int a, int b)
  │           │        │       │             │
Access     Modifier  Return   Method      Parameters
Modifier             Type     Name
                         │
                         ▼
                    Method Body
```

### 1. Access Modifier

``` java
public
```

Determines where the method can be accessed.

Common access modifiers include:

``` text
public
private
protected
default
```

### 2. `static`

``` java
static
```

Indicates that the method belongs to the class rather than a specific
object.

Not every method needs to be static.

### 3. Return Type

``` java
int
```

Specifies the type of value the method returns.

Examples:

``` java
int
double
String
boolean
void
```

### 4. Method Name

``` java
add
```

The name identifies what the method does.

### 5. Parameters

``` java
int a, int b
```

Parameters are values that the method receives as input.

### 6. Method Body

``` java
{
    return a + b;
}
```

The method body contains the instructions that the method performs.

------------------------------------------------------------------------

# 3. Basic Method Syntax

The general syntax is:

``` java
accessModifier static returnType methodName(parameters) {

    // Statements

    return value;
}
```

For example:

``` java
public static int multiply(int a, int b) {
    return a * b;
}
```

However, not every method needs every component.

For example:

``` java
void display() {
    System.out.println("Hello");
}
```

------------------------------------------------------------------------

# 4. Methods with No Parameters

A method does not always need parameters.

Example:

``` java
static void displayWelcome() {
    System.out.println("Welcome to Java Programming!");
}
```

Calling the method:

``` java
displayWelcome();
```

Output:

``` text
Welcome to Java Programming!
```

The parentheses are still required:

``` java
displayWelcome();
```

The empty parentheses indicate that the method does not require input.

------------------------------------------------------------------------

# 5. Methods with Parameters

A method can receive values through parameters.

Example:

``` java
static void greet(String name) {
    System.out.println("Hello, " + name);
}
```

Calling the method:

``` java
greet("Juan");
```

Output:

``` text
Hello, Juan
```

Another call:

``` java
greet("Maria");
```

Output:

``` text
Hello, Maria
```

The method is reusable because the name can change.

------------------------------------------------------------------------

# 6. Parameters vs. Arguments

Students often confuse **parameters** and **arguments**.

Consider:

``` java
static int add(int a, int b) {
    return a + b;
}
```

The variables:

``` java
int a
int b
```

are called **parameters**.

When we call:

``` java
add(10, 20);
```

the values:

``` text
10
20
```

are called **arguments**.

### Simple distinction

``` text
METHOD DEFINITION

add(int a, int b)
     ↑       ↑
  Parameter Parameter


METHOD CALL

add(10, 20)
     ↑   ↑
  Argument Argument
```

------------------------------------------------------------------------

# 7. Methods with Return Values

A method can calculate or produce a value and return it to the caller.

Example:

``` java
static int add(int a, int b) {
    return a + b;
}
```

We can store the result:

``` java
int result = add(10, 20);

System.out.println(result);
```

Output:

``` text
30
```

The method:

``` java
return a + b;
```

sends the calculated value back to the code that called the method.

------------------------------------------------------------------------

# 8. The `return` Statement

The `return` statement is used to send a value back to the caller.

Example:

``` java
static int square(int number) {
    return number * number;
}
```

Calling:

``` java
int result = square(5);

System.out.println(result);
```

Output:

``` text
25
```

The flow is:

``` text
square(5)
    ↓
number = 5
    ↓
5 * 5
    ↓
25
    ↓
return 25
    ↓
result
```

------------------------------------------------------------------------

# 9. `void` Methods

A method does not always need to return a value.

When a method does not return a value, we use:

``` java
void
```

Example:

``` java
static void displayMessage() {
    System.out.println("Hello Java!");
}
```

We call it:

``` java
displayMessage();
```

There is no returned value.

### Comparison

Method that returns a value:

``` java
static int add(int a, int b) {
    return a + b;
}
```

Method that does not return a value:

``` java
static void displayMessage() {
    System.out.println("Hello Java!");
}
```

------------------------------------------------------------------------

# 10. Designing Methods Based on Responsibility

One of the most important principles in method design is:

> **A method should have one clear responsibility.**

For example, avoid creating a method that does everything:

``` java
static void processStudent() {

    // Calculate grade
    // Save student
    // Print report
    // Send email
}
```

This method has too many responsibilities.

A better design is:

``` java
calculateGrade();
saveStudent();
printReport();
sendEmail();
```

Each method has a clear purpose.

For example:

``` java
static double calculateGrade(double prelim, double midterm, double finalExam) {
    return (prelim + midterm + finalExam) / 3;
}
```

Another method:

``` java
static void printReport(String name, double grade) {
    System.out.println("Student: " + name);
    System.out.println("Grade: " + grade);
}
```

This approach makes the program easier to understand and maintain.

------------------------------------------------------------------------

# 11. Method Naming

Good method names make code easier to understand.

### Good Examples

``` java
calculateTotal();
calculateAverage();
displayStudent();
printReport();
saveStudent();
getName();
setName();
validateInput();
```

These names describe what the method does.

### Poor Examples

``` java
doSomething();
process();
test();
abc();
method1();
```

These names do not clearly communicate the purpose of the method.

------------------------------------------------------------------------

# 12. Naming Convention: camelCase

Java commonly uses **camelCase** for method names.

The first word begins with a lowercase letter, and subsequent words
begin with uppercase letters.

Correct:

``` java
calculateAverage();
displayStudentInfo();
calculateFinalGrade();
saveStudentRecord();
```

Avoid:

``` java
CalculateAverage();
calculate_average();
calculate-average();
```

The standard Java convention is:

``` text
lowerCamelCase
```

------------------------------------------------------------------------

# 13. Avoiding Duplicate Code

Consider this example:

``` java
System.out.println("Welcome Juan");
System.out.println("Welcome Maria");
System.out.println("Welcome Pedro");
```

We can create a reusable method:

``` java
static void welcome(String name) {
    System.out.println("Welcome " + name);
}
```

Then:

``` java
welcome("Juan");
welcome("Maria");
welcome("Pedro");
```

Output:

``` text
Welcome Juan
Welcome Maria
Welcome Pedro
```

### Why is this better?

The logic exists in only one place.

If we need to change the message:

``` java
System.out.println("Welcome, " + name + "!");
```

we only need to change the method.

------------------------------------------------------------------------

# 14. Designing Methods with Appropriate Parameters

Parameters allow a method to work with different values.

Good design:

``` java
static double calculateArea(double length, double width) {
    return length * width;
}
```

Usage:

``` java
double area1 = calculateArea(10, 5);
double area2 = calculateArea(20, 8);
```

The same method works with different values.

### Avoid unnecessary parameters

For example:

``` java
calculateArea(10, 5, "rectangle", "meters", true, 100);
```

If the method only needs length and width, the additional parameters
make the method harder to understand.

Better:

``` java
calculateArea(10, 5);
```

------------------------------------------------------------------------

# 15. Method Overloading

**Method overloading** occurs when multiple methods have the same name
but different parameter lists.

Example:

``` java
static int add(int a, int b) {
    return a + b;
}

static int add(int a, int b, int c) {
    return a + b + c;
}
```

We can call:

``` java
System.out.println(add(10, 20));
```

or:

``` java
System.out.println(add(10, 20, 30));
```

Output:

``` text
30
60
```

Java determines which method to use based on the arguments.

------------------------------------------------------------------------

# 16. Rules for Method Overloading

Methods can be overloaded when their parameter lists are different.

Valid:

``` java
add(int a, int b)
add(int a, int b, int c)
add(double a, double b)
```

The differences can be:

-   Number of parameters
-   Parameter types
-   Order of parameter types

For example:

``` java
calculate(int a, double b)
calculate(double a, int b)
```

However, changing only the return type is **not enough**.

This is invalid:

``` java
int add(int a, int b) {
    return a + b;
}

double add(int a, int b) {
    return a + b;
}
```

The parameter lists are identical, so Java cannot distinguish the
methods.

------------------------------------------------------------------------

# 17. Static vs. Instance Methods

Methods can be either static or instance methods.

### Static Method

A static method belongs to the class.

``` java
class Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Call it using:

``` java
Calculator.add(10, 20);
```

### Instance Method

An instance method belongs to an object.

``` java
class Student {

    String name;

    void displayName() {
        System.out.println(name);
    }
}
```

Create an object:

``` java
Student student = new Student();

student.name = "Juan";

student.displayName();
```

### General Rule

Use an **instance method** when the operation depends on an object's
data.

Use a **static method** when the operation does not depend on a specific
object's data.

------------------------------------------------------------------------

# 18. Access Modifiers in Methods

Methods can have different access levels.

## `public`

Accessible from other classes.

``` java
public void displayStudent() {
}
```

## `private`

Accessible only inside the same class.

``` java
private double calculateSalary() {
}
```

Private methods are useful for internal operations that should not be
directly accessible from outside the class.

## `protected`

Accessible within the same package and by subclasses.

``` java
protected void displayDetails() {
}
```

## Default Access

If no access modifier is specified, the method has package-private
access.

``` java
void display() {
}
```

It can be accessed by classes in the same package.

------------------------------------------------------------------------

# 19. Method Parameters and Data Validation

Methods can also be designed to validate input.

Example:

``` java
static boolean isValidGrade(double grade) {

    return grade >= 0 && grade <= 100;
}
```

Usage:

``` java
double grade = 85;

if (isValidGrade(grade)) {
    System.out.println("Valid grade.");
} else {
    System.out.println("Invalid grade.");
}
```

Output:

``` text
Valid grade.
```

This makes the validation reusable.

------------------------------------------------------------------------

# 20. Designing Methods for Reusability

A well-designed method should ideally be reusable.

Poor design:

``` java
static void calculateJuanGrade() {
    double grade = (80 + 85 + 90) / 3;
    System.out.println(grade);
}
```

This method only works for Juan's specific values.

Better:

``` java
static double calculateGrade(
        double prelim,
        double midterm,
        double finalExam) {

    return (prelim + midterm + finalExam) / 3;
}
```

Now it can be reused:

``` java
double juanGrade = calculateGrade(80, 85, 90);
double mariaGrade = calculateGrade(90, 88, 95);
double pedroGrade = calculateGrade(75, 80, 85);
```

------------------------------------------------------------------------

# 21. Avoid Extremely Long Methods

A method containing hundreds of lines can be difficult to understand and
maintain.

For example:

``` java
static void processEnrollment() {

    // Validate student
    // Find course
    // Check prerequisites
    // Calculate tuition
    // Save enrollment
    // Generate schedule
    // Print receipt
    // Send notification
}
```

Instead, divide the process:

``` java
validateStudent();
checkPrerequisites();
calculateTuition();
saveEnrollment();
generateSchedule();
printReceipt();
sendNotification();
```

Each method can handle one specific responsibility.

------------------------------------------------------------------------

# 22. Methods Can Call Other Methods

Methods do not have to perform everything themselves.

Example:

``` java
static void processStudent() {

    double average = calculateAverage(85, 90, 88);

    displayResult(average);
}
```

Supporting methods:

``` java
static double calculateAverage(
        double prelim,
        double midterm,
        double finalExam) {

    return (prelim + midterm + finalExam) / 3;
}
```

And:

``` java
static void displayResult(double average) {

    System.out.println("Average: " + average);
}
```

This creates a chain of responsibilities:

``` text
processStudent()
      │
      ├── calculateAverage()
      │
      └── displayResult()
```

------------------------------------------------------------------------

# 23. Complete Example: Student Grade System

The following example demonstrates several method-design principles.

``` java
class Student {

    String name;
    double prelim;
    double midterm;
    double finalExam;

    // Calculates the student's average
    double calculateAverage() {
        return (prelim + midterm + finalExam) / 3;
    }

    // Determines whether the student passed
    boolean isPassed() {
        return calculateAverage() >= 75;
    }

    // Returns the student's remark
    String getRemark() {

        if (isPassed()) {
            return "PASSED";
        }

        return "FAILED";
    }

    // Displays the student's information
    void displayStudentInfo() {

        System.out.println("Name: " + name);
        System.out.println("Average: " + calculateAverage());
        System.out.println("Remark: " + getRemark());
    }
}
```

Main class:

``` java
public class Main {

    public static void main(String[] args) {

        Student student = new Student();

        student.name = "Juan";
        student.prelim = 85;
        student.midterm = 90;
        student.finalExam = 88;

        student.displayStudentInfo();
    }
}
```

Output:

``` text
Name: Juan
Average: 87.66666666666667
Remark: PASSED
```

### What Makes This Design Good?

Each method has a clear responsibility:

``` text
calculateAverage()
        ↓
Calculates the average

isPassed()
        ↓
Checks whether the student passed

getRemark()
        ↓
Determines the remark

displayStudentInfo()
        ↓
Displays the information
```

Instead of creating one huge method, the program is divided into
smaller, meaningful methods.

------------------------------------------------------------------------

# 24. Method Design Rules

The following rules can serve as a checklist when designing Java
methods.

## Rule 1 --- Give the Method a Meaningful Name

Good:

``` java
calculateAverage();
displayStudent();
saveRecord();
validateInput();
```

Avoid:

``` java
method1();
test();
doSomething();
abc();
```

------------------------------------------------------------------------

## Rule 2 --- Follow Java Naming Conventions

Use `lowerCamelCase`.

``` java
calculateFinalGrade();
displayStudentInfo();
```

------------------------------------------------------------------------

## Rule 3 --- Give Each Method a Clear Responsibility

A method should ideally focus on one logical task.

``` java
calculateAverage();
```

is clearer than:

``` java
processEverything();
```

------------------------------------------------------------------------

## Rule 4 --- Use Parameters for Input

Instead of hard-coding values:

``` java
static double calculateGrade() {
    return (80 + 85 + 90) / 3;
}
```

Use parameters:

``` java
static double calculateGrade(
        double prelim,
        double midterm,
        double finalExam) {

    return (prelim + midterm + finalExam) / 3;
}
```

------------------------------------------------------------------------

## Rule 5 --- Return a Value When a Result Is Needed

If a method calculates something, returning the result is often useful.

``` java
static int square(int number) {
    return number * number;
}
```

------------------------------------------------------------------------

## Rule 6 --- Use `void` When No Result Needs to Be Returned

Example:

``` java
static void displayMessage() {
    System.out.println("Hello");
}
```

------------------------------------------------------------------------

## Rule 7 --- Avoid Unnecessary Parameters

Only pass information that the method actually needs.

------------------------------------------------------------------------

## Rule 8 --- Avoid Duplicate Code

If the same logic appears multiple times, consider moving it into a
method.

------------------------------------------------------------------------

## Rule 9 --- Keep Methods Manageable

Avoid methods that are unnecessarily long or perform many unrelated
tasks.

------------------------------------------------------------------------

## Rule 10 --- Use Appropriate Access Modifiers

Use:

``` java
private
```

for internal helper methods when they do not need to be accessed outside
the class.

Use:

``` java
public
```

when other classes need to access the method.

------------------------------------------------------------------------

## Rule 11 --- Use Static Methods When Object State Is Not Required

Example:

``` java
static int add(int a, int b) {
    return a + b;
}
```

------------------------------------------------------------------------

## Rule 12 --- Use Instance Methods When Object Data Is Required

Example:

``` java
void displayName() {
    System.out.println(name);
}
```

------------------------------------------------------------------------

## Rule 13 --- Use Method Overloading When It Improves Readability

Example:

``` java
add(10, 20);
add(10, 20, 30);
```

------------------------------------------------------------------------

## Rule 14 --- Avoid Overloading Just to Make Code Complicated

Overloading should make a class easier to use, not harder to understand.

------------------------------------------------------------------------

## Rule 15 --- Use Comments or Javadoc for Complex Methods

For example:

``` java
/**
 * Calculates the final grade using the student's
 * prelim, midterm, and final examination scores.
 */
static double calculateFinalGrade(
        double prelim,
        double midterm,
        double finalExam) {

    return (prelim + midterm + finalExam) / 3;
}
```

------------------------------------------------------------------------

# 25. Common Method Design Mistakes

## Mistake 1 --- Creating One Giant Method

``` java
static void processEverything() {
    // Hundreds of lines
}
```

### Better

Break the process into smaller methods.

------------------------------------------------------------------------

## Mistake 2 --- Using Unclear Names

``` java
static void process() {
}
```

Better:

``` java
static void calculateStudentGrade() {
}
```

------------------------------------------------------------------------

## Mistake 3 --- Hard-Coding Values

Poor:

``` java
static int calculate() {
    return 80 + 90;
}
```

Better:

``` java
static int calculate(int a, int b) {
    return a + b;
}
```

------------------------------------------------------------------------

## Mistake 4 --- Returning the Wrong Type

For example:

``` java
static int calculateAverage() {
    return 87.5;
}
```

This is invalid because `87.5` is a `double`, not an `int`.

Correct:

``` java
static double calculateAverage() {
    return 87.5;
}
```

------------------------------------------------------------------------

## Mistake 5 --- Forgetting `return`

Incorrect:

``` java
static int add(int a, int b) {
    a + b;
}
```

Correct:

``` java
static int add(int a, int b) {
    return a + b;
}
```

------------------------------------------------------------------------

# 26. A Practical Method Design Process

When creating a method, follow these steps:

``` text
Step 1
Identify the task
        ↓
Step 2
Give the method a meaningful name
        ↓
Step 3
Determine what input it needs
        ↓
Step 4
Determine whether it needs to return a value
        ↓
Step 5
Choose the appropriate access modifier
        ↓
Step 6
Decide whether it should be static or instance
        ↓
Step 7
Write the method
        ↓
Step 8
Test the method
```

### Example

Suppose the task is:

> Calculate the area of a rectangle.

### Step 1 --- Identify the task

``` text
Calculate rectangle area
```

### Step 2 --- Choose a name

``` java
calculateArea()
```

### Step 3 --- Identify required input

We need:

``` text
length
width
```

### Step 4 --- Determine the return value

The method should return the area:

``` text
double
```

### Step 5 --- Determine whether it needs object data

No.

Therefore, a static method is appropriate.

### Final method

``` java
static double calculateArea(double length, double width) {
    return length * width;
}
```

------------------------------------------------------------------------

# 27. Visual Summary

``` text
                    DESIGNING A METHOD
                           │
             ┌─────────────┴─────────────┐
             │                           │
         INPUT                        OUTPUT
             │                           │
        Parameters                 Return Value
             │                           │
             └─────────────┬─────────────┘
                           │
                     METHOD LOGIC
                           │
                           ▼
                     Specific Task
```

A well-designed method generally answers three questions:

``` text
1. What does this method do?
2. What information does it need?
3. What result does it produce?
```

For example:

``` java
static double calculateAverage(
        double prelim,
        double midterm,
        double finalExam) {

    return (prelim + midterm + finalExam) / 3;
}
```

It answers:

``` text
What does it do?
→ Calculates an average.

What does it need?
→ Prelim, midterm, and final exam grades.

What does it produce?
→ A double representing the average.
```

------------------------------------------------------------------------

# 28. Key Takeaways

Remember these core principles when designing Java methods:

1.  **A method should perform a specific task.**
2.  **Give methods meaningful names.**
3.  **Follow the Java `camelCase` naming convention.**
4.  **Use parameters to provide input.**
5.  **Use return values when a result needs to be produced.**
6.  **Use `void` when the method does not return a value.**
7.  **Avoid unnecessary parameters.**
8.  **Avoid duplicate code.**
9.  **Keep methods focused and manageable.**
10. **Use appropriate access modifiers.**
11. **Use instance methods when object data is required.**
12. **Use static methods when object state is not required.**
13. **Use method overloading when it improves readability.**
14. **Break large processes into smaller methods.**
15. **Document complex methods when necessary.**

## The Most Important Principle

> **A well-designed method should have a clear purpose, receive only the
> information it needs, perform its responsibility, and return an
> appropriate result when necessary.**

A useful mental model is:

``` text
             METHOD

              INPUT
                ↓
           Parameters
                ↓
             PROCESS
                ↓
          Method Logic
                ↓
             OUTPUT
                ↓
          Return Value
```

Good method design is not simply about making code shorter. It is about
making code **organized, reusable, understandable, testable, and
maintainable**.
