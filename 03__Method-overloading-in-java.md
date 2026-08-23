# Method Overloading in Java

## 1. What is Method Overloading?

Method Overloading is a feature in Java that allows a class to have multiple methods with the **same name** but **different parameter lists**.

The methods perform related operations, but they can accept different types or numbers of inputs.

For example:

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

Both methods are named `add`, but they have different parameter lists:

```
add(int, int)
add(int, int, int)
```

When we call the method, Java determines which version to use based on the arguments we provide.

```java
Calculator calc = new Calculator();

System.out.println(calc.add(10, 20));       // 30
System.out.println(calc.add(10, 20, 30));   // 60
```

This is called **compile-time polymorphism** because Java determines the appropriate overloaded method during compilation.

---

## 2. Why Do We Use Method Overloading?

Method overloading is useful when several operations have the same general purpose but require different inputs.

Without overloading, we might create different method names:

```java
int addTwoNumbers(int a, int b) {
    return a + b;
}

int addThreeNumbers(int a, int b, int c) {
    return a + b + c;
}
```

This works, but it can make the API harder to remember.

With method overloading:

```java
int add(int a, int b) {
    return a + b;
}

int add(int a, int b, int c) {
    return a + b + c;
}
```

We can simply use:

```java
add(10, 20);
add(10, 20, 30);
```

The method name communicates the operation, while the parameters determine the specific version.

---

## 3. Basic Example

Consider a `Printer` class:

```java
class Printer {

    void print(String text) {
        System.out.println("Text: " + text);
    }

    void print(int number) {
        System.out.println("Number: " + number);
    }

    void print(String text, int copies) {
        for (int i = 0; i < copies; i++) {
            System.out.println(text);
        }
    }
}
```

We can call:

```java
Printer printer = new Printer();

printer.print("Hello");
printer.print(100);
printer.print("Hello", 3);
```

Java identifies the appropriate method:

```
print("Hello")
      ↓
print(String)

print(100)
      ↓
print(int)

print("Hello", 3)
      ↓
print(String, int)
```

---

## 4. How Can Methods Be Overloaded?

There are three primary ways to overload a method.

### A. Different Number of Parameters

```java
class Calculator {

    int multiply(int a, int b) {
        return a * b;
    }

    int multiply(int a, int b, int c) {
        return a * b * c;
    }
}
```

The parameter count is different:

```
multiply(int, int)
multiply(int, int, int)
```

Usage:

```java
Calculator calculator = new Calculator();

System.out.println(calculator.multiply(2, 3));       // 6
System.out.println(calculator.multiply(2, 3, 4));    // 24
```

---

## 5. Different Parameter Types

Methods can also be overloaded by changing the data types of their parameters.

```java
class Display {

    void show(int value) {
        System.out.println("Integer: " + value);
    }

    void show(double value) {
        System.out.println("Double: " + value);
    }

    void show(String value) {
        System.out.println("String: " + value);
    }
}
```

Usage:

```java
Display display = new Display();

display.show(10);
display.show(10.5);
display.show("Hello");
```

Java chooses the appropriate method based on the argument's type.

---

## 6. Different Order of Parameter Types

The order of parameters can also be changed.

```java
class Student {

    void register(String name, int age) {
        System.out.println(name + " is " + age + " years old.");
    }

    void register(int age, String name) {
        System.out.println(name + " is " + age + " years old.");
    }
}
```

These are considered different methods:

```
register(String, int)
register(int, String)
```

Therefore:

```java
Student student = new Student();

student.register("Juan", 20);
student.register(20, "Juan");
```

Both calls are valid.

---

## 7. Changing Only the Return Type Is NOT Overloading

This is one of the most important rules.

The following is **not** valid method overloading:

```java
class Calculator {

    int calculate(int a, int b) {
        return a + b;
    }

    double calculate(int a, int b) {
        return a + b;
    }
}
```

The parameter lists are exactly the same:

```
calculate(int, int)
calculate(int, int)
```

Only the return type is different.

Java cannot determine which method to call based on the return type.

For example:

```java
calculate(10, 20);
```

There is no information in the call that tells Java whether it should use:

```
int calculate(int, int)
```

or:

```
double calculate(int, int)
```

Therefore, this produces a **compilation error**.

> **Important Rule:** Return type alone cannot be used to overload a method.

---

## 8. Method Overloading and `static`

Static methods can also be overloaded.

```java
class MathHelper {

    static int square(int number) {
        return number * number;
    }

    static double square(double number) {
        return number * number;
    }
}
```

We can call them using the class name:

```java
System.out.println(MathHelper.square(5));
System.out.println(MathHelper.square(5.5));
```

The same rules apply to static methods:

- Same method name
- Different parameter list
- Return type alone is insufficient

---

## 9. Constructor Overloading

Method overloading concepts also apply to constructors.

A class can have multiple constructors with different parameter lists.

```java
class Student {

    String name;
    int age;

    Student() {
        name = "Unknown";
        age = 0;
    }

    Student(String name) {
        this.name = name;
        age = 0;
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

We can create objects in different ways:

```java
Student s1 = new Student();

Student s2 = new Student("Juan");

Student s3 = new Student("Maria", 20);
```

Java determines which constructor to execute based on the arguments.

---

## 10. Method Overloading with Type Conversion

Java may perform automatic type conversion when selecting an overloaded method.

Consider:

```java
class Demo {

    void display(int value) {
        System.out.println("Integer");
    }

    void display(double value) {
        System.out.println("Double");
    }
}
```

If we call:

```java
Demo demo = new Demo();

demo.display(10);
```

Java selects:

```
display(int)
```

because `10` is an integer.

But:

```java
demo.display(10.5);
```

selects:

```
display(double)
```

Java can also perform **widening conversion**:

```
int → long → float → double
```

For example:

```java
void display(double value) {
    System.out.println(value);
}

display(10);
```

The integer `10` can be widened to `double`.

However, Java does **not** automatically perform **narrowing conversion**:

```
double → int
```

So this should be kept in mind when designing overloaded methods.

---

## 11. Method Overloading with `null`

Overloading can become ambiguous when `null` is passed.

For example:

```java
class Demo {

    void display(String value) {
        System.out.println("String");
    }

    void display(Integer value) {
        System.out.println("Integer");
    }
}
```

This call:

```java
Demo demo = new Demo();

demo.display(null);
```

causes an ambiguity because `null` can be assigned to both:

- `String`
- `Integer`

Java cannot determine which method should be called.

However, if one type is more specific than another, Java may be able to select the more specific method.

---

## 12. Rules for Implementing Method Overloading

Students should remember these rules:

### Rule 1 — The method name must be the same

```java
void display(int value)
void display(double value)
```

Both methods use `display`.

### Rule 2 — The parameter list must be different

The difference can be based on:

- Number of parameters
- Parameter types
- Order of parameter types

Example:

```java
void display(int value)
void display(int value, int count)
```

### Rule 3 — Return type alone cannot create overloading

❌ Invalid:

```java
int calculate(int a)
double calculate(int a)
```

The parameter lists are identical.

### Rule 4 — Parameter names do not make methods different

❌ Invalid:

```java
void display(int number)
void display(int value)
```

Even though the parameter names are different, both methods have the signature `display(int)`. Java considers them the same signature.

### Rule 5 — Access modifiers can be different

Overloaded methods can have different access modifiers.

```java
public void display(int value) {
}

private void display(String value) {
}
```

The parameter lists are different, so they can coexist. However, changing only the access modifier does not create overloading.

### Rule 6 — `static` does not prevent overloading

Static methods can be overloaded:

```java
static void print(int value) {
}

static void print(String value) {
}
```

### Rule 7 — Instance methods can be overloaded

Regular non-static methods can also be overloaded:

```java
void print(int value) {
}

void print(String value) {
}
```

### Rule 8 — Constructors can be overloaded

```java
Student() {
}

Student(String name) {
}

Student(String name, int age) {
}
```

### Rule 9 — Java selects the method at compile time

Method overloading is known as **compile-time polymorphism**. Java determines the method to invoke based primarily on the arguments supplied to the method call.

### Rule 10 — Avoid unnecessary or confusing overloads

Although Java allows many overloaded methods, excessive overloading can make code difficult to understand.

For example:

```java
process(int a)
process(long a)
process(float a)
process(double a)
process(String a)
process(Object a)
```

Technically valid, but potentially confusing. Overloading should be used when the methods represent the same logical operation.

---

## 13. Real-World Example

Consider a banking application.

We might want to deposit money into an account.

A simple version:

```java
class BankAccount {

    void deposit(double amount) {
        System.out.println("Deposited: " + amount);
    }

    void deposit(double amount, String description) {
        System.out.println("Deposited: " + amount);
        System.out.println("Description: " + description);
    }
}
```

Usage:

```java
BankAccount account = new BankAccount();

account.deposit(5000);

account.deposit(5000, "Monthly Salary");
```

Both methods represent the same operation — **deposit money** — but the second version allows additional information. This is a good use of method overloading.

---

## 14. Another Real-World Example: Student System

Suppose we have a student registration system.

```java
class StudentRegistration {

    void register(String studentName) {
        System.out.println("Registered: " + studentName);
    }

    void register(String studentName, String course) {
        System.out.println("Student: " + studentName);
        System.out.println("Course: " + course);
    }

    void register(String studentName, String course, int yearLevel) {
        System.out.println("Student: " + studentName);
        System.out.println("Course: " + course);
        System.out.println("Year Level: " + yearLevel);
    }
}
```

Usage:

```java
StudentRegistration registration = new StudentRegistration();

registration.register("Juan");

registration.register("Juan", "BSIT");

registration.register("Juan", "BSIT", 2);
```

Instead of having:

```
registerStudent()
registerStudentWithCourse()
registerStudentWithCourseAndYear()
```

we maintain one logical operation `register()` and allow the parameters to determine which version is used.

---

## 15. Method Signature

To properly understand overloading, students need to understand the concept of a **method signature**.

In Java, a method signature consists of:

```
Method Name + Parameter Types
```

For example:

```java
void calculate(int a, int b)
```

has the signature:

```
calculate(int, int)
```

While:

```java
void calculate(double a, double b)
```

has:

```
calculate(double, double)
```

These are different signatures, so they can be overloaded.

The following:

```java
void calculate(int a, int b)
void calculate(int x, int y)
```

have the same signature `calculate(int, int)`. Therefore, they cannot be overloaded.

---

## 16. Method Overloading vs. Method Overriding

Students often confuse these two concepts.

| Method Overloading | Method Overriding |
|---|---|
| Same class normally | Requires inheritance |
| Same method name | Same method name |
| Different parameter list | Same parameter list |
| Compile-time polymorphism | Runtime polymorphism |
| Usually occurs within the same class | Occurs between parent and child classes |
| Return type alone cannot differ | Return type generally must be compatible |

**Overloading:**

```java
void print(int value) {
}

void print(String value) {
}
```

**Overriding:**

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
```

A simple way to remember:

- **Overloading** = same name, different parameters.
- **Overriding** = child class provides a new implementation of an inherited method.

---

## 17. Quick Rules to Remember

Students can use this checklist when implementing method overloading:

- Same method name
- Different parameter list
- Different number of parameters is allowed
- Different parameter types is allowed
- Different order of parameter types is allowed
- Different parameter names alone are **not** enough
- Different return types alone are **not** enough
- Static methods can be overloaded
- Instance methods can be overloaded
- Constructors can be overloaded
- Overloading is compile-time polymorphism
- Java determines the appropriate overloaded method from the arguments supplied
- Use overloading when methods perform the same general operation
- Avoid excessive overloads that make method calls ambiguous or difficult to understand

---

## Key Takeaway

The central idea of Method Overloading is:

```
ONE METHOD NAME
       +
DIFFERENT PARAMETERS
       =
METHOD OVERLOADING
```

For example:

```
add(int, int)
add(int, int, int)
add(double, double)
add(String, String)
```

All of these can exist in the same class because they have different parameter lists. This allows Java programs to provide a consistent and readable interface while supporting different kinds of input.
