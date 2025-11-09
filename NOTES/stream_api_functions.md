````markdown
# Streams Operations in Java

## Stream Sources
- Collections  
- Arrays  

➡️ Stream source decides whether you can create a **sequential Stream** or a **parallel Stream**.

---

## Stream Operations

### Intermediate Operations

#### i) `filter()`
- `filter()` function is used to **filter the stream data** and pass the filtered stream to another function.  
- `filter()` function takes a **Predicate** as a parameter.

---

### Predicate (Functional Interface)

- `Predicate` is a **predefined functional interface**, introduced in **Java 1.8**.  
- It has **one abstract method**: `test()`.  
- The `test()` method takes **one parameter** and returns a **boolean** value (`true` / `false`).  
- It is part of the package:  
  ```java
  java.util.function
````

#### Example Snippet

```java
Predicate<Integer> predicate = new Predicate<Integer>() {
    @Override
    public boolean test(Integer i) {
        return i % 2 == 0;
    }
};
```

---

### `sorted()`

* `sorted()` function is used to **sort stream data** in **ascending order** by default.
* It can also take a **Comparator** as a parameter.

---

### `map()`

* `map()` is used to **transform** the stream data into another form (or the same form).
* `map()` function takes a **Function** as a parameter.

---

### Function (Functional Interface)

* `Function` is a **predefined functional interface**, introduced in **Java 1.8**.
* It has **one abstract method**: `apply()`.
* The `apply()` method takes **one parameter** and returns a **value** (element).
* It is part of the package:

  ```java
  java.util.function
  ```

#### Example Snippet

```java
Function<Integer, Integer> function = new Function<Integer, Integer>() {
    @Override
    public Integer apply(Integer i) {
        return i * i;
    }
};
```

---

### Notes

* We can perform **intermediate operations** anywhere after stream creation — there is **no strict rule**.
* Java recommends placing intermediate operations based on code design to **improve performance**.
* We can perform a **minimum of 0** and a **maximum of n** intermediate operations.

# Terminal Operations in Streams

### i) `forEach`, `count`

- We can perform **only one terminal operation** on a stream.  
- **Terminal operations** either return a value or perform an action without returning anything — but **they mark the end of the stream pipeline**.  
- Once a terminal operation is completed, **no further intermediate operations** can be performed.  
- Without a **terminal operation**, the stream **will not execute**.

---

## Difference Between `peek()` and `forEach()`

| Feature | `peek()` | `forEach()` |
|----------|-----------|-------------|
| Type | Intermediate operation | Terminal operation |
| Input / Output | Takes input as stream and produces output as stream | Takes input as stream but does **not** produce output |
| Parameter | Both take a **Consumer** as a parameter | Both take a **Consumer** as a parameter |

---

## Arrays and Streams

- Streams do **not have direct support** for arrays.  
  You can create a stream from an array using:
  ```java
  Arrays.stream(arr);
`

* Streams and collections work seamlessly with **generics** — meaning they support **wrapper classes**, not **primitive types**.


# `of()` Method in Java

## Overview
- `of()` method was **introduced in Java 1.9**.  
- Used to create a **List** quickly and conveniently.  
- The list created using `of()` is **immutable** (unmodifiable).  
- You **cannot perform** `add`, `remove`, or `replace` operations — doing so will throw an **UnsupportedOperationException**.  
- If a **null** value is present, it will throw a **NullPointerException**.  
- **Duplicates are allowed.**  
- Suitable for creating **small, fixed-length lists.**  
- Internally, it uses a **special unmodifiable list implementation**, not `ArrayList` or `LinkedList`.

---

## Example Program

```java
public class StreamPratice {

    public static void main(String[] args) {
        List<Integer> numbers = List.of(4, 9, 65, 2, 5, 8, 8, 67);
        Integer[] arr = {4, 9, 65, 2, 5, 8, 8, 67};

        /*
        Predicate<Integer> predicate = new Predicate<Integer>() {
            @Override
            public boolean test(Integer i) {
                return i % 2 == 0;
            }
        };
        */

        /*
        Function<Integer, Integer> function = new Function<Integer, Integer>() {
            @Override
            public Integer apply(Integer i) {
                return i * i;
            }
        };
        */

        long count = numbers.stream()
                .filter(i -> i % 2 == 0)
                .sorted(Comparator.reverseOrder())
                .map(i -> {
                    StringBuilder s = new StringBuilder("");
                    for (int k = 1; k <= i; k++) {
                        s.append("*");
                    }
                    return s;
                })
                .peek(System.out::println)
                .count();

        System.out.println("count is " + count);
        System.out.println("=========================================");

        /*
         Stream direct support for Array is not there.
         */
        long countArr = Arrays.stream(arr)
                .filter(i -> i % 2 == 0)
                .sorted(Comparator.reverseOrder())
                .map(i -> {
                    StringBuilder s = new StringBuilder("");
                    for (int k = 1; k <= i; k++) {
                        s.append("*");
                    }
                    return s;
                })
                .peek(System.out::println)
                .count();

        System.out.println("Array count: " + countArr);
    }
}
````

---

## Output

```
********
********
****
**
count is 4
=========================================
********
********
****
**
Array count: 4



