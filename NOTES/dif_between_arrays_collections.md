# Arrays vs Collections in Java — Detailed Explanation with Examples & Diagrams

## 1. Basic Difference

| Feature / Criteria | Arrays | Collections |
|-------------------|--------|-------------|
| **Definition** | Fixed-size container for storing elements of the same type | Dynamic, growable data structures part of the Java Collection Framework |

### Example
```java
// Array
int[] nums = new int[5];

// Collection
List<Integer> numsList = new ArrayList<>();
````

---

## 2. Size (Fixed vs Dynamic)

| Arrays                     | Collections                       |
| -------------------------- | --------------------------------- |
| Size is fixed once created | Size grows or shrinks dynamically |

### Example

```java
int[] arr = new int[3];
// arr cannot grow

List<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);
// list keeps growing
```

---

## 3. Data Type Storage (Primitive vs Object)

| Arrays                           | Collections                                     |
| -------------------------------- | ----------------------------------------------- |
| Can store primitives and objects | Can store only objects (Integer instead of int) |

### Example

```java
int[] arr = {1, 2, 3};     // primitive storage
List<Integer> list = List.of(1, 2, 3);  // wrapper types only
```

---

## 4. Performance

| Arrays                      | Collections                                                          |
| --------------------------- | -------------------------------------------------------------------- |
| Fast — direct memory access | Slightly slower — overhead of dynamic resizing and framework methods |

### Diagram (Conceptual)

```
ARRAY: [10][20][30][40] -> Direct access arr[2]

COLLECTION (ArrayList internally):
[10][20][30][40][null][null]  -> resize when full
```

---

## 5. Memory Allocation

| Arrays            | Collections               |
| ----------------- | ------------------------- |
| Contiguous memory | Dynamic memory allocation |

---

## 6. Built-in Methods

| Arrays                     | Collections                                                |
| -------------------------- | ---------------------------------------------------------- |
| Very limited (length only) | Rich set: add, remove, contains, size, sort, reverse, etc. |

### Example

```java
Collections.sort(list);
list.contains(10);
list.remove(0);
```

---

## 7. Part of Framework?

| Arrays                                | Collections                            |
| ------------------------------------- | -------------------------------------- |
| Not part of Java Collection Framework | Core part of Java Collection Framework |

---

## 8. Iteration Support

| Arrays              | Collections                 |
| ------------------- | --------------------------- |
| for, for-each loops | Iterator, for-each, Streams |

### Example

```java
for (int n : arr) { }

list.stream().forEach(System.out::println);
```

---

## 9. Homogeneous vs Heterogeneous

| Arrays                    | Collections                                                       |
| ------------------------- | ----------------------------------------------------------------- |
| Must store same data type | Can store different types (if raw type used, but not recommended) |

### Example

```java
List list = new ArrayList(); // raw type
list.add(10);
list.add("Hello");
```

---

## 10. Generics Support

| Arrays      | Collections             |
| ----------- | ----------------------- |
| No generics | Fully supports generics |

---

## 11. Multidimensional Support

| Arrays                           | Collections                 |
| -------------------------------- | --------------------------- |
| Supports multidimensional arrays | Nested collections required |

### Example

```java
int[][] matrix = new int[3][3];

List<List<Integer>> grid = new ArrayList<>();
```

---

## 12. Use Case Summary

| Arrays Best For         | Collections Best For           |
| ----------------------- | ------------------------------ |
| Fixed-size data storage | Dynamic data storage           |
| High performance tasks  | Flexible operations            |
| Storing primitives      | Using advanced data structures |

---

# Summary Table (Quick View)

| Feature     | Arrays               | Collections                  |
| ----------- | -------------------- | ---------------------------- |
| Size        | Fixed                | Dynamic                      |
| Data Type   | Primitives + Objects | Objects only                 |
| Performance | Faster               | Slightly slower              |
| Framework   | Not part             | Part of Collection Framework |
| Methods     | Limited              | Rich utility methods         |
| Generics    | Not supported        | Supported                    |
| Memory      | Contiguous           | Dynamic                      |
| Use Case    | Fixed data           | Flexible, resizable data     |

---
