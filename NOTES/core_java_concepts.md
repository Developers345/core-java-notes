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

# Java Switch Expression Example

## Enum Declaration

```java
enum TRAFFIC_LIGHT {
    RED, YELLOW, GREEN, P;
}
````

---

## Switch Expression Example in Java

```java
public class SwitchExpressionTest {

    public static void main(String[] args) {

        int number = 11;
        String resultNumber;

        resultNumber = switch (Integer.compare(number, 0)) {
            case 0 -> "Zero";
            case 1 -> {
                if (number % 2 == 0)
                    yield "Positive & Even";
                else
                    yield "Positive & Odd";
            }
            case -1 -> "Negative";
            default -> throw new IllegalStateException("Unexpected value: " + Integer.compare(number, 0));
        };

        System.out.println(resultNumber);

        TRAFFIC_LIGHT light = TRAFFIC_LIGHT.P;
        String trafficLightResult;

        trafficLightResult = switch (light) {
            case RED -> "Please Stop";
            case YELLOW -> "Be ready to slow down";
            case GREEN -> "Please Go";
            default -> "Please check the signal and go";
        };

        System.out.println(trafficLightResult);

        int monthNumber = 7;
        String result;

        result = switch (monthNumber) {
            case 1 -> "Jan";
            case 2 -> "Feb";
            case 3 -> "Mar";
            case 4 -> "Apr";
            case 5 -> "May";
            case 6 -> "Jun";
            case 7 -> "Jul";
            case 8 -> "Aug";
            case 9 -> "Sep";
            case 10 -> "Oct";
            case 11 -> "Nov";
            case 12 -> "Dec";
            /*
             * Default is optional but when you return anything, then default is mandatory
             */
            default -> throw new IllegalStateException("Unexpected value: " + monthNumber);
        };

        System.out.println("Month is: " + result);
    }
}
```

---

## Traditional Switch Statement Example (Commented)

```java
/* 
int monthNumber = 7;

switch(monthNumber) {
    case 1:
        System.out.println("January");
        break;
    case 2:
        System.out.println("February");
        break;
    case 3:
        System.out.println("March");
        break;
    case 4:
        System.out.println("April");
        break;
    case 5:
        System.out.println("May");
        break;
    case 6:
        System.out.println("June");
        break;
    case 7:
        System.out.println("July");
        break;
    case 8:
        System.out.println("August");
        break;
    case 9:
        System.out.println("September");
        break;
    case 10:
        System.out.println("October");
        break;
    case 11:
        System.out.println("November");
        break;
    case 12:
        System.out.println("December");
        break;
    default:
        System.out.println("Invalid option");
}
*/
```

---

## String-Based Switch Example (Commented)

```java
/*
String browser = "safari";

switch(browser) {
    case "chrome":
        System.out.println("Loading Chrome");
        break;
    case "firefox":
        System.out.println("Loading Firefox");
        break;
    default:
        System.out.println("Loading another browser");
}
*/
```

---

## Enum-Based Switch Example (Commented)

```java
/*
DayOfWeek dayOfWeek = DayOfWeek.FRIDAY;

switch(dayOfWeek) {
    case MONDAY:
        System.out.println("Weekday");
        break;
    case FRIDAY:
        System.out.println("Weekend near to you");
        break;
    default:
        System.out.println("Weekend");
}
*/
```

---

## Integer-Based Switch Example (Commented)

```java
/*
int day = 1;

switch(day) {
    case 1:
        System.out.println("Start of weekday");
        break;
    case 2:
        System.out.println("Weekday");
        break;
    case 3:
        System.out.println("Mid weekday");
        break;
    default:
        System.out.println("Weekday! Enjoy");
}
*/
```

# Program Output

```
Positive & Odd
Please check the signal and go
Month is: Jul


# Enum in Java

## What is Enum? Why was it introduced?

- To store multiple constants, Java introduced **Enum** in version **1.5**.  
- Enum provides **type-safety**, ensuring only predefined constants are used.  
- Enum is a **special data type** in Java.  
- `enum` is a **keyword**, and internally every enum is treated as a **class**.

---

## Syntax of Enum

```java
enum Days {
    MONDAY, TUESDAY; // These are all objects of the Days class
}
````

---

### Internally, it looks like:

```java
class Days {
    public static final Days MONDAY = new Days();
    public static final Days TUESDAY = new Days();
}
```

* We **cannot** create explicit objects for an enum.
* We **cannot** extend an enum class because it implicitly extends `java.lang.Enum`.

---

## Example: Simple Enum

```java
enum Days {
    MONDAY, TUESDAY; // These are all objects of the Days class
}
```

# Example Program — Enum in Java

---

## Own Enum Class (Without Using `enum` Keyword)

```java
public final class Days {
    static final String MONDAY = "Mon";
    static final String TUEDAY = "Tue";
    static final String WEDNESDAY = "Wed";
    static final String THURSDAY = "Thu";
    static final String FRIDAY = "Fri";
    static final String SATURDAY = "Sat";
    static final String SUNDAY = "Sun";

    final static String[] days = {
        MONDAY,
        TUEDAY,
        WEDNESDAY,
        THURSDAY,
        FRIDAY,
        SATURDAY,
        SUNDAY
    };
}
````

---

## Enum Test Class

```java
public class EnumTest {

    public static void main(String[] args) {

        /*
        Earlier, constants were declared inside methods or classes using final variables.
        The problem was lack of type-safety and possibility of modification.
        Enum solves these issues.
        */

        Week day = Week.MONDAY; // Using Enum we achieve type-safety

        // Enum class default methods
        System.out.println(day.getClass().getSuperclass());

        Week[] days = Week.values();
        for (Week d : days) {
            System.out.println(d);
        }

        System.out.println(day.name());
        System.out.println(day.ordinal());

        if (day == Week.MONDAY ||
            day == Week.TUEDAY ||
            day == Week.WENDESDAY ||
            day == Week.THURSDAY ||
            day == Week.FRIDAY) {
            System.out.println("This is weekday, it is " + day.dayNo + "th day " + day + " >>> " + day.status);
        }
        else if (day == Week.SATURDAY || day == Week.SUNDAY) {
            System.out.println("Weekend!!!, it is " + day.dayNo + "th day " + day + " >>> " + day.status);
        }

        String result;
        result = switch (day) {
            case MONDAY, TUEDAY, WENDESDAY, THURSDAY, FRIDAY ->
                "weekday, it is " + day.dayNo + "th day " + day + " >>> " + day.status;
            case SATURDAY, SUNDAY ->
                "weekend!!!, it is " + day.dayNo + "th day " + day + " >>> " + day.status;
        };
        System.out.println(result);
    }
}
```

---

## Enum Class Definition

```java
public enum Week {
    // Enum constants — must be declared before variables
    MONDAY(1, "Opened"),
    TUEDAY(2, "Opened"),
    WENDESDAY(3),
    THURSDAY(4, "Opened"),
    FRIDAY(5),
    SATURDAY(6),
    SUNDAY(7);

    // Instance variables (implicitly final)
    int dayNo;
    String status = "Closed";

    /*
     * We cannot create enum objects explicitly.
     * Therefore, Java makes the constructor private.
     * When you define your own constructor, the default constructor is removed.
     */

    private Week(int dayNo) {
        this.dayNo = dayNo;
    }

    // Use case: Specific days where the status differs (e.g., book stall opened only on a few days)
    private Week(String status) {
        this.status = status;
    }

    private Week(int dayNo, String status) {
        this.dayNo = dayNo;
        this.status = status;
    }
}
```

---

## Output

```
class java.lang.Enum
MONDAY
TUEDAY
WENDESDAY
THURSDAY
FRIDAY
SATURDAY
SUNDAY
MONDAY
0
This is weekday, it is 1th day MONDAY >>> Opened
weekday, it is 1th day MONDAY >>> Opened
```

# Generics in Java

The below example shows how we can create multiple box classes for storing almonds and cashews.  
However, this is **not a good practice** because if your project supports 20 types of dry fruits,  
then the code becomes very large and may lead to **code duplication**.  
To resolve this problem, we can use **Object type** or, more effectively, **Generics**.

---

## Before Generic Example

```java
public class GenericTest {

    public static void main(String[] args) {

        AlmondBox almondBox = new AlmondBox(2);
        almondBox.add(new Almond("India Almond"));
        almondBox.add(new Almond("America Almond"));

        System.out.println(almondBox.getAlmond(1));

        CashewBox cashewBox = new CashewBox(2);
        cashewBox.add(new Cashew("India Cashew"));
        cashewBox.add(new Cashew("America Cashew"));

        System.out.println(cashewBox.getCashew(1));
    }
}

class Almond {
    String name;

    public Almond(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Almond{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Cashew {
    String name;

    public Cashew(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Cashew{" +
                "name='" + name + '\'' +
                '}';
    }
}

class AlmondBox {
    Almond[] almonds;
    int index;

    public AlmondBox(int size) {
        almonds = new Almond[size];
        index = 0;
    }

    public void add(Almond almond) {
        almonds[index] = almond;
        index++;
    }

    public Almond getAlmond(int i) {
        return almonds[i];
    }
}

class CashewBox {
    Cashew[] cashews;
    int index;

    public CashewBox(int size) {
        cashews = new Cashew[size];
        index = 0;
    }

    public void add(Cashew cashew) {
        cashews[index] = cashew;
        index++;
    }

    public Cashew getCashew(int i) {
        return cashews[i];
    }
}
````

---

## Output

Almond{name='America Almond'}
Cashew{name='America Cashew'}
-----

# Using Object Type in Java

### Concept

- Create one class and make it as an **Object type** with multiple objects.  
  However, there is one problem — we **cannot restrict what type of data** can be stored in that object.

- Instead of creating multiple classes, create **one Object-type class** and then create multiple objects for that class.

### Limitations of Using Object Type

Even though this reduces code duplication, there are **two major problems**:

1. We cannot prevent adding the wrong data type into the object.
2. If wrong data is added, the error will occur at **runtime**, not at **compile time**.

---

## Example Program Using Object Type

```java
public class GenericTest {

    public static void main(String[] args) {

        Box box = new Box(3);
        box.add(new Almond("India Almond"));
        box.add(new Almond("America Almond"));
        box.add("Hello");

        System.out.println((Almond) box.get(2));

        Box box1 = new Box(3);
        box1.add(new Cashew("India Cashew"));
        box1.add(new Cashew("America Cashew"));

        System.out.println((Cashew) box1.get(1));
    }
}

class Almond {
    String name;

    public Almond(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Almond{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Cashew {
    String name;

    public Cashew(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Cashew{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Box {
    Object[] dryFruits;
    int index;

    public Box(int size) {
        dryFruits = new Object[size];
        index = 0;
    }

    public void add(Object almond) {
        dryFruits[index] = almond;
        index++;
    }

    public Object get(int i) {
        return dryFruits[i];
    }
}
````

---

## Output

Almond{name='America Almond'}
Cashew{name='America Cashew'}


# Runtime Exception Example in Java (Using Object Type)

### Description

The below snippet demonstrates how using the **Object type** without proper type checking can lead to a **runtime exception** (`ClassCastException`).

---

## Example Code

```java
Box box = new Box(3);
box.add(new Almond("India Almond"));
box.add(new Almond("America Almond"));

System.out.println((Almond) box.get(1));

Box box1 = new Box(3);
box1.add(new Cashew("India Cashew"));
box1.add(new Cashew("America Cashew"));
box1.add("Hello");

System.out.println((Cashew) box1.get(2));
````

---

## Runtime Exception

```
Exception in thread "main" java.lang.ClassCastException:
class java.lang.String cannot be cast to class com.jp.generics.Cashew
(java.lang.String is in module java.base of loader 'bootstrap';
 com.jp.generics.Cashew is in unnamed module of loader 'app')
```

---

### Explanation

* The above error occurs because `"Hello"` (a `String` object) is added to the `Box` which is expected to contain `Cashew` objects.
* During retrieval, the code attempts to cast `"Hello"` to a `Cashew`, resulting in a **`ClassCastException`**.
* This demonstrates why **Generics** were introduced in Java — to ensure **type safety at compile time** and prevent such **runtime errors**.


# What Are Generics in Java

### Definition

- **Generics** means **type-inferer**.  
- Generics are used to enable **type safety**, **compile-time checks**, and **code reusability**.

---

## Example Program with Generics

```java
public class GenericTest {

    public static void main(String[] args) {

        // Box<Almond> box = new Box<Almond>(3);
        // -> Here, writing <Almond> again on the right side is not required.
        // Because of type inference, Java automatically understands it.
        // To avoid redundancy, we can simply write:

        Box<Almond> box = new Box<>(3);
        box.add(new Almond("India Almond"));
        box.add(new Almond("America Almond"));
        // box.add("World"); -> Compile-time error: prevents adding wrong data.

        System.out.println((Almond) box.get(1));

        Box<Cashew> box1 = new Box<>(3);
        box1.add(new Cashew("India Cashew"));
        box1.add(new Cashew("America Cashew"));
        // box1.add("Hello"); -> Compile-time error: type safety check.

        System.out.println((Cashew) box1.get(1));
    }
}

class Almond {
    String name;

    public Almond(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Almond{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Cashew {
    String name;

    public Cashew(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Cashew{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Box<T> {
    T[] dryFruits;
    int index;

    public Box(int size) {
        dryFruits = (T[]) new Object[size];
        index = 0;
    }

    public void add(T object) {
        dryFruits[index] = object;
        index++;
    }

    public T get(int i) {
        return dryFruits[i];
    }
}
````

---

## Output

```
Almond{name='America Almond'}
Cashew{name='America Cashew'}
```

---

### Key Points

* Generics help ensure that **only the correct data type** is used in collections or classes.
* **Compile-time checking** prevents type mismatch errors early.
* Promotes **code reusability** by allowing the same class to work with different data types.


# Generic Methods in Java

## Introduction

Generic methods allow you to write **a single method** that can operate on objects of various types, without the need for method overloading.  
They enable **type safety**, **code reusability**, and **compile-time checks**.

---

## Example Program Without Generics

```java
public class GenericTest {

    public static void main(String[] args) {
        swap(1, 2);
        swap("Hello", "World");
    }

    public static void swap(int i, int j) {
        System.out.println("Before Swapping " + i + " >> " + j);
        int temp;
        temp = i;
        i = j;
        j = temp;
        System.out.println("After Swapping --- " + i + " >> " + j);
    }

    public static void swap(String i, String j) {
        System.out.println("Before Swapping " + i + " >> " + j);
        String temp;
        temp = i;
        i = j;
        j = temp;
        System.out.println("After Swapping --- " + i + " >> " + j);
    }
}
````

### Output

```
Before Swapping 1 >> 2
After Swapping---2 >> 1
```

---

## Example Program **With Generics**

Using Generics, there’s no need to write multiple versions of the same method.
A single generic method can handle all data types.

```java
public class GenericTest {

    public static void main(String[] args) {
        swap("Hello", "World");
        swap(1, 2);
        swap(12.5, 16.7);
        swap(true, false);
    }

    public static <T> void swap(T i, T j) {
        System.out.println("Before Swapping " + i + " >> " + j);
        T temp;
        temp = i;
        i = j;
        j = temp;
        System.out.println("After Swapping --- " + i + " >> " + j);
    }
}
```

### Output

```
Before Swapping Hello >> World
After Swapping---World >> Hello

Before Swapping 1 >> 2
After Swapping---2 >> 1

Before Swapping 12.5 >> 16.7
After Swapping---16.7 >> 12.5

Before Swapping true >> false
After Swapping---false >> true
```

---

## Notes

* `public static <T> void swap(T i, T j)`
  → Here, `<T>` tells Java that this method uses **Generics**.

* You can use a **combination of generic and non-generic types** in a method:

  ```java
  swap("Hello", "World", 45);

  public static <T> void swap(T i, T j, int result) {
      // combination of generic and normal datatype
  }
  ```

* You can also **return generic types** from methods:

  ```java
  public static <T> T getValue(T value) {
      return value;
  }
  ```

---

### Key Advantages of Generic Methods

* Eliminate method overloading for multiple data types.
* Provide **compile-time type safety**.
* Improve **code reusability** and **readability**.

# Bounded Generics in Java

## Definition

- **Bounded Generics** allow us to **restrict (or limit)** the types that can be used as type parameters in a generic class or method.

---

## Types of Bounds

### 1. Upper Bound (`extends`)
- Syntax: `<T extends Number>`
- Meaning: This is an **Upper Bound Generic** — it allows **`Number`** and all its **subclasses** (e.g., `Integer`, `Double`, `Float`, etc.).

Example:
```java
class Box<T extends Number> {
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}
````

---

### 2. Lower Bound (`super`)

* Syntax: `<? super String>`
* Meaning: This is a **Lower Bound Generic** — it allows **superclasses of `String`** (like `Object`).

Example:

```java
List<? super String> list = new ArrayList<Object>();
```

---

### Note on Methods

```java
public static <? super Number> void swap(T i, T j);
```

❌ **Not possible** — you cannot directly use lower-bounded wildcards in method declarations like this.

---

## Common Alphabet Conventions in Generics

| Symbol            | Meaning                   | Usage Example                                   |
| :---------------- | :------------------------ | :---------------------------------------------- |
| **T**             | Type                      | General-purpose generic type                    |
| **E**             | Element                   | Used in collections (e.g., `List<E>`)           |
| **K**             | Key                       | Used in maps (e.g., `Map<K, V>`)                |
| **V**             | Value                     | Used in maps for values                         |
| **N**             | Number                    | Numeric types                                   |
| **S, U, V, etc.** | Secondary type parameters | Used when multiple type parameters are involved |
| **?**             | Wildcard                  | Represents an unknown type                      |

---

## Type Erasure

* During **compilation**, Java **removes all generic type information** and sends the compiled bytecode to the **JVM**.
* This process is known as **Type Erasure**.

### Example:

```java
List<Integer> list = new ArrayList<>();
```

At runtime, this becomes:

```java
List list = new ArrayList();
```

* Because of **type erasure**, programmers **cannot perform type-related mistakes** at runtime — errors are caught during **compile time**.

---

## Restrictions with Generics

* ❌ **Generics cannot be used with exceptions.**

  * You cannot create generic exception classes or catch blocks like:

    ```java
    class MyException<T> extends Exception {} // Not allowed
    ```
  * You also cannot do:

    ```java
    catch (T e) { } // Not allowed
    ```

---

### Summary

* **Upper bound** → `<T extends Class>` → Limits to subclasses.
* **Lower bound** → `<? super Class>` → Limits to superclasses.
* **Type Erasure** → Generic info is removed at compile time.
* **No Generics in Exceptions** → Not supported in Java.





