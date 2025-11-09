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

# Stream Source Primitive

### Stream creation not available for:
- `byte`
- `short`
- `float`
- `char`
- `boolean`

### Stream creation available for:
- `int`
- `long`
- `double`

> We cannot create stream on primitive types directly.  
> Java provides special interfaces like `IntStream`, `DoubleStream`, and `LongStream` to create primitive streams.

---

## `IntStream.of()` Method
Used when you already know a specific group of values to create the stream.

```java
IntStream.of(1, 3, 5, 7, 9);
````

---

## `IntStream.range(startInclusive, endExclusive)`

Used when you want to create a stream of numbers within a range.

```java
IntStream.range(10, 100);
```

* `10` → startInclusive (includes 10)
* `100` → endExclusive (excludes 100)

If you want to include the ending value also, use `rangeClosed()` method.

> `of()`, `range()`, and `rangeClosed()` create **finite streams** (specific number of known values).

---

## Infinite Stream Using `iterate()`

```java
IntStream.iterate(0, n -> n + 2).limit(10).forEach(System.out::println);
```

* Creates an **infinite stream** but `limit(10)` restricts the output to 10 elements.

> `limit(10)` — displays up to 10 elements;
> If fewer than 10 exist, it simply returns the available ones (no error).

---

### Example with Predicate and Unary Operator

```java
IntStream.iterate(1, value -> value <= 50, n -> n + 2)
         .limit(10)
         .skip(2)
         .forEach(System.out::println);
```

* `1` → seed (starting value)
* `value -> value <= 50` → condition (IntPredicate)
* `n -> n + 2` → unary operator

---

## Creating Infinite Streams for Random or Constant Data

* For random data: use infinite streams generated by `IntSupplier`.
* For constant data: can also use infinite streams.

> Returns an infinite sequential unordered stream where each element is generated by the provided `IntSupplier`.

### Example Snippet

```java
long count = IntStream.generate(() -> new Random().nextInt(100))
                      .limit(200)
                      .distinct()
                      .count();
System.out.println(count);
```

---

## `concat()` Method

Combine two streams:

```java
IntStream is1 = IntStream.of(1, 3, 5, 6, 8, 10);
IntStream is2 = IntStream.of(2, 7, 9, 0, 6, 7, 4, 7);
IntStream.concat(is1, is2).forEach(System.out::println);
```

> `IntStream`, `LongStream`, and `DoubleStream` are similar.
> Note: `DoubleStream` does not have `range()` method.

---

## Stream Interface

* `Stream` is a **generic interface**.

```java
Stream.of(1, "Hello").forEach(System.out::println);
```

---

## String Streams

We cannot perform a direct stream operation on `String`.

A `String` can be treated as:

* a sequence of **characters**
* a sequence of **words**
* a sequence of **lines**

### `mapToObj` Method

Used to convert primitive data to object (non-primitive) form.

#### Example Snippet

```java
String s2 = "Gireesh";
s2.chars().mapToObj(c -> (char)c).forEach(System.out::println);
```

---

## Example Program

```java
public class StreamMethodPratice {

    public static void main(String[] args) {

        System.out.println("============Using of() method===========");
        IntStream.of(1,3,5,6,8,10).filter(n -> n % 2 == 0).forEach(System.out::println);

        System.out.println("==========Using range() method============");
        IntStream.range(10,100).filter(n -> n % 2 == 0).forEach(System.out::println);

        System.out.println("==========Using rangeClosed() method========");
        IntStream.rangeClosed(10,100).filter(n -> n % 2 == 0).forEach(System.out::println);

        System.out.println("==========Using iterate() method to create infinite stream and limit() to print certain elements========");
        IntStream.iterate(0, n -> n + 2).limit(10).forEach(System.out::println);

        System.out.println("==========Using limit() method below 10 numbers========");
        IntStream.of(1,3,5,6,8,10).limit(10).forEach(System.out::println);

        System.out.println("==========Using iterate() method like for loop and combine limit() and skip() methods========");
        IntStream.iterate(1, value -> value <= 50, n -> n + 2)
                 .limit(10)
                 .skip(2)
                 .forEach(System.out::println);

        System.out.println("==========Using generate() method below 100 numbers and distinct() method========");
        long count = IntStream.generate(() -> new Random().nextInt(100))
                              .limit(200)
                              .distinct()
                              .count();
        System.out.println(count);

        System.out.println("==========Using concat() method ========");
        IntStream is1 = IntStream.of(1,3,5,6,8,10);
        IntStream is2 = IntStream.of(2,7,9,0,6,7,4,7);
        IntStream.concat(is1, is2).forEach(System.out::println);

        System.out.println("==========Using toArray() method ========");
        IntStream is3 = IntStream.of(1,3,5,6,8,10);
        IntStream is4 = IntStream.of(2,7,9,0,6,7,4,7);
        int[] array = IntStream.concat(is3, is4).distinct().toArray();
        System.out.println(Arrays.toString(array));

        List<Integer> list1 = List.of(1,4,6,8,3,8,3,9,5);
        Object[] obj = list1.stream().distinct().toArray();
        System.out.println(Arrays.toString(obj));

        System.out.println("==========Using Stream() interface ========");
        Stream.of(1, "Hello").forEach(System.out::println);

        System.out.println("==========String Stream ========");
        String s = "Hi\nHow Are you\nAre you Fine?";
        s.lines().forEach(System.out::println);

        String s1 = "Ganesh Kumar";
        Arrays.stream(s1.split(" ")).forEach(System.out::println);

        String s2 = "Gireesh";
        s2.chars().mapToObj(c -> (char)c).forEach(System.out::println);
    }
}
```

---

## Output

```
============Using of() method===========
6
8
10
==========Using range() method============
10
12
14
...
98
==========Using rangeClosed() method========
10
12
...
100
==========Using iterate() method create infinite stream and limit()========
0
2
4
6
8
10
12
14
16
18
==========Using limit() method below 10 numbers========
1
3
5
6
8
10
==========Using iterate() method like for loop and combine limit() and skip()========
5
7
9
11
13
15
17
19
==========Using generate() method below 100 numbers and distinct()========
83
==========Using concat() method ========
1
3
5
6
8
10
2
7
9
0
6
7
4
7
==========Using toArray() method ========
[1, 3, 5, 6, 8, 10, 2, 7, 9, 0, 4]
[1, 4, 6, 8, 3, 9, 5]
==========Using Stream() interface ========
1
Hello
==========String Stream ========
Hi
How Are you
Are you Fine?
Ganesh
Kumar
G
i
r
e
e
s
h
```

