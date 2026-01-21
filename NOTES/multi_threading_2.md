# Multi-threading in Java

## Overview
- Java application → creates **multiple threads**
- Threads run inside **one process**
- JVM maps **Java threads to OS threads**
- OS **CPU allocation & scheduler** decide execution order

> 🔹 Thread execution order **cannot be predicted**, but we can give **hints** to the OS scheduler using **thread priority**

---

## Thread Priority
Thread priority range: **1 to 10**

```text
LOW_PRIORITY     = 1
DEFAULT_PRIORITY = 5
HIGH_PRIORITY    = 10
````

> Priority is only a *hint* to the OS scheduler, not a guarantee.

---

## Synchronization

### Synchronized Method & Block

* `synchronized` keyword ensures **mutual exclusion**
* Thread must acquire a **monitor lock**

Types of locks:

* **Method-level lock**
* **Block-level (object) lock**
* **Class-level lock**

> Need to understand how **method lock, block lock, and class lock** work internally.

---

## Deadlock in Threads

### When Deadlock Happens

* Two threads
* Two shared resources

Scenario:

* Thread-1 locks **Resource-1** and waits for **Resource-2**
* Thread-2 locks **Resource-2** and waits for **Resource-1**
* Neither releases its lock

➡️ **Result: DEADLOCK**

---

## Deadlock Sample Program

```java
package com.jp.multithreading.java.thread;

public class DeadLockTest {

    public static void main(String[] args) {

        Object resource1 = new Object();
        Object resource2 = new Object();

        Thread t1 = new Thread(() -> {
            synchronized (resource1) {
                System.out.println("Thread-1 locked resource1");

                try {
                    Thread.sleep(100);
                } catch (Exception e) {
                    e.printStackTrace();
                }

                synchronized (resource2) {
                    System.out.println("Thread-1 locked resource2");
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (resource2) {
                System.out.println("Thread-2 locked resource2");

                try {
                    Thread.sleep(100);
                } catch (Exception e) {
                    e.printStackTrace();
                }

                synchronized (resource1) {
                    System.out.println("Thread-2 locked resource1");
                }
            }
        });

        t1.start();
        t2.start();
    }
}
```

---

## How Deadlock Happens (Execution Flow)

### 🔄 Step-by-Step

1. Thread-1 locks `resource1`
2. Thread-2 locks `resource2`
3. Thread-1 waits for `resource2`
4. Thread-2 waits for `resource1`

❌ **Both threads wait forever → DEADLOCK**

---

## Prevent Deadlock

### 1. Lock Ordering

Always acquire locks in the **same order**

#### Example

```java
package com.jp.multithreading.java.thread;

public class DeadlockPrevention {

    static Object resource1 = new Object();
    static Object resource2 = new Object();

    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {
            synchronized (resource1) {
                synchronized (resource2) {
                    System.out.println("Thread-1 done");
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (resource1) {   // SAME ORDER
                synchronized (resource2) {
                    System.out.println("Thread-2 done");
                }
            }
        });

        t1.start();
        t2.start();
    }
}
```

---

### 2. Using `ReentrantLock`

* Try-lock mechanism
* Timeout-based locking
* More control than `synchronized`

> (Need to explore in detail)

---

### 3. Use Higher-Level Concurrency APIs

* `ExecutorService`
* `ConcurrentHashMap`
* `BlockingQueue`

---

## Types of Threads

### 1. User Thread

* JVM waits until **all user threads complete**
* Contains **business / important logic**

### 2. Daemon Thread

* Runs in background
* Performs cleanup tasks
* JVM terminates when **only daemon threads remain**

```java
thread.setDaemon(true);
```

---

## Example: User Thread vs Daemon Thread

```java
package com.jp.multithreading.java.thread;

public class UserThreadAndDemonThreadTest {

    public static void main(String[] args) {

        Thread thread1 = new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                System.out.println("i :" + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        Thread thread2 = new Thread(() -> {
            for (int j = 11; j <= 50; j++) {
                System.out.println("j: " + j);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        thread1.setName("Thread-1");
        thread2.setName("Thread-2");
        thread2.setDaemon(true);

        thread1.start();
        thread2.start();
    }
}
```

---

## Executor Framework

### Overview

* Introduced in **Java 1.5**
* Simplifies multithreading
* Package: `java.util.concurrent`
* Uses **thread pools**
* Improves performance by reusing threads

---

### Core Interfaces & Classes

1. `Executor` (Interface)
2. `ExecutorService` (Interface)
3. `Executors` (Utility Class)

---

### Types of Executors

1. Single Thread Pool Executor
2. Fixed Thread Pool Executor
3. Cached Thread Pool Executor

---

## Defining Tasks for Executor

### 1. `Runnable`

* `void run()`
* Does **not return value**

### 2. `Callable<V>`

* `V call()`
* **Returns value**

---

## Using Runnable with Executor

```java
package com.jp.multithreading.java.thread.executors;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class ExecutorsTest {

    public static void main(String[] args) {

        ExecutorService executorService = Executors.newFixedThreadPool(3);

        for (int i = 1; i <= 10; i++) {
            int taskNumber = i;
            executorService.execute(() -> {
                System.out.println("The current thread is :" +
                        Thread.currentThread().getName() +
                        " executing task :" + taskNumber);
                try {
                    TimeUnit.MILLISECONDS.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                    Thread.currentThread().interrupt();
                }
            });
        }

        executorService.shutdown();
    }
}
```

### Output

```text
The current thread is :pool-1-thread-2 executing task :2
The current thread is :pool-1-thread-1 executing task :1
The current thread is :pool-1-thread-3 executing task :3
The current thread is :pool-1-thread-3 executing task :4
The current thread is :pool-1-thread-1 executing task :5
The current thread is :pool-1-thread-2 executing task :6
The current thread is :pool-1-thread-1 executing task :7
The current thread is :pool-1-thread-3 executing task :9
The current thread is :pool-1-thread-2 executing task :8
The current thread is :pool-1-thread-1 executing task :10
```

---

## Using Callable with Executor

```java
package com.jp.multithreading.java.thread.executors;

import java.util.concurrent.*;

public class CallableExecutorTest {

    public static void main(String[] args) {

        ExecutorService executorService = Executors.newFixedThreadPool(3);
        System.out.println("Main Thread is Starting...");

        System.out.println("Submit the Task!");

        Future<String> future = executorService.submit(() -> {
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
                Thread.currentThread().interrupt();
            }
            return "Hello World";
        });

        executorService.shutdown();
        System.out.println("Main Thread is working on...");

        try {
            System.out.println("Result of submitted task: " + future.get());
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```

### Output

```text
Main Thread is Starting...
Submit the Task!
Main Thread is working on...
Result of submitted task: Hello World
```
