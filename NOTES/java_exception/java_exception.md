# Exceptions in Java (Basic to Advanced)

---

## 1. What is an Exception?
----------------------------

An **exception** is an **abnormal event** that occurs during the execution of a program and **disrupts the normal flow** of instructions.

### Examples:
- Dividing a number by zero
- Accessing an invalid array index
- Opening a file that does not exist
- Database or network failure

---

## 2. Why Exception Handling is Required?
----------------------------------------

Without exception handling:
- Program terminates abnormally
- JVM prints stack trace
- Remaining code is not executed

With exception handling:
- Program continues gracefully
- Errors are handled logically
- Application becomes robust and maintainable

---

## 3. Exception vs Error
-----------------------

| Exception | Error |
|----------|-------|
| Recoverable | Not recoverable |
| Caused by application | Caused by JVM/system |
| Handled using try-catch | Cannot be handled |

### Examples:
- Exception: `NullPointerException`
- Error: `OutOfMemoryError`

---

## 4. Exception Hierarchy in Java
--------------------------------

```text
java.lang.Object
   |
   └── java.lang.Throwable
         |
         ├── java.lang.Exception
         |       |
         |       ├── RuntimeException
         |       |
         |       └── Checked Exceptions
         |
         └── java.lang.Error
````

---

## 5. Types of Exceptions

---

### 5.1 Checked Exceptions

* Checked at **compile time**
* Must be handled using `try-catch` or `throws`

#### Examples:

* `IOException`
* `SQLException`
* `ClassNotFoundException`

---

### 5.2 Unchecked Exceptions (Runtime Exceptions)

* Checked at **runtime**
* Programmer mistakes

#### Examples:

* `NullPointerException`
* `ArithmeticException`
* `ArrayIndexOutOfBoundsException`

---

### 5.3 Errors

* Serious system failures
* Not handled by applications

#### Examples:

* `StackOverflowError`
* `OutOfMemoryError`

---

## 6. try-catch Block

---

### Syntax:

```java
try {
    // risky code
} catch(Exception e) {
    // handling code
}
```

### Example:

```java
try {
    int a = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Division by zero not allowed");
}
```

---

## 7. Multiple catch Blocks

---

```java
try {
    String s = null;
    System.out.println(s.length());
} catch (NullPointerException e) {
    System.out.println("Null value found");
} catch (Exception e) {
    System.out.println("Generic exception");
}
```

### Rule:

* Child exception must come **before** parent exception

---

## 8. finally Block

---

* Executes **always**
* Used for cleanup (closing DB, files, sockets)

```java
try {
    int a = 10 / 2;
} catch (Exception e) {
    e.printStackTrace();
} finally {
    System.out.println("Cleanup code");
}
```

### Note:

* `finally` executes even if `return` is present
* Not executed only if `System.exit(0)` is called

---

## 9. throw Keyword

---

Used to **explicitly throw an exception**.

```java
throw new ArithmeticException("Invalid operation");
```

---

## 10. throws Keyword

---

Used to **declare exceptions** to caller.

```java
void readFile() throws IOException {
    FileReader fr = new FileReader("abc.txt");
}
```

### Rule:

* Caller must handle the exception

---

## 11. Custom (User-Defined) Exceptions

---

### Step 1: Create Exception Class

```java
class InvalidAgeException extends Exception {
    InvalidAgeException(String msg) {
        super(msg);
    }
}
```

### Step 2: Use It

```java
if(age < 18) {
    throw new InvalidAgeException("Age must be 18+");
}
```

---

## 12. Checked vs Unchecked Custom Exceptions

---

| Checked                    | Unchecked                  |
| -------------------------- | -------------------------- |
| Extends `Exception`        | Extends `RuntimeException` |
| Compiler enforces handling | No compiler enforcement    |

---

## 13. Exception Propagation

---

Exception travels from **method → caller → JVM**.

```java
void m1() {
    m2();
}
void m2() {
    m3();
}
void m3() {
    int a = 10 / 0;
}
```

---

## 14. Exception Handling with Method Overriding

---

### Rules:

* Overridden method **cannot throw broader checked exception**
* Can throw same or subclass exception

```java
class Parent {
    void m() throws IOException {}
}

class Child extends Parent {
    void m() throws FileNotFoundException {}
}
```

---

## 15. try-with-resources (Java 7+)

---

Automatically closes resources.

```java
try (FileReader fr = new FileReader("a.txt")) {
    System.out.println("Reading file");
}
```

### Advantages:

* No finally block needed
* Prevents resource leaks

---

## 16. Common Runtime Exceptions

---

* `NullPointerException`
* `NumberFormatException`
* `ClassCastException`
* `IllegalArgumentException`

---

## 17. Best Practices

---

* Catch **specific exceptions**
* Never swallow exceptions
* Use logging frameworks
* Avoid empty catch blocks
* Do not use exceptions for normal flow

---

## 18. Real-Time Example (Service Layer)

---

```java
public User getUser(int id) {
    if(id <= 0) {
        throw new IllegalArgumentException("Invalid user id");
    }
    return userRepo.findById(id)
           .orElseThrow(() -> new UserNotFoundException("User not found"));
}
```

---

## 19. Interview Key Points

---

* Exception is an object
* Throwable is root class
* finally always executes
* Runtime exceptions are unchecked
* Custom exceptions improve clarity

---

## 20. Summary

---

✔ Java exception handling improves reliability
✔ Use checked exceptions for recoverable conditions
✔ Use unchecked exceptions for programming errors
✔ Always clean resources

---
