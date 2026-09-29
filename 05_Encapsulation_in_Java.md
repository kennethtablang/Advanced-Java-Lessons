# Encapsulation in Java

## 1. What is Encapsulation?

**Encapsulation** is one of the fundamental principles of Object-Oriented Programming (OOP).

Encapsulation means **bundling data and the methods that operate on that data inside a class while controlling direct access to the data**.

In Java, encapsulation is commonly implemented by:

1. Declaring fields as `private`.
2. Providing `public` methods to access or modify those fields when necessary.
3. Using methods to control how data is accessed or changed.
4. Applying validation before accepting new values.

A simple way to understand encapsulation is:

```text
DATA
  ↓
private fields
  ↓
Controlled access through methods
  ↓
Public interface
```

The main idea is:

> **Keep an object's internal data protected and provide controlled ways to interact with it.**

---

# 2. Why Do We Need Encapsulation?

Consider this class without encapsulation:

```java
class Student {

    String name;
    int age;
}
```

Because the fields are not private, another class can directly change them:

```java
Student student = new Student();

student.name = "Juan";
student.age = -50;
```

The problem is that `age = -50` does not make sense for a student.

There is no protection preventing invalid data from being assigned.

With encapsulation:

```java
class Student {

    private String name;
    private int age;

    public void setAge(int age) {
        if (age >= 0) {
            this.age = age;
        }
    }

    public int getAge() {
        return age;
    }
}
```

Now the class controls how the `age` field can be changed.

```java
Student student = new Student();

student.setAge(20);
```

But:

```java
student.age = -50;
```

is not allowed because `age` is private.

---

# 3. The Main Purpose of Encapsulation

Encapsulation provides several important benefits:

- **Data protection**
- **Controlled access**
- **Validation**
- **Reduced coupling**
- **Better maintainability**
- **Improved security**
- **Abstraction of implementation details**

The most important idea is **controlled access**.

We do not necessarily want other classes to directly manipulate an object's internal state.

Instead, the object provides methods that determine what can be done.

---

# 4. Access Modifiers and Encapsulation

Java provides access modifiers that control visibility.

The most commonly used modifiers are:

| Modifier | Same Class | Same Package | Subclass | Other Packages |
|---|---:|---:|---:|---:|
| `private` | Yes | No | No directly | No |
| default/package-private | Yes | Yes | Depends on package | No |
| `protected` | Yes | Yes | Yes | Yes, through inheritance rules |
| `public` | Yes | Yes | Yes | Yes |

For basic encapsulation, the most common pattern is:

```java
private fields
+
public methods
```

Example:

```java
class Product {

    private String name;
    private double price;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

---

# 5. Private Fields

A field declared as `private` can only be directly accessed inside the class that declares it.

Example:

```java
class Student {

    private String name;
    private int age;
}
```

This is not allowed from another class:

```java
Student student = new Student();

student.name = "Juan";
student.age = 20;
```

The compiler will report an access error because the fields are private.

Instead, the class can provide methods for controlled access.

---

# 6. Getters

A **getter** is a method used to retrieve the value of a private field.

Example:

```java
class Student {

    private String name;

    public String getName() {
        return name;
    }
}
```

Usage:

```java
Student student = new Student();

System.out.println(student.getName());
```

The getter provides read access without making the field public.

---

# 7. Setters

A **setter** is a method used to modify the value of a private field.

Example:

```java
class Student {

    private String name;

    public void setName(String name) {
        this.name = name;
    }
}
```

Usage:

```java
Student student = new Student();

student.setName("Juan");
```

The setter gives the class an opportunity to validate or process the value before assigning it.

---

# 8. Getters and Setters Together

A common encapsulation pattern is:

```java
class Student {

    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

Usage:

```java
Student student = new Student();

student.setName("Juan");
student.setAge(20);

System.out.println(student.getName());
System.out.println(student.getAge());
```

The fields remain private while the class provides controlled access.

---

# 9. Encapsulation with Validation

One of the most useful applications of encapsulation is **validation**.

Consider a bank account.

We should not allow a negative balance to be assigned directly.

```java
class BankAccount {

    private double balance;

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {

        if (balance >= 0) {
            this.balance = balance;
        }
    }
}
```

Usage:

```java
BankAccount account = new BankAccount();

account.setBalance(5000);

System.out.println(account.getBalance());
```

If we attempt:

```java
account.setBalance(-1000);
```

the class can reject the invalid value.

This is an important benefit of encapsulation:

> **The class can protect its own data.**

---

# 10. A Better Bank Account Example

Instead of allowing the balance to be changed arbitrarily, we can expose operations that represent legitimate banking actions.

```java
class BankAccount {

    private double balance;

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(double amount) {

        if (amount <= 0) {
            return false;
        }

        if (amount > balance) {
            return false;
        }

        balance -= amount;
        return true;
    }
}
```

Usage:

```java
BankAccount account = new BankAccount();

account.deposit(5000);

System.out.println(account.getBalance());

boolean successful = account.withdraw(1000);

System.out.println(successful);
System.out.println(account.getBalance());
```

Notice that there is no:

```java
setBalance()
```

The class controls the balance through:

```java
deposit()
withdraw()
```

This is stronger encapsulation because the class controls the rules governing its internal state.

---

# 11. Why Not Make Everything Public?

Consider:

```java
class BankAccount {

    public double balance;
}
```

Another class could do:

```java
account.balance = -999999;
```

The `BankAccount` class has no opportunity to prevent the invalid value.

With:

```java
private double balance;
```

the class controls how the balance changes.

This allows the class to enforce business rules.

---

# 12. Encapsulation with a Student System

Consider a student record.

```java
class Student {

    private int studentId;
    private String name;
    private int age;
    private String course;

    public int getStudentId() {
        return studentId;
    }

    public void setStudentId(int studentId) {
        if (studentId > 0) {
            this.studentId = studentId;
        }
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        if (name != null && !name.isBlank()) {
            this.name = name;
        }
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        if (age >= 15 && age <= 100) {
            this.age = age;
        }
    }

    public String getCourse() {
        return course;
    }

    public void setCourse(String course) {
        if (course != null && !course.isBlank()) {
            this.course = course;
        }
    }
}
```

Now the class controls the values that can enter the object.

For example:

```java
Student student = new Student();

student.setStudentId(1001);
student.setName("Juan Dela Cruz");
student.setAge(20);
student.setCourse("BSIT");
```

The object can reject invalid values.

---

# 13. Encapsulation Does Not Mean "Everything Must Have a Getter and Setter"

This is an important concept.

A common beginner pattern is:

```java
private String name;

public String getName() {
    return name;
}

public void setName(String name) {
    this.name = name;
}
```

Although this is valid encapsulation, **good encapsulation does not require every private field to have both a getter and a setter**.

Sometimes a field should only be readable.

Example:

```java
class Student {

    private final int studentId;

    public Student(int studentId) {
        this.studentId = studentId;
    }

    public int getStudentId() {
        return studentId;
    }
}
```

There is no:

```java
setStudentId()
```

The ID can be read but cannot be changed after construction.

This protects the object's identity.

---

# 14. Read-Only Data

A field can be made effectively read-only using `private final`.

```java
class Product {

    private final int productId;
    private String name;

    public Product(int productId, String name) {
        this.productId = productId;
        this.name = name;
    }

    public int getProductId() {
        return productId;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

Usage:

```java
Product product = new Product(101, "Keyboard");

System.out.println(product.getProductId());

product.setName("Mechanical Keyboard");
```

The product ID cannot be changed after construction.

---

# 15. Encapsulation and the `this` Keyword

The `this` keyword is frequently used with encapsulation.

Example:

```java
class Employee {

    private String name;

    public void setName(String name) {
        this.name = name;
    }
}
```

There are two `name` variables:

```text
this.name
   ↓
instance variable

name
   ↓
method parameter
```

Therefore:

```java
this.name = name;
```

means:

```text
object's name = supplied name
```

---

# 16. Encapsulation and Constructors

Constructors can work together with encapsulation to initialize private fields.

```java
class Employee {

    private int employeeId;
    private String name;
    private double salary;

    public Employee(int employeeId, String name, double salary) {
        this.employeeId = employeeId;
        this.name = name;
        this.salary = salary;
    }

    public int getEmployeeId() {
        return employeeId;
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }
}
```

Usage:

```java
Employee employee =
    new Employee(1001, "Juan", 25000);

System.out.println(employee.getName());
```

The fields are private, but the constructor is responsible for establishing the initial state.

---

# 17. Encapsulation with Business Logic

Encapsulation is not simply about getters and setters.

A better design often places behavior inside the class that owns the data.

Consider a bank account:

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }

    public void withdraw(double amount) {

        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

The `BankAccount` class owns:

```text
balance
```

and also owns the operations that modify it:

```text
deposit()
withdraw()
```

This follows the principle:

> **An object should control its own state and behavior.**

---

# 18. Real-World Example: Product Inventory

Encapsulation is useful in inventory systems.

```java
class Product {

    private String name;
    private double price;
    private int stock;

    public Product(String name, double price, int stock) {
        this.name = name;
        this.price = price;
        this.stock = stock;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    public int getStock() {
        return stock;
    }

    public void addStock(int quantity) {

        if (quantity > 0) {
            stock += quantity;
        }
    }

    public boolean removeStock(int quantity) {

        if (quantity <= 0 || quantity > stock) {
            return false;
        }

        stock -= quantity;
        return true;
    }
}
```

Usage:

```java
Product product =
    new Product("Keyboard", 1500.00, 10);

product.addStock(5);

boolean removed = product.removeStock(3);

System.out.println(product.getStock());
```

Notice that the program cannot directly do:

```java
product.stock = -100;
```

The `Product` class controls stock changes.

---

# 19. Encapsulation in a Payroll System

Consider an employee salary.

```java
class Employee {

    private String name;
    private double salary;

    public Employee(String name, double salary) {
        this.name = name;

        if (salary >= 0) {
            this.salary = salary;
        }
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public void increaseSalary(double amount) {

        if (amount > 0) {
            salary += amount;
        }
    }
}
```

Usage:

```java
Employee employee =
    new Employee("Juan", 25000);

employee.increaseSalary(3000);

System.out.println(employee.getSalary());
```

Instead of allowing:

```java
employee.salary = -50000;
```

the class controls salary modifications through a meaningful operation.

---

# 20. Encapsulation vs. Data Hiding

These terms are related but are not exactly identical.

### Data Hiding

Data hiding focuses on preventing direct access to internal data.

Example:

```java
private double balance;
```

### Encapsulation

Encapsulation focuses on **bundling data and behavior together and controlling how the object can be accessed and modified**.

Example:

```java
private double balance;

public void deposit(double amount) {
    // controlled operation
}
```

A useful distinction is:

```text
Data Hiding
    ↓
Protect internal data

Encapsulation
    ↓
Protect data + control interaction with the object
```

---

# 21. Encapsulation and Abstraction

Encapsulation and abstraction are related OOP concepts, but they have different focuses.

### Encapsulation

Focuses on:

> **How do we protect and control access to an object's data and behavior?**

Example:

```java
private double balance;
```

### Abstraction

Focuses on:

> **What details should the user of the class need to know?**

For example:

```java
account.withdraw(1000);
```

The user does not need to know every internal step used to process the withdrawal.

Simple distinction:

```text
Encapsulation → Protect and control
Abstraction   → Hide unnecessary complexity
```

---

# 22. Common Encapsulation Pattern

A common Java class follows this structure:

```java
class Person {

    // Private fields
    private String name;
    private int age;

    // Constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getter
    public String getName() {
        return name;
    }

    // Setter
    public void setName(String name) {
        if (name != null && !name.isBlank()) {
            this.name = name;
        }
    }

    // Getter
    public int getAge() {
        return age;
    }

    // Setter with validation
    public void setAge(int age) {
        if (age >= 0) {
            this.age = age;
        }
    }
}
```

The structure is:

```text
Class
│
├── private fields
│
├── constructor
│
├── getters
│
├── setters
│
└── business methods
```

---

# 23. Rules for Implementing Encapsulation

Students should remember these rules when implementing encapsulation in Java.

## Rule 1 — Keep Important Fields Private

Prefer:

```java
private String name;
private double salary;
```

instead of:

```java
public String name;
public double salary;
```

Private fields prevent direct external modification.

---

## Rule 2 — Use Methods to Control Access

Provide methods when external code needs to interact with the data.

Example:

```java
public String getName() {
    return name;
}
```

and:

```java
public void setName(String name) {
    this.name = name;
}
```

---

## Rule 3 — Use Getters for Controlled Read Access

Example:

```java
public double getSalary() {
    return salary;
}
```

A getter should provide access to data without exposing the field itself.

---

## Rule 4 — Use Setters Only When Modification Is Appropriate

Do not automatically create a setter for every field.

For example, an employee ID might not be allowed to change:

```java
private final int employeeId;
```

Provide:

```java
public int getEmployeeId() {
    return employeeId;
}
```

but no setter.

---

## Rule 5 — Validate Data Before Modifying Fields

Example:

```java
public void setAge(int age) {

    if (age >= 0) {
        this.age = age;
    }
}
```

Validation prevents invalid state.

---

## Rule 6 — Keep Business Rules Inside the Class That Owns the Data

Instead of:

```java
account.balance -= amount;
```

prefer:

```java
account.withdraw(amount);
```

The `BankAccount` class can then enforce rules such as:

```text
amount must be positive
amount cannot exceed balance
```

---

## Rule 7 — Use `this` When Necessary to Distinguish Fields from Parameters

Example:

```java
public void setName(String name) {
    this.name = name;
}
```

---

## Rule 8 — Use Constructors to Establish the Initial State

Example:

```java
public Product(int id, String name, double price) {
    this.id = id;
    this.name = name;
    this.price = price;
}
```

---

## Rule 9 — Do Not Expose Mutable Internal Data Unnecessarily

Be careful when returning mutable objects directly.

For example:

```java
public ArrayList<String> getItems() {
    return items;
}
```

External code can potentially modify the internal collection.

Depending on the design, a safer approach may be to return an unmodifiable view or a copy.

Example:

```java
public List<String> getItems() {
    return Collections.unmodifiableList(items);
}
```

This prevents callers from directly modifying the internal list through the returned reference.

---

## Rule 10 — Use the Most Restrictive Access Level That Works

If a field does not need to be accessible outside the class, make it:

```java
private
```

If a method is only an internal helper, it may also be:

```java
private
```

Do not make members `public` unnecessarily.

---

## Rule 11 — Do Not Treat Getters and Setters as the Entire Concept of Encapsulation

This:

```java
private double balance;

public double getBalance() {
    return balance;
}

public void setBalance(double balance) {
    this.balance = balance;
}
```

does provide controlled access, but if arbitrary values can be assigned, the class may not be protecting its state very effectively.

A stronger design can expose meaningful operations:

```java
public void deposit(double amount) {
    // validation
}

public boolean withdraw(double amount) {
    // validation
}
```

---

## Rule 12 — Protect the Object's Valid State

A well-encapsulated class should prevent invalid combinations of data whenever practical.

For example:

```java
public void setStock(int stock) {

    if (stock >= 0) {
        this.stock = stock;
    }
}
```

The class controls its own rules.

---

## Rule 13 — Use `final` When a Field Should Not Change

Example:

```java
private final int studentId;
```

Initialize it in the constructor:

```java
public Student(int studentId) {
    this.studentId = studentId;
}
```

This prevents reassignment after initialization.

---

## Rule 14 — Avoid Public Fields for Object State

Generally avoid:

```java
public double balance;
```

Prefer:

```java
private double balance;
```

and provide controlled operations.

---

## Rule 15 — Encapsulation Should Reduce Unnecessary Coupling

External classes should not need to know how an object's internal data is stored.

For example, prefer:

```java
account.withdraw(1000);
```

instead of exposing:

```java
account.balance -= 1000;
```

This allows the internal implementation to change without requiring every external class to change.

---

# 24. Common Mistakes

## Mistake 1: Making All Fields Public

```java
class Student {

    public String name;
    public int age;
}
```

This exposes the object's internal state.

Better:

```java
class Student {

    private String name;
    private int age;
}
```

---

## Mistake 2: Creating Setters Without Validation

```java
public void setAge(int age) {
    this.age = age;
}
```

This allows:

```java
student.setAge(-100);
```

If negative ages are invalid, validate the value:

```java
public void setAge(int age) {

    if (age >= 0) {
        this.age = age;
    }
}
```

---

## Mistake 3: Giving Every Field a Setter Automatically

Not every field should necessarily be changeable.

For example:

```java
private final int employeeId;
```

may be intended to remain unchanged.

---

## Mistake 4: Directly Manipulating Internal State

Avoid designs where other classes need to modify internal fields.

Instead of:

```java
account.balance += 5000;
```

use:

```java
account.deposit(5000);
```

---

## Mistake 5: Returning Mutable Internal Objects Without Considering Side Effects

Example:

```java
public List<String> getItems() {
    return items;
}
```

This can expose the internal collection to modification.

Consider whether callers should receive:

- A copy
- An unmodifiable view
- A read-only interface

---

# 25. Complete Example: Encapsulated Bank Account

```java
class BankAccount {

    private final String accountNumber;
    private String owner;
    private double balance;

    public BankAccount(
        String accountNumber,
        String owner,
        double initialBalance
    ) {

        this.accountNumber = accountNumber;
        this.owner = owner;

        if (initialBalance >= 0) {
            this.balance = initialBalance;
        } else {
            this.balance = 0;
        }
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public String getOwner() {
        return owner;
    }

    public void setOwner(String owner) {

        if (owner != null && !owner.isBlank()) {
            this.owner = owner;
        }
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(double amount) {

        if (amount <= 0) {
            return false;
        }

        if (amount > balance) {
            return false;
        }

        balance -= amount;
        return true;
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        BankAccount account =
            new BankAccount(
                "ACC001",
                "Juan Dela Cruz",
                5000
            );

        account.deposit(2000);

        boolean success = account.withdraw(1000);

        System.out.println("Owner: " + account.getOwner());
        System.out.println("Balance: " + account.getBalance());
        System.out.println("Withdrawal successful: " + success);
    }
}
```

The outside code does not directly modify:

```text
accountNumber
owner
balance
```

Instead, it interacts with the object through:

```text
getAccountNumber()
getOwner()
setOwner()
getBalance()
deposit()
withdraw()
```

This is encapsulation in practice.

---

# 26. Encapsulation Checklist

When creating a Java class, ask:

```text
□ Are important fields private?
□ Is direct modification prevented where necessary?
□ Are getters provided only when read access is needed?
□ Are setters provided only when modification is appropriate?
□ Are setter inputs validated?
□ Are important business rules inside the class?
□ Does the constructor establish a valid initial state?
□ Should any fields be final?
□ Are mutable internal objects protected from unwanted modification?
□ Are access modifiers as restrictive as practical?
```

---

# 27. Key Takeaways

Encapsulation is about **protecting an object's internal state and controlling how other parts of the program interact with it**.

The basic pattern is:

```text
PRIVATE DATA
     ↓
CONTROLLED METHODS
     ↓
VALIDATION / BUSINESS RULES
     ↓
SAFE OBJECT STATE
```

The most common implementation is:

```java
class Student {

    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        if (age >= 0) {
            this.age = age;
        }
    }
}
```

However, good encapsulation is more than simply creating getters and setters.

A stronger design gives the object **meaningful operations** that control its own state:

```java
account.deposit(5000);
account.withdraw(1000);
```

rather than allowing external code to directly manipulate:

```java
account.balance = 5000;
account.balance -= 1000;
```

## Final Reminder

> **Encapsulation = Protect the object's data + Control how the data is accessed and modified.**

A useful rule for students to remember is:

```text
PRIVATE FIELDS
      +
CONTROLLED ACCESS
      +
VALIDATION
      =
ENCAPSULATION
```

And the most important design principle is:

> **An object should control its own state instead of allowing other classes to freely modify its internal data.**
