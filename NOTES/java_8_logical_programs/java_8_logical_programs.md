# Java 8 Stream API – Logical Programs

---

## Program 1: Find Duplicate Elements in a List

### Problem Statement

Given a list of integers, find all **duplicate elements** using Java 8 Stream API.

---

### Code Example

```java
import java.util.*;
import java.util.stream.Collectors;

public class DuplicateElements {
    public static void main(String[] args) {

        List<Integer> numbers = Arrays.asList(10, 20, 30, 20, 40, 10, 50);

        Set<Integer> duplicates = numbers.stream()
                .filter(n -> Collections.frequency(numbers, n) > 1)
                .collect(Collectors.toSet());

        System.out.println("Duplicate Elements: " + duplicates);
    }
}
```

---

### Step-by-Step Explanation

1. **Create Input List**

   ```java
   List<Integer> numbers = Arrays.asList(10, 20, 30, 20, 40, 10, 50);
   ```

   This list contains duplicate values `10` and `20`.

2. **Convert List to Stream**

   ```java
   numbers.stream()
   ```

   Converts the list into a stream for functional processing.

3. **Apply filter()**

   ```java
   filter(n -> Collections.frequency(numbers, n) > 1)
   ```

   * `Collections.frequency()` counts how many times `n` appears in the list.
   * If frequency > 1, the element is a duplicate.

4. **Collect into Set**

   ```java
   collect(Collectors.toSet())
   ```

   * `Set` automatically removes repeated duplicates.
   * Final output contains unique duplicate values.

---

### Output

```
Duplicate Elements: [20, 10]
```

---

## Program 2: Find First Non-Repeating Character in a String

### Problem Statement

Given a string, find the **first non-repeating character** using Stream API.

---

### Code Example

```java
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;

public class FirstNonRepeatingChar {
    public static void main(String[] args) {

        String input = "javaarticles";

        Character result = input.chars()
                .mapToObj(c -> (char) c)
                .collect(Collectors.groupingBy(
                        Function.identity(),
                        LinkedHashMap::new,
                        Collectors.counting()))
                .entrySet()
                .stream()
                .filter(entry -> entry.getValue() == 1)
                .map(Map.Entry::getKey)
                .findFirst()
                .orElse(null);

        System.out.println("First Non-Repeating Character: " + result);
    }
}
```

---

### Step-by-Step Explanation

1. **Convert String to IntStream**

   ```java
   input.chars()
   ```

   Returns ASCII values of characters.

2. **Convert int to Character**

   ```java
   mapToObj(c -> (char) c)
   ```

   Converts each ASCII value to `Character`.

3. **Group Characters with Count**

   ```java
   Collectors.groupingBy(
       Function.identity(),
       LinkedHashMap::new,
       Collectors.counting()
   )
   ```

   * `Function.identity()` → character itself as key
   * `LinkedHashMap` → preserves insertion order
   * `counting()` → counts occurrences

4. **Filter Non-Repeating Characters**

   ```java
   filter(entry -> entry.getValue() == 1)
   ```

   Keeps characters that appear only once.

5. **Get First Entry**

   ```java
   findFirst()
   ```

   Retrieves the first non-repeating character.

---

### Output

```
First Non-Repeating Character: j
```

---

## Program 3: Find Maximum Salary from Employee List

### Problem Statement

Find the **highest salary** from a list of employees using Java 8 Stream API.

---

### Employee Class

```java
class Employee {
    private int id;
    private String name;
    private double salary;

    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }
}
```

---

### Main Program

```java
import java.util.*;

public class MaxSalary {
    public static void main(String[] args) {

        List<Employee> employees = Arrays.asList(
                new Employee(1, "A", 45000),
                new Employee(2, "B", 65000),
                new Employee(3, "C", 55000)
        );

        double maxSalary = employees.stream()
                .map(Employee::getSalary)
                .max(Double::compare)
                .orElse(0.0);

        System.out.println("Maximum Salary: " + maxSalary);
    }
}
```

---

### Step-by-Step Explanation

1. **Create Employee List**

   ```java
   List<Employee> employees = Arrays.asList(...)
   ```

2. **Convert List to Stream**

   ```java
   employees.stream()
   ```

3. **Extract Salary**

   ```java
   map(Employee::getSalary)
   ```

   Converts `Stream<Employee>` → `Stream<Double>`.

4. **Find Maximum Value**

   ```java
   max(Double::compare)
   ```

   Compares salary values and returns the highest.

5. **Handle Empty List**

   ```java
   orElse(0.0)
   ```

   Prevents `NoSuchElementException`.

---

### Output

```
Maximum Salary: 65000.0
```

---

## Summary Table

| Program            | Logic Used       | Key Stream Methods                  |
| ------------------ | ---------------- | ----------------------------------- |
| Duplicates         | Frequency check  | `filter`, `collect`                 |
| Non-Repeating Char | Grouping + order | `groupingBy`, `filter`, `findFirst` |
| Max Salary         | Aggregation      | `map`, `max`                        |

---

