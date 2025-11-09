# Streams Operations in Java

## Stream Sources
- **Collections**  
- **Arrays**

> Stream source decides whether you can create a sequential stream or a parallel stream.

---

## Stream Operations

### Intermediate Operations

#### i) filter
- `filter()` function is used to filter the stream data and pass the filtered stream to another function.
- `filter()` function accepts a **Predicate** as a parameter.

#### Predicate (Functional Interface)
- `Predicate` is a predefined functional interface introduced in Java 1.8.  
- It has one abstract method: `test()`.  
- `test()` function takes one parameter and returns a boolean value (`true` / `false`).  
- It is available in the package `java.util.function`.

##### Example Snippet
```java
Predicate<Integer> predicate = new Predicate<Integer>() {
    @Override
    public boolean test(Integer i) {
        return i % 2 == 0;
    }
};
````

---

#### sorted

* `sorted()` function is used to sort the stream data in ascending order by default.
* `sorted()` function can take a **Comparator** as a parameter for custom sorting.

---

#### map

* `map()` is used to transform the stream data into another or the same form.
* `map()` function accepts a **Function** as a parameter.

#### Function (Functional Interface)

* `Function` is a predefined functional interface introduced in Java 1.8.
* It has one abstract method: `apply()`.
* `apply()` function takes one parameter and returns a value (element).
* It is available in the package `java.util.function`.

##### Example Snippet

```java
Function<Integer, Integer> function = new Function<Integer, Integer>() {
    @Override
    public Integer apply(Integer i) {
        return i * i;
    }
};
```

---

* Intermediate operations can be performed anywhere after stream creation.
* There is **no strict rule** for their order — Java recommends arranging them based on your logic for performance improvement.
* You can perform **minimum 0 and maximum n** intermediate operations.

---

### Terminal Operations

#### i) forEach, count

* Only **one terminal operation** can be performed on a stream.
* Terminal operations may or may not return a value, but **they end the stream pipeline**.
* Once a terminal operation completes, **no further intermediate operations** can be performed.
* Without a terminal operation, **the stream will not execute**.

---

### Difference between `peek()` and `forEach()`

| Feature        | peek()                                           | forEach()                                 |
| -------------- | ------------------------------------------------ | ----------------------------------------- |
| Type           | Intermediate operation                           | Terminal operation                        |
| Input / Output | Takes input as stream, produces output as stream | Takes input as stream, produces no output |
| Parameter      | Both take **Consumer** as a parameter            |                                           |

---

### Streams and Arrays

* Stream **direct support for arrays** is not there; instead, use:

  ```java
  Arrays.stream(arr);
  ```
* Streams work with **collections and generics** (i.e., wrapper classes, not primitive types).

---

## `of()` Method

* Introduced in **Java 1.9**.
* Used to create immutable lists (unmodifiable lists).
* Cannot perform `add`, `remove`, or `replace` operations — will throw **UnsupportedOperationException**.
* **Null values** not allowed — throws **NullPointerException**.
* **Duplicates are allowed**.
* Suitable for creating **fixed-length, small lists**.
* Internally implemented as a **special unmodifiable list** (not `ArrayList` or `LinkedList`).

---

## Example Program

```java
public class StreamPratice {

    public static void main(String[] args) {
        List<Integer> numbers = List.of(4, 9, 65, 2, 5, 8, 8, 67);
        Integer[] arr = {4, 9, 65, 2, 5, 8, 8, 67};

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
```

---

### Output

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
```


