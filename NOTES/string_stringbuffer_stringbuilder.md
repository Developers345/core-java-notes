# Understanding Mutability (Mutable and Immutable)

## What is Mutability?

- **Mutability** means the ability to change the value or data stored in a variable.
- If a variable is **mutable**, it means you **can modify** its value after it has been created.

## What is Immutability?

- **Immutability** means the value of a variable **cannot be changed** once it is assigned.
- **Strings** in Java are always **immutable**.

## Problem with String Immutability

Because strings are immutable, every time you perform data manipulation operations (like concatenation or replacement),  
a **new memory location** is created in the background to store the updated string.  
This can lead to **increased memory usage** and **reduced performance** when handling large amounts of string data.

# Example Program: String Immutability in Java

## Program Code

```java
public class StringImmutableTest {

    public static void main(String[] args) {

        String s1 = "Gireesh";
        String s2 = "Gireesh";
        System.out.println("s1 == s2: " + (s1 == s2));

        s1 = s1 + "Kumar";
        System.out.println("s1 == s2: " + (s1 == s2));

        String s3 = "GireeshKumar";
        System.out.println("s1 == s3: " + (s1 == s3));
    }
}
````

## Output

```
s1 == s2: true  
s1 == s2: false  
s1 == s3: false
```

## Explanation

* Initially, both `s1` and `s2` refer to the **same memory location** in the **String Constant Pool**, hence `s1 == s2` is `true`.
* When `s1` is modified using concatenation (`s1 = s1 + "Kumar";`), a **new string object** is created in a **different memory location**, so `s1 == s2` becomes `false`.
* Even though `s3` contains the same text `"GireeshKumar"`, it is a **different object reference**, so `s1 == s3` is also `false`.

## Note

- In the above example, the `(==)` operator is used for **reference comparison**, which checks whether two variables point to the **same memory location**.
- The methods `.equals()` and `.equalsIgnoreCase()` are used for **content comparison**, which checks whether the **actual text values** of two strings are the same.
- Therefore, in the example above, each time a string undergoes data manipulation, a **new memory address** is created to store the updated value.  
  This behavior demonstrates that **strings in Java are immutable** by nature.


# What Are StringBuffer and StringBuilder?

Java provides two **mutable string classes** to allow modification of string data without creating new objects each time.

## Purpose of Mutable Strings

1. **StringBuffer** – Designed for use in **multithreaded environments**, where multiple threads might access or modify the same string.  
   - It is **synchronized**, meaning it is **thread-safe**.  
   - However, synchronization makes it **slower** compared to `StringBuilder`.

2. **StringBuilder** – Designed for **single-threaded environments**, where synchronization is not required.  
   - It is **non-synchronized**, meaning it is **not thread-safe**.  
   - As a result, it offers **better performance** than `StringBuffer`.

## Version Information

- `String` has been available since **Java 1.0**  
- `StringBuffer` has also been available since **Java 1.0**  
- `StringBuilder` was introduced in **Java 1.5**

## Important Note

Unlike `String`, **`StringBuffer`** and **`StringBuilder`** objects **cannot be created using string literals**.  
They must be created **explicitly using the `new` keyword**, for example:

```java
StringBuffer sb1 = new StringBuffer("Hello");
StringBuilder sb2 = new StringBuilder("World");
````

# Example Program: String, StringBuffer, and StringBuilder Comparison

## Program Code

```java
public class StringImmutableTest {

    public static void main(String[] args) {

        StringBuffer sb1 = new StringBuffer("Gireesh"); // reference location: 726
        StringBuffer sb2 = new StringBuffer("Gireesh"); // reference location: 838
        System.out.println("sb1 == sb2 " + (sb1 == sb2)); // 726 == 838

        sb1 = sb1.append("Kumar"); // same reference location: 726
        System.out.println("sb1 == sb2 " + (sb1 == sb2)); // 726 == 838

        StringBuffer sb3 = new StringBuffer("GireeshKumar"); // reference location: 874
        System.out.println("sb1 == sb3 " + (sb1 == sb3)); // 726 == 874

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>");

        StringBuilder sbd1 = new StringBuilder("Gireesh"); // reference location: 726
        StringBuilder sbd2 = new StringBuilder("Gireesh"); // reference location: 838
        System.out.println("sbd1 == sbd2 " + (sbd1 == sbd2)); // 726 == 838

        sbd1 = sbd1.append("Kumar"); // same reference location: 726
        System.out.println("sbd1 == sbd2 " + (sbd1 == sbd2)); // 726 == 838

        StringBuilder sbd3 = new StringBuilder("GireeshKumar"); // reference location: 874
        System.out.println("sbd1 == sbd3 " + (sbd1 == sbd3)); // 726 == 874

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>");

        String s1 = "Gireesh"; // reference location: 871
        String s2 = "Gireesh"; // same reference location: 871
        System.out.println("s1 == s2 " + (s1 == s2)); // 871 == 871

        s1 = s1 + "Kumar"; // new reference location: 880
        System.out.println("s1 == s2 " + (s1 == s2)); // 880 == 871

        String s3 = "GireeshKumar"; // reference location: 886
        System.out.println("s1 == s3 " + (s1 == s3)); // 880 == 886
    }
}
````

## Output

```
sb1 == sb2 false  
sb1 == sb2 false  
sb1 == sb3 false  
>>>>>>>>>>>>>>>>>>>>>>>  
sbd1 == sbd2 false  
sbd1 == sbd2 false  
sbd1 == sbd3 false  
>>>>>>>>>>>>>>>>>>>>>>>  
s1 == s2 true  
s1 == s2 false  
s1 == s3 false
```

## Explanation

* **StringBuffer** and **StringBuilder** objects are **mutable**, meaning their values can be modified **without creating new objects**.
  However, each new object created using the `new` keyword occupies a **different memory location**, hence reference comparisons (`==`) return `false`.

* In contrast, **String** objects are **immutable**.

  * When created using **string literals**, identical values share the same memory reference in the **String Constant Pool**, making `(s1 == s2)` return `true`.
  * When a string is modified (e.g., concatenation), a **new object** is created in a **different memory location**, so `(s1 == s2)` or `(s1 == s3)` returns `false`.

This program demonstrates the difference between **mutability** and **immutability** in Java string classes.


# Example Program: Difference Between String, StringBuffer, and StringBuilder

## Program Code

```java
public class StringStringBufferStringBulierDiffTest {

    public static void main(String[] args) {
        StringBuilder sb1 = new StringBuilder("Gireesh");
        StringBuilder sb2 = new StringBuilder("Gireesh");

        System.out.println("sb1 == sb2 " + (sb1.equals(sb2))); // Reference comparison
        System.out.println("sb1 == sb2 " + (sb1.compareTo(sb2))); // Content comparison

        sb1.insert(1, "Kumar");
        System.out.println(sb1);

        sb1.delete(0, 5);
        System.out.println(sb1);

        System.out.println(sb1.capacity());

        sb1.ensureCapacity(100);
    }
}
````

## Output

```
sb1 == sb2 false  
sb1 == sb2 0  
GKumarireesh  
rireesh  
23  
rireesh
```

## Explanation

* `equals()` in `StringBuilder` and `StringBuffer` classes performs **reference comparison**, not content comparison.
  Hence, even though both objects contain the same text (`"Gireesh"`), `sb1.equals(sb2)` returns **false** because they refer to **different memory locations**.

* The `compareTo()` method performs a **lexicographical (content-based)** comparison.
  Since both have identical content, it returns **0**, meaning both strings are equal in content.

* The `insert()` method allows adding text at a specific index.

  * Example: `sb1.insert(1, "Kumar")` inserts `"Kumar"` after the first character, producing `GKumarireesh`.

* The `delete(int start, int end)` method removes characters within the specified range.

  * Example: `sb1.delete(0, 5)` removes characters from index `0` to `4`.

* The `capacity()` method returns the **current capacity** of the `StringBuilder` object.
  It represents the amount of space available before a new memory allocation is required.

* The `ensureCapacity(int minimumCapacity)` method increases the capacity if the current capacity is less than the specified minimum.

This program illustrates how `StringBuilder` provides flexible and efficient methods for **modifying string content**, unlike immutable `String` objects.

## Note

- In Java, the **exact memory address** of an object is **not displayed** to the user, as it is managed internally by the **Java Virtual Machine (JVM)**.  
- However, we can observe the **reference identity** of an object (its reference location) through variables that **point to those objects** in memory.

# String vs StringBuffer vs StringBuilder
<img width="1560" height="897" alt="Screenshot 2025-10-18 165012" src="https://github.com/user-attachments/assets/32e7a6e5-d88d-44a7-8ee2-b6dc79133a22" />


