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

```

