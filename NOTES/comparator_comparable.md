# Comparable and Comparator Interfaces in Java

## What is Comparator

- **Comparator** is an **interface**.  
- Comparator is introduced for **sorting a group of elements** in either **ascending** or **descending** order.  
- Comparator is applicable for both **arrays** and **collections**.  
- By default, the **sorting order is ascending**.

# What is Comparable

- **Comparable** is an **interface**.  
- Comparable is introduced for **sorting a group of elements** in either **ascending** or **descending** order.  
- Comparable is applicable for both **arrays** and **collections**.  
- By default, the **sorting order is ascending**.  
- Whenever you perform sorting on a **custom class**, Java does not know how to sort the objects.  
  So, Java internally uses the **Comparable interface**.  
  If your class does not implement the `Comparable` interface, Java throws the following exception:

---

## Example Program

### Student.java
```java
public class Student {
    private int rollNo;
    private String name;
    private int marks;

    public Student(int rollNo, String name, int marks) {
        this.rollNo = rollNo;
        this.name = name;
        this.marks = marks;
    }

    @Override
    public String toString() {
        return "Student{" +
                "rollNo=" + rollNo +
                ", name='" + name + '\'' +
                ", marks=" + marks +
                '}';
    }
}
````

---

### Test.java

```java
import java.util.Arrays;

public class SortingPratice {

    public static void main(String[] args) {
        Student[] students = {
            new Student(105, "Rajesh", 60),
            new Student(103, "Amar", 70),
            new Student(101, "Balu", 80),
            new Student(104, "David", 68),
            new Student(102, "Yada", 55),
        };

        Arrays.sort(students);
        System.out.println(Arrays.toString(students));
    }
}
```

---

### Output

```
Exception in thread "main" java.lang.ClassCastException: 
class com.jp.comparator.comparable.Student cannot be cast to class java.lang.Comparable 
(com.jp.comparator.comparable.Student is in unnamed module of loader 'app'; 
java.lang.Comparable is in module java.base of loader 'bootstrap')
```
# Using `compareTo()` Method in Comparable

- We can use the **`compareTo()`** method to sort elements in either **ascending** or **descending** order for data types like **Integer** or **String**.

---

## Example Program

### Student.java
```java
public class Student implements Comparable<Student> {
    private int rollNo;
    private String name;
    private int marks;

    public Student(int rollNo, String name, int marks) {
        this.rollNo = rollNo;
        this.name = name;
        this.marks = marks;
    }

    @Override
    public String toString() {
        return "Student{" +
                "rollNo=" + rollNo +
                ", name='" + name + '\'' +
                ", marks=" + marks +
                '}';
    }

    @Override
    public int compareTo(Student anotherStudent) {
        // return (this.rollNo < anotherStudent.rollNo) ? -1 : ((this.rollNo == anotherStudent.rollNo) ? 0 : 1);
        // return this.rollNo - anotherStudent.rollNo;
        return Integer.compare(this.rollNo, anotherStudent.rollNo); // -> These 3 are ascending order examples

        // return Integer.compare(anotherStudent.rollNo, this.rollNo); // -> Descending order for integer
        // return this.name.compareTo(anotherStudent.name);            // -> Name ascending order
        // return anotherStudent.name.compareTo(this.name);            // -> Name descending order
    }
}
````

---

### Test.java

```java
import java.util.*;

public class SortingPratice {

    public static void main(String[] args) {

        Student[] students = {
            new Student(105, "Rajesh", 60),
            new Student(103, "Amar", 70),
            new Student(101, "Balu", 80),
            new Student(104, "David", 68),
            new Student(102, "Yada", 55),
        };

        Arrays.sort(students);
        System.out.println(Arrays.toString(students));

        List<Student> studentList = new ArrayList<>(Arrays.asList(students));
        Collections.sort(studentList);
        System.out.println(studentList);
    }
}
```

---

### Output

```
[Student{rollNo=101, name='Balu', marks=80}, 
 Student{rollNo=102, name='Yada', marks=55}, 
 Student{rollNo=103, name='Amar', marks=70}, 
 Student{rollNo=104, name='David', marks=68}, 
 Student{rollNo=105, name='Rajesh', marks=60}]
```


# Why Do We Use Comparator If Comparable Is Present

---

### 1. Limitations of Comparable
- We **cannot change the default sorting order (ascending)** for Java-defined classes like `Integer`, `String`, etc.  
- If you want to change the sorting order (e.g., descending), you can use the **Comparator** interface.  

---

### 2. Purpose of Comparator
- **Comparator** is used to perform **custom sorting**.  
- For example, suppose we want to sort students in multiple ways:  

```java
// sort by name
Collections.sort(studentList);
System.out.println(studentList);

// sort by marks
Collections.sort(studentList);
System.out.println(studentList);

// sort by height
Collections.sort(studentList);
System.out.println(studentList);
````

* By using **Comparable**, we can only achieve **one type of sorting** (e.g., by name **or** marks **or** height) because `Comparable` is implemented **directly in the Student class**.
* For this reason, we use the **Comparator interface** — it allows multiple different sorting criteria without modifying the original class.

---

### 3. Comparator Sorting Behavior

* **Descending order logic:**

  * **Positive integer →** No swap
  * **Negative integer →** Swap
  * **Zero (equal) →** No swap

---

### 4. How to Use Comparator

```java
Comparator<Student> nameComparator = Comparator.comparing((Student s) -> s.name);
Arrays.sort(students, nameComparator);
```

* The above code internally calls `Arrays.sort(students, nameComparator);` which compares **two Student objects**.
* You only need to specify **based on which attribute** (e.g., `name`, `marks`, `height`) the sorting should happen.

---

### 5. Handling Null Values Using Comparator

* By default, the above code can place null values **either first or last** depending on how you define it.
* Example to handle nulls safely:

```java
Comparator<Student> nameComparator =
    Comparator.comparing(s -> s.name, Comparator.nullsFirst(Comparator.naturalOrder()));
```

---

### 6. Combining Multiple Comparators

* When multiple fields (e.g., marks, mathsMarks, physicsMarks) have the same values, we can **combine two or more comparators** to define sorting precedence.

```java
Collections.sort(studentList,
    marksComparator
        .thenComparing(mathsMarksComparator)
        .thenComparing(physicsMarksComparator));
```

---

### 7. Priority Between Comparable and Comparator

* If both **Comparable** and **Comparator** are used,
  → **Comparator** takes **higher priority** (Comparable is the least priority).

# Ascending Order Logic

- The below explains the **ascending order** comparison logic:  
  - **Positive integer →** swap  
  - **Negative integer →** no swap  
  - **Zero (equal) →** no swap  

---

# Final Example Program

## Student.java
```java
public class Student implements Comparable<Student> {
    public int rollNo;
    public String name;
    public int marks;
    public int mathMarks;
    public int physicsMarks;

    public Student(int rollNo, String name, int marks, int mathMarks, int physicsMarks) {
        this.rollNo = rollNo;
        this.name = name;
        this.marks = marks;
        this.mathMarks = mathMarks;
        this.physicsMarks = physicsMarks;
    }

    @Override
    public String toString() {
        return "Student{" +
                "rollNo=" + rollNo +
                ", name='" + name + '\'' +
                ", marks=" + marks +
                ", mathMarks=" + mathMarks +
                ", physicsMarks=" + physicsMarks +
                '}';
    }

    @Override
    public int compareTo(Student anotherStudent) {
        // return (this.rollNo < anotherStudent.rollNo) ? -1 : ((this.rollNo == anotherStudent.rollNo) ? 0 : 1);
        // return this.rollNo - anotherStudent.rollNo;
        return Integer.compare(this.rollNo, anotherStudent.rollNo); // -> These 3 are ascending order examples
        // return Integer.compare(anotherStudent.rollNo, this.rollNo); -> descending order for integer
        // return this.name.compareTo(anotherStudent.name); -> Name ascending order
        // return anotherStudent.name.compareTo(this.name); -> Name descending order
    }
}
````

---

## Test.java

```java
import java.util.*;

public class SortingPratice {

    public static void main(String[] args) {

        /*Comparator<Integer> comparator = new Comparator<>() {
            @Override
            public int compare(Integer x, Integer y) {
                return (x < y) ? 1 : ((x == y) ? 0 : -1);
            }
        };*/

        // Comparator<Integer> comparator = (x, y) -> (x < y) ? 1 : ((x == y) ? 0 : -1);
        // Comparator<Integer> comparator = (x, y) -> Integer.compare(y, x);
        Comparator<Integer> comparator = Comparator.comparingInt((Integer i) -> i).reversed();

        Integer[] arr = {2, 8, 4, 10, 67};
        Arrays.sort(arr);
        Arrays.sort(arr, comparator);
        // System.out.println(Arrays.toString(arr));

        List<Integer> list = new ArrayList<>(Arrays.asList(arr));
        Collections.sort(list);
        // System.out.println(list);

        /*Comparator<Student> nameComparator = new Comparator<>() {
            @Override
            public int compare(Student s1, Student s2) {
                return s1.name.compareTo(s2.name);
                // return s2.name.compareTo(s1.name);
            }
        };*/

        // Comparator<Student> nameComparator = (s1, s2) -> s1.name.compareTo(s2.name);
        Comparator<Student> nameComparator = Comparator.comparing(
                s -> s.name,
                Comparator.nullsFirst(Comparator.naturalOrder())
        );

        Comparator<Student> marksComparator = Comparator.comparing(s -> s.marks);
        Comparator<Student> mathsMarksComparator = Comparator.comparing(s -> s.mathMarks);
        Comparator<Student> physicsMarksComparator = Comparator.comparing(s -> s.physicsMarks);

        Student[] students = {
                new Student(105, "Rajesh", 60, 20, 30),
                new Student(103, "Amar", 70, 40, 80),
                new Student(101, "Balu", 80, 20, 50),
                new Student(104, "David", 80, 50, 80),
                new Student(102, null, 55, 40, 50)
        };

        // Arrays.sort(students, nameComparator.reversed());
        // Arrays.sort(students, nameComparator);
        // System.out.println(Arrays.toString(students));

        List<Student> studentList = new ArrayList<>(Arrays.asList(students));

        // Sort by name
        Collections.sort(studentList, nameComparator);
        System.out.println(studentList);

        // Sort by marks
        Collections.sort(studentList,
                marksComparator
                        .thenComparing(mathsMarksComparator)
                        .thenComparing(physicsMarksComparator));
        System.out.println(studentList);

        // Sort by rollNo
        Collections.sort(studentList);
        System.out.println(studentList);
    }
}
```

---

## Output

### By Name

```
[Student{rollNo=102, name='null', marks=55, mathMarks=40, physicsMarks=50},
 Student{rollNo=103, name='Amar', marks=70, mathMarks=40, physicsMarks=80},
 Student{rollNo=101, name='Balu', marks=80, mathMarks=20, physicsMarks=50},
 Student{rollNo=104, name='David', marks=80, mathMarks=50, physicsMarks=80},
 Student{rollNo=105, name='Rajesh', marks=60, mathMarks=20, physicsMarks=30}]
```

### By Marks

```
[Student{rollNo=102, name='null', marks=55, mathMarks=40, physicsMarks=50},
 Student{rollNo=105, name='Rajesh', marks=60, mathMarks=20, physicsMarks=30},
 Student{rollNo=103, name='Amar', marks=70, mathMarks=40, physicsMarks=80},
 Student{rollNo=101, name='Balu', marks=80, mathMarks=20, physicsMarks=50},
 Student{rollNo=104, name='David', marks=80, mathMarks=50, physicsMarks=80}]
```

### By Roll Number

```
[Student{rollNo=101, name='Balu', marks=80, mathMarks=20, physicsMarks=50},
 Student{rollNo=102, name='null', marks=55, mathMarks=40, physicsMarks=50},
 Student{rollNo=103, name='Amar', marks=70, mathMarks=40, physicsMarks=80},
 Student{rollNo=104, name='David', marks=80, mathMarks=50, physicsMarks=80},
 Student{rollNo=105, name='Rajesh', marks=60, mathMarks=20, physicsMarks=30}]
```



