# Multitasking and Multithreading in Java

## What is a Task? What is Multitasking?

**Multitasking** in Java refers to the ability of a program (or the operating system running it) to execute multiple tasks — that is, multiple **processes** or **threads** — simultaneously.  
Each task represents a specific unit of work that can be executed independently.

---

## What is a Thread? What is Multithreading?

A **Thread** is an independent unit of execution within a program.  
**Multithreading** refers to executing multiple independent parts (threads) of the same program **simultaneously**.

For example, consider an application like **WhatsApp**:

- When you open WhatsApp, the operating system allocates memory and resources to it — this is known as a **process**.  
  Every process has a unique **process ID**.
- Within that process, multiple **threads** are created to perform different tasks:
  - Sending a message → handled by one thread  
  - Making a voice or video call → handled by another thread  

By running multiple threads at the same time, the application can perform several operations concurrently — this is **multithreading**.  
When multiple applications (processes) run simultaneously, it is known as **multiprocessing**, and together these concepts enable **multitasking** and **multithreading**.

---

## Relationship Between Process, Task, and Thread

```

Application → Process → Process ID → Task → Thread (executes the task)

```

Each process is responsible for managing its own threads, which carry out specific tasks within that process.

---

## Key Insights

- **Without multithreading**, multitasking can still be achieved using multiple processes.  
  However, this approach is **less efficient**, as each process requires its own memory space and resources.
- **Multithreading** allows multiple tasks to run concurrently within the same process, leading to:
  - Better CPU utilization  
  - Faster execution  
  - Improved application performance  

In short, **multithreading** enables efficient multitasking by allowing multiple parts of a program to execute simultaneously.


# Default (Predefined) Threads in Java

## Overview

By default, **Java** creates a single thread called the **main thread** to execute the code inside the `main()` method.  
Even if your `main()` method contains thousands of lines of code, Java still uses **only one thread** — the **main thread** — to execute it.

---

## Thread Class and Package

The `Thread` class is part of the **`java.lang`** package, which means it is available by default without any import statements.

---

## Viewing the Current Thread

You can retrieve the current executing thread using the following method:

```java
Thread.currentThread();
````

Every thread in Java has two important attributes:

* **Thread ID**
* **Thread Name**

You can obtain these values using:

```java
Thread.currentThread().getName(); // Returns the name of the thread
Thread.currentThread().getId();   // Returns the ID of the thread
```

---

## Thread Priority

Each thread in Java has a **priority level**, which helps the scheduler decide the order of execution.
Thread priorities range from **0 to 10**, where:

| Priority Level | Description               |
| -------------- | ------------------------- |
| 10             | Highest Priority          |
| 5              | Medium Priority (Default) |
| 0              | Lowest Priority           |

> Note: The **main thread** has a **default priority** of **5** (medium priority).

---

## Checking Active Threads

You can check how many threads are currently active in a Java program using:

```java
Thread.activeCount();
```

This method returns an **integer value** representing the total number of active threads currently running in the Java Virtual Machine (JVM).

---

## Summary

* Java automatically creates a **main thread** to execute the program.
* The `Thread` class is part of the `java.lang` package.
* You can access thread information using methods like:

  * `getName()`
  * `getId()`
  * `activeCount()`
* Thread priorities range from **1 (lowest)** to **10 (highest)**, with **5** being the default for the main thread.

---


Here’s your text rewritten in **professional English** and formatted as a clean, well-structured **Markdown (.md)** document:

---

````markdown
# Example: Default Thread Created by Java

## Program Example

```java
public class MultithreadingTest {
    public static void main(String[] args) {
        System.out.println("By default, the current execution thread: " + Thread.currentThread().getName());
        System.out.println("Active threads by default: " + Thread.activeCount());
        System.out.println("Current priority of the main thread: " + Thread.currentThread().getPriority());

        // Display all threads and their types (daemon or user)
        Thread.getAllStackTraces().keySet().forEach(t -> {
            System.out.println(t.getName() + " - " + (t.isDaemon() ? "daemon" : "user"));
        });
    }
}
````

---

## Output

```
By default, the current execution thread: main
Active threads by default: 2
Current priority of the main thread: 5
Common-Cleaner - daemon
Reference Handler - daemon
Finalizer - daemon
main - user
Monitor Ctrl-Break - daemon
Attach Listener - daemon
Signal Dispatcher - daemon
Notification Thread - daemon
```

---

## Explanation

* **Main Thread:**
  Java automatically creates the `main` thread to execute the `main()` method.

* **Daemon Threads:**
  Daemon threads are background threads that provide system-level services (e.g., garbage collection, signal handling, etc.).
  They run in the background and do not prevent the JVM from exiting once all user threads finish.

* **User Thread:**
  The `main` thread is a **user thread** — it must complete before the JVM terminates.

* **`Thread.getAllStackTraces()` Method:**
  This method returns all currently running threads in the JVM and their corresponding stack traces.
  Here, it’s used to list all thread names and whether they are daemon or user threads.

---

# Default Threads in Modern Java Versions

## Overview

You might expect only **one thread** — the `main` thread — to exist when a Java program starts.  
However, in practice, you often see **two or more threads** in newer Java versions (Java 11, Java 17, Java 21, etc.).

---

## Why You Might See Multiple Threads

Starting from newer Java versions, the **JVM** and its core libraries automatically start **background helper threads** for internal housekeeping tasks.  
The exact number and purpose of these threads can vary depending on the **JDK vendor** (e.g., OpenJDK, Oracle JDK, GraalVM) and the **platform** being used.

Typical threads you might observe:

| Thread Name       | Type         | Description |
|-------------------|--------------|--------------|
| `main`            | User Thread  | Executes the main program code inside the `main()` method. |
| `Common-Cleaner`  | Daemon Thread | Used by `java.lang.ref.Cleaner` to run cleanup actions for classes like `FileChannel`, `DirectByteBuffer`, etc. |

---

## The Cleaner API

- **Introduced in Java 9**, the **Cleaner API** replaced the deprecated `finalize()` mechanism as a **safer and more efficient** way to manage resource cleanup.  
- The **JVM lazily initializes** the `Common-Cleaner` daemon thread **only when needed**.  
- From **Java 17 onward**, many core APIs (such as I/O and NIO) rely on the `Cleaner`, which means this background thread often starts automatically when your program begins.

---

## What Is a Daemon Thread?

A **daemon thread** in Java is a **background service thread** that supports other threads, primarily **user threads**.

### Characteristics of a Daemon Thread:
- Runs in the background with **low priority**.
- Provides services to user threads (for example, memory cleanup or resource management).
- The **JVM automatically terminates** all daemon threads once **all user threads** have finished execution.
- Common examples include:
  - Garbage Collector threads  
  - Finalizer / Cleaner threads  
  - Signal Dispatcher threads  

---

# Implementing Multithreading in Java

## Creating Threads Using the `Thread` Class

### Example 1: Basic Thread Creation

```java
public class ThreadCreationTest {
    public static void main(String[] args) {
        Thread t1 = new Thread();
        Thread t2 = new Thread();
        Thread t3 = new Thread();
        Thread t4 = new Thread();

        t1.start();
        t2.start();
        t3.start();
        t4.start();

        System.out.println("Current active threads: " + Thread.activeCount());
    }
}
````

**Output:**

```
Current active threads: 6
```

Explanation:

* `4` user-defined threads (`t1, t2, t3, t4`)
* `1` main thread
* `1` `Common-Cleaner` daemon thread

---

### Key Points:

1. `Thread t1 = new Thread();`

   * Only creates a thread object; it does **not start execution**.

2. `t1.start();`

   * Changes the thread state to **running**.
   * Internally, `start()` calls the `run()` method of the thread.

3. In **newer Java versions**, Java automatically creates **2 default threads** (e.g., `main` and `Common-Cleaner`).

   * These default threads are **not removed** when new user-defined threads are created.

4. Once a thread completes execution, it is immediately **removed from the JVM thread stack**.

5. Thread execution is **not sequential**; the order cannot be predicted.

   * Thread **priority** can influence the order of execution but does not guarantee it.

---

## Example 2: Thread with Custom Names

```java
public class ThreadCreationTest {
    public static void main(String[] args) {
        Employee e1 = new Employee("Employee-Thread");
        e1.start();

        Manager m1 = new Manager("Manager-Thread");
        m1.start();

        System.out.println("Current active threads: " + Thread.activeCount());
    }
}

class Employee extends Thread {
    public Employee(String tName) {
        super(tName);
    }

    @Override
    public void run() {
        System.out.println("Employee: " + Thread.currentThread().getName() + " >> " + Thread.currentThread().getId());
    }
}

class Manager extends Thread {
    public Manager(String tName) {
        super(tName);
    }

    @Override
    public void run() {
        System.out.println("Manager: " + Thread.currentThread().getName() + " >> " + Thread.currentThread().getId());
    }
}
```

**Output:**

```
Current active threads: 4
Employee: Employee-Thread >> 16
Manager: Manager-Thread >> 17
```

---

### Key Points:

1. **Setting Thread Names:**

   * The `Thread` class constructor allows you to set a thread name:

   ```java
   public Employee(String tName) {
       super(tName);
   }
   Employee e1 = new Employee("Employee-Thread");
   ```

2. **Thread Execution:**

   * Threads execute **concurrently**, and the exact execution order cannot be guaranteed.
   * Thread priority can be set to influence scheduling.

3. **Thread Class Constructor:**

```java
public Thread(String name) {
    this(null, null, name, 0);
}
```

* Allows creating a thread with a **custom name**.

 **Picture Respresentation**
 
<img width="1413" height="524" alt="Screenshot 2025-10-16 111956" src="https://github.com/user-attachments/assets/d6fc8e58-bb13-4fe5-bc3d-178bf11256a6" />

Here’s your text rewritten in **professional English** and formatted as a **Markdown (.md)** document:

---

````markdown
# Creating Threads and Thread Methods in Java

## Creating Threads Using the Runnable Interface

In Java, threads are **not directly created using the `Runnable` interface** because `Runnable` does **not have a `start()` method**.  
Instead, we can:

1. Implement the `Runnable` interface.
2. Pass the `Runnable` object to the **`Thread` class constructor**.
3. Call `start()` on the `Thread` object to begin execution.

Example:
```java
Runnable task = new MyRunnable();
Thread t = new Thread(task);
t.start();
````

---

## `join()` Method

The `join()` method allows one thread to **wait for another thread to complete** before continuing execution.

**Example:**

```java
t1.join();  // t1 completes first, then the next thread executes sequentially
```

* Useful when one thread depends on the result or completion of another thread.

---

## `sleep()` Method

The `sleep()` method **pauses a thread for a specified period**.
This is commonly used to delay execution or simulate wait time.

**Example:**

```java
Thread.sleep(10000); // Pauses the thread for 10 seconds
```

* `sleep()` does not release locks held by the thread.

---

## Thread Lifecycle

A thread in Java goes through several **states** during its lifecycle:

| State             | Description                                                        |
| ----------------- | ------------------------------------------------------------------ |
| **New**           | Thread object is created but not started yet.                      |
| **Runnable**      | Thread is ready to run and waiting for CPU scheduling.             |
| **Waiting**       | Thread waits indefinitely for another thread (e.g., via `join()`). |
| **Timed Waiting** | Thread waits for a specified period (e.g., via `sleep(timeout)`).  |
| **Terminated**    | Thread has completed execution and cannot be restarted.            |

* You can get the current state of a thread using:

```java
t1.getState();
```

* Once a thread is **terminated**, it cannot be restarted.
  Attempting to call `start()` on a terminated thread will throw an exception:

```text
IllegalThreadStateException
```
Here’s your text rewritten in **professional English** and formatted as a clean, structured **Markdown (.md)** document:

---

````markdown
# Example: Creating Threads Using the Runnable Interface

## Program Example

```java
public class RunnableInterfaceTest {
    public static void main(String[] args) throws InterruptedException {

        // Runnable for Employee Thread
        Runnable r1 = () -> {
            for(int i = 0; i < 10; i++) {
                System.out.println("Employee1 --- " + Thread.currentThread().getName() + " >> " + Thread.currentThread().getId());
            }
        };

        Thread t1 = new Thread(r1, "employee-thread");
        System.out.println(t1.getState()); // NEW
        t1.start();
        System.out.println(t1.getState()); // RUNNABLE
        t1.sleep(10000); // Pause current thread for 10 seconds

        // Runnable for Manager Thread
        Runnable r2 = () -> {
            for(int i = 0; i < 20; i++) {
                System.out.println("Manager1 --- " + Thread.currentThread().getName() + " >> " + Thread.currentThread().getId());
            }
        };

        Thread t2 = new Thread(r2, "manager-thread");
        t2.start();
        System.out.println(t1.getState()); // TERMINATED after completion

        // Attempting to restart t1 will throw an exception
        t1.start(); 
        // System.out.println("Current active threads: " + Thread.activeCount());
    }
}
````

---

## Sample Output

```
NEW
RUNNABLE
Employee1 --- employee-thread >> 16
Employee1 --- employee-thread >> 16
Employee1 --- employee-thread >> 16
Employee1 --- employee-thread >> 16
Employee1 --- employee-thread >> 16
Employee1 --- employee-thread >> 16
Employee1 --- employee-thread >> 16
Employee1 --- employee-thread >> 16
Employee1 --- employee-thread >> 16
Employee1 --- employee-thread >> 16
TERMINATED
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Manager1 --- manager-thread >> 17
Exception in thread "main" java.lang.IllegalThreadStateException
	at java.base/java.lang.Thread.start(Thread.java:802)
	at com.jp.multithreading.RunnableInterfaceTest.main(RunnableInterfaceTest.java:57)
```

## Race Condition

A **race condition** occurs when **multiple threads update shared data simultaneously**, causing unexpected results.  
This is a common problem in multithreaded programs.

---

## Synchronization

**Synchronization** ensures that **only one thread at a time can access or modify shared data**, preventing race conditions.  

- **Updating data:** Use synchronization to allow threads to update data sequentially.  
- **Reading data:** Multiple threads can read shared data concurrently without issues.  

---

## `synchronized` Keyword

The `synchronized` keyword in Java is used to **control access to shared resources**:

- When multiple threads attempt to update the same data concurrently, **synchronization ensures one thread executes at a time**, resolving race conditions.
- For **read-only shared data**, use the `volatile` keyword instead of synchronization.

**Caution:**  
Overuse of synchronization can lead to **deadlock**:

- If a synchronized thread executes a long task, all other threads accessing the same resource are blocked, potentially causing a deadlock.

---

## Example Program: Race Condition and Synchronization

```java
public class BrickDiary {

    public int brickCount = 0;
    public int brickCount2 = 0;

    public void incrementBrickCount() {
        synchronized (this) {
            brickCount += 50;
        }

        brickCount2 += 50; // Non-synchronized update
    }
}
````

```java
public class RaceConditionTest {
    public static void main(String[] args) throws InterruptedException {

        BrickDiary bd = new BrickDiary();

        Runnable r1 = () -> {
            for(int i = 0; i < 10000; i += 50) {
                bd.incrementBrickCount();
            }
        };

        Runnable r2 = () -> {
            for(int i = 0; i < 15000; i += 50) {
                bd.incrementBrickCount();
            }
        };

        Runnable r3 = () -> {
            for(int i = 0; i < 5000; i += 50) {
                bd.incrementBrickCount();
            }
        };

        Thread t1 = new Thread(r1, "Worker-1");
        Thread t2 = new Thread(r2, "Worker-2");
        Thread t3 = new Thread(r3, "Worker-3");

        t1.start();
        t2.start();
        t3.start();

        t1.join();
        t2.join();
        t3.join();

        System.out.println(bd.brickCount);   // Synchronized update
        System.out.println(bd.brickCount2);  // Non-synchronized update
    }
}
```

---

## Sample Output

```
30000
30000
```

---

## Key Points

1. **Race condition:** Occurs when multiple threads update shared data simultaneously.
2. **Synchronization:** Ensures one thread updates shared data at a time, preventing race conditions.
3. **`synchronized` keyword:** Controls thread access to critical sections of code.
4. **Volatile keyword:** Suitable for read-only shared data accessed by multiple threads.
5. **Deadlock risk:** Excessive synchronization can block threads, causing deadlocks.


## What is Thread pool?
## What is inter-thread communication?

