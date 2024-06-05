

## Multi-threading in Java

Multithreading in Java allows concurrent execution of two or more threads, making optimal use of the available CPU resources. Here’s a detailed guide on how to implement and run multithreading in Java:

### 1. Understanding Threads in Java

- Thread: A thread is the smallest unit of a process that can be scheduled for execution. Java provides built-in support for multithreaded programming through the `java.lang.Thread` class and the `java.util.concurrent` package.
- Runnable Interface: This interface should be implemented by any class whose instances are intended to be executed by a thread. The class must define a method of no arguments called `run`.

### 2. Creating Threads in Java

There are two primary ways to create and run threads in Java:

#### Method 1: Extending the `Thread` Class

1. Create a new class that extends `Thread`:
2. Override the `run` method:
3. Create an instance of the class and call `start` to begin execution:

```java
class MyThread extends Thread {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getId() + " Value: " + i);
            try {
                Thread.sleep(500); // Sleep for 500 milliseconds
            } catch (InterruptedException e) {
                System.out.println(e);
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();
        t1.start();
        t2.start();
    }
}
```

#### Method 2: Implementing the `Runnable` Interface

1. Create a new class that implements `Runnable`:
2. Override the `run` method:
3. Create an instance of `Thread`, passing an instance of the class to the constructor, and call `start`:

```java
class MyRunnable implements Runnable {
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getId() + " Value: " + i);
            try {
                Thread.sleep(500); // Sleep for 500 milliseconds
            } catch (InterruptedException e) {
                System.out.println(e);
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread(new MyRunnable());
        Thread t2 = new Thread(new MyRunnable());
        t1.start();
        t2.start();
    }
}
```

### 3. Thread Lifecycle

- New: A thread that is newly created but not yet started.
- Runnable: A thread that is ready to run and is waiting for CPU time.
- Running: A thread that is currently executing.
- Blocked: A thread that is blocked waiting for a monitor lock.
- Waiting: A thread that is waiting indefinitely for another thread to perform a particular action.
- Timed Waiting: A thread that is waiting for another thread to perform an action within a specified waiting time.
- Terminated: A thread that has completed its execution or has been terminated.

### 4. Thread Synchronization

When multiple threads access shared resources, synchronization is required to avoid data inconsistency. Java provides several mechanisms for synchronization:

#### Synchronized Method

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

public class Main {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Count: " + counter.getCount());
    }
}
```

#### Synchronized Block

```java
class Counter {
    private int count = 0;

    public void increment() {
        synchronized (this) {
            count++;
        }
    }

    public int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Count: " + counter.getCount());
    }
}
```

### 5. Java Concurrency Utilities

Java provides high-level concurrency utilities in the `java.util.concurrent` package to simplify working with threads.

#### Executor Framework

The Executor framework provides a higher-level replacement for working with threads directly.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        Runnable task1 = () -> {
            for (int i = 0; i < 5; i++) {
                System.out.println(Thread.currentThread().getId() + " Task 1 - Value: " + i);
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    System.out.println(e);
                }
            }
        };

        Runnable task2 = () -> {
            for (int i = 0; i < 5; i++) {
                System.out.println(Thread.currentThread().getId() + " Task 2 - Value: " + i);
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    System.out.println(e);
                }
            }
        };

        executor.submit(task1);
        executor.submit(task2);

        executor.shutdown();
    }
}
```

### 6. Advanced Synchronization Utilities

Java provides several advanced synchronization utilities in the `java.util.concurrent` package:

#### CountDownLatch

A synchronization aid that allows one or more threads to wait until a set of operations being performed in other threads completes.

```java
import java.util.concurrent.CountDownLatch;

public class Main {
    public static void main(String[] args) {
        CountDownLatch latch = new CountDownLatch(3);

        Runnable task = () -> {
            System.out.println(Thread.currentThread().getId() + " Task started");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                System.out.println(e);
            }
            System.out.println(Thread.currentThread().getId() + " Task completed");
            latch.countDown();
        };

        for (int i = 0; i < 3; i++) {
            new Thread(task).start();
        }

        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("All tasks are completed.");
    }
}
```

#### CyclicBarrier

A synchronization aid that allows a set of threads to all wait for each other to reach a common barrier point.

```java
import java.util.concurrent.CyclicBarrier;

public class Main {
    public static void main(String[] args) {
        CyclicBarrier barrier = new CyclicBarrier(3, () -> {
            System.out.println("All parties have reached the barrier.");
        });

        Runnable task = () -> {
            System.out.println(Thread.currentThread().getId() + " Task started");
            try {
                Thread.sleep(1000);
                barrier.await();
            } catch (Exception e) {
                System.out.println(e);
            }
            System.out.println(Thread.currentThread().getId() + " Task completed");
        };

        for (int i = 0; i < 3; i++) {
            new Thread(task).start();
        }
    }
}
```

### Conclusion

Multithreading in Java allows you to run multiple threads concurrently, which can improve the performance and responsiveness of your applications. By understanding the basic concepts, creating threads using different methods, managing thread lifecycle, and using synchronization techniques and concurrency utilities, you can effectively implement multithreading in Java. The examples provided illustrate various ways to achieve multithreading and concurrency in Java, making your applications more efficient and robust.
