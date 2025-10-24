# Type Conversion in Java

## 1. What is Type Conversion? Why Do We Need It?

In Java, every value must be stored in a specific data type such as:

- **byte**, **short**, **int**, **long**, **float**, **double** → for numeric data  
- **char** → to store a single character  
- **String** → a special non-primitive data type used to store text  

Type conversion is the process of **converting one data type into another**, for example:  
- `short` → `int`  
- `int` → `double`  
- `int` → `String`

Java introduced the concept of **type conversion** to make it easier to work with different data types in programs.  
By default, **Java does not perform type conversion automatically** in some cases — the **developer must explicitly instruct** Java when and how to perform the conversion.

- When we **convert primitive data types** (like `int`, `short`, `byte`), it is called **Type Conversion**.  
- When we **convert non-primitive data types** (like `String`, `Class`, `Interface`), it is called **Type Casting**. 

# Different Types of Type Conversions in Java

In Java, there are **two types of type conversions**:

1. **Implicit Conversion** (also known as **Automatic Conversion**, **Widening**, or **Upcasting**)  
2. **Explicit Conversion** (also known as **Manual Conversion**, **Narrowing**, or **Downcasting**)

---

## a. Implicit Conversion (Automatic Conversion / Widening / Upcasting)

- In this type of conversion, **Java automatically performs the conversion**.  
- A **larger range data type** can store a **smaller range data type** value.

**Example:**
```java
short s = 10;
byte b = 30;
s = b;  // Implicit conversion from byte to short
````

---

## b. Explicit Conversion (Manual Conversion / Narrowing / Downcasting)

* In this type of conversion, the **developer explicitly instructs Java** to perform the conversion.
* A **smaller range data type** can store a **larger range data type** value, but **data loss may occur**.
* Java will **not perform this automatically**.

**Example:**

```java
int i = 100;
byte b;
b = (byte) i;  // Explicit conversion from int to byte
```

# Limitations of Type Conversion in Java

## a. Boolean Type Conversion Limitation

- We **cannot convert a `boolean` data type** to any other primitive data type.  
- This is because `boolean` represents **logical values** (`true` or `false`), not numeric data.  
- Attempting such a conversion will result in a **compile-time error**: *incompatible types*.

**Example:**
```java
boolean boo = true;
short s = (short) boo;  // ❌ Error: Inconvertible types; cannot cast 'boolean' to 'short'
````

**Error Message:**

```
Inconvertible types; cannot cast 'boolean' to 'short'
```

# Automatic Type Promotion in Java

## What is Automatic Type Promotion?

- When we perform **arithmetic operations** (such as addition, subtraction, multiplication, or division) on **`byte`**, **`char`**, or **`short`** primitive data types,  
  Java **automatically promotes** these values to the **`int`** data type before performing the operation.

---

### Example 1: Promotion During Arithmetic Operations
```java
byte a = 10;
byte b = 20;
byte c = a + b;  // ❌ Error: result is promoted to int
````

In the above example, although both operands are `byte`,
the result of `a + b` is automatically **promoted to `int`**,
so explicit casting is required:

```java
byte c = (byte) (a + b);  // ✅ Correct
```

---

### Example 2: Automatic Promotion in Character Operations

When we perform any arithmetic operation between **two `char` values**,
Java converts them to their **ASCII (Unicode) integer values**, performs the operation,
and returns an **integer result**.

```java
System.out.println('A' + 'B');  // Output: 131
```

Explanation:

* `'A'` has ASCII value **65**
* `'B'` has ASCII value **66**
* `65 + 66 = 131` → automatic type promotion from `char` to `int`


# Example Program: Type Conversion in Java

```java
public class TypeConversionTest {

    public static void main(String[] args) {

        byte b = 40;
        short s = 100;
        int i = 400;
        float f = 6000f;
        double d = 100.09;
        char c = 'A';
        boolean boo = true;

        // Implicit type conversion
        s = b;  // byte to short
        i = b;  // byte to int

        // Explicit type conversion
        b = (byte) i;  // int to byte
        s = (short) i; // int to short

        /*
         * If range is less than integer, then Auto-Promotion to integer occurs.
         * If range is higher than integer, then Auto-Promotion to higher range occurs.
         */
        int j = s + 'V';  // char 'V' promoted to int
        float l = i + f;  // int promoted to float

        // s = (short) boo;  // ❌ Error: cannot cast boolean to short

        System.out.println(b);
    }
}
````

# Wrapper Classes in Java

## What is a Wrapper Class? Why Do We Need It?

- Java supports **primitive data types** such as `int`, `float`, `char`, `boolean`, etc.  
  Because of this, **Java is not a fully object-oriented programming language**,  
  since primitives are **not objects**.

- When Java introduced the **Collections Framework**,  
  it only supported **objects**, not primitive data types.  
  Therefore, to store primitive values in collections like `ArrayList`, `HashMap`, etc.,  
  Java introduced **Wrapper Classes** — which convert primitive data types into objects.


# How to Use Wrapper Classes in Java

## What is a Wrapper Class?

- A **Wrapper Class** is a class that **wraps a primitive data type** into an **object**.  
- Each primitive type in Java has a corresponding wrapper class:

| Primitive Type | Wrapper Class |
|-----------------|----------------|
| `byte`          | `Byte`         |
| `short`         | `Short`        |
| `int`           | `Integer`      |
| `float`         | `Float`        |
| `double`        | `Double`       |
| `char`          | `Character`    |
| `boolean`       | `Boolean`      |

- All **Wrapper Classes** start with a **capital letter**.

---

### Example:
```java
Integer i = new Integer(); // ❌ Compilation error
````

> Wrapper classes **do not have a default (no-argument) constructor**.
> Their main purpose is to **wrap primitive data types into objects**.

---

## Boxing

Boxing means **converting a primitive data type into its corresponding wrapper object**.

```java
int i = 39;
Integer i2 = new Integer(i);  // Boxing (Deprecated)
```

* The above statement **wraps the primitive value `i` into an `Integer` object**.
* However, the `new Integer(i)` constructor is **deprecated**.
* The recommended way is:

```java
Integer i2 = Integer.valueOf(i);  // ✅ Preferred method
```

---

## Auto-Boxing

**Auto-Boxing** is when Java **automatically converts a primitive type into a wrapper object** internally.

```java
int i = 39;
Integer i3 = i;  // Auto-Boxing
```

> Java automatically creates a wrapper object for the primitive value.

---

## Unboxing

**Unboxing** is the reverse process — converting a **wrapper object back into a primitive type**.

```java
Integer i3 = Integer.valueOf(55);
int ii = i3.intValue();  // Unboxing
```

---

## Auto-Unboxing

**Auto-Unboxing** is when Java **automatically converts a wrapper object to its corresponding primitive type**.

```java
Integer i3 = Integer.valueOf(55);
int ii = i3;  // Auto-Unboxing
```

> Java automatically extracts the primitive value from the wrapper object.


# Example Program: Wrapper Classes in Java

```java
import java.util.ArrayList;

public class WrapperClassTest {
    public static void main(String[] args) {

        int i = 20;

        // Integer(55) is deprecated
        // Integer ii = new Integer(55);
        Integer ii = Integer.valueOf(88); // Boxing
        System.out.println(ii);

        Integer i2 = 89; // Auto-boxing
        System.out.println(i2);

        int i3 = i2.intValue(); // Unboxing
        int i4 = i2;            // Auto-unboxing

        System.out.println(i3);
        System.out.println(i4);

        ArrayList<Integer> al = new ArrayList<Integer>();
        al.add(34);                  // Auto-boxing
        al.add(Integer.valueOf(45)); // Boxing
        int i5 = al.get(0);          // Auto-unboxing
        System.out.println(i5);

        String s = "14";
        int i7 = Integer.valueOf(s);   // Returns reference/object type (Integer)
        int i8 = Integer.parseInt(s);  // Returns primitive data type (int)
        System.out.println(i7);
    }
}
````

# Ternary Operator in Java

## What is a Ternary Operator? Why Do We Need It?

- The **Ternary Operator** is a **conditional operator** used to **shorten simple `if-else` statements** in Java.  
- It helps make the code **more compact and readable**.  
- The **Ternary Operator** was introduced in **Java 1.0**.

---

## Syntax of the Ternary Operator

```java
condition ? expression1 : expression2
````

* The term **"ternary"** means it is composed of **three parts**:

  1. **Condition** → a boolean expression that evaluates to `true` or `false`.
  2. **Expression1** → executed if the condition is `true`.
  3. **Expression2** → executed if the condition is `false`.

---

# How to Use the Ternary Operator in Java

## Example Program

```java
public class TernaryOperatorTest {
    public static void main(String[] args) {

        int i = 100;
        int j = 70;
        int max;

        // Old style using if-else
        if (i > j)
            max = i;
        else
            max = j;

        System.out.println(max);

        // Using ternary operator
        System.out.println((i > j) ? i : j);

        // Old way for checking 3 conditions
        int number = 10;
        if (number == 0)
            System.out.println("The number is zero");
        else if (number > 0)
            System.out.println("The number is positive");
        else if (number < 0)
            System.out.println("The number is Negative");

        // Using ternary operator for checking three conditions
        System.out.println(
            (number == 0) ? "Zero" :
            (number > 0) ? "Positive" : "Negative"
        );
    }
}
````

---

## Output

```
100
100
The number is positive
Positive
```

# Important Points to Remember About Ternary Operator

- Whenever we use the **ternary operator**, it must **always return a value**.  
  The `void` return type is **not suitable** for ternary operators.

- **Java recommends** using the ternary operator **only for simple `if-else` or two-condition checks**,  
  because using it for **multiple `if-else` conditions** can reduce **code readability**.

- In **ternary operators**, we **cannot create code blocks** (`{ }`).  
  Only expressions that return values are allowed.


# **Switch Statement in Java – In-Depth Explanation**

## **1. Introduction**

The **`switch` statement** in Java is a **multi-branch selection statement** that allows the execution of one block of code among many alternatives, based on the value of an **expression**.  
It is often used as an alternative to multiple `if-else` statements for better readability and performance.

---

## **2. Syntax of Switch Statement**

```java
switch (expression) {
    case value1:
        // statements
        break;

    case value2:
        // statements
        break;

    ...

    default:
        // statements
}
````

---

## **3. Explanation of Each Part**

| Part           | Description                                                                                                         |
| -------------- | ------------------------------------------------------------------------------------------------------------------- |
| **expression** | This is evaluated once. The result is compared with the values of each `case`.                                      |
| **case value** | Each `case` label represents a possible value of the expression.                                                    |
| **break**      | It ends the current case and exits the switch block. Without `break`, control will "fall through" to the next case. |
| **default**    | Optional. Executes when none of the `case` values match the expression.                                             |

---

## **4. Rules for Using Switch Statement**

1. The **expression** must evaluate to:

   * `byte`, `short`, `char`, `int`
   * `enum` type (Java 5+)
   * `String` type (Java 7+)
   * `var` (Java 10+, depending on inferred type)

2. **Duplicate case values are not allowed.**

3. **`break`** is optional — if omitted, execution continues into the next case (known as **fall-through**).

4. **`default`** is optional but recommended.

5. **Expression and case values must be of the same type** (or compatible type).

---

## **5. Flowchart**

```
      +-------------------+
      |   Evaluate expr   |
      +--------+----------+
               |
       +-------v--------+
       | Compare value1  |
       +-------+--------+
               |
      +--------v---------+
      | Match? Execute   |
      |   case block     |
      +--------+---------+
               |
             break?
             /   \
          yes     no
         exit   continue next case
```

---

## **6. Example 1: Basic Integer Example**

```java
public class SwitchExample {
    public static void main(String[] args) {
        int day = 3;

        switch (day) {
            case 1:
                System.out.println("Monday");
                break;
            case 2:
                System.out.println("Tuesday");
                break;
            case 3:
                System.out.println("Wednesday");
                break;
            default:
                System.out.println("Invalid day");
        }
    }
}
```

**Output:**

```
Wednesday
```

---

## **7. Example 2: String in Switch**

```java
public class StringSwitch {
    public static void main(String[] args) {
        String browser = "chrome";

        switch (browser) {
            case "chrome":
                System.out.println("Launching Chrome...");
                break;
            case "firefox":
                System.out.println("Launching Firefox...");
                break;
            case "edge":
                System.out.println("Launching Edge...");
                break;
            default:
                System.out.println("Unsupported Browser");
        }
    }
}
```

**Output:**

```
Launching Chrome...
```

---

## **8. Example 3: Fall-Through Example**

```java
public class FallThroughExample {
    public static void main(String[] args) {
        int number = 2;

        switch (number) {
            case 1:
                System.out.println("One");
            case 2:
                System.out.println("Two");
            case 3:
                System.out.println("Three");
                break;
            default:
                System.out.println("Invalid");
        }
    }
}
```

**Output:**

```
Two
Three
```

**Explanation:**
Since there is **no `break` after case 2**, the execution “falls through” to case 3.

---

## **9. Example 4: Using Enum in Switch**

```java
enum Day {
    MON, TUE, WED, THU, FRI, SAT, SUN
}

public class EnumSwitch {
    public static void main(String[] args) {
        Day today = Day.WED;

        switch (today) {
            case MON:
                System.out.println("Start of the week!");
                break;
            case WED:
                System.out.println("Midweek day!");
                break;
            case FRI:
                System.out.println("Weekend is near!");
                break;
            default:
                System.out.println("Enjoy your day!");
        }
    }
}
```

**Output:**

```
Midweek day!
```

---

## **10. Example 5: Java 14 Enhanced Switch Statement**

Java 14 introduced an **enhanced switch expression** that allows returning values directly.

### **Syntax:**

```java
String result = switch (expression) {
    case value1 -> "Result 1";
    case value2 -> "Result 2";
    default -> "Default Result";
};
```

### **Example:**

```java
public class EnhancedSwitch {
    public static void main(String[] args) {
        int day = 5;

        String type = switch (day) {
            case 1, 2, 3, 4, 5 -> "Weekday";
            case 6, 7 -> "Weekend";
            default -> "Invalid";
        };

        System.out.println(type);
    }
}
```

**Output:**

```
Weekday
```

---

## **11. Advantages of Switch Statement**

✅ **Improves code readability**
✅ **Faster execution** compared to multiple `if-else` conditions
✅ **Compact structure** for multiple comparisons
✅ **Supports modern features** like switch expressions (Java 14+)

---

## **12. Limitations**

❌ Only works with **discrete values**, not ranges or conditions (like `<`, `>`).
❌ **Fall-through** can cause unintended results if `break` is omitted.
❌ **Not ideal** for complex decision-making logic.

---

## **13. Summary**

| Feature              | Description                          |
| -------------------- | ------------------------------------ |
| **Introduced in**    | Java 1.0                             |
| **Supports**         | byte, short, char, int, enum, String |
| **Enhanced version** | Java 14 (switch expressions)         |
| **Default block**    | Optional                             |
| **Break keyword**    | Prevents fall-through                |

---

## **14. Practice Exercise**

**Q1:** Write a program using a switch statement to display the month name based on month number.
**Q2:** Modify the program to use **enhanced switch** syntax (Java 14+).
**Q3:** Create a switch that uses an **enum** to represent traffic light colors.

---

## **15. Conclusion**

The `switch` statement is a **powerful decision-making construct** that simplifies code where multiple conditions depend on a single variable.
With Java’s **enhanced switch expression**, it’s now even more concise and expressive.

---
