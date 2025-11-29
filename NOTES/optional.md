# Optional Class in Java

## What is Optional?
- `Optional` is a class introduced in Java 1.8. It is used to avoid `NullPointerException`.
- Optional works like a **container**. Developers can check whether the value is present or not (null check).  
  This helps prevent `NullPointerException` while accessing values.

---

## Example Program

### Student.java
```java
import java.util.Optional;

public class Student {
    private int rollNo;
    private String Name;
    private String address;

    public Student(int rollNo, String name, String address) {
        this.rollNo = rollNo;
        Name = name;
        this.address = address;
    }

    public int getRollNo() {
        return rollNo;
    }

    public void setRollNo(int rollNo) {
        this.rollNo = rollNo;
    }

    public String getName() {
        return Name;
    }

    public void setName(String name) {
        Name = name;
    }

    public Optional<String> getAddress() {
        // return address;
        // return Optional.of(address); // of() accepts only non-null values
        return Optional.ofNullable(address); // Returns Optional with value or empty Optional
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
````

---

### OptionalTest.java

```java
import java.util.List;
import java.util.Optional;

public class OptionalTest {
    public static void main(String[] args) {
        List<Student> studentList = List.of(
                new Student(101, "Suresh", "Bajara Hills"),
                new Student(102, "Ramesh", null),
                new Student(103, "Ganesh", "Hitech City"),
                new Student(104, "Shiva", ""),
                new Student(105, "Gowtham", "Madharpur")
        );

        Student s1 = studentList.get(3);

        Optional<String> address = s1.getAddress();

        /*
          Developer may not know if address is null or has a value.
          Without Optional, accessing address.length() may cause:

          java.lang.NullPointerException:
          Cannot invoke "String.length()" because "address" is null

          Normally we handle using:
            if(address != null) { ... }

          Java 1.8 introduced Optional to avoid such NullPointerExceptions.
        */

        /*
          Optional simplifies null checks:

          if(address.isPresent()) {
              System.out.println(address.get().length());
          } else {
              System.out.println("NA");
          }
        */

        /*
          ifPresent() accepts a Consumer.
          ifPresentOrElse() accepts a Consumer and a Runnable.
        */

        address.ifPresentOrElse(
                a -> System.out.println(a.length()),
                () -> System.out.println("NA")
        );

        String name = s1.getName();
        System.out.println(name);

        int rollNo = s1.getRollNo();
        System.out.println(rollNo);
    }
}
```

```
