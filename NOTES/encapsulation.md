# Encapsulation in Java (In-Depth)

## 📌 Definition
Encapsulation is one of the fundamental **Object-Oriented Programming (OOP)** principles in Java. It refers to **wrapping data (variables) and code (methods) into a single unit**, typically a **class**, and **restricting direct access** to some of the class's components. This is done to protect data from unauthorized access or unintended modifications.

Encapsulation is also known as **data hiding**, but technically, **data hiding is the outcome** of encapsulation.

---

## 🎯 Goals of Encapsulation
1. **Control access** to object data
2. **Protect internal state** from accidental or malicious changes
3. **Improve modularity and maintainability**
4. **Increase flexibility for future enhancements**
5. **Promote loose coupling**

---

## 🧱 How Encapsulation Works in Java
Encapsulation is achieved using two key concepts:

1. **Access Modifiers** (primarily `private` for fields)
2. **Getter and Setter methods** to access or modify private fields

---

## 🔐 Access Modifiers Summary

| Modifier     | Same Class | Same Package | Subclass | Other Packages |
|--------------|------------|---------------|----------|----------------|
| private      | ✔️          | ❌             | ❌        | ❌              |
| default      | ✔️          | ✔️             | ❌        | ❌              |
| protected    | ✔️          | ✔️             | ✔️        | ❌              |
| public       | ✔️          | ✔️             | ✔️        | ✔️              |

To encapsulate data properly, we mark fields as **private** and expose them using **public** getter/setter methods.

---

## 🧪 Example of Encapsulation

```java
public class Account {
    private double balance; // Encapsulated variable

    // Public getter
    public double getBalance() {
        return balance;
    }

    // Public setter with validation (control access)
    public void setBalance(double balance) {
        if (balance > 0) {
            this.balance = balance;
        } else {
            System.out.println("Invalid amount!");
        }
    }
}
````

---

## 🔍 Key Benefits

| Benefit            | Description                                               |
| ------------------ | --------------------------------------------------------- |
| Data Protection    | Prevents direct modification of fields                    |
| Validation Control | Allows adding logic to prevent invalid data               |
| Maintainability    | Changing internal structure does not impact external code |
| Flexibility        | Implementation can change without breaking code           |

---

## 🧠 Real-World Analogy

Think of encapsulation like a **capsule medicine** — it hides all complex ingredients from the user, exposing only what is necessary. Another analogy is **ATM machine**: you cannot directly access bank data, you interact through secure functions.

---

## 🚫 Without Encapsulation (Bad Practice)

```java
public class Student {
    public int age;
}
```

Issue: Anyone can modify `age` to negative or unrealistic values.

---

## ✔ With Encapsulation (Good Practice)

```java
public class Student {
    private int age;
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        if(age > 0 && age < 120) {
            this.age = age;
        } else {
            System.out.println("Invalid age!");
        }
    }
}
```

---

## ❓ Common Interview Questions

1. What is Encapsulation?
2. How do we achieve encapsulation in Java?
3. Is encapsulation the same as data hiding?
4. Why do variables need to be private?
5. Can you give real-life examples of encapsulation?

---

## 🧾 Summary

* Encapsulation binds data and methods into a unit (class).
* Achieved using `private` fields + `public` getters/setters.
* Protects data and improves maintainability.
* Allows controlled and validated access.

---


## 🚦 **Real-Time Scenario 1: Banking System (Most Common)**

When working with **bank accounts**, the balance must never be accessed or changed directly. It should only change through validated operations like **deposit, withdrawal, fund transfer**, etc.

```java
public class BankAccount {
    private double balance; // sensitive data
    private String accountNumber;

    public BankAccount(String accountNumber, double openingBalance) {
        this.accountNumber = accountNumber;
        this.balance = openingBalance;
    }

    // Controlled deposit
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        } else {
            throw new IllegalArgumentException("Amount must be positive");
        }
    }

    // Controlled withdrawal
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        } else {
            throw new IllegalArgumentException("Insufficient balance");
        }
    }

    // Only reading allowed
    public double getBalance() {
        return balance;
    }
}
```

### 🔍 Why Encapsulation Here?

* Prevents illegal access or tampering (`balance` stays protected)
* Allows centralized validation
* Business logic stays consistent across entire application

---

## 🛒 **Real-Time Scenario 2: E-Commerce Order System**

When customers place orders, the `orderStatus` should **never** be modified freely by any class. It must change only through proper workflow methods like **confirm**, **pack**, **ship**, **deliver**, **cancel**.

```java
public class Order {
    private String orderId;
    private String status = "CREATED";

    public Order(String orderId) {
        this.orderId = orderId;
    }

    public void confirmOrder() {
        if (status.equals("CREATED")) {
            status = "CONFIRMED";
        }
    }

    public void shipOrder() {
        if (status.equals("CONFIRMED")) {
            status = "SHIPPED";
        }
    }

    public void deliverOrder() {
        if (status.equals("SHIPPED")) {
            status = "DELIVERED";
        }
    }

    public String getStatus() {
        return status;
    }
}
```

### 🔍 Why Encapsulation?

Ensures order lifecycle is **valid and sequential**.

---

## 🚕 **Real-Time Scenario 3: Ride-Hailing App (Uber/Ola) Driver Ratings**

A driver’s rating cannot be directly set by other classes. It must be controlled through rating logic.

```java
public class Driver {
    private String name;
    private double rating;
    private int ratingCount;

    public Driver(String name) {
        this.name = name;
    }

    public void submitRating(int stars) {
        if (stars >= 1 && stars <= 5) {
            rating = ((rating * ratingCount) + stars) / (++ratingCount);
        } else {
            throw new IllegalArgumentException("Stars must be between 1 and 5");
        }
    }

    public double getRating() {
        return rating;
    }
}
```

### 🔍 Why Encapsulation?

Stops malicious users from setting rating to `100`, `-50`, etc.

---

## 🚑 **Real-Time Scenario 4: Medical Application (Patient Records)**

Patient medical history must remain confidential. Only authorized methods can update or view data.

```java
public class Patient {
    private String name;
    private String diagnosis;
    private String medicine;

    // Only doctor can update
    public void updateMedicalRecord(String diagnosis, String medicine, boolean isDoctor) {
        if (isDoctor) {
            this.diagnosis = diagnosis;
            this.medicine = medicine;
        } else {
            throw new SecurityException("Only doctors can update medical records");
        }
    }

    // Patients or doctors can view
    public String getMedicalDetails() {
        return diagnosis + " | " + medicine;
    }
}
```

---

## 📌 Summary of Real-Time Usefulness

| Need                             | Solution via Encapsulation   |
| -------------------------------- | ---------------------------- |
| Protect sensitive data           | Use private fields           |
| Control modification             | Use methods with validation  |
| Ensure business rule enforcement | Keep logic inside class      |
| Maintain future flexibility      | Modify internal logic safely |


