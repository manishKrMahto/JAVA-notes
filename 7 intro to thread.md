# Intro to Thread 


## <u>Thread </u>
Threads in Java are a fundamental feature for performing concurrent and parallel execution of tasks, allowing a program to perform multiple operations simultaneously. Here is a comprehensive explanation of threads in Java, covering the basics, how to create and manage threads, synchronization, and best practices.

### 1. What is a Thread?

A thread is a lightweight subprocess, the smallest unit of processing. A thread has its own call stack but shares the same process memory space, enabling threads within the same application to share resources and data more efficiently than separate processes.

### 2. The Java Thread Model

Java provides built-in support for multithreading at the language level. Every Java application runs at least one thread (the main thread), which is created by the Java Virtual Machine (JVM) at the program’s start.

### 3. Creating Threads in Java

There are two main ways to create a thread in Java:

#### a. Extending the `Thread` Class

You can create a new thread by extending the `Thread` class and overriding its `run()` method. 

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running");
    }
    
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start(); // Start the thread
    }
}
```

#### b. Implementing the `Runnable` Interface

A more flexible way is to implement the `Runnable` interface and pass an instance of the class to a `Thread` object.

```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread is running");
    }
    
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread t1 = new Thread(myRunnable);
        t1.start(); // Start the thread
    }
}
```

### 4. Thread States

A thread in Java can be in one of several states:

- **New**: The thread is created but not yet started.
- **Runnable**: The thread is ready to run and waiting for CPU time.
- **Blocked**: The thread is blocked, waiting for a monitor lock.
- **Waiting**: The thread is waiting indefinitely for another thread to perform a particular action.
- **Timed Waiting**: The thread is waiting for another thread to perform an action for up to a specified waiting time.
- **Terminated**: The thread has completed execution.

### 5. Thread Methods

Java provides several methods to control the execution of threads:

- `start()`: Starts the execution of the thread.
- `run()`: Contains the code that defines the task of the thread (should not be called directly, but invoked by `start()`).
- `sleep(long millis)`: Causes the currently executing thread to sleep for a specified number of milliseconds.
- `join()`: Waits for the thread to die.
- `yield()`: Causes the currently executing thread to pause and allow other threads to execute.
- `interrupt()`: Interrupts a thread, causing it to throw an `InterruptedException`.

### 6. Synchronization

When multiple threads need to access shared resources, synchronization is required to prevent data inconsistency and thread interference.

#### Synchronizing a Method

You can use the `synchronized` keyword to make a method synchronized.

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

#### Synchronizing a Block of Code

You can also synchronize a specific block of code within a method.

```java
class Counter {
    private int count = 0;

    public void increment() {
        synchronized(this) {
            count++;
        }
    }

    public int getCount() {
        return count;
    }
}
```

### 7. Deadlock

Deadlock is a situation where two or more threads are blocked forever, waiting for each other. This occurs when multiple threads need the same locks but obtain them in different orders.

#### Example of Deadlock

```java
public class DeadlockExample {
    public static void main(String[] args) {
        final String resource1 = "Resource1";
        final String resource2 = "Resource2";
        
        Thread t1 = new Thread(() -> {
            synchronized(resource1) {
                System.out.println("Thread 1: locked resource 1");
                
                try { Thread.sleep(100);} catch (Exception e) {}
                
                synchronized(resource2) {
                    System.out.println("Thread 1: locked resource 2");
                }
            }
        });
        
        Thread t2 = new Thread(() -> {
            synchronized(resource2) {
                System.out.println("Thread 2: locked resource 2");
                
                try { Thread.sleep(100);} catch (Exception e) {}
                
                synchronized(resource1) {
                    System.out.println("Thread 2: locked resource 1");
                }
            }
        });
        
        t1.start();
        t2.start();
    }
}
```

### 8. Best Practices

- **Minimize the scope of synchronization**: Use synchronized blocks instead of synchronized methods when possible.
- **Avoid nested locks**: This reduces the risk of deadlock.
- **Prefer higher-level concurrency utilities**: Use classes from `java.util.concurrent` package, such as `ExecutorService`, `CountDownLatch`, and `Semaphore`, which provide more robust and flexible concurrency control mechanisms.
- **Use `volatile` for visibility**: Use the `volatile` keyword for variables that are accessed by multiple threads to ensure visibility of changes.
- **Thread-safe collections**: Use thread-safe collections like `ConcurrentHashMap`, `CopyOnWriteArrayList`, etc.

### Example Using `ExecutorService`

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorServiceExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        Runnable task1 = () -> {
            System.out.println("Task 1: " + Thread.currentThread().getName());
        };
        
        Runnable task2 = () -> {
            System.out.println("Task 2: " + Thread.currentThread().getName());
        };
        
        executor.submit(task1);
        executor.submit(task2);
        
        executor.shutdown();
    }
}
```

This example uses `ExecutorService` to manage a pool of threads, making it easier to handle thread creation and lifecycle management.

### Conclusion

Understanding and effectively utilizing threads in Java can significantly enhance the performance and responsiveness of your applications. By following best practices and using the appropriate synchronization mechanisms, you can avoid common pitfalls such as deadlocks and race conditions.

## <u>single treaded and multithreaded applications </u>
### Single-Threaded vs. Multi-Threaded Applications in Java

In Java, applications can be designed to run as single-threaded or multi-threaded programs. Understanding the differences between these two approaches is crucial for effective programming and optimization.

### Single-Threaded Applications

A single-threaded application has only one thread of execution. This thread sequentially processes tasks one after another, meaning that one task must complete before the next one starts. This approach is straightforward and easy to manage but can lead to performance bottlenecks, especially if the application performs I/O operations or other long-running tasks.

#### Characteristics

- **Sequential Execution:** All tasks are executed in sequence.
- **Simplicity:** Easier to write, understand, and debug due to the lack of concurrency issues.
- **Blocking Operations:** The application can be blocked by time-consuming operations, reducing overall performance.

#### Example

```java
public class SingleThreadedExample {
    public static void main(String[] args) {
        // Task 1
        for (int i = 0; i < 5; i++) {
            System.out.println("Task 1 - Count: " + i);
            try { Thread.sleep(1000); } catch (InterruptedException e) { e.printStackTrace(); }
        }
        
        // Task 2
        for (int i = 0; i < 5; i++) {
            System.out.println("Task 2 - Count: " + i);
            try { Thread.sleep(1000); } catch (InterruptedException e) { e.printStackTrace(); }
        }
    }
}
```

### Multi-Threaded Applications

A multi-threaded application uses multiple threads to execute tasks concurrently. This approach can improve the responsiveness and performance of the application by leveraging parallelism and allowing one thread to continue working while another thread waits for I/O operations to complete.

#### Characteristics

- **Concurrent Execution:** Multiple threads execute tasks simultaneously.
- **Complexity:** More complex to write, understand, and debug due to concurrency issues such as race conditions, deadlocks, and thread synchronization.
- **Responsiveness:** Improved performance and responsiveness, especially in I/O-bound or CPU-bound applications.

#### Example

```java
public class MultiThreadedExample {
    public static void main(String[] args) {
        Thread task1 = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Task 1 - Count: " + i);
                try { Thread.sleep(1000); } catch (InterruptedException e) { e.printStackTrace(); }
            }
        });
        
        Thread task2 = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Task 2 - Count: " + i);
                try { Thread.sleep(1000); } catch (InterruptedException e) { e.printStackTrace(); }
            }
        });
        
        task1.start();
        task2.start();
    }
}
```

### Advantages of Multi-Threaded Applications

1. **Improved Performance:** Better CPU utilization by performing multiple tasks concurrently.
2. **Increased Responsiveness:** Particularly beneficial for GUI applications where the user interface can remain responsive while performing background tasks.
3. **Better Resource Sharing:** Efficient use of resources by sharing memory and data between threads.

### Challenges of Multi-Threaded Applications

1. **Complexity:** Writing and maintaining concurrent code is more complex than single-threaded code.
2. **Synchronization Issues:** Ensuring that shared resources are accessed safely requires careful synchronization to avoid race conditions and data corruption.
3. **Deadlocks:** Threads can become deadlocked if they wait indefinitely for each other to release resources.

### Synchronization in Multi-Threaded Applications

To prevent concurrent access issues, synchronization mechanisms are used. Java provides several ways to synchronize threads:

#### Synchronized Methods

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

#### Synchronized Blocks

```java
class Counter {
    private int count = 0;

    public void increment() {
        synchronized(this) {
            count++;
        }
    }

    public int getCount() {
        return count;
    }
}
```

### Using Higher-Level Concurrency Utilities

Java provides the `java.util.concurrent` package, which includes higher-level concurrency utilities such as `ExecutorService`, `ConcurrentHashMap`, and `CountDownLatch`. These utilities simplify thread management and synchronization.

#### Example Using `ExecutorService`

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorServiceExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        Runnable task1 = () -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Task 1 - Count: " + i);
                try { Thread.sleep(1000); } catch (InterruptedException e) { e.printStackTrace(); }
            }
        };
        
        Runnable task2 = () -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Task 2 - Count: " + i);
                try { Thread.sleep(1000); } catch (InterruptedException e) { e.printStackTrace(); }
            }
        };
        
        executor.submit(task1);
        executor.submit(task2);
        
        executor.shutdown();
    }
}
```

### Conclusion

Choosing between single-threaded and multi-threaded design depends on the requirements of your application. Single-threaded applications are simpler to implement but can be inefficient for I/O-bound or long-running tasks. Multi-threaded applications, while more complex, can significantly improve performance and responsiveness by enabling concurrent execution of tasks.

When working with multi-threaded applications, it's crucial to handle thread synchronization and avoid common pitfalls such as race conditions and deadlocks. Utilizing Java's concurrency utilities can help manage complexity and improve the robustness of your multi-threaded programs.

## <u> life cycle of a thread </u>
The life cycle of a thread in Java involves several states that a thread can be in during its execution. Understanding these states is crucial for effective multi-threaded programming. The life cycle of a thread in Java includes the following states:

1. **New (or Born)**
2. **Runnable**
3. **Blocked**
4. **Waiting**
5. **Timed Waiting**
6. **Terminated (or Dead)**

### 1. New (or Born)

When a thread is created, it is in the New state. At this point, the thread is not yet running. This state is represented by creating an instance of the `Thread` class but not starting it.

#### Example

```java
Thread thread = new Thread(() -> {
    // Task to be performed by the thread
});
```

### 2. Runnable

A thread enters the Runnable state when its `start()` method is called. In this state, the thread is considered "ready to run" and is moved to the thread pool where it waits for the CPU time to execute.

#### Example

```java
thread.start(); // Thread enters the Runnable state
```

### 3. Running

When the thread scheduler picks a thread from the runnable pool, it enters the Running state. This is when the thread's `run()` method is executed.

### 4. Blocked

A thread enters the Blocked state when it attempts to access a synchronized resource that is currently locked by another thread. The thread remains in this state until it can acquire the lock on the synchronized resource.

### 5. Waiting

A thread enters the Waiting state when it waits for another thread to perform a specific action. This can happen in several scenarios:

- When a thread calls the `wait()` method.
- When a thread calls the `join()` method on another thread without a timeout.
- When a thread calls the `park()` method from the `LockSupport` class.

#### Example

```java
synchronized (obj) {
    obj.wait(); // Thread enters the Waiting state
}
```

### 6. Timed Waiting

A thread enters the Timed Waiting state when it waits for another thread to perform an action for a specified waiting time. This can happen in several scenarios:

- When a thread calls the `sleep(long millis)` method.
- When a thread calls the `wait(long timeout)` method with a timeout.
- When a thread calls the `join(long millis)` method on another thread with a timeout.
- When a thread calls the `parkNanos(long nanos)` or `parkUntil(long deadline)` methods from the `LockSupport` class.

#### Example

```java
Thread.sleep(1000); // Thread enters the Timed Waiting state
```

### 7. Terminated (or Dead)

A thread enters the Terminated state when it has finished executing its `run()` method or when it is terminated by an exception. In this state, the thread's execution is complete, and it cannot be restarted.

#### Example

```java
public class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running");
    }
    
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start();
        
        try {
            thread.join(); // Main thread waits for the child thread to finish
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("Thread is terminated");
    }
}
```

### Summary

The following diagram provides a visual representation of the thread life cycle:

```
        +--------+
        |  New   |
        +--------+
            |
            v
        +--------+
        |Runnable|
        +--------+
            |
            v
        +--------+          +------------+
        | Running |<--------|  Waiting   |
        +--------+          +------------+
            |                   |
            v                   v
        +----------+         +------------+
        | Terminated |<------|Timed Waiting|
        +------------+         +------------+
                                |      ^
                                v      |
                              +---------+
                              | Blocked |
                              +---------+
```

- **New:** Thread is created but not yet started.
- **Runnable:** Thread is ready to run and waiting for CPU time.
- **Running:** Thread is currently executing.
- **Blocked:** Thread is blocked, waiting to acquire a monitor lock.
- **Waiting:** Thread is waiting indefinitely for another thread to perform a particular action.
- **Timed Waiting:** Thread is waiting for another thread to perform an action for a specified waiting time.
- **Terminated:** Thread has completed execution.

### Practical Example with All States

Here's a comprehensive example that demonstrates a thread going through various states:

```java
public class ThreadLifeCycleDemo {
    public static void main(String[] args) {
        final Object lock = new Object();

        Thread thread = new Thread(() -> {
            try {
                System.out.println("Thread is in Runnable state");

                synchronized (lock) {
                    System.out.println("Thread has acquired the lock and is Running");
                    lock.wait(); // Thread enters the Waiting state
                }

                Thread.sleep(1000); // Thread enters the Timed Waiting state
                System.out.println("Thread is waking up from sleep");

            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        System.out.println("Thread is in New state");
        thread.start(); // Thread enters the Runnable state

        try {
            Thread.sleep(500); // Main thread sleeps to ensure child thread enters Waiting state
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        synchronized (lock) {
            lock.notify(); // Notify the waiting thread to continue execution
            System.out.println("Main thread has notified the waiting thread");
        }

        try {
            thread.join(); // Wait for the thread to terminate
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Thread has terminated");
    }
}
```

### Conclusion

Understanding the life cycle of a thread is essential for effective multi-threaded programming in Java. By managing thread states and transitions properly, you can develop robust, efficient, and responsive applications. Always be mindful of synchronization issues and strive to write thread-safe code to avoid common pitfalls such as race conditions and deadlocks.

### understand through images 

<img src = 'https://media.geeksforgeeks.org/wp-content/uploads/20240318155846/Lifecycle-and-States-of-a-Thread-in-Java-1.png' alt = 'image 1 of life cycle of thread'> </img>

<br>
<hr>

<img src = 'https://www.tutorialspoint.com/java/images/thread_life_cycle.jpg' alt = 'image 2 of life cycle of thread'> </img>

## <u>the current thread </u>
In Java, every thread has a reference to itself, which is known as the "current thread." This reference can be obtained by calling the static method `Thread.currentThread()`. Understanding and effectively using the current thread is crucial for managing thread behavior and debugging multi-threaded applications. Here is a full explanation of the current thread in Java:

### The `Thread.currentThread()` Method

The `Thread.currentThread()` method returns a reference to the currently executing thread object. This method is a static method of the `Thread` class.

#### Syntax

```java
public static Thread currentThread()
```

### Use Cases for `Thread.currentThread()`

1. **Identifying the Current Thread:**
   - Useful for logging or debugging purposes to know which thread is executing a particular piece of code.
   
2. **Interrupting the Current Thread:**
   - Can be used to check or handle the interrupt status of the current thread.

3. **Manipulating Thread Properties:**
   - You can get or set various properties of the current thread, such as its name, priority, or daemon status.

### Example: Identifying the Current Thread

Here's a simple example that demonstrates how to use `Thread.currentThread()` to identify the current thread:

```java
public class CurrentThreadExample {
    public static void main(String[] args) {
        System.out.println("Main thread: " + Thread.currentThread().getName());
        
        Thread thread = new Thread(() -> {
            System.out.println("Worker thread: " + Thread.currentThread().getName());
        });
        
        thread.start();
    }
}
```

### Example: Handling Interrupts

Here's an example that shows how to check and handle the interrupt status of the current thread:

```java
public class InterruptExample {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            try {
                for (int i = 0; i < 5; i++) {
                    System.out.println("Working...");
                    Thread.sleep(1000); // Simulate work
                }
            } catch (InterruptedException e) {
                System.out.println("Thread was interrupted: " + Thread.currentThread().isInterrupted());
            }
        });
        
        thread.start();
        
        try {
            Thread.sleep(2000); // Let the thread run for a bit
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        thread.interrupt(); // Interrupt the thread
    }
}
```

### Example: Setting Thread Properties

Here's an example of getting and setting various properties of the current thread:

```java
public class ThreadPropertiesExample {
    public static void main(String[] args) {
        Thread currentThread = Thread.currentThread();
        
        // Get the current thread name
        System.out.println("Current thread name: " + currentThread.getName());
        
        // Set a new name for the current thread
        currentThread.setName("MyMainThread");
        System.out.println("New thread name: " + currentThread.getName());
        
        // Get and set thread priority
        System.out.println("Current thread priority: " + currentThread.getPriority());
        currentThread.setPriority(Thread.MAX_PRIORITY);
        System.out.println("New thread priority: " + currentThread.getPriority());
        
        // Check if the current thread is a daemon thread
        System.out.println("Is current thread a daemon? " + currentThread.isDaemon());
        
        // Set the current thread as a daemon thread (note: cannot set the main thread as daemon)
        // currentThread.setDaemon(true); // Uncommenting this line will throw an exception
    }
}
```

### Working with `ThreadLocal`

`ThreadLocal` provides thread-local variables. Each thread accessing such a variable has its own independently initialized copy of the variable. `ThreadLocal` is particularly useful for maintaining per-thread context (e.g., user sessions) in a multi-threaded environment.

#### Example: Using `ThreadLocal`

```java
public class ThreadLocalExample {
    private static ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 1);
    
    public static void main(String[] args) {
        Runnable task = () -> {
            int value = threadLocal.get();
            System.out.println(Thread.currentThread().getName() + " initial value: " + value);
            threadLocal.set(value + 1);
            System.out.println(Thread.currentThread().getName() + " new value: " + threadLocal.get());
        };
        
        Thread thread1 = new Thread(task, "Thread 1");
        Thread thread2 = new Thread(task, "Thread 2");
        
        thread1.start();
        thread2.start();
    }
}
```

### Understanding `Thread.currentThread()` in Different Contexts

1. **Main Thread:**
   - In the main method, `Thread.currentThread()` returns the main thread, which is created by the JVM to execute the main method.
   
2. **Worker Threads:**
   - In any other method executed by a worker thread, `Thread.currentThread()` returns the reference to that worker thread.

### Practical Considerations

- **Avoid Excessive Use:** While `Thread.currentThread()` is useful, excessive use for simple logging can clutter the code. Use it judiciously.
- **Consistency in Naming:** When setting thread names, use a consistent naming convention to simplify debugging and log analysis.
- **Exception Handling:** Always handle exceptions properly when dealing with interrupts or thread properties to ensure robust multi-threaded applications.

### Conclusion

The `Thread.currentThread()` method is a powerful tool for managing and understanding the behavior of threads in Java. By leveraging this method, developers can gain insights into thread execution, manage thread properties, handle interruptions, and maintain thread-local variables, thus enabling more efficient and effective multi-threaded programming.

## <u>the thread class </u>
The `Thread` class in Java is a fundamental part of the Java concurrency framework. It provides the means to create and manage threads, allowing concurrent execution of multiple tasks. Here is a comprehensive explanation of the `Thread` class, including its methods, usage, and various important concepts.

### Overview of the `Thread` Class

The `Thread` class belongs to the `java.lang` package and extends the `Object` class while implementing the `Runnable` interface. This class provides constructors and methods to create and manage threads at a high level.

### Creating a Thread

There are two primary ways to create a thread in Java:

1. **Extending the `Thread` Class**
2. **Implementing the `Runnable` Interface**

#### 1. Extending the `Thread` Class

You can create a new thread by creating a class that extends the `Thread` class and overriding its `run()` method.

```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running");
    }
    
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start(); // Starts the thread
    }
}
```

#### 2. Implementing the `Runnable` Interface

Another way is to implement the `Runnable` interface and pass an instance of the implementing class to a `Thread` object.

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Thread is running");
    }
    
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread thread = new Thread(myRunnable);
        thread.start(); // Starts the thread
    }
}
```

### Thread States

A thread can be in one of several states during its lifecycle:

1. **New**: A thread that has been created but not yet started.
2. **Runnable**: A thread that is ready to run and is waiting for CPU time.
3. **Blocked**: A thread that is blocked, waiting for a monitor lock.
4. **Waiting**: A thread that is waiting indefinitely for another thread to perform a particular action.
5. **Timed Waiting**: A thread that is waiting for another thread to perform an action for up to a specified waiting time.
6. **Terminated**: A thread that has exited its `run()` method and is no longer alive.

### Important Methods in the `Thread` Class

The `Thread` class provides numerous methods to control the behavior of threads. Here are some of the most important ones:

#### 1. `start()`

This method starts a new thread and calls the `run()` method of the `Thread` object.

```java
thread.start();
```

#### 2. `run()`

This method contains the code that constitutes the new thread's task. It should be overridden in a subclass of `Thread`.

```java
@Override
public void run() {
    // Code to be executed in the new thread
}
```

#### 3. `sleep(long millis)`

This method causes the currently executing thread to sleep (temporarily cease execution) for a specified number of milliseconds.

```java
try {
    Thread.sleep(1000); // Sleep for 1 second
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

#### 4. `join()`

This method allows one thread to wait for the completion of another thread.

```java
try {
    thread.join();
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

#### 5. `yield()`

This method causes the currently executing thread object to temporarily pause and allow other threads to execute.

```java
Thread.yield();
```

#### 6. `interrupt()`

This method interrupts a thread, causing it to throw an `InterruptedException` if it is blocked.

```java
thread.interrupt();
```

#### 7. `isInterrupted()`

This method tests whether the thread has been interrupted.

```java
if (thread.isInterrupted()) {
    // Handle interruption
}
```

#### 8. `getName()` and `setName(String name)`

These methods get and set the name of the thread.

```java
thread.setName("MyThread");
String name = thread.getName();
```

#### 9. `getPriority()` and `setPriority(int newPriority)`

These methods get and set the priority of the thread. Priority values range from `Thread.MIN_PRIORITY` (1) to `Thread.MAX_PRIORITY` (10), with `Thread.NORM_PRIORITY` (5) as the default.

```java
thread.setPriority(Thread.MAX_PRIORITY);
int priority = thread.getPriority();
```

#### 10. `isAlive()`

This method tests if the thread is alive (i.e., it has been started and has not yet terminated).

```java
if (thread.isAlive()) {
    // Thread is still running
}
```

### Daemon Threads

Daemon threads are background threads that do not prevent the JVM from exiting when the program finishes. They are typically used for background tasks such as garbage collection.

#### Setting a Daemon Thread

```java
thread.setDaemon(true);
```

### Example: Creating and Managing Threads

Here is a comprehensive example that demonstrates creating, starting, and managing multiple threads using the `Thread` class:

```java
class MyThread extends Thread {
    public MyThread(String name) {
        super(name);
    }
    
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " - Count: " + i);
            try {
                Thread.sleep(1000); // Sleep for 1 second
            } catch (InterruptedException e) {
                System.out.println(Thread.currentThread().getName() + " was interrupted");
            }
        }
        System.out.println(Thread.currentThread().getName() + " has finished executing");
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread("Thread 1");
        MyThread thread2 = new MyThread("Thread 2");
        
        thread1.start();
        thread2.start();
        
        try {
            thread1.join(); // Wait for thread1 to finish
            thread2.join(); // Wait for thread2 to finish
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("Both threads have finished executing");
    }
}
```

### Summary

The `Thread` class in Java provides a robust framework for creating and managing threads. Key points include:

- **Creating Threads**: Through extending `Thread` or implementing `Runnable`.
- **Thread Lifecycle**: New, Runnable, Blocked, Waiting, Timed Waiting, Terminated.
- **Thread Management Methods**: `start()`, `run()`, `sleep()`, `join()`, `yield()`, `interrupt()`, `isInterrupted()`, `getName()`, `setName()`, `getPriority()`, `setPriority()`, `isAlive()`.
- **Daemon Threads**: Background threads that do not prevent the JVM from exiting.

By leveraging these tools and techniques, developers can efficiently manage concurrent execution in Java applications, leading to improved performance and responsiveness.

## <u>problems in multithreading </u>
Multithreading in Java can lead to various issues and challenges that developers must understand and address to write effective and robust concurrent programs. Here is a full explanation of the common problems in multithreading and how to handle them:

### 1. **Race Conditions**

A race condition occurs when two or more threads access shared data and try to change it simultaneously. The outcome depends on the sequence of execution, leading to unpredictable and erroneous results.

#### Example

```java
class Counter {
    private int count = 0;

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class RaceConditionExample {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread thread1 = new Thread(task);
        Thread thread2 = new Thread(task);

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println("Final count: " + counter.getCount());
    }
}
```

#### Solution: Synchronization

Use `synchronized` blocks or methods to ensure that only one thread can access the shared data at a time.

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

### 2. **Deadlock**

A deadlock occurs when two or more threads are blocked forever, waiting for each other to release resources. This happens when threads acquire locks in different orders, creating a circular dependency.

#### Example

```java
public class DeadlockExample {
    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Thread 1: Holding lock 1...");
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                System.out.println("Thread 1: Waiting for lock 2...");
                synchronized (lock2) {
                    System.out.println("Thread 1: Holding lock 1 and 2...");
                }
            }
        });

        Thread thread2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("Thread 2: Holding lock 2...");
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                System.out.println("Thread 2: Waiting for lock 1...");
                synchronized (lock1) {
                    System.out.println("Thread 2: Holding lock 2 and 1...");
                }
            }
        });

        thread1.start();
        thread2.start();
    }
}
```

#### Solution: Lock Ordering and Timeout

1. **Lock Ordering**: Ensure all threads acquire locks in the same order.
2. **Timeout**: Use timed locks (e.g., `tryLock` in `ReentrantLock`) to avoid waiting indefinitely.

```java
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.Lock;

public class DeadlockSolution {
    private static final Lock lock1 = new ReentrantLock();
    private static final Lock lock2 = new ReentrantLock();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            try {
                if (lock1.tryLock() && lock2.tryLock()) {
                    System.out.println("Thread 1: Holding lock 1 and 2...");
                }
            } finally {
                lock1.unlock();
                lock2.unlock();
            }
        });

        Thread thread2 = new Thread(() -> {
            try {
                if (lock2.tryLock() && lock1.tryLock()) {
                    System.out.println("Thread 2: Holding lock 2 and 1...");
                }
            } finally {
                lock1.unlock();
                lock2.unlock();
            }
        });

        thread1.start();
        thread2.start();
    }
}
```

### 3. **Livelock**

A livelock occurs when threads keep changing their state in response to each other without making any progress. Unlike deadlocks, threads are not blocked but are too busy responding to each other.

#### Example

```java
public class LivelockExample {
    static class Spoon {
        private Diner owner;
        public Spoon(Diner d) { owner = d; }
        public synchronized void use() { System.out.println(owner.name + " is eating!"); }
    }

    static class Diner {
        private String name;
        private boolean isHungry;
        public Diner(String n) { name = n; isHungry = true; }

        public String getName() { return name; }
        public boolean isHungry() { return isHungry; }
        public void eatWith(Spoon spoon, Diner spouse) {
            while (isHungry) {
                if (spoon.owner != this) {
                    try { Thread.sleep(1); } catch (InterruptedException e) { continue; }
                    continue;
                }

                if (spouse.isHungry()) {
                    System.out.println(name + ": You eat first, " + spouse.getName());
                    spoon.owner = spouse;
                    continue;
                }

                spoon.use();
                isHungry = false;
                System.out.println(name + ": I am done eating.");
                spoon.owner = spouse;
            }
        }
    }

    public static void main(String[] args) {
        Diner husband = new Diner("Husband");
        Diner wife = new Diner("Wife");
        Spoon spoon = new Spoon(husband);

        new Thread(() -> husband.eatWith(spoon, wife)).start();
        new Thread(() -> wife.eatWith(spoon, husband)).start();
    }
}
```

#### Solution: Coordination and Backoff

Implement proper coordination between threads and use a backoff strategy to avoid continuous retries.

### 4. **Starvation**

Starvation occurs when a thread is perpetually denied access to resources it needs for execution, often because other threads are monopolizing the resources.

#### Example

```java
public class StarvationExample {
    public static void main(String[] args) {
        Runnable task = () -> {
            for (int i = 0; i < 5; i++) {
                System.out.println(Thread.currentThread().getName() + " is running");
            }
        };

        Thread highPriorityThread = new Thread(task);
        Thread lowPriorityThread = new Thread(task);

        highPriorityThread.setPriority(Thread.MAX_PRIORITY);
        lowPriorityThread.setPriority(Thread.MIN_PRIORITY);

        highPriorityThread.start();
        lowPriorityThread.start();
    }
}
```

#### Solution: Fairness Policy

Use fair locks (e.g., `ReentrantLock(true)`) to ensure all threads get a fair chance.

```java
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.Lock;

public class FairLockExample {
    private static final Lock lock = new ReentrantLock(true);

    public static void main(String[] args) {
        Runnable task = () -> {
            for (int i = 0; i < 5; i++) {
                try {
                    lock.lock();
                    System.out.println(Thread.currentThread().getName() + " is running");
                } finally {
                    lock.unlock();
                }
            }
        };

        Thread thread1 = new Thread(task, "Thread 1");
        Thread thread2 = new Thread(task, "Thread 2");

        thread1.start();
        thread2.start();
    }
}
```

### 5. **Thread Interference**

Thread interference occurs when multiple threads access shared data without proper synchronization, leading to inconsistent results.

#### Example

```java
class Counter {
    private int count = 0;

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class ThreadInterferenceExample {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread thread1 = new Thread(task);
        Thread thread2 = new Thread(task);

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println("Final count: " + counter.getCount()); // Inconsistent result
    }
}
```

#### Solution: Synchronization

Use `synchronized` methods or blocks to ensure atomicity.

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

### 6. **Memory Consistency Errors**

Memory consistency errors occur when different threads have inconsistent views of the shared data, leading to unpredictable results.

#### Example

```java
class SharedData {
    private boolean ready = false;

    public void setReady() {
        ready = true;
    }

    public boolean isReady() {
        return ready;
    }
}

public class MemoryConsistencyExample {
    public static void main(String[] args) {
        SharedData data = new SharedData();

        Thread writer = new Thread(() -> {
            data.setReady();
        });

        Thread reader = new Thread(() -> {
            while (!data.isReady()) {
                // Busy-waiting
            }
            System.out.println("Data is ready!");
        });

        writer.start();
        reader.start();
    }
}
```

#### Solution: Volatile Keyword and Synchronization

Use the `volatile` keyword or synchronization to

 ensure visibility of shared data.

```java
class SharedData {
    private volatile boolean ready = false;

    public void setReady() {
        ready = true;
    }

    public boolean isReady() {
        return ready;
    }
}
```

### Conclusion

Multithreading in Java introduces several complex problems, including race conditions, deadlocks, livelocks, starvation, thread interference, and memory consistency errors. By understanding these issues and applying appropriate solutions such as synchronization, lock ordering, fair locks, and the `volatile` keyword, developers can write robust and efficient concurrent programs. Proper use of concurrency utilities provided by the `java.util.concurrent` package also helps in managing multithreading effectively.

## <u>synchronization </u>
Synchronization in Java is a mechanism that ensures that multiple threads can access shared resources or data without leading to inconsistent states. This is essential in concurrent programming to prevent race conditions, maintain data integrity, and avoid unexpected behavior.

### Why Synchronization is Necessary

In a multithreaded environment, multiple threads might try to read or modify shared data simultaneously. Without proper synchronization, this can lead to race conditions where the output depends on the sequence of thread execution. Synchronization ensures that only one thread can access the shared resource at a time, thus maintaining consistency and correctness.

### Types of Synchronization

1. **Synchronized Methods**
2. **Synchronized Blocks**
3. **Static Synchronization**

### 1. Synchronized Methods

When a method is declared synchronized, the thread holds the lock (monitor) for that object's method. No other thread can access synchronized methods of that object until the lock is released.

#### Example

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}

public class SynchronizedMethodExample {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread thread1 = new Thread(task);
        Thread thread2 = new Thread(task);

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println("Final count: " + counter.getCount());
    }
}
```

### 2. Synchronized Blocks

Synchronized blocks are used to synchronize only a part of a method. This is useful when you need to lock only a specific section of code rather than the entire method, thus improving concurrency.

#### Example

```java
class Counter {
    private int count = 0;
    private final Object lock = new Object();

    public void increment() {
        synchronized (lock) {
            count++;
        }
    }

    public int getCount() {
        synchronized (lock) {
            return count;
        }
    }
}

public class SynchronizedBlockExample {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread thread1 = new Thread(task);
        Thread thread2 = new Thread(task);

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println("Final count: " + counter.getCount());
    }
}
```

### 3. Static Synchronization

When you synchronize a static method, the lock is on the class object, not on an instance of the class. This is useful when you want to prevent multiple threads from accessing static data concurrently.

#### Example

```java
class Counter {
    private static int count = 0;

    public static synchronized void increment() {
        count++;
    }

    public static synchronized int getCount() {
        return count;
    }
}

public class StaticSynchronizationExample {
    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                Counter.increment();
            }
        };

        Thread thread1 = new Thread(task);
        Thread thread2 = new Thread(task);

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println("Final count: " + Counter.getCount());
    }
}
```

### Synchronization Locks

1. **Intrinsic Locks (Monitor Locks)**: Every object in Java has an intrinsic lock associated with it. When a thread enters a synchronized method or block, it acquires the intrinsic lock for that object. Other threads cannot enter any synchronized method/block for that object until the lock is released.

2. **Explicit Locks**: The `java.util.concurrent.locks` package provides explicit lock classes like `ReentrantLock`, which offer more flexible locking mechanisms.

#### Example with `ReentrantLock`

```java
import java.util.concurrent.locks.ReentrantLock;

class Counter {
    private int count = 0;
    private final ReentrantLock lock = new ReentrantLock();

    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }

    public int getCount() {
        lock.lock();
        try {
            return count;
        } finally {
            lock.unlock();
        }
    }
}

public class ReentrantLockExample {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread thread1 = new Thread(task);
        Thread thread2 = new Thread(task);

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println("Final count: " + counter.getCount());
    }
}
```

### Volatile Keyword

The `volatile` keyword in Java ensures that the value of a variable is always read from and written to the main memory, thus providing visibility guarantees. It is often used for flags or state indicators but does not provide atomicity or mutual exclusion.

#### Example

```java
class VolatileExample {
    private volatile boolean running = true;

    public void run() {
        while (running) {
            System.out.println("Running...");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }

    public void stop() {
        running = false;
    }

    public static void main(String[] args) throws InterruptedException {
        VolatileExample example = new VolatileExample();
        Thread thread = new Thread(example::run);
        thread.start();

        Thread.sleep(500);
        example.stop();
    }
}
```

### Best Practices for Synchronization

1. **Minimize the Scope of Synchronization**: Synchronize only the necessary parts of the code to improve concurrency.
2. **Prefer Synchronized Blocks Over Methods**: Synchronized blocks give more control and flexibility.
3. **Use Concurrent Utilities**: Leverage classes from `java.util.concurrent` package like `ConcurrentHashMap`, `AtomicInteger`, and others which provide thread-safe operations.
4. **Avoid Nested Locks**: Nested locks can lead to deadlocks.
5. **Use Volatile for Simple Flags**: For simple state flags or indicators, `volatile` is simpler and more performant than synchronization.
6. **Consider Read-Write Locks**: For scenarios with many read operations and fewer write operations, use `ReadWriteLock` to allow concurrent reads.

### Conclusion

Synchronization is a crucial aspect of concurrent programming in Java, ensuring thread safety and data consistency. By understanding and properly using synchronization mechanisms like synchronized methods, blocks, static synchronization, and explicit locks, developers can prevent common concurrency issues such as race conditions, deadlocks, and visibility problems. Proper synchronization practices and the use of concurrent utilities help build robust and efficient multithreaded applications.