## 1. What is the `this` Keyword? Why Do We Need It?

- `this` is a **reserved keyword** in Java. It cannot be used as a user-defined name such as a variable, method, class, or object name.  
- Java **reserved keywords** cannot be reused for identifiers (e.g., variable names, method names, class names, or object names).  
- The `this` keyword is primarily used to **refer to the current object** within a class — helping to differentiate between instance variables and local variables when they share the same name.

# Limitations of the `this` Keyword in Java

- The `this` keyword can **only be used** within:
  - **Non-static methods**
  - **Non-static blocks**
  - **Constructors**
  - **Abstract classes**
  - **Interfaces** (in default or non-static methods)

- The `this` keyword **cannot be used** inside:
  - **Static methods**
  - **Static blocks**

### Explanation:
The reason behind this limitation is that the `this` keyword always refers to the **current object** of a class.  
However, **static methods and static blocks** belong to the **class itself**, not to any particular object.  

# Example Programs Demonstrating the Use of the `this` Keyword in Java

## Example 1: Without Using the `this` Keyword

```java
public class Employee {

    int age = 25;

    public static void main(String[] args) {

        Employee emp = new Employee();
        emp.setAge(17);
        System.out.println(emp.getAge());
    }

    public void setAge(int age) {
        // age = age;
        /*
         * Here, the statement `age = age;` assigns the local variable `age` to itself.
         * The method first checks if a local variable named `age` exists.
         * Since it does, it never refers to the instance variable (object-level variable).
         */

        // new Employee().age = age;
        /*
         * In this case, a new anonymous object is created each time.
         * Once the `setAge()` method execution completes, the object is removed from the
         * stack memory and becomes eligible for garbage collection.
         * The garbage collector (GC) eventually deletes this anonymous object.
         */

        // emp.age = age;
        /*
         * This approach would work if we passed the current object (emp) as a method parameter.
         * The operation would then be performed on the current object itself.
         * However, this method reduces code readability, especially when there are multiple
         * attributes in the class, as we would need to repeatedly pass the current object.
         */
    }

    public int getAge() {
        return age;
    }
}
````

### Output

```
25
17
```

---

## Example 2: Using the `this` Keyword

```java
public class Employee {

    int age = 25;

    public static void main(String[] args) {

        Employee emp = new Employee();
        emp.setAge(17);
        System.out.println(emp.getAge());
    }

    public void setAge(int age) {
        this.age = age;
        /*
         * In this case, Java automatically passes the reference of the current object.
         * The `this` keyword helps to differentiate between the local variable `age`
         * and the instance variable `age`.
         * Therefore, the developer does not need to pass the object explicitly.
         */
    }

    public int getAge() {
        return age;
    }
}
```

### Output

```
17
```

---

## Example 3: Method Chaining Using the `this` Keyword

```java
public class Employee {

    int age = 25;
    String name;
    String jobRole;

    public String getJobRole() {
        return jobRole;
    }

    public Employee setJobRole(String jobRole) {
        this.jobRole = jobRole;
        return this;
    }

    public String getName() {
        return name;
    }

    public Employee setName(String name) {
        this.name = name;
        return this;
    }

    public static void main(String[] args) {

        Employee emp = new Employee();
        emp.setAge(17)
           .setName("Giri")
           .setJobRole("Developer");

        System.out.println(emp.getAge() + " " + emp.getName() + " " + emp.getJobRole());
    }

    public Employee setAge(int age) {
        this.age = age;
        return this;
    }

    public int getAge() {
        return age;
    }
}
```

### Output

```
17 Giri Developer
```

---

## Explanation of Method Chaining

Method chaining is a technique in Java that allows multiple method calls to be linked together in a single statement.
By returning `this` from each setter method, the current object reference is passed along, enabling a sequence of calls such as:

```java
emp.setAge(17).setName("Giri").setJobRole("Developer");
```

This approach improves **readability** and **fluency** in code, especially when initializing or configuring objects.

```


Since no instance is associated with static members, there is **no `this` reference** available in static contexts.
```

# Using the `this` Keyword in Interfaces and Abstract Classes in Java

## 1. `this` Keyword in Interfaces

The `this` keyword can also be used **inside an interface**, but it cannot access the **instance variables** of the interface directly.  
This is because all variables declared in an interface are **implicitly `public`, `static`, and `final`**, meaning they are constants and cannot be modified.

However, the `this` keyword can be used inside a **default method** of an interface to access the **implemented class’s members** or methods at runtime.

### Example: `this` Keyword in Interface

```java
import java.util.Arrays;

public class TestInterfaceThis {
    public static void main(String[] args) {
        Car j = new Jagur();
        j.setCarName("Audi");
    }
}

public interface Car {
    String name = "Jagur";
    void drive();

    default void setCarName(String name) {
        System.out.println(Arrays.toString(this.getClass().getDeclaredMethods()));
        // this.name = name;
        /*
         * The line above will cause a compilation error:
         * "Cannot assign a value to final variable 'name'"
         *
         * All variables in an interface are by default final and cannot be reassigned.
         * Although we can use the 'this' keyword in an interface, it cannot access
         * or modify interface variables directly. Instead, it can refer to
         * the object of the implementing class that is executing the interface method.
         */
    }
}
````

### Implementation Class

```java
public class Jagur implements Car {
    @Override
    public void drive() {
        // Implementation code
    }

    public void breaks() {
        // Additional method
    }
}
```

### Output

```
[public void com.jp.thisexample.Jagur.breaks(), public void com.jp.thisexample.Jagur.drive()]
```

### Explanation

* In the above program, when `setCarName()` is called, the `this` keyword refers to the **object of the implementing class (`Jagur`)**.
* Using `this.getClass().getDeclaredMethods()` prints the list of methods defined in the `Jagur` class.
* The `this` keyword here does **not** access interface variables (since they are static and final), but it does provide access to the runtime object that implements the interface.

---

## 2. `this` Keyword in Abstract Classes

The `this` keyword can also be used in **abstract classes**.
In this case, it refers to the **current instance of the subclass** that extends or implements the abstract class.

### Example: `this` Keyword in Abstract Class

```java
public class TestAbstractThis {
    public static void main(String[] args) {

        Jagur j = new Jagur() {
            @Override
            public void drive() {
                super.drive();
            }
        };
        j.setCarName("Tata");
        System.out.println(j.getName());
    }
}

public abstract class Jagur implements Car {
    String name = "Jagur";

    @Override
    public void drive() {
        // Default implementation
    }

    public void setCarName(String name) {
        this.name = name; // 'this' refers to the current object of the subclass
    }

    public String getName() {
        return name;
    }
}
```

### Output

```
Tata
```

### Explanation

* In this example, the abstract class `Jagur` implements the `Car` interface.
* The `this` keyword inside the `setCarName()` method refers to the **current object** of the anonymous subclass that extends `Jagur`.
* The method successfully updates the instance variable `name` of the abstract class object to `"Tata"`.
* Hence, when `getName()` is called, it returns `"Tata"`.

---

## Key Takeaways

* The `this` keyword can be used in **interfaces** (inside default methods) and **abstract classes**.
* In **interfaces**, it refers to the implementing class’s object but **cannot** modify interface variables (since they are static and final).
* In **abstract classes**, it behaves like in normal classes — referring to the current instance and allowing access or modification of instance variables.


# Understanding Memory Management When Using the `this` Keyword in Java

- The `this` keyword **does not create a new object** in memory.  
  Instead, it refers to the **current object** that is already created in the heap memory.  

- When a method or constructor uses the `this` keyword, it simply **holds a reference** to the existing object whose method or constructor is being executed.

- The use of `this` **does not affect the Garbage Collector (GC)** in any way.  
  Since no additional object is created, there is **no increase in memory usage**, and no new object becomes eligible for garbage collection because of `this`.

### Key Points:
- `this` only points to the **current instance** of a class.  
- It does **not allocate new memory** on the heap.  
- It helps maintain clear reference to the current object without impacting performance or memory management.

In summary, using the `this` keyword is **memory-efficient**, as it only provides a reference to the current object rather than creating a duplicate or new one.

### Picture Respresentation:
<img width="1332" height="685" alt="Screenshot 2025-10-19 065945" src="https://github.com/user-attachments/assets/8e154a6b-f23e-4352-975e-fe9c1824a469" />

<img width="1307" height="683" alt="Screenshot 2025-10-19 065958" src="https://github.com/user-attachments/assets/42d53a35-90a5-4ff1-99fc-943aa9ae13ee" />

# Final Keyword in Java

## What is `final`? Why is it introduced?

- `final` is a **keyword** and it is a **reserved word** in Java.  
- Whenever we want to make a value **constant**, Java provides the `final` keyword to achieve this.

---

## How to Use `final` with Variables?

- Once you assign a value to a `final` variable, **reassignment is not possible**.  
- We can declare `final` variables inside **both static and non-static methods** without any issue.

---

### Example Program: Local Final Variables in Static and Non-Static Methods

```java
public class FinalKeywordTest {

    public static void main(String[] args) {
        final int i = 10;
        i = 67; // ❌ Reassign not possible

        final int k;
        k = 12; // ✅ First-time assignment is possible
        k = 56; // ❌ Reassign not possible
    }

    public void show() {
        final int j = 90;
        j = 78; // ❌ Reassign not possible
    }
}
````
# Object Scope of Non-Static Final Variables in Java

## Explanation

- The **object scope** of variables (non-static) declared in a class means that each object of the class gets its own copy of those variables.  
- For `final` non-static variables, **you cannot reassign them inside any method** of the class because Java cannot determine which method will be called first.  
- However, **you can assign a value in the constructor**, since the constructor is executed during **object creation**, making it the **first-time initialization** point.

---

### Example Program: Non-Static Final Variables with Object Scope

```java
public class FinalKeywordTest {

    final int h; // Object-scope, class-level declared non-static attribute

    public FinalKeywordTest() {
        h = 89; // ✅ Assignment through constructor is allowed (first-time initialization)
    }

    public static void main(String[] args) {
        // Object creation
        FinalKeywordTest obj = new FinalKeywordTest();
    }

    public void show() {
        h = 67; // ❌ Not allowed — reassignment to final variable
    }
}
````

# Static Final Variables in Java

## Explanation

- For **class-level static variables**, you **cannot assign values inside a constructor**, not even for the first time.  
- This is because **static variables’ memory is allocated before object creation**, meaning constructors (which run during object creation) cannot modify them.  
- Static `final` variables must be initialized **at the time of declaration** or **inside a static block**.

---

### Example Program: Static Final Variables

```java
public class FinalKeywordTest {

    final static int h = 0; // ✅ Initialization at declaration
    static final int y = 0; // ✅ Initialization at declaration

    public FinalKeywordTest() {
        h = 89; // ❌ Not allowed — cannot assign inside constructor
        y = 90; // ❌ Not allowed — cannot assign inside constructor
    }

    public static void main(String[] args) {
        // Object creation
        FinalKeywordTest obj = new FinalKeywordTest();
    }
}
````

## Naming Convention for Constants

* Constant names or `final` variable names should be written in **UPPER_CASE** to distinguish them from regular variables.
* This convention improves **readability** and **understanding** of the code.

> ⚠️ Note: This is a **Java recommendation**, not a mandatory rule.

# Example: Regular vs Final Variables in Java

```java
int age = 50;       // Regular variable
final double PI = 3.14;  // Final variable (constant)
````

---

✅ **Explanation:**

* `age` is a **regular variable**, so its value can be changed later in the program.
* `PI` is declared as **final**, meaning its value **cannot be modified** after initialization.


# Using `final` with Method Parameters in Java

## Explanation

- When a parameter is declared as **`final`**, its value **cannot be modified** within the method.  
- This ensures that the parameter acts as a **read-only variable** inside the method.

---

### Example Program: Final Parameters

```java
public class FinalKeywordTest {

    public static void main(String[] args) {
        FinalKeywordTest finalKeywordTest = new FinalKeywordTest();
        finalKeywordTest.show(100);
    }

    public void show(final int p) {
        p = 78; // ❌ Not possible — reassignment to final parameter
        System.out.println(p);
    }
}
````

---

✅ **Key Points:**

* `final` parameters prevent **accidental changes** to the passed arguments.
* Commonly used to ensure **data integrity** within methods.
* The parameter can be **read** but **not modified** inside the method.

# Using `final` with Methods in Java

## How to Use `final` with Methods?

- If you don't want other classes to **override** a method, declare it using the `final` keyword.  
- You can also add the `final` keyword to the `main` method.  
  However, even without `final`, the `main` method **cannot be overridden** (since it’s static).

---

## Example

```java
public class FinalKywordTest {

    public final static void main(String[] args) {
        // Main method declared as final
    }

    public final void show(final int p) {
        // Method declared as final
    }
}

class Program2 extends FinalKywordTest {

    @Override
    public final void show(final int p) { // ❌ Not possible to override
        // Compilation error
    }

    public static void main(String[] args) { // ❌ Not possible to override
        args = new String[]{"Gireesh"};
        System.out.println(args);
    }
}
````

---

## Compilation Errors

```
'show(int)' cannot override 'show(int)' in 'com.jp.finalkeyword.FinalKywordTest';
overridden method is final

'main(String[])' cannot override 'main(String[])' in 'com.jp.finalkeyword.FinalKywordTest';
overridden method is final
```

---

✅ **Key Point:**
A `final` method **cannot be overridden** by a subclass. This ensures that the method’s behavior remains consistent across inheritance.

# Using `final` with Classes in Java

## How to Use `final` with Classes?

- If you don't want other classes to perform **inheritance**, declare the class as `final`.  
- When a class is marked as `final`, **method overriding is also not possible**, since no class can extend it.

---

## Example Program

```java
public final class FinalKywordTest {

    static final int h = 0;

    public FinalKywordTest() {
        // h = 89; // ❌ Not allowed because 'h' is final
    }

    public final static void main(String[] args) {
        // Main method
    }

    public final void show(final int p) {
        // Final method
    }
}

class Program2 extends FinalKywordTest { // ❌ Inheritance not possible because class is final

    // @Override
    /*
    public final void show(final int p) {
        // Not possible — parent class is final
    }
    */

    /*
    public static void main(String[] args) {
        args = new String[]{"Gireesh"};
        System.out.println();
    }
    */
}
````

---

## Compilation Error

```
Cannot inherit from final 'com.jp.finalkeyword.FinalKywordTest'
```

---

✅ **Key Point:**
A `final` class **cannot be extended** by any other class. This ensures the class’s design and implementation remain unchanged, providing immutability and security in your program.

# Abstract Class in Java

## Key Points

- We **cannot make an abstract class as final** because the main purpose of an abstract class is to allow **other classes to implement its abstract methods**.  
  If we declare it as `final`, it **cannot be extended**, and hence the purpose of abstraction is lost.

- However, we **can declare non-abstract methods as `final`** inside an abstract class.  
  This prevents subclasses from overriding those specific methods.

---

## Example

```java
final abstract class Test1 {
    public final void display() {
        System.out.println("display");
    }

    abstract void draw();
}
````

### Compilation Error

```
Illegal combination of modifiers 'abstract' and 'final'
```

---

## Additional Note

* Inside an **interface**, all variables are **public**, **static**, and **final** by default.
  This means interface variables act as constants and cannot be modified.


# Important Points to Remember

---

## Difference Between `final` and `const`

- In **C and C++**, developers use the `const` keyword to make a variable constant.  
- In **Java**, the same functionality is achieved using the **`final`** keyword.  
- The **`const`** keyword exists in Java syntax but is **not used** — it is a **reserved keyword** with no implementation purpose.

---

## Difference Between `final`, `finally`, and `finalize()`

| Keyword / Method | Type | Usage |
|------------------|------|--------|
| `final` | Keyword | Used to declare constants, prevent method overriding, or prevent inheritance of a class. |
| `finally` | Block | Used in **exception handling**. The `finally` block always executes whether an exception occurs or not. |
| `finalize()` | Method | Used by the **Garbage Collector** to clean up unreferenced objects before memory deallocation. *(Deprecated since Java 9)* |

---

## Example — `final` with Objects

```java
final StringBuilder sb = new StringBuilder("Hello");
sb.append("Reedy");               // ✅ Allowed — content modification
sb = new StringBuilder("Hi");     // ❌ Not allowed — cannot reassign final reference
System.out.println(sb);
````

### Explanation:

* The `final` keyword **does not make the object immutable**.
* It only prevents the variable reference (`sb`) from pointing to a new object.
* You can still **modify the internal state** of the object (like appending text to `StringBuilder`).

---

## Additional Notes

* A `final` variable **can be assigned to `null`**, but only once.
* You can also declare a **blank final variable**, which must be **initialized later** (for example, inside a constructor).


## How to create custom immutable object?
