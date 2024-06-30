---
title: Executor Framework in Java
date: 2016-05-20 08:25
categories: Java
tags:
- java
- executor
- thread
---
#### Overview
Typically, when use Java to develop a simple concurrent applications, will create some *Runnable* object, and then create the corresponding *Thread* object to perform them. However, if we need to develop a program to run a large number of concurrent tasks, method highlights a lot of disadvantages. For example, we nned to create a thread ojbect for each task, creating too many thread will cause the system to overload.

We all known about that there are two ways to create a thread in java. Creating a thread in java is very expensive process which includes memory overhead also. So it's a good idea if we can re-use these thread once created, to run our future runnables.

*Executors Framework* (java.util.concurrent.Executor), released with the JDK 1.5 in package java.util.concurrent is used to run the Runnable objects without creating new threads every time and mostly re-using the already created thread.

The `Executor Framework` separates the creation and execution of tasks by using an *Executor*, which requires only objects that implement the *Runnable* interface, and the sends them to the *Executor*. The *Executor* also privides a thread pool to improve the performance of the application. When a task is sent to the *Executor*, The Executor tries to use the thread in thread pool to perform this task, avoiding the continuous creation and destruction of the thread, resulting in system performance degradation.

Another advantages of this framework is the *Callable* interface, which is similar to the *Runnable* interface, but it provides two enhancements.
- This interface in the *call* method can return a result.
- When a Callable object is sent to *Executor* will get an object implemented the *Future* interface, you can use this object to control the status and results of the *callable* object.

#### Basic Usage
Demo 1

``` java

import java.util.Date;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

/**
 * GradleDemo Created by lechance .
 */
public class ExecutorDemo {


    public static void main(String[] args) {

        Server server = new ExecutorDemo.Server();
        for (int i = 0; i < 10; i++) {
            Task task = new Task("task" + i);
            server.executeTask(task);
        }
        server.endServer();
    }


    static class Server {
        private ThreadPoolExecutor mExecutor;
        private Task mTask;

        public Server() {
//            mExecutor = (ThreadPoolExecutor) Executors.newCachedThreadPool();
            mExecutor = (ThreadPoolExecutor) Executors.newFixedThreadPool(5);
        }

        public void executeTask(final Task task) {
            System.out.printf("Server: A new task[%s] has arrived%n", task.getName());
            mExecutor.execute(task);
            System.out.printf("Server: Pool size: %d%n", mExecutor.getPoolSize());
            System.out.printf("Server: Active count: %d%n", mExecutor.getActiveCount());
            System.out.printf("Server: Completed task: %d%n", mExecutor.getCompletedTaskCount());
            System.out.printf("Server: Core pool size: %d%n", mExecutor.getCorePoolSize());
            System.out.printf("Server: Task count: %d%n", mExecutor.getTaskCount());
        }

        public void endServer() {
            mExecutor.shutdown();
        }
    }
}


class Task implements Runnable {

    private Date initDate;
    private String name;

    public Task(String name) {
        initDate = new Date();
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
    public void run() {
        System.out.printf("%s: Task %s: Created on: %s%n", Thread.currentThread().getName(), name, initDate);
        System.out.printf("%s: Task %s: Started on: %s%n", Thread.currentThread().getName(), name, new Date());

        //let thread sleep for random time
        try {
            int duration = (int) (Math.random() * 10);
            System.out.printf("%s: Task %s: Doing a task during %d seconds%n", Thread.currentThread().getName(), name, duration);
            TimeUnit.SECONDS.sleep(duration);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.printf("%s: Task %s: Finished on: %s%n", Thread.currentThread().getName(), name, new Date());
    }
}


```

Demo 2

```java

import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.concurrent.*;

/**
 * GradleDemo Created by lechance .
 */
public class CallableDemo {

    public static void main(String[] args) {
        ThreadPoolExecutor mExecutor = (ThreadPoolExecutor) Executors.newFixedThreadPool(2);
        List<Future<Integer>> results = new ArrayList<>();
        Random random = new Random();

        for (int i = 0; i < 10; i++) {
            int num = random.nextInt(10);
            FactorialCalculator fc = new FactorialCalculator(num);
            Future<Integer> future = mExecutor.submit(fc);
            results.add(future);
        }

        do {
            System.out.printf("Main: number of completed task: %d%n", mExecutor.getCompletedTaskCount());
            for (int i = 0; i < results.size(); i++) {
                Future<Integer> future = results.get(i);
                System.out.printf("Main: Task %d %s%n", i, future.isDone());

                try {
                    TimeUnit.SECONDS.sleep(5);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.printf("");
        } while (mExecutor.getCompletedTaskCount() < results.size());

        System.out.println("Main: Result");

        for (int i = 0; i < results.size(); i++) {
            Future<Integer> f = results.get(i);
            try {
                System.out.printf("Main: Task %d: result: %d%n", i, f.get());
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }
        }

        //finally shutdown executor
        mExecutor.shutdown();
    }
}

class FactorialCalculator implements Callable<Integer> {

    private int number;

    public FactorialCalculator(Integer number) {
        this.number = number;
    }

    @Override
    public Integer call() throws Exception {
        int result = 1;
        if (number == 0 || number == 1) {
            result = 1;
        } else {
            for (int i = 2; i <= number; i++) {
                result *= i;
                TimeUnit.SECONDS.sleep(2);
            }
        }
        return result;
    }
}


```

#### Related Articles

[The volatile Keyword](http://www.cnblogs.com/dolphin0520/p/3920373.html)


