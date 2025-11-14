# Functional Interfaces

## 1. What is a Functional Interface?
A **functional interface** is an interface that defines **exactly one abstract method**. It can also include any number of **non-abstract methods** such as `default`, `static`, or `private` methods.  

---

## 2. When and Why Was It Introduced?
- Introduced in **Java 1.8**.
- Functional interfaces were introduced to support **lambda expressions** and **streams**.
- Also known as **SAM (Single Abstract Method)** interfaces.
- There is no restriction on the number of non-abstract methods present in a functional interface.

---

## 3. How to Define and Implement a Functional Interface?

```java
@FunctionalInterface // optional but recommended
public interface Car {
    void drive(); // single abstract method

    default void honk() {
        // implementation
    }

    static void drive2() {
        // implementation
    }

    private void honk2() {
        // implementation
    }
}
````

> You can explore the **`java.util.function`** package to see the built-in functional interfaces provided by Java.

---

## 4. Can We Have More Than One Method in a Functional Interface?

Yes, but only **one abstract method** is allowed. You can include **any number of non-abstract methods** (default, static, private).

---

## 5. How Do Anonymous Classes Help in Implementing Functional Interfaces?

Anonymous classes allow you to implement a functional interface **without creating a separate class**.

### Example Program

```java
@FunctionalInterface
public interface Car {
    void drive();
}

public class Test {
    public static void main(String[] args) {
        Car car = new Car() {
            @Override
            public void drive() {
                System.out.println("Drive method called");
            }
        };
        car.drive();
    }
}
```

### Output

```
Drive method called
```

# Lambda Expressions in Java

## 1. What is a Lambda Expression?
- A **lambda expression** provides a concise way to write code, reducing the number of lines.
- It allows the creation of **anonymous functions**.
- Enables **functional programming** in Java.
- Lambda expressions are primarily used to **implement functional interfaces**, or in other words, a functional interface can invoke a lambda expression.

---

## 2. Implementing Functional Interfaces with Lambda Expressions
Lambda expressions can be used to implement functional interfaces without creating separate classes.  

---

## 3. Lambda Expressions with Parameters

### a. No Parameters

#### Example Program
```java
public class LambdaExpressionTest {
    public static void main(String[] args) {
        // Traditional class implementation
        Audi audi = new Audi();
        audi.drive();

        // Anonymous class implementation
        Car c1 = new Car() {
            @Override
            public void drive() {
                System.out.println("BMW Driving");
            }
        };
        c1.drive();

        // Lambda expression implementation
        Car c2 = () -> System.out.println("TATA car driving");
        c2.drive();
    }
}

class Audi implements Car {
    @Override
    public void drive() {
        System.out.println("Audi car driving");
    }
}

@FunctionalInterface
interface Car {
    void drive();
}
````

---

### b. Single Parameter

#### Example Program

```java
public class LambdaExpressionTest {
    public static void main(String[] args) {
        // Traditional implementation
        Audi audi = new Audi();
        audi.drive(200);

        // Anonymous class implementation
        Car c1 = new Car() {
            @Override
            public void drive(int speed) {
                System.out.println("BMW Driving");
            }
        };
        c1.drive(150);

        // Lambda expression implementation
        Car c2 = speed -> System.out.println("TATA car driving " + speed);
        c2.drive(100);
    }
}

class Audi implements Car {
    @Override
    public void drive(int speed) {
        System.out.println("Audi car driving " + speed);
    }
}

@FunctionalInterface
interface Car {
    void drive(int speed);
}
```

---

### c. Multiple Parameters

#### Example Program

```java
public class LambdaExpressionTest {
    public static void main(String[] args) {
        // Traditional implementation
        Audi audi = new Audi();
        audi.drive(200, "Audi");

        // Anonymous class implementation
        Car c1 = new Car() {
            @Override
            public void drive(int speed, String model) {
                System.out.println("Driving " + model);
            }
        };
        c1.drive(150, "BMW");

        // Lambda expression implementation
        Car c2 = (speed, model) -> System.out.println("TATA car driving " + speed + ", model is " + model);
        c2.drive(100, "TATA");
    }
}

class Audi implements Car {
    @Override
    public void drive(int speed, String model) {
        System.out.println("Car driving " + speed + ", Model is " + model);
    }
}

@FunctionalInterface
interface Car {
    void drive(int speed, String model);
}
```

---

## 4. Lambda Expressions with Return Type

#### Example Program

```java
public class LambdaExpressionReturnTypeTest {
    public static void main(String[] args) {
        Square square = (length, width) -> length * width;
        System.out.println(square.getArea(12, 34));
    }
}

@FunctionalInterface
interface Square {
    int getArea(int length, int width);
}
```

---

## Important Points to Remember About Lambda Expressions

1. **Single-line functions** do not require curly braces `{}`. Multiple lines require curly braces.

   ```java
   Car c2 = () -> System.out.println("TATA car driving");
   ```

2. **Single argument** does not require parentheses `()`. Multiple arguments require parentheses.

   ```java
   Car c2 = speed -> System.out.println("TATA car driving " + speed);
   ```

3. **No need to specify argument data types**, as the compiler can infer them from the functional interface.

   ```java
   Car c2 = (speed, model) -> System.out.println("TATA car driving " + speed + ", model is " + model);
   ```

4. **Single-line return values** do not require the `return` keyword.

   ```java
   Square square = (length, width) -> length * width;
   ```

## Note
Lambda expressions are recommended when the method is used in multiple places in the class or outside of the class.
   
# forEach Method in Java

## Overview
- If you want to perform any operations (like reversing a list), using the **traditional for loop** is fine.  
- However, if you just want to **print the list values**, then declaring variables, incrementing them, and putting conditions only for printing is not a good approach.  
  To simplify this, Java introduced the **Enhanced For Loop (for-each loop)**.

---

## When to Use Enhanced For Loop
- If you **do not require an index** and want to **retrieve elements one by one** from top to bottom, go for the **enhanced for-each loop**.
- In **Java 8**, functional programming was introduced. It allows using **lambda expressions**, **method chaining**, and writing code in a concise way.
- However, the **enhanced for loop** still requires multiple lines of code and is considered **external looping**.

---

## Method Chaining and Looping
- **Method chaining** is **not possible** with traditional for loops or enhanced for-each loops.
- Traditional and enhanced for loops are **external looping mechanisms**.
- Java introduced the **forEach() method** in **version 1.8**, which supports:
  - **Method chaining**
  - **Functional programming**

---

## Usage
- The `forEach()` method can be used with **Collection Framework interfaces** and their **implementation classes**.
- It provides an **internal looping mechanism**.

---

## Consumer Interface
- The `forEach()` method accepts an instance of the **Consumer** pre-defined functional interface as a parameter.
- **Consumer** comes from the `java.util.function` package and was introduced in **Java 1.8**.

### Consumer Functional Interface
```java
void accept(T t);
````

* Takes **one parameter** but **returns no value**.

---

## BiConsumer Interface (for Map)

* For the **Map interface**, the `forEach()` method accepts a **BiConsumer** pre-defined functional interface instance as a parameter.
* **BiConsumer** also comes from the `java.util.function` package and was introduced in **Java 1.8**.

### BiConsumer Functional Interface

```java
void accept(T t, U u);
```

* Takes **two parameters (key, value pair)** but **returns no value**.

---

## Important Notes

* The `forEach()` method **cannot be used directly with arrays**.


# Example Program for forEach() Method
---------------------------------------------

```java
import java.util.*;
import java.util.function.*;

public class ForeachMethodPratice {

    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1, 5, 7, 8, 10);

        // Traditional for loop
        /*
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
        */

        // Enhanced for loop
        /*
        for (Integer i : list) {
            System.out.println(i);
        }
        */

        // forEach method
        // Anonymous inner class
        /*
        Consumer<Integer> con = new Consumer<Integer>() {
            @Override
            public void accept(Integer i) {
                System.out.println(i);
            }
        };
        */

        // Using Lambda Expression
        // Consumer<Integer> con = i -> System.out.println(i);
        list.forEach(i -> System.out.println(i));

        Map<Integer, String> map = new HashMap<>();
        map.put(101, "Ramesh");
        map.put(102, "Ramu");
        map.put(103, "Rajesh");

        // Anonymous inner class
        /*
        BiConsumer<Integer, String> biCon = new BiConsumer<Integer, String>() {
            @Override
            public void accept(Integer key, String value) {
                System.out.println(key + " >> " + value);
            }
        };
        */

        // Using Lambda Expression for Map
        // BiConsumer<Integer, String> biCon = (key, value) -> System.out.println(key + " >> " + value);
        map.forEach((key, value) -> System.out.println(key + " >> " + value));

        int[] arr = {1, 7, 9, 4, 7, 0};
        // arr.forEach -> directly support forEach method is not available for arrays.
    }
}
````

---

# Output

---

1
5
7
8
10
101 >> Ramesh
102 >> Ramu
103 >> Rajesh

# Method Reference in Java

## What is Method Reference?

→ **Method References** in Java are a shorthand, more readable way to use **Lambda Expressions** for invoking methods or constructors.  
They allow you to reference methods or constructors directly using the `(::)` symbol.

- Using method references, we can simplify lambda expressions.  
- Simple lambda expressions can be converted into method references.  
- Complex lambda expressions **cannot** be converted into method references.

---

## Why Do We Need It?

- Cleaner and more concise code  
- Reduces boilerplate code  
- Better type inference and generics handling  
- Reusability of existing methods  
- Improves performance as well


# Types of Method Reference

## 1. Reference to Static Methods

**Example:**

```java
names.forEach(MethodReferencePratice::greet);
````

---

## 2. Reference to Instance Methods of a Particular Object

**Example:**

```java
MethodReferencePratice methodReferencePratice = new MethodReferencePratice();
names.forEach(methodReferencePratice::print);
```

---

## 3. Reference to an Instance Method of an Arbitrary Object of a Particular Type

### Description

→ This type of method reference is used when you want to call an **instance method** on each object of a particular class, without referring to any specific instance directly.
It allows you to refer to an instance method that can be executed on any object of that class.

In Java, this concept is represented using the syntax:

```java
ClassName::instanceMethodName
```

It is commonly used with functional interfaces like `Consumer`, `Function`, or `Predicate` — especially in combination with methods such as `forEach()`, `map()`, or `filter()` in the **Stream API**.

---

### Example

```java
import java.util.Arrays;
import java.util.List;

public class Example {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Ravi", "Kiran", "Sneha", "Arjun");

        // Using a lambda expression
        names.forEach(name -> System.out.println(name.toUpperCase()));

        // Using method reference (reference to an instance method of a particular type)
        names.forEach(String::toUpperCase); // creates a stream of uppercase names (no print yet)

        // If you want to print each uppercase name
        names.stream()
             .map(String::toUpperCase)
             .forEach(System.out::println);
    }
}
```

---

### Explanation

* `String::toUpperCase` is a **reference to an instance method** of the `String` class.
* It means: “Call the `toUpperCase()` method on each `String` object present in the list.”
* The method reference doesn’t specify which instance to use — it applies to *each* element in the collection automatically.

This approach improves **readability**, **reusability**, and **conciseness** of your code by replacing verbose lambda expressions with simple method references.

# Reference to a Constructor

## Example

```java
names.forEach(Employeee::new);

```

# Final Example Program

```java
import java.util.*;

public class MethodReferencePratice {

    public static void main(String[] args) {

        List<String> names = Arrays.asList("Rama", "Ganesh", "Suresh", "Banny");

        // System.out.println(greet("Rama")); method call will not be possible

        // Lambda expression
        // names.forEach(name -> greet(name));

        // Reference to static methods
        names.forEach(MethodReferencePratice::greet);

        System.out.println("=============================");

        // Reference to instance method of a particular object
        MethodReferencePratice methodReferencePratice = new MethodReferencePratice();
        names.forEach(methodReferencePratice::print);

        System.out.println("============================");

        // Here 'out' is an instance of PrintStream
        names.forEach(System.out::println);

        System.out.println("=============================");

        // Collections.sort(names, (s1, s2) -> s1.compareTo(s2));
        // Reference to an instance method of an arbitrary object of a particular type
        Collections.sort(names, String::compareTo);
        names.forEach(System.out::println);

        // Reference to a Constructor
        names.forEach(Employeee::new);
    }

    public static void greet(String name) {
        System.out.println("Hey, Hello! my name is " + name);
    }

    public void print(String name) {
        System.out.println("My name is " + name + " Good Morning!");
    }
}

class Employeee {
    String name;

    public Employeee(String name) {
        this.name = name;
    }
}
````

---

## Output

```
Hey, Hello! my name is Rama
Hey, Hello! my name is Ganesh
Hey, Hello! my name is Suresh
Hey, Hello! my name is Banny
=============================
My name is Rama Good Morning!
My name is Ganesh Good Morning!
My name is Suresh Good Morning!
My name is Banny Good Morning!
============================
Rama
Ganesh
Suresh
Banny
=============================
Banny
Ganesh
Rama
Suresh
```



