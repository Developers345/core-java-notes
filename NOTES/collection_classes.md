# **List in Java — Detailed Explanation**

## **1. What is a List?**

In Java, a **List** is an ordered collection of elements. It is part of the **Java Collections Framework** and is defined in the `java.util` package.

A `List`:

* Maintains **insertion order**
* Allows **duplicate elements**
* Supports **index-based access** (like arrays)
* Can **grow or shrink dynamically**

`List` is an **interface**, so it cannot be instantiated directly—you must use one of its implementing classes such as **ArrayList**, **LinkedList**, or **Vector**.

---

## **2. List Interface Hierarchy**

```
Collection (interface)
     ↑
List (interface)
     ↑
------------------------------
|            |               |
ArrayList   LinkedList      Vector
```

---

## **3. Key Features of List**

### ✔ Maintains Order

Elements are stored in the same sequence in which they are added.

### ✔ Allows Duplicates

You can add the same value multiple times.

### ✔ Dynamic Size

Automatically grows or shrinks when elements are inserted or removed.

### ✔ Index-Based Access

You can access elements using indexes like:

```java
list.get(0);
```

---

## **4. Commonly Used List Implementations**

### ### **1. ArrayList**

* Backed by a **dynamic array**
* **Fast** for retrieving elements (O(1))
* Slower for inserting/removing in the middle (O(n))
* Most commonly used List

Example:

```java
List<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");
```

---

### ### **2. LinkedList**

* Backed by a **doubly linked list**
* Fast insertion/removal at the **beginning/middle**
* Slower for getting elements (O(n))
* Useful for queue/stack operations

Example:

```java
List<Integer> numbers = new LinkedList<>();
numbers.add(10);
numbers.add(20);
```

---

### ### **3. Vector**

* Synchronized (thread-safe)
* Slower because of synchronization
* Legacy class (rarely used now)

Example:

```java
List<String> vector = new Vector<>();
vector.add("A");
vector.add("B");
```

---

## **5. Important List Methods**

| Method                | Description                |
| --------------------- | -------------------------- |
| `add(E e)`            | Adds an element            |
| `add(int index, E e)` | Inserts element at index   |
| `get(int index)`      | Retrieves element          |
| `set(int index, E e)` | Updates element            |
| `remove(int index)`   | Removes element by index   |
| `remove(Object o)`    | Removes matching object    |
| `size()`              | Returns number of elements |
| `contains(Object o)`  | Checks if element exists   |
| `isEmpty()`           | Checks if list is empty    |
| `clear()`             | Removes all elements       |

---

## **6. Example Program Using List**

```java
import java.util.*;

public class ListExample {
    public static void main(String[] args) {
        List<String> fruits = new ArrayList<>();

        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Orange");
        fruits.add("Banana"); // duplicate allowed

        System.out.println("Fruits: " + fruits);

        System.out.println("First Element: " + fruits.get(0));

        fruits.remove("Banana"); // removes first Banana
        System.out.println("After Remove: " + fruits);

        fruits.set(1, "Grapes"); // update element
        System.out.println("After Update: " + fruits);
    }
}
```

---

## **7. When Should You Use Which List?**

| List Type      | Use Case                                            |
| -------------- | --------------------------------------------------- |
| **ArrayList**  | Frequent access, rare insert/delete in middle       |
| **LinkedList** | Frequent insert/delete, queue/stack implementations |
| **Vector**     | When thread safety is required (rare today)         |

---

## **8. Difference Between List and Array**

| Array                      | List                   |
| -------------------------- | ---------------------- |
| Fixed size                 | Dynamic size           |
| Can store primitives       | Can store objects only |
| Not part of Collection API | Part of Collection API |
| Faster for fixed-size data | More flexible          |

---

## **Summary**

* `List` is an **ordered**, **dynamic**, **duplicate-allowing** collection.
* Implemented by **ArrayList**, **LinkedList**, and **Vector**.
* Supports **index-based operations** and **flexible resizing**.
* Most commonly used implementation → **ArrayList**.

---
