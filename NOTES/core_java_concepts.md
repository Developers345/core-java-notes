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


